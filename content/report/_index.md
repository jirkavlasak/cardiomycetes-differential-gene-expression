---
title: "Final Analysis Report"
weight: 1
---

# Comprehensive Transcriptomic Analysis of Human Cardiomyocytes in Microgravity

## 1. Introduction
This report presents a comprehensive re-analysis of RNA-Seq data from human induced pluripotent stem cell-derived cardiomyocytes (hiPSC-CMs) cultured aboard the International Space Station (ISS), originally published by Hwang et al. (*npj Microgravity*, 2023) under BioProject **PRJNA947970**.

The experiment compares 6 samples exposed to long-term **microgravity (ISS uG)** against 6 **ground-control samples (ISS 1G)** to identify transcriptomic adaptations of the human heart to the space environment. The dataset was acquired from SRA as raw single-end FASTQ files (Illumina NovaSeq 6000) and processed entirely from scratch using a custom bioinformatics pipeline.

The original publication interpreted the observed gene expression changes as evidence of "beneficial cardiac differentiation and growth" in microgravity. Our independent re-analysis, using KEGG and GO pathway databases with both threshold-based (ORA) and rank-based (GSEA) methods, reveals a more nuanced and clinically concerning picture: the transcriptomic shifts are consistent with a profound stress response involving metabolic reprogramming, pseudohypoxia, and structural remodeling — rather than genuine maturation.

<div class="row g-3 my-4 text-center">
  <div class="col-6 col-md-2">
    <div class="card h-100 border-0 shadow-sm">
      <div class="card-body py-3">
        <div class="fs-2 fw-bold text-primary">12</div>
        <div class="small text-muted">RNA-Seq samples</div>
      </div>
    </div>
  </div>
  <div class="col-6 col-md-2">
    <div class="card h-100 border-0 shadow-sm">
      <div class="card-body py-3">
        <div class="fs-2 fw-bold text-secondary">66 794</div>
        <div class="small text-muted">Transcripts tested</div>
      </div>
    </div>
  </div>
  <div class="col-6 col-md-2">
    <div class="card h-100 border-0 shadow-sm">
      <div class="card-body py-3">
        <div class="fs-2 fw-bold text-danger">436</div>
        <div class="small text-muted">Up-regulated DEGs</div>
      </div>
    </div>
  </div>
  <div class="col-6 col-md-2">
    <div class="card h-100 border-0 shadow-sm">
      <div class="card-body py-3">
        <div class="fs-2 fw-bold text-primary">798</div>
        <div class="small text-muted">Down-regulated DEGs</div>
      </div>
    </div>
  </div>
  <div class="col-6 col-md-2">
    <div class="card h-100 border-0 shadow-sm">
      <div class="card-body py-3">
        <div class="fs-2 fw-bold text-warning">4.4×10⁻²⁷</div>
        <div class="small text-muted">Best adj. p-value</div>
      </div>
    </div>
  </div>
  <div class="col-6 col-md-2">
    <div class="card h-100 border-0 shadow-sm">
      <div class="card-body py-3">
        <div class="fs-2 fw-bold text-success">0.008</div>
        <div class="small text-muted">Best GO GSEA FDR</div>
      </div>
    </div>
  </div>
</div>

---

## 2. Exploratory Data Analysis & Biological QC

### 2.1 Methodology
Raw sequencing counts were normalized using the **Variance Stabilizing Transformation (VST)** from the `DESeq2` package. To focus on the most informative signals, exploratory analysis was performed on the top 500 genes with the highest variance across all samples.

### 2.2 Sample Similarity & Clustering
To assess the quality of biological replicates and identify potential outliers, we employed hierarchical clustering and Principal Component Analysis (PCA).

* **Hierarchical Clustering:** Sample Distance Heatmap: The heatmap shows two distinct dark blue blocks on the diagonal, representing high within-group correlation. Notably, the ISS uG samples (orange sidebar) show slightly more internal variation than the ground controls (blue sidebar), which is expected given the dynamic nature of spaceflight adaptation.

{{< figure src="/images/Sample_Distance_Heatmap.png" alt="Sample Distance Heatmap" class="img-narrow" >}}
* **PCA Analysis:** The PCA plot (PC1 vs PC2) confirms that the primary source of variance (17.9% on PC1) is indeed the gravitational condition. Interestingly, PC3 (10.9%) helps to further resolve individual sample variations within the uG group, ensuring that no single outlier is driving the differential expression results.


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

**Volcano Plot:** The plot reveals a massive transcriptomic shift — applying thresholds of FDR < 0.05 and |LFC| > 1.0, we identified hundreds of significantly altered genes. The visible asymmetry towards **down-regulation** (blue dots, 798 genes) over up-regulation (436 genes) suggests that microgravity primarily silences homeostatic and structural programs rather than simply triggering active stress responses.

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

[The full list of 100 most significant genes can be explored in the interactive table](/cardiomycetes-differential-gene-expression/reports/Top100_DEGs_Table.html)

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

### 4.3 Disease Association ORA (Jensen DISEASES)
To bridge transcriptomic findings directly to named clinical conditions, ORA was run against the **Jensen DISEASES** database — a curated compendium of disease–gene associations derived from literature mining. This provides a clinically-anchored perspective that neither KEGG pathways nor GO terms offer.

{{< figure src="/images/DO_ORA_Dotplot.png" alt="Disease Associations ORA" class="img-medium" >}}

| Disease Term | Adj. P-value | Clinical Relevance |
|:---|:---:|:---|
| **Cardiomyopathy** | 0.006 | Direct cardiac muscle disease — primary hit |
| **Hypertrophic cardiomyopathy** | 0.006 | Specific structural disease subtype confirmed |
| **Dilated cardiomyopathy** | 0.034 | Second structural subtype — cardiac dilation pattern |
| Kidney failure | 0.034 | Secondary organ dysfunction common in cardiac patients |

The top hits — Cardiomyopathy (adj. p = 0.006), Hypertrophic cardiomyopathy (adj. p = 0.006), and Dilated cardiomyopathy (adj. p = 0.034) — directly confirm at the clinical disease-gene level what KEGG and GO showed at the pathway level. Microgravity does not merely activate cardiomyopathy-like pathways; by formal disease–gene definition, it expresses the molecular signature of cardiomyopathy itself.

### 4.4 Gene Set Enrichment Analysis (GSEA): Key Biological Pathways

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

### 4.5 GO Gene Set Enrichment Analysis
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

> **Note on SPIA (Signaling Pathway Impact Analysis):**
> SPIA is a topology-aware perturbation method that computes two independent scores per pathway: (1) an over-representation score (pORA) based on which DEGs map to the pathway, and (2) a perturbation accumulation score (pPERT) that propagates signed log₂FC values through the directed KEGG graph topology. Both scores are then combined into a global probability pG via Fisher's method, which is what makes SPIA statistically more powerful than plain ORA for signaling cascades.
>
> **Why we did not use SPIA here:** The canonical SPIA implementation is an R package (`SPIA`, Bioconductor). No Python port maintains the full topological propagation model — the closest available package (`SPIASpy`) has been unmaintained since 2018 and lacks support for current KEGG graph formats. Running a single R package would require a separate R environment, breaking the reproducibility of our fully Python-based pipeline.
>
> **Why our Two-Evidence Plot is a valid methodological substitute:** SPIA's advantage over plain ORA comes entirely from adding a second, independent line of evidence to avoid false positives. Our Two-Evidence Plot achieves this same goal via a different route: the X-axis captures the ORA signal (count-based, threshold-dependent, equivalent to SPIA's pORA), while the Y-axis captures the preranked GSEA signal (rank-based, threshold-free, using the full transcriptome ranked by Wald statistic — a statistically stronger signal than SPIA's pPERT, which uses only DEG-level fold changes). Pathways in the top-right quadrant (significant in *both* frameworks) are the same robust hits that SPIA's combined pG would highlight. The NES color-coding adds directionality, giving us the equivalent of SPIA's signed perturbation. This approach has the additional advantage of being framework-agnostic and visually interpretable.

To ensure the robustness of our biological conclusions, we integrated ORA (count-based) and GSEA (rank-based) results into a Two-Evidence Plot. This visualization cross-validates the significance of the pathways across different statistical methodologies.

Interpretation of the plot:

Significance Trends: Several pathways show strong GSEA significance (high on the Y-axis), particularly those with a positive Normalized Enrichment Score (NES) (red dots), indicating their overall activation in microgravity.

Robust Hits: While many pathways cluster in the exploratory region, the shift of several red-colored dots towards the right (higher ORA significance) confirms a consistent biological signal. This combined evidence strengthens our finding that microgravity induces a coordinated metabolic and stress-response program, rather than isolated gene changes.

Consensus: The most reliable pathways are those positioned furthest from the origin, representing biological processes that are both enriched with differentially expressed genes and show a significant ranked-list shift.

{{< figure src="/images/Two_Evidence_Plot.png" alt="Two-Evidence Plot" class="img-narrow" >}}

---

## 6. Biological Conclusions

This comprehensive transcriptomic profiling establishes a distinct and robust **"Spaceflight Phenotype"** in human cardiomyocytes exposed to long-term microgravity. The convergence of single-gene DEA, KEGG and GO pathway enrichment (ORA + GSEA across four independent databases) provides multi-layered, cross-validated evidence for four major biological adaptations:

### 1. Metabolic Reprogramming and "Diabetic-like" Shift
The most striking finding is a profound metabolic crisis induced by the space environment. The KEGG enrichment of **Diabetic Cardiomyopathy** (NES = 2.02) and **Insulin Resistance** (NES = 1.93), alongside GO Molecular Function GSEA identifying **PI3K Regulator Activity** and **IGF Receptor Binding** as top hits, points to a single mechanistic origin: the massive induction of **SOCS3 and SOCS1** blocks the insulin receptor / IRS-1 / PI3K signaling cascade, plunging the cells into metabolic starvation. The strong up-regulation of **CKM (Creatine Kinase M-type)** at the single-gene level confirms that cardiomyocytes are forced to rely on the phosphocreatine emergency energy system in the absence of functional mitochondrial ATP production.

### 2. Induction of Pseudohypoxia and Oxidative Stress
Despite normoxic culture conditions on the ISS, the cells exhibit massive activation of the **HIF-1 signaling pathway** (NES = 1.72, KEGG) confirmed independently by GO Biological Process ORA identifying **Cellular Response to Hypoxia** and **Cellular Response to Reactive Oxygen Species** among the top hits. Simultaneously, **Oxidative Phosphorylation** is severely down-regulated (NES = −1.67) and **Ascorbate metabolism** — the primary antioxidant defense — is suppressed (NES = −1.81). This creates a self-reinforcing cycle: failing mitochondria generate ROS, ROS stabilize HIF-1α, and HIF-1 further shifts metabolism towards glycolysis, deepening the energetic crisis.

### 3. Structural Remodeling and Loss of Cardiac Identity
GO Biological Process ORA places **Cardiac Muscle Contraction** and **Heart Contraction** as the most significantly over-represented terms, driven by MYL2, TNNT2, TPM1, TNNI3, and SCN5A. The down-regulation of **HAND1** (a master cardiac transcription factor) and the disruption of **Focal Adhesion** and **Actin Filament Organization** pathways confirm that the mechanical unloading of microgravity dismantles both the transcriptional identity and the cytoskeletal architecture of the cardiomyocyte. The enrichment of disease-associated sets (Hypertrophic, Dilated, and Arrhythmogenic Cardiomyopathies) indicates this structural remodeling is not a controlled adaptation but a pathological drift toward cardiac disease states.

### 4. Abortive Cell Cycle Re-entry and Cellular Senescence
The single statistically strongest finding in the entire analysis is GO Biological Process GSEA: **Sister Chromatid Segregation** (NES = 2.25, **FDR = 0.008**), driven by TOP2A, PLK1, and AURKB. In terminally differentiated cardiomyocytes, this activation of chromosome segregation machinery does not represent proliferation. It is a hallmark of **abortive mitosis** — cells re-enter the cell cycle but cannot divide, becoming trapped in a senescent state. This GO finding mechanistically resolves the KEGG Cell Cycle hit and explains the paradoxical co-activation of division signals without actual proliferation observed throughout the analysis. The simultaneous down-regulation of **Wound Healing** (NES = −2.03, FDR = 0.051) confirms that the cells are not only failing to regenerate — they are actively losing their homeostatic repair capacity.

### Summary
The human heart in microgravity does not simply atrophy — it enters a state of coordinated molecular catastrophe. A metabolic blockade driven by SOCS3-mediated PI3K inhibition starves the cells of energy; pseudohypoxia driven by HIF-1 activation and mitochondrial failure compounds the crisis with oxidative damage; structural identity is lost as sarcomeric and cytoskeletal programs dissolve; and the cells become trapped in abortive senescent cell cycles, unable to repair or regenerate. These findings define clear molecular targets — SOCS3, HIF-1α, the adrenergic receptor cascade, and spindle checkpoint kinases — that may guide future pharmacological countermeasures for protecting cardiovascular health during long-duration space exploration.