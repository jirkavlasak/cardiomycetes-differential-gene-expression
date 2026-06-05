---
title: "Cardiomyocytes in Microgravity"
---

# Welcome to the Space Heart RNA-Seq Portal

This interactive portal presents the findings of our comprehensive bioinformatics semestral project. We analyze the profound transcriptomic changes in human induced pluripotent stem cell-derived cardiomyocytes (hiPSC-CMs) exposed to long-term microgravity.

### Background & Motivation
Astronauts returning from prolonged missions aboard the International Space Station (ISS) frequently suffer from cardiovascular deconditioning, including muscle atrophy, arrhythmias, and diminished cardiac output. While the physiological symptoms are well documented, the underlying molecular mechanisms remain a critical area of space biology research. 

### Project Objectives
The primary goal of this project is to perform an *in silico* re-analysis of RNA-Seq data comparing **ISS microgravity (uG)** samples against **Earth ground controls (1G)**. Specifically, we aimed to:
* Identify key Differentially Expressed Genes (DEGs) triggered by the absence of gravitational mechanical loading.
* Map these genetic shifts to functional biological pathways to understand systemic cellular responses.
* Define the molecular "Spaceflight Phenotype" of the human heart, focusing on metabolic reprogramming and structural remodeling.

### Our Analytical Approach
Rather than relying on black-box web tools, we built a robust, custom bioinformatics pipeline from scratch. The workflow begins with raw read quality control (FastQC) and trimming (Trimmomatic), followed by pseudoalignment (Salmon). The downstream statistical modeling and biological interpretation were implemented entirely in Python, utilizing `pydeseq2` for differential expression and `gseapy` for pathway enrichment (KEGG and GO terms).

> **Output format note:** While the project specification suggests RMarkdown/Quarto HTML, we deliberately chose a static Hugo web portal as a more structured and navigable format for presenting multi-section bioinformatics results. All analytical code, visualizations, and biological interpretations are fully documented within this portal.

### How to Navigate This Portal
Use the top navigation menu to explore the different phases of our research:

* **[The Report](/report/):** The main findings, featuring PCA clustering, Volcano plots, and our core biological conclusions regarding diabetic-like metabolic shifts and pseudohypoxia.
* **[Extended Gallery](/gallery/):** A deep dive into specific cellular pathways (e.g., TGF-beta, Adrenergic signaling, Oxidative stress) with highly detailed biological interpretations.
* **[Methodology & Code](/methodology/):** The exact Python code, statistical logic, and visualizations used to transform raw counts into biological meaning.
* **[Data Preprocessing](/data-preprocessing/):** The upstream quality control steps and multi-tool summaries ensuring data integrity prior to analysis.
* **[AI Analysis & Study Comparison](/ai-analysis/):** AI-assisted biological interpretation and comparison of our results with the original publication.

### Where to find data and code?
Just follow this link to Google Drive.
[**Google Drive — full data (raw, trimmed, rRNA-filtered, code)**](https://drive.google.com/drive/folders/1d_qXQj8n-N5JepKf7TfNIGuQsnm-bhjl?usp=drive_link)

---
*Developed as a Bioinformatics Semestral Project by **Jiří Vlasák** and **Tobias Krynek**.*