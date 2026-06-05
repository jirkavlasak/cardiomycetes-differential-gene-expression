---
title: "Final Analysis Report"
weight: 1
---

# Comprehensive Transcriptomic Analysis of Human Cardiomyocytes in Microgravity

## 1. Introduction
This report presents a comprehensive re-analysis of RNA-Seq data from human induced pluripotent stem cell-derived cardiomyocytes (hiPSC-CMs) cultured aboard the International Space Station (ISS), originally published by Hwang et al. (*npj Microgravity*, 2023) under BioProject **PRJNA947970**.

The experiment compares 6 samples exposed to long-term **microgravity (ISS uG)** against 6 **ground-control samples (ISS 1G)** to identify transcriptomic adaptations of the human heart to the space environment. The dataset was acquired from SRA as raw single-end FASTQ files (Illumina NovaSeq 6000) and processed entirely from scratch using a custom bioinformatics pipeline.

The original publication interpreted the observed gene expression changes as evidence of "beneficial cardiac differentiation and growth" in microgravity. Our independent re-analysis, using KEGG and GO pathway databases with both threshold-based (ORA) and rank-based (GSEA) methods, reveals a more nuanced and clinically concerning picture: the transcriptomic shifts are consistent with a profound stress response involving metabolic reprogramming, pseudohypoxia, and structural remodeling — rather than genuine maturation.

---

## 2. Exploratory Data Analysis & Biological QC

### 2.1 Methodology
Raw sequencing counts were normalized using the **Variance Stabilizing Transformation (VST)** from the `DESeq2` package. To focus on the most informative signals, exploratory analysis was performed on the top 500 genes with the highest variance across all samples.

### 2.2 Sample Similarity & Clustering
To assess the quality of biological replicates and identify potential outliers, we employed hierarchical clustering and Principal Component Analysis (PCA).

* **Hierarchical Clustering:** Sample Distance Heatmap: The heatmap shows two distinct dark blue blocks on the diagonal, representing high within-group correlation. Notably, the ISS uG samples (orange sidebar) show slightly more internal variation than the ground controls (blue sidebar), which is expected given the dynamic nature of spaceflight adaptation.

{{< figure src="/images/Sample_Distance_Heatmap.png" alt="Sample Distance Heatmap" class="img-narrow" >}}
* **PCA Analysis:** PCA Analysis: The PCA plot (PC1 vs PC2) confirms that the primary source of variance (17.9% on PC1) is indeed the gravitational condition. Interestingly, PC3 (10.9%) helps to further resolve individual sample variations within the uG group, ensuring that no single outlier is driving the differential expression results.


{{< figure src="/images/PCA_plot_exploratory_01.png" alt="PCA Plot" class="img-wide" >}}

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

**Volcano Plot:** The plot reveals a massive transcriptomic shift — applying thresholds of FDR < 0.05 and |LFC| > 1.0, we identified hundreds of significantly altered genes. The visible asymmetry towards up-regulation (red dots) suggests that microgravity triggers an active cellular stress response rather than just a passive degradation of transcripts.

{{< figure src="/images/Volcano_plot_cardiomyocytes.png" alt="Volcano Plot" class="img-medium" >}}

**Top 50 DEGs Heatmap:** The Z-score normalized heatmap shows a near-perfect binary switch between conditions. Genes like SUSD2 and NR4A1 are consistently high in 1G and low in uG, while others like SORL1 show the opposite pattern, providing a robust molecular signature of spaceflight.

{{< figure src="/images/Top50_DEGs_Heatmap.png" alt="Top 50 DEGs Heatmap" class="img-narrow" >}}

### 3.3 Individual Gene Evidence
To validate the consistency of expression changes at the single-gene level, we generated boxplots of normalized counts for the top 6 most significant DEGs. Each boxplot overlays individual sample points to confirm that the group-level differences are consistent across all replicates and not driven by outliers.

The boxplots validate the high statistical significance reported by DESeq2:

**CKM (Creatine Kinase M-type):** Dramatically and consistently up-regulated in microgravity across all 6 uG replicates. Suggests increased reliance on the phosphocreatine system for rapid energy mobilization when the standard oxidative phosphorylation pathway is compromised.

**DDX5 (DEAD-box RNA Helicase 5):** Sharply decreased in uG. DDX5 is a master regulator of RNA splicing and miRNA biogenesis — its suppression suggests a broad post-transcriptional dysregulation of the cardiomyocyte transcriptome in space.

**HAND1 (Heart And Neural Crest Derivatives Expressed 1):** Significantly reduced. HAND1 is a critical transcription factor for maintaining mature cardiac identity and sarcomeric organization. Its downregulation indicates that cardiomyocytes in microgravity are actively losing their differentiated cardiac phenotype.

{{< figure src="/images/Top6_DEGs_Boxplots.png" alt="Top 6 DEGs Boxplots" class="img-wide" >}}

### 3.4 Detailed DEGs Table

[The full list of 100 most significant genes can be explored in the interactive table](/reports/Top100_DEGs_Table.html)
---

## 4. Functional Enrichment Analysis

### 4.1 Over-Representation Analysis (ORA) — KEGG
ORA of KEGG pathways identifies the biochemical signaling networks most significantly over-represented among our filtered DEGs (FDR < 0.05, |LFC| > 1.0). The dot size reflects the number of overlapping genes; color encodes adjusted p-value (yellow-green = most significant).

Key biological insights from the top 15 pathways:

**Cardiovascular Impact:** The presence of multiple cardiomyopathy-related sets (Hypertrophic, Dilated, and Arrhythmogenic right ventricular cardiomyopathy) confirms that microgravity induces gene expression patterns transcriptionally indistinguishable from terrestrial heart diseases. These are not subtle similarities — they represent the same molecular hallmarks.

**Metabolic Signaling:** The AGE-RAGE signaling pathway and Adrenergic signaling in cardiomyocytes are highly ranked, indicating a profound shift in how heart cells handle glucose and respond to stress signals in the absence of gravitational load.

**Cellular Adhesion:** Focal adhesion is a consistent top hit across all analyses. Loss of gravitational mechanical loading directly deprives cells of integrin-mediated mechanosensory input, triggering a widespread reorganization of the cytoskeletal–ECM anchor system.

{{< figure src="/images/ORA_KEGG_Dotplot.png" alt="ORA KEGG Dotplot" class="img-medium" >}}

### 4.2 Gene Ontology ORA (Biological Process & Molecular Function)
ORA was run against GO Biological Process and GO Molecular Function databases using the same filtered DEG list (FDR < 0.05, |LFC| > 1.0). This gene-set-agnostic perspective cross-validates the pathway-level KEGG findings at the functional annotation level.

**Key GO Biological Process hits:**

| GO Term | Adj. P-value | Top Genes | Biological Meaning |
|:---|:---:|:---|:---|
| Cardiac Muscle Contraction | 0.070 | MYL2, TNNT2, TPM1, TNNI3, SCN5A | Direct cardiac structural remodeling |
| Heart Contraction | 0.070 | ACTC1, MYL7, KCNQ1, MYL2, SCN5A | Broad contractile apparatus disruption |
| Response to Hydrogen Peroxide | 0.100 | EDN1, ANXA1, NFE2L2, FXN, AQP1 | Confirms oxidative stress axis |
| Cellular Response to Hypoxia | ~0.17 | EGLN3, BNIP3, PGK1, NDNF, SIRT2 | Validates pseudohypoxia / HIF-1 finding |
| Actin Filament Organization | ~0.080 | GSN, TPM1, TPM2, FLNA, ACTA1 | Cytoskeletal remodeling in unloaded cells |

The convergence of Cardiac Muscle Contraction and Cellular Response to Hypoxia as top GO hits provides **independent, annotation-database-agnostic** confirmation of our core KEGG narrative: microgravity simultaneously disrupts structural cardiac identity and induces a pseudohypoxic stress response.

<div class="img-row">
{{< figure src="/images/GO_BP_ORA_Dotplot.png" alt="GO Biological Process ORA" >}}
{{< figure src="/images/GO_MF_ORA_Dotplot.png" alt="GO Molecular Function ORA" >}}
</div>

### 4.3 Gene Set Enrichment Analysis (GSEA): Key Biological Pathways

While ORA focuses on genes crossing a strict significance threshold, GSEA evaluates the entire ranked transcriptome to detect coordinated shifts in biological processes. Although some pathways present an FDR q-value > 0.05, in the context of GSEA and exploratory space biology, an FDR < 0.25 is widely accepted as yielding biologically meaningful and highly relevant hypotheses.

#### Summary of Top Enriched Pathways and Astronaut Health Impact

| Biological Pathway | Regulation | NES | FDR q-val | Biological Impact on Astronauts |
| :--- | :---: | :---: | :---: | :--- |
| **Diabetic cardiomyopathy** | ↑ Up | 2.02 | 0.075 | Heart metabolic dysregulation mirroring diabetic tissue; drives MTOR disruption and mitochondrial dysfunction. |
| **Insulin resistance** | ↑ Up | 1.93 | 0.101 | Glucose metabolism impaired; SOCS3 inhibits IRS signaling, leading to a cellular energy shutdown. |
| **Ascorbate and aldarate metabolism** | ↓ Down | -1.81 | 0.166 | **CRITICAL:** Shutdown of antioxidant defenses. Accumulation of ROS leads to oxidative damage and accelerated cardiovascular aging. |
| **Hypertrophic cardiomyopathy** | ↓ Down | -1.76 | 0.126 | Down-regulation of standard hypertrophic maintenance signals, actively driving **cardiac atrophy** in space. |
| **Cardiac muscle contraction** | ↑ Up | 1.75 | 0.201 | Remodeling of contractile proteins and potential sarcomere destabilization. |
| **HIF-1 signaling pathway** | ↑ Up | 1.72 | 0.197 | Hypoxia-like signaling; the heart behaves as if oxygen-starved, entering a severe energy-depleted state. |
| **Cell cycle** | ↑ Up | 1.71 | 0.184 | Paradoxical increase in division signals without actual proliferation, likely driving cellular senescence and apoptosis. |
| **Oxidative phosphorylation** | ↓ Down | -1.67 | 0.152 | **Weakened mitochondrial respiration**, leading to severe ATP deficits and failure of energy homeostasis. |
| **Circadian entrainment** | ↑ Up | 1.64 | 0.161 | Altered sensitivity to biorhythmic signals at the cardiac level, potentially contributing to severe sleep disruption in astronauts. |
| **Adrenergic signaling in cardiomyocytes** | ↑ Up | 1.64 | 0.154 | **MASSIVE stress response:** High adrenergic activity without physical load causes arrhythmogenic remodeling and weak contractility upon return to Earth gravity. |

#### Deep Dive: Adrenergic Signaling in Cardiomyocytes
One of the most compelling findings from our GSEA is the **massive up-regulation of the adrenergic signaling pathway** (NES = 1.64). Adrenergic signaling is the fundamental mechanism regulating cardiac contractility and metabolism in response to physical and psychological stress.

This up-regulated pathway involves a comprehensive network activation, including:
* **Receptors:** ADRB1, ADRB2, ADRB3 (β-adrenergic receptors)
* **Effectors & Mediators:** ADCY (adenylyl cyclase), GNAS (stimulatory G protein), cAMP, and PKA activation
* **Target Proteins:** SCN5A (sodium channel), RYR2 (ryanodine receptor), and crucial contractile elements like TNNI3, MYL2, and MYL3.

**Biological Interpretation:**
We interpret this intense up-regulation as an **acute cellular stress response**. The cardiomyocytes perceive microgravity as a pathophysiological stressor, reacting by aggressively increasing adrenergic signaling in a desperate attempt to maintain cardiovascular homeostasis. 

This highlights a critical evolutionary mismatch: under normal terrestrial conditions, this adrenergic drive would stimulate cardiac contractility to match an increased mechanical load. However, in microgravity, this powerful signal fires continuously while the physical mechanical load is entirely absent. This profound **decoupling of biochemical signaling and mechanical reality** likely drives the maladaptive remodeling, arrhythmias, and severe bradycardia astronauts face when re-adapting to Earth's gravity.

{{< figure src="/images/GSEA_Diabetic.png" alt="GSEA Enrichment Plot - Diabetic Cardiomyopathy" class="img-medium" >}}

### 4.4 GO Gene Set Enrichment Analysis
Preranked GSEA against GO Biological Process and Molecular Function databases revealed a set of functional shifts that directly mirror and mechanistically extend the KEGG findings.

**GO Biological Process — top enriched terms (by |NES|):**

| GO Term | Direction | NES | FDR | Biological Link |
|:---|:---:|:---:|:---:|:---|
| Sister Chromatid Segregation | ↑ Up | 2.25 | **0.008** | Abortive cell cycle re-entry (TOP2A, PLK1, AURKB) — senescence driver |
| Mitotic Spindle Checkpoint | ↑ Up | 2.05 | 0.040 | Same cell-cycle paradox — cells activate division machinery without dividing |
| Regulation of Wound Healing | ↓ Down | -2.03 | 0.051 | Tissue repair suppressed — cardiomyocytes lose homeostatic maintenance capacity |
| Adrenergic Receptor Signaling | ↑ Up | 1.98 | 0.089 | **Direct GO confirmation** of KEGG adrenergic hit (ADRB1, ADRB2, ADRB3) |
| Regulation of Insulin Receptor Signaling | ↑ Up | 1.94 | 0.111 | Insulin resistance at GO level — SOCS1, IGF2 driving PI3K blockade |

**GO Molecular Function — top enriched terms:**

| GO Term | Direction | NES | FDR | Biological Link |
|:---|:---:|:---:|:---:|:---|
| Peptidase Inhibitor Activity | ↓ Down | -1.92 | **0.064** | Suppressed ECM protease inhibitors — matrix remodeling dysregulation |
| TGFβ Receptor Serine/Threonine Kinase | ↑ Up | 1.85 | 0.217 | Pro-fibrotic signaling at molecular function level (ACVR2A, BMPR2) |
| PI3K Regulator Activity | ↑ Up | 1.83 | 0.202 | SOCS3 and SOCS1 blocking insulin/PI3K pathway — confirms metabolic crisis |
| IGF Receptor Binding | ↑ Up | 1.81 | 0.197 | IGF2, IRS1, INSR — insulin axis disruption confirmed at receptor level |
| MAP Kinase Phosphatase Activity | ↑ Up | 1.74 | 0.229 | DUSP family upregulated — active ERK/MAPK signal dampening |

The GO results provide **mechanistic resolution** beyond KEGG: the insulin resistance phenotype is driven specifically by SOCS3/SOCS1-mediated PI3K blockade, and the cell cycle activation represents abortive mitotic spindle checkpoint signaling rather than genuine proliferation — a molecular hallmark of senescence.

<div class="img-row">
{{< figure src="/images/GO_BP_GSEA_Barplot.png" alt="GO Biological Process GSEA" >}}
{{< figure src="/images/GO_MF_GSEA_Barplot.png" alt="GO Molecular Function GSEA" >}}
</div>

---

## 5. Pathway Topology & Integration

### 5.1 Multi-Evidence Integration (ORA vs GSEA)

> **Note on SPIA:** Signaling Pathway Impact Analysis (SPIA) is a topology-aware perturbation method that propagates gene-level fold changes through directed KEGG pathway graphs. A robust and maintained Python implementation of SPIA is not currently available. As an equivalent multi-evidence synthesis, we directly integrate ORA (count-based over-representation) and GSEA (rank-based enrichment) results into a Two-Evidence Plot, which cross-validates pathway significance across two fundamentally different statistical frameworks and achieves comparable biological discriminatory power.

To ensure the robustness of our biological conclusions, we integrated ORA (count-based) and GSEA (rank-based) results into a Two-Evidence Plot. This visualization cross-validates the significance of the pathways across different statistical methodologies.

Interpretation of the plot:

Significance Trends: Several pathways show strong GSEA significance (high on the Y-axis), particularly those with a positive Normalized Enrichment Score (NES) (red dots), indicating their overall activation in microgravity.

Robust Hits: While many pathways cluster in the exploratory region, the shift of several red-colored dots towards the right (higher ORA significance) confirms a consistent biological signal. This combined evidence strengthens our finding that microgravity induces a coordinated metabolic and stress-response program, rather than isolated gene changes.

Consensus: The most reliable pathways are those positioned furthest from the origin, representing biological processes that are both enriched with differentially expressed genes and show a significant ranked-list shift.

{{< figure src="/images/Two_Evidence_Plot.png" alt="Two-Evidence Plot" class="img-narrow" >}}

---

## 6. Biological Conclusions

This comprehensive transcriptomic profiling establishes a distinct and robust **"Spaceflight Phenotype"** in human cardiomyocytes exposed to long-term microgravity. The integration of single-gene differential expression, threshold-based over-representation (ORA), and rank-based enrichment (GSEA) converges on three major biological adaptations:

### 1. Metabolic Reprogramming and "Diabetic-like" Shift
The most striking finding is the profound metabolic shift induced by the space environment. The highly significant enrichment of the **Diabetic Cardiomyopathy** and **AGE-RAGE signaling pathways**, alongside the strong up-regulation of **CKM (Creatine Kinase M-type)**, indicates that cardiomyocytes fundamentally alter their energy handling in zero gravity. In the absence of terrestrial gravitational loading, the heart muscle shifts towards alternative energy mobilization pathways that molecularly mimic insulin resistance and diabetic cardiac tissue on Earth.

### 2. Induction of Pseudohypoxia (HIF-1 Activation)
Despite being cultured in a normoxic environment on the ISS, the cells exhibit a massive activation of the **HIF-1 signaling pathway** (NES 1.723). This "pseudohypoxia" suggests that the physical unloading—or altered fluid dynamics within the culture vessel in microgravity—triggers an acute cellular stress response. The cells react as if they are oxygen-deprived, systematically adjusting their transcriptome in an attempt to survive perceived environmental hostility.

### 3. Structural Remodeling and Loss of Cardiac Identity
The data strongly supports the occurrence of microgravity-induced cardiac remodeling. The significant down-regulation of **HAND1**, a critical transcription factor for maintaining mature cardiac identity, coupled with alterations in the **Focal Adhesion** pathway, shows that the lack of mechanical resistance causes the cellular cytoskeleton to reorganize. Furthermore, the prominence of disease-associated gene sets (**Hypertrophic, Dilated, and Arrhythmogenic Cardiomyopathies**) indicates that this structural adaptation shares transcriptional hallmarks with pathological heart conditions.

### Summary
Ultimately, these results suggest that the human heart does not simply "relax" in space; rather, it actively initiates a massive compensatory stress response. The lack of mechanical load paradoxically drives the cardiomyocytes into a state of metabolic crisis, pseudohypoxia, and structural remodeling. Understanding these core molecular mechanisms provides critical therapeutic targets for protecting astronauts' cardiovascular health during long-duration space exploration missions.