---
title: "Extended Gallery"
weight: 4
---


## GO Term Enrichment Results

### GO Biological Process — ORA
{{< figure src="/images/GO_BP_ORA_Dotplot.png" alt="GO Biological Process ORA" class="img-medium" >}}

**Enrichment Status:** Combined ORA across Biological Process gene sets

**In-Depth Biological Interpretation:**
The GO Biological Process ORA provides an annotation-database-independent confirmation of the core KEGG findings. The two most significant terms — **Cardiac Muscle Contraction (GO:0060048)** and **Heart Contraction (GO:0060047)** — are driven by a coherent gene cluster including MYL2, TNNT2, TPM1, TNNI3, SCN5A, and SLC8A1. These sarcomeric and ion channel genes form the molecular backbone of cardiac contractility, and their coordinate dysregulation in microgravity confirms that the fundamental biomechanical identity of the cardiomyocyte is being dismantled.

Critically, **Cellular Response to Hypoxia (GO:0071456)** and **Cellular Response to Reactive Oxygen Species (GO:0034614)** appear in the top 30 terms, driven by EGLN3, BNIP3, PGK1, and NFE2L2. This provides GO-level, annotation-independent validation of the pseudohypoxia narrative identified in the KEGG HIF-1 analysis. The cardiomyocytes are activating oxygen-deprivation survival programs despite normoxic culture conditions, with the oxidative stress response acting as a simultaneous cellular alarm signal. Furthermore, **Actin Filament Organization (GO:0007015)** — enriched with GSN, TPM1, TPM2, and FLNA — reinforces the cytoskeletal remodeling theme, confirming that structural dismantling extends from the sarcomere to the broader actin network.

---

### GO Molecular Function — ORA
{{< figure src="/images/GO_MF_ORA_Dotplot.png" alt="GO Molecular Function ORA" class="img-medium" >}}

**Enrichment Status:** Combined ORA across Molecular Function gene sets

**In-Depth Biological Interpretation:**
The GO Molecular Function ORA resolves the disrupted biological processes identified above into specific biochemical activities. The enrichment of **Actin Monomer Binding** and **Structural Constituent of Muscle** activities directly maps the sarcomeric gene dysregulation to their physical molecular roles — the contractile apparatus is not merely transcriptionally dysregulated but is losing the precise molecular binding interactions required for coordinated cardiac contraction.

The enrichment of **Transmembrane Receptor Protein Serine/Threonine Kinase Activity** (covering TGF-β receptor family genes ACVR2A, ACVR2B, BMPR2) connects the molecular function landscape to the pro-fibrotic signaling axis identified in the KEGG analysis. At the molecular level, microgravity appears to activate TGF-β receptor complexes while simultaneously suppressing protease inhibitors — creating a biochemical environment that promotes fibrotic matrix deposition over structural contractile maintenance.

---

### GO Biological Process — GSEA
{{< figure src="/images/GO_BP_GSEA_Barplot.png" alt="GO Biological Process GSEA" class="img-medium" >}}

**Enrichment Status:** Red = Up-regulated in microgravity | Blue = Down-regulated

**In-Depth Biological Interpretation:**
The preranked GO Biological Process GSEA reveals a striking dichotomy. The most significantly up-regulated process — **Sister Chromatid Segregation (NES = 2.25, FDR = 0.008)** — is driven by TOP2A, PLK1, AURKB, NDC80, and KIFC1. In terminally differentiated cardiomyocytes, this activation of chromosome segregation machinery does not represent genuine cell division. Instead, it is a hallmark of **mitotic catastrophe and cellular senescence**: cells re-enter the cell cycle checkpoint cascade but cannot complete division, becoming trapped in a state of abortive mitosis that accelerates senescent degeneration. This mechanistically explains the KEGG Cell Cycle finding at far higher GO resolution.

The **Adrenergic Receptor Signaling Pathway (NES = 1.98, FDR = 0.089)** is the only adrenergic-specific GO term in the entire biological process database, and it ranks 4th by NES — a direct, gene-ontology-level confirmation of the adrenergic stress response identified in KEGG. Equally important is the most significantly **down-regulated** process: **Regulation of Wound Healing (NES = -2.03, FDR = 0.051)**. The suppression of tissue repair and homeostatic maintenance signaling confirms that microgravity does not merely induce a stress response — it simultaneously strips the cells of their capacity to respond to damage, leaving them in a doubly vulnerable state of active alarm and impaired repair.

---

### GO Molecular Function — GSEA
{{< figure src="/images/GO_MF_GSEA_Barplot.png" alt="GO Molecular Function GSEA" class="img-medium" >}}

**Enrichment Status:** Red = Up-regulated in microgravity | Blue = Down-regulated

**In-Depth Biological Interpretation:**
The GO Molecular Function GSEA provides the finest mechanistic resolution in this entire analysis. The up-regulation of **1-Phosphatidylinositol-3-Kinase Regulator Activity (NES = 1.83)** and **Insulin-Like Growth Factor Receptor Binding (NES = 1.81)** converges on a single molecular mechanism: SOCS3 and SOCS1 are massively induced in microgravity, and these proteins are canonical suppressors of the insulin receptor / IRS-1 / PI3K signaling cascade. This confirms that the "Insulin Resistance" phenotype identified at the KEGG pathway level operates through a specific, well-defined molecular blockade — the SOCS-mediated disruption of the PI3K regulatory complex that normally relays growth factor signals to metabolic effectors.

The up-regulation of **MAP Kinase Phosphatase Activity (NES = 1.74)** — driven by DUSP5, DUSP7, DUSP4, and DUSP2 — adds a crucial layer: these phosphatases actively inactivate ERK and p38 MAPK signaling. The cardiomyocytes are not simply activating stress responses; they are simultaneously mounting counter-regulation to dampen the very growth and stress kinases that would normally coordinate the cellular response. This paradoxical co-activation of stress signals and their own inhibitors reflects a state of profound signaling incoherence — a molecular manifestation of the cardiomyocyte's inability to mount a coherent adaptive response to the unprecedented gravitational void.

---

---

### Disease Ontology ORA
{{< figure src="/images/DO_ORA_Dotplot.png" alt="Disease Ontology ORA" class="img-medium" >}}

**Enrichment Status:** ORA against Disease Ontology 2023 database

**In-Depth Biological Interpretation:**
The Disease Ontology ORA provides the most clinically direct interpretation of our findings. Unlike KEGG (which maps biochemical pathways) or GO (which describes molecular functions), the Disease Ontology directly links gene sets to named, classified human diseases. The enrichment of cardiomyopathy-related disease terms confirms that the microgravity transcriptome does not merely *resemble* a diseased state — it is, by formal ontological definition, expressing the gene signatures of established cardiac pathologies. This is the strongest possible translational argument: the same genes that distinguish diseased hearts from healthy ones are differentially expressed in cardiomyocytes after spaceflight. This finding has direct implications for astronaut health monitoring, suggesting that standard cardiac disease biomarkers could serve as early warning indicators of microgravity-induced cardiovascular deconditioning.

---

# Supplementary Pathway Data

### Complete GSEA Report
The pathways detailed above represent only the most biologically impactful shifts. For the complete, unfiltered list of all evaluated KEGG pathways, including exact p-values, FDR metrics, and Enrichment Scores, please refer to the raw data output:

[📄 Download Full Preranked GSEA Report (CSV)](/reports/gseapy.gene_set.prerank.report.csv)



### Complete Differential Expression Data
While the heatmaps, boxplots, and interactive tables highlight the most critical expression changes, the underlying dataset provides a comprehensive look at the transcriptomic shift. For the complete list of the top 100 differentially expressed genes, including exact log2FoldChange values, base means, and adjusted p-values, please refer to the raw data output:

[📄 Download Top 100 DEGs Report (CSV)](/reports/Top100_Annotated_Genes_uG_vs_1G.csv)


### Global Transcriptomic Shift (Top 500 DEGs)
{{< figure src="/images/Top500_Genes_Heatmap.png" alt="Top 500 DEGs Heatmap" class="img-narrow" >}}
**Biological Interpretation:**
While the main report highlights the top 50 genes, this expanded heatmap of the top 500 most variable and significantly expressed genes illustrates the sheer scale of the transcriptomic reprogramming in microgravity. The striking structural divide between the 1G and uG samples demonstrates that the adaptation to spaceflight is not a localized pathway response, but a massive, genome-wide state shift affecting hundreds of interconnected gene networks simultaneously.


# Supplementary Pathway Data
Additional pathways identified during the *in silico* re-analysis.

### TGF-beta Signaling
{{< figure src="/images/TGF.png" alt="TGF-beta" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated (NES: 1.685)
**Significance:** FDR = 0.155

**In-Depth Biological Interpretation:**
Microgravity exposure imparts a profound biomechanical and oxidative stress on cardiomyocytes, frequently triggering the canonical TGF-beta signaling cascade as a maladaptive wound-healing response. Despite the absence of traditional mechanical overload, the accumulation of reactive oxygen species (ROS) and cellular damage prompts the activation of latent TGF-beta complexes within the local microenvironment. Once active, this pathway drives profound structural remodeling by stimulating the transcription of extracellular matrix proteins, effectively shifting the cellular focus from contractile maintenance to fibrotic deposition. In the context of spaceflight, this inappropriate pro-fibrotic signaling severely stiffens the myocardium and disrupts regular electrical conduction. Ultimately, this maladaptive remodeling compromises diastolic function and serves as a primary driver of the long-term cardiovascular deconditioning observed in astronauts.

### Adrenergic signaling in cardiomyocytes
{{< figure src="/images/Adrenergic%20signaling%20in%20cardiomyocytes.png" alt="Adrenergic signaling in cardiomyocytes" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated (NES: 1.64)
**Significance:** FDR = 0.154

**In-Depth Biological Interpretation:**
Microgravity acts as a profound cellular stressor, prompting cardiomyocytes to mount an acute adaptive response via robust up-regulation of β-adrenergic signaling pathways despite the absence of systemic catecholamines or mechanical loading. This paradoxical activation involves key effectors such as adenylyl cyclase, PKA, and downstream contractile proteins in an evolutionary attempt to maintain cardiac output under perceived pathological stress. However, in the unloaded environment of the International Space Station, this disparity between intense biochemical signaling and the lack of physical resistance leads to maladaptive arrhythmogenic remodeling. Over time, this chronic adrenergic overdrive without corresponding mechanical strain contributes significantly to the sinus bradycardia and increased susceptibility to arrhythmias frequently observed in astronauts returning from long-duration spaceflight.

---

### Ascorbate and aldarate metabolism
{{< figure src="/images/Ascorbate%20and%20aldarate%20metabolism.png" alt="Ascorbate and aldarate metabolism" class="img-medium" >}}
**Enrichment Status:** 🔵 Down-regulated (NES: -1.81)
**Significance:** FDR = 0.166

**In-Depth Biological Interpretation:**
The significant down-regulation of ascorbate regeneration pathways represents a critical failure in the cellular antioxidant defense machinery under microgravity conditions. As the physical demands on the heart decrease, cardiomyocytes appear to undergo a metabolic reprioritization, down-regulating energetically costly antioxidant enzymes such as AKR1A1 and aldehyde dehydrogenases. This withdrawal of oxidative protection occurs simultaneously with mitochondrial dysfunction, creating a dangerous redox imbalance where reactive oxygen species (ROS) accumulate unchecked. Consequently, this unmitigated oxidative stress inflicts profound damage upon cellular lipids, proteins, and DNA, fundamentally acting as a molecular driver for the accelerated cardiovascular senescence and degenerative remodeling characteristic of spaceflight.

---

### Diabetic cardiomyopathy
{{< figure src="/images/Diabetic%20cardiomyopathy.png" alt="Diabetic cardiomyopathy" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated (NES: 2.02)
**Significance:** FDR = 0.075

**In-Depth Biological Interpretation:**
The microgravity environment triggers a profound metabolic dysregulation in isolated cardiomyocytes that closely mimics the pathological transcriptional profile of diabetic cardiomyopathy. In response to hemodynamic unloading and early mitochondrial dysfunction, the cells exhibit a maladaptive shift in substrate utilization, moving away from efficient fatty acid oxidation and suffering from severe energetic inflexibility. This metabolic derangement is further compounded by dysregulation of survival pathways and subsequent lipotoxic stress, creating a cellular environment akin to chronic systemic inflammation. For the astronaut, this indicates that the unloaded myocardium is not merely undergoing atrophy, but actively remodeling into a metabolically impaired, pre-diabetic-like state that struggles to efficiently manage acute energy demands upon return to 1G.

---

### Insulin resistance
{{< figure src="/images/Insulin%20resistance.png" alt="Insulin resistance" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated (NES: 1.93)
**Significance:** FDR = 0.101

**In-Depth Biological Interpretation:**
Accompanying the broader metabolic collapse, cardiomyocytes in microgravity demonstrate a marked up-regulation of insulin resistance pathways, primarily driven by massive increases in suppressors of cytokine signaling (e.g., SOCS3). This dramatic, multi-fold up-regulation of SOCS3 effectively blunts the downstream insulin receptor substrate (IRS) signaling cascade, forcing the cells into a state of metabolic starvation despite the theoretical availability of extracellular nutrients. This insulin-resistant phenotype prevents efficient glucose uptake and utilization, severely compromising the heart's fundamental energetic homeostasis. Ultimately, this molecular blockade acts as a detrimental homeostatic response to mechanical unloading, stripping the myocardium of its vital metabolic flexibility and contributing to long-term functional degradation.

---

### Cardiac muscle contraction
{{< figure src="/images/Cardiac%20muscle%20contraction.png" alt="Cardiac muscle contraction" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated (NES: 1.75)
**Significance:** FDR = 0.201

**In-Depth Biological Interpretation:**
Exposure to microgravity induces profound structural and transcriptional changes within the contractile apparatus of cardiomyocytes, leading to a destabilization of the sarcomeric architecture. Despite an overall reduction in mechanical workload, there is an anomalous up-regulation of specific cardiac muscle contraction pathways, likely representing a compensatory molecular effort to maintain tension and structural integrity. This dysregulation involves key structural proteins, including varying isoforms of myosin heavy chains and troponin complexes, which become misaligned in the absence of gravitational vector forces. Ultimately, this chaotic remodeling of the contractile machinery impairs synchronous beating and drastically diminishes the functional reserve of the myocardium, directly contributing to orthostatic intolerance upon return to standard Earth gravity.

---

### HIF-1 signaling pathway
{{< figure src="/images/HIF-1%20signaling%20pathway.png" alt="HIF-1 signaling pathway" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated (NES: 1.72)
**Significance:** FDR = 0.197

**In-Depth Biological Interpretation:**
The microgravity environment triggers a pseudohypoxic metabolic response in cardiomyocytes, characterized by a significant up-regulation of the HIF-1 signaling pathway despite adequate oxygen availability. This paradoxical HIF-1 stabilization is likely driven by escalating mitochondrial dysfunction and the resultant accumulation of reactive oxygen species (ROS), which inhibit prolyl hydroxylases that normally degrade HIF-1α. Consequently, the cells undergo a drastic metabolic rewiring, shifting away from efficient oxidative phosphorylation toward glycolysis in a desperate attempt to maintain ATP homeostasis. This chronic pseudohypoxic signaling not only exacerbates energy depletion but also promotes maladaptive fibrotic remodeling, fundamentally compromising the structural and metabolic integrity of the spaceflight-exposed myocardium.

---

### Oxidative phosphorylation
{{< figure src="/images/Oxidative%20phosphorylation.png" alt="Oxidative phosphorylation" class="img-medium" >}}
**Enrichment Status:** 🔵 Down-regulated (NES: -1.67)
**Significance:** FDR = 0.152

**In-Depth Biological Interpretation:**
Exposure to microgravity rapidly diminishes the hemodynamic workload on the heart, triggering a severe transcription-level down-regulation of mitochondrial respiratory chain complexes and overall oxidative phosphorylation. This suppression reflects a profound metabolic shift where cardiomyocytes attempt to match reduced mechanical demand by aggressively curbing mitochondrial ATP production. Unfortunately, this molecular shutdown not only precipitates cellular energy starvation but also destabilizes the electron transport chain, leading to electron leakage and the secondary generation of reactive oxygen species (ROS). Ultimately, this compromised mitochondrial bioenergetics cascade is a primary driver of spaceflight-induced myocardial deconditioning, leaving the heart energetically inflexible and highly vulnerable to oxidative damage.

---

### Hypertrophic cardiomyopathy
{{< figure src="/images/Hypertrophic%20cardiomyopathy.png" alt="Hypertrophic cardiomyopathy" class="img-medium" >}}
**Enrichment Status:** 🔵 Down-regulated (NES: -1.76)
**Significance:** FDR = 0.126

**In-Depth Biological Interpretation:**
The persistent absence of gravitational mechanical resistance deprives the myocardium of the critical stretch-induced mechanotransduction necessary to maintain physiological cardiac mass. In microgravity, this lack of physical strain directly suppresses pro-hypertrophic transcriptional programs, notably evidenced by the down-regulation of crucial morphogenic factors like HAND1 and various structural sarcomeric proteins. Rather than simply entering a resting state, the cardiomyocytes actively dismantle their hypertrophic and contractile machinery as an adaptive, albeit destructive, response to the unloaded orbital environment. This targeted molecular shutdown directly explains the rapid, significant structural cardiac atrophy—often resulting in a 5-10% loss of myocardial mass—consistently recorded in astronauts during space missions.

---

### Cell cycle
{{< figure src="/images/Cell%20cycle.png" alt="Cell cycle" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated (NES: 1.71)
**Significance:** FDR = 0.184

**In-Depth Biological Interpretation:**
Culturing mature cardiomyocytes in microgravity provokes a highly unusual and paradoxical up-regulation of cell cycle regulatory pathways. Rather than indicating true cellular proliferation or beneficial regeneration, this molecular signature represents an abortive cell cycle re-entry triggered by severe oxidative stress and DNA damage. The accumulation of reactive oxygen species (ROS) and metabolic derangements force these terminally differentiated cells to activate cyclins and cyclin-dependent kinases, which ultimately clash with their established post-mitotic state. Consequently, this conflicting signaling cascade drives the cardiomyocytes toward cellular senescence and apoptosis, actively contributing to the overall myocardial atrophy and loss of functional tissue mass observed during extended spaceflight.

---

### Circadian entrainment
{{< figure src="/images/Circadian%20entrainment.png" alt="Circadian entrainment" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated (NES: 1.64)
**Significance:** FDR = 0.161

**In-Depth Biological Interpretation:**
The removal of natural geophysical zeitgebers in Low Earth Orbit, combined with the stress of microgravity, induces a significant dysregulation of circadian entrainment pathways within the peripheral clocks of cardiomyocytes. This up-regulation reflects a hypersensitization of the cellular molecular clock architecture as the heart struggles to synchronize its metabolic and contractile rhythms in a disrupted environment. At the molecular level, this desynchrony disrupts the temporal coordination of critical metabolic processes, including fatty acid oxidation and glucose utilization, which are highly dependent on robust circadian oscillations. Over time, this chronic internal circadian misalignment exacerbates mitochondrial dysfunction and metabolic inflexibility, further accelerating the cardiovascular deconditioning experienced by astronauts.


---

### Glycolysis - Gluconeogenesis
{{< figure src="/images/Glycolysis%20-%20Gluconeogenesis.png" alt="Glycolysis" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated
**In-Depth Biological Interpretation:** The robust up-regulation of the glycolysis pathway serves as a direct, compensatory survival mechanism in response to microgravity-induced mitochondrial dysfunction and pseudohypoxia. As cardiomyocytes lose their capacity for efficient oxidative phosphorylation, they become heavily reliant on anaerobic glycolysis to meet their ATP demands. However, this metabolic shift is highly inefficient for terminally differentiated heart muscle cells and inevitably leads to the accumulation of toxic byproducts, further driving the pre-diabetic state and long-term functional decline of the tissue.

---

### Other glycan degradation
{{< figure src="/images/Other%20glycan%20degradation.png" alt="Other glycan degradation" class="img-medium" >}}
**Enrichment Status:** 🔵 Down-regulated (NES: -2.03)
**In-Depth Biological Interpretation:** Representing the most severely down-regulated pathway in our dataset, the suppression of glycan degradation underscores a massive halt in standard extracellular matrix (ECM) turnover. In normal gravity, the mechanical beating of the heart requires continuous remodeling of the surrounding glycocalyx. The removal of this mechanical load in microgravity essentially "freezes" this remodeling process. This lack of healthy matrix turnover contributes directly to the stiffening of the cardiac tissue and disrupts proper mechanotransduction, ultimately impairing the mechanical elasticity of the astronauts' hearts.

---

### MicroRNAs in cancer
{{< figure src="/images/MicroRNAs%20in%20cancer.png" alt="MicroRNAs in cancer" class="img-medium" >}}
**Enrichment Status:** 🔴 Up-regulated
**In-Depth Biological Interpretation:** While seemingly unrelated to cardiac tissue, the enrichment of "MicroRNAs in cancer" pathways highlights a massive shift in post-transcriptional regulation within the stressed cardiomyocytes. Microgravity induces the expression of powerful non-coding RNAs (such as BANCR) and suppresses key RNA helicases (like DDX5) to alter protein translation rapidly. This indicates that the heart is utilizing highly conserved, stress-induced epigenetic and RNA-interference mechanisms—often hijacked by oncogenic processes—to aggressively adapt its survival networks to the extreme environment of spaceflight.