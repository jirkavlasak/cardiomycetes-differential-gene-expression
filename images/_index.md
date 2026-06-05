---
title: "Final Analysis Report"
weight: 1
---

# Comprehensive Transcriptomic Analysis of Human Cardiomyocytes in Microgravity

## 1. Introduction
This report presents a full bioinformatic pipeline analysis of RNA-Seq data from human iPSC-derived cardiomyocytes cultured on the International Space Station (ISS). The study compares cells exposed to long-term microgravity (**uG**) against ground-control 1G samples (**1G**) to identify molecular adaptations to the space environment.

---

## 2. Exploratory Data Analysis & Biological QC

### 2.1 Methodology
Raw sequencing counts were normalized using the **Variance Stabilizing Transformation (VST)** from the `DESeq2` package. To focus on the most informative signals, exploratory analysis was performed on the top 500 genes with the highest variance across all samples.

### 2.2 Sample Similarity & Clustering
To assess the quality of biological replicates and identify potential outliers, we employed hierarchical clustering and Principal Component Analysis (PCA).

* **Hierarchical Clustering:** The sample distance heatmap reveals clear primary clusters corresponding to the experimental conditions (uG vs 1G).
* **PCA Analysis:** The PCA plot shows that Principal Component 1 (PC1) captures the majority of the variance, effectively separating the two gravitational conditions.

![Sample Distance Heatmap](/images/Sample_Distance_Heatmap.png)
*Figure 1: Euclidean distance between samples based on VST-normalized counts.*

![PCA Plot](/images/PCA_plot_exploratory.png)
*Figure 2: PCA analysis of the top 500 variable genes. PC1 clearly separates the uG and 1G groups.*

### 2.3 Batch Effect Assessment
A critical step in our QC was the search for technical batch effects. Visual inspection of the PCA and clustering results confirmed that samples grouped strictly by biological condition rather than technical parameters (e.g., sequencing run or library prep date). **As no significant batch effect was detected, no further correction (e.g., ComBat) was required for this dataset.**

---

## 3. Differential Expression Analysis (DEA)

### 3.1 Statistical Modeling
Differential expression was modeled using the negative binomial distribution provided by `DESeq2`. The experimental design was defined as `~ condition` (uG vs 1G). 

**Thresholds for Significance:**
* **FDR (Adjusted P-value):** < 0.05
* **Effect Size (|log2FoldChange|):** > 1.0

### 3.2 Results Visualization
The global transcriptional response is visualized via a Volcano Plot and a heatmap of the most significant Differentially Expressed Genes (DEGs).

![Volcano Plot](/images/Volcano_plot_cardiomyocytes.png)
![Top DEGs Heatmap](/images/Top50_DEGs_Heatmap.png)

### 3.3 Individual Gene Evidence
To validate the consistency of the expression changes, we generated boxplots of normalized counts for the top 6 most significant genes, including **CKM** (Creatine Kinase) and **H4C3**.

![Top Gene Boxplots](/images/Top6_DEGs_Boxplots.png)

### 3.4 Detailed DEGs Table
The full list of 100 most significant genes can be explored in the interactive table below:

<iframe src="../../reports/Top100_DEGs_Table.html" width="100%" height="500px" frameborder="0"></iframe>

---

## 4. Functional Enrichment Analysis

### 4.1 Over-Representation Analysis (ORA)
Using the `clusterProfiler` and `goseq` methods (correcting for gene length bias), we identified biological processes and pathways over-represented among our DEGs. Key findings include the **HIF-1 Signaling Pathway** and **Focal Adhesion** disruptions.

![ORA KEGG Dotplot](/images/ORA_KEGG_Dotplot.png)

### 4.2 Gene Set Enrichment Analysis (GSEA)
GSEA was performed on the entire ranked transcriptome to capture subtle but coordinated shifts in entire pathways. 

![GSEA Enrichment Plot - Diabetic Cardiomyopathy](/images/GSEA_Diabetic.png)
*Figure 3: GSEA enrichment score for the Diabetic Cardiomyopathy pathway, showing strong positive enrichment in microgravity.*

---

## 5. Pathway Topology & Integration

### 5.1 SPIA and Two-Evidence Plot
To gain a robust understanding of the results, we integrated ORA (focusing on count) and GSEA (focusing on ranking) into a **Two-Evidence Plot**. This allows us to pinpoint pathways that are significant in both methodologies.

![Two-Evidence Plot](/images/Two_Evidence_Plot.png)
*Figure 4: Integration of ORA and GSEA evidence. Pathways in the top-right quadrant represent the most robust biological signals.*

### 5.2 KEGG Pathview
Significant metabolic pathways were further visualized using the `pathview` package, overlaying log2FC values onto the official KEGG pathway maps to identify specific bottlenecks and activated nodes.

---

## 6. Biological Conclusions
The analysis reveals that cardiomyocytes in microgravity undergo a profound **metabolic shift** (insulin resistance and diabetic-like cardiomyopathy signatures) and experience **hypoxic-like stress** (HIF-1 activation). These findings suggest that the lack of gravitational load fundamentally alters the energetic and structural homeostasis of heart muscle cells.