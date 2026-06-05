---
title: "Methodology & Code"
weight: 3
---

# Methodology: Downstream Analysis Pipeline
**From Raw Counts to Biological Pathways**

This section details the analytical pipeline used to process the transcriptomic data after the initial preprocessing (FastQC, Trimmomatic, and Salmon quantification). The entire downstream analysis was implemented in Python using `pydeseq2`, `gseapy`, and `seaborn`.

### 1. Data Import and Normalization
Raw transcript counts from Salmon (`quant.sf` files) were aggregated into a master count matrix. Before generating exploratory plots, the data required normalization to account for library size differences and heteroscedasticity. We utilized the **Variance Stabilizing Transformation (VST)** from the `pydeseq2` package.


```python
import pandas as pd
import os
from pydeseq2.dds import DeseqDataSet

# Load metadata and prepare sample associations
meta = pd.read_csv("SraRunTable.csv")
coldata = meta[['Run', 'treatment']].copy()
coldata.rename(columns={'Run': 'sample', 'treatment': 'condition'}, inplace=True)
coldata.set_index('sample', inplace=True)

# Construct the raw count matrix from Salmon output
counts_list = []
for sample_id in coldata.index:
    file_path = os.path.join("salmon_quants", f"{sample_id}_quant", "quant.sf")
    df = pd.read_csv(file_path, sep='\t', usecols=['Name', 'NumReads'])
    df.set_index('Name', inplace=True)
    df.columns = [sample_id]
    counts_list.append(df)

count_matrix = pd.concat(counts_list, axis=1).T.round().astype(int)

# Initialize DESeq2 Dataset and apply Variance Stabilizing Transformation (VST)
dds = DeseqDataSet(counts=count_matrix, metadata=coldata, design_factors="condition", n_cpus=8)
dds.vst()
vst_df = pd.DataFrame(dds.layers['vst_counts'], index=dds.obs_names, columns=dds.var_names)
```
### 2. Exploratory Data Analysis
To assess biological replicate consistency and rule out technical batch effects, we performed Principal Component Analysis (PCA) and Hierarchical Clustering using the top 500 most variable genes. We evaluated three principal components to ensure accurate spatial representation of the samples.

```Python 
from sklearn.decomposition import PCA
from scipy.spatial import distance
import seaborn as sns
import matplotlib.pyplot as plt

# Isolate the top 500 most variable genes across all samples
top_genes = vst_df.var(axis=0).sort_values(ascending=False).index[:500]

# Perform PCA
pca = PCA(n_components=3)
pca_results = pca.fit_transform(vst_df[top_genes])
pca_df = pd.DataFrame(pca_results, columns=['PC1', 'PC2', 'PC3'], index=vst_df.index).join(coldata)

# Calculate Euclidean sample distances for hierarchical clustering
sample_dists = distance.squareform(distance.pdist(vst_df, metric='euclidean'))
dist_df = pd.DataFrame(sample_dists, index=vst_df.index, columns=vst_df.index)

# Generate clustered heatmap
cg = sns.clustermap(dist_df, cmap="Blues_r", annot=True, fmt=".0f")
plt.savefig("Sample_Distance_Heatmap.png")
```


### 3. Differential Expression Analysis (DEA)
Differential expression was modeled using the Negative Binomial distribution natively in pydeseq2. The contrast evaluated the effect of long-term microgravity (ISS uG) versus 1G ground controls. Strict thresholds were applied to define significantly altered genes: Adjusted P-value (FDR) < 0.05 and |Log2 Fold Change| > 1.0.

```python
from pydeseq2.ds import DeseqStats

# Fit the DESeq2 model
dds.deseq2()

# Extract statistics for the specific contrast (uG vs 1G)
stat_res = DeseqStats(dds, contrast=('condition', 'ISS uG', 'ISS 1G'))
stat_res.summary()
res_df = stat_res.results_df

# Filter for significant Differentially Expressed Genes (DEGs)
sig_genes = res_df[(res_df['padj'] < 0.05) & (res_df['log2FoldChange'].abs() > 1.0)]
```
Note: Transcript IDs (ENST...) were translated to official Gene Symbols using the mygene API prior to downstream pathway analysis.

### 4. Pathway and Functional Enrichment Analysis
To translate raw gene lists into biological meaning, we employed two distinct but complementary approaches using the `gseapy` library across both KEGG and Gene Ontology gene set databases.

### 4.1 Over-Representation Analysis (ORA) — KEGG
ORA was performed using the strictly filtered list of significant DEGs to identify explicitly over-represented biochemical pathways. Visualizations were constructed manually using seaborn to retain control over the top-N selection.

```python
import gseapy as gp

# Run Enrichr API for Over-Representation Analysis
ora_res = gp.enrichr(
    gene_list=sig_genes['Gene_Symbol'].tolist(),
    gene_sets=['KEGG_2021_Human'],
    organism='human'
)

# Extract and manually plot the top 15 terms to retain exploratory hits
kegg_df = ora_res.res2d.sort_values('Adjusted P-value').head(15)
sns.scatterplot(data=kegg_df, x='Combined Score', y='Term', size='Overlap', hue='Adjusted P-value')
```
### 4.2 Gene Set Enrichment Analysis (GSEA) — KEGG
Unlike ORA, GSEA does not rely on arbitrary fold-change thresholds. The entire mapped transcriptome was ranked using the `stat` parameter (Wald test statistic) from DESeq2, allowing detection of subtle, coordinated shifts across entire gene networks.

```python
# Create a ranked list of all genes based on the DESeq2 Wald statistic
ranked_df = df[['Gene_Symbol', 'stat']].groupby('Gene_Symbol').mean().sort_values(by='stat', ascending=False)

# Execute Preranked GSEA
gsea_res = gp.prerank(
    rnk=ranked_df,
    gene_sets="KEGG_2021_Human.gmt", 
    permutation_num=1000,
    seed=6
)
```

### 4.3 GO Over-Representation Analysis
To complement the KEGG pathway analysis, ORA was also performed against two Gene Ontology databases — Biological Process and Molecular Function — using the same filtered DEG list. This satisfies the requirement to use both GO terms and KEGG as gene set sources.

```python
go_ora_res = gp.enrichr(
    gene_list=sig_genes,
    gene_sets=['GO_Biological_Process_2023', 'GO_Molecular_Function_2023'],
    organism='human',
    outdir='Enrichr_ORA_Results/GO'
)

for go_db in ['GO_Biological_Process_2023', 'GO_Molecular_Function_2023']:
    go_subset = go_ora_res.res2d[
        go_ora_res.res2d['Gene_set'] == go_db
    ].sort_values('Adjusted P-value').head(15)

    short = 'GO_BP' if 'Biological' in go_db else 'GO_MF'
    sns.scatterplot(data=go_subset, x='Combined Score', y='Term',
                    size='Overlap', hue='Adjusted P-value', palette='viridis_r')
    plt.savefig(f"webPortal/static/images/{short}_ORA_Dotplot.png", dpi=300)
```

### 4.4 GO Gene Set Enrichment Analysis
Preranked GSEA was executed against GO Biological Process and GO Molecular Function databases using the same Wald-statistic ranked list as the KEGG GSEA. A horizontal barplot of the top 10 terms by |NES| was generated for each database as a summary visualization.

```python
for go_db, outname in [('GO_Biological_Process_2023', 'GO_BP_GSEA'),
                        ('GO_Molecular_Function_2023', 'GO_MF_GSEA')]:
    go_gsea = gp.prerank(
        rnk=ranked_df,
        gene_sets=go_db,
        threads=4,
        min_size=10,
        max_size=500,
        permutation_num=1000,
        outdir=f'GSEA_Results/{outname}',
        seed=6
    )
    # top 10 by |NES| barplot saved to webPortal/static/images/
    short = 'GO_BP' if 'Biological' in go_db else 'GO_MF'
    plt.savefig(f"webPortal/static/images/{short}_GSEA_Barplot.png", dpi=300)
```

### 5. Multi-Evidence Integration (ORA vs GSEA)
To synthesize the findings and isolate the most robust biological signals, ORA and GSEA metrics were integrated into a single coordinate system. The Two-Evidence Plot maps the $-log_{10}(\text{Adjusted P-value})$ from ORA against the $-log_{10}(\text{FDR q-value})$ from GSEA.

> **Note on SPIA:** Signaling Pathway Impact Analysis (SPIA) requires KEGG pathway topology graphs and lacks a robust, maintained Python implementation. As an equivalent multi-evidence synthesis, we perform a direct ORA × GSEA integration, which captures both count-based over-representation and rank-based enrichment signals in a single coordinate system.

```python
import numpy as np

# Merge ORA and GSEA results on cleaned pathway names
combined_df = pd.merge(ora_df, gsea_df, on='Term_clean', how='inner')

# Calculate log-transformed significance safely
combined_df['ORA_logP'] = -np.log10(combined_df['Adjusted P-value'] + 1e-10)
combined_df['GSEA_logFDR'] = -np.log10(combined_df['FDR q-val'] + 1e-10)

# Plot integration
plt.scatter(
    combined_df['ORA_logP'], 
    combined_df['GSEA_logFDR'], 
    c=combined_df['NES'], 
    cmap='coolwarm'
)
# Add significance thresholds
plt.axhline(-np.log10(0.05), color='k', linestyle='--')
plt.axvline(-np.log10(0.05), color='k', linestyle='--')
plt.savefig("Two_Evidence_Plot.png")
```