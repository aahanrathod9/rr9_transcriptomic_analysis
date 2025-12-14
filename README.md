# rr9_transcriptomic_analysis
This repo analyzes NASA OSD-255 mouse retina RNA-Seq data to identify gene signatures correlated with spaceflight-induced oxidative stress (HNE) and apoptosis (TUNEL). It uses machine learning models to link gene expression to phenotypic damage, resulting in key gene lists for MSigDB analysis (Tables 5 &amp; 7).



# Transcriptomic Analysis of Spaceflight Effects on Mouse Retina

This repository contains the Jupyter Notebook used for the transcriptomic analysis of mouse retinal tissue following spaceflight (NASA Open Science Data Repository, OSD-255).

The notebook performs data loading, filtering, differential gene expression analysis (DGEA), and various machine learning regression tasks (ElasticNet, Random Forest, Perceptron, etc.) to link gene expression with phenotypic outcomes (HNE and TUNEL staining) in the retina.

The primary goal of this README is to provide clear instructions for replicating the analysis and for reviewers to easily find the key results, particularly the gene lists for Gene Ontology (GO) analysis mentioned in the associated manuscript (Tables 5 and 7).

## Getting Started

### 1. Environment Setup

The notebook, `rr9_final_notebook.ipynb`, is designed to run in **Google Colaboratory** due to its use of the `google.colab` library for mounting Google Drive and handling file downloads.

#### A. Running in Google Colab

1.  **Open the Notebook:** Upload `rr9_final_notebook.ipynb` to your Google Drive and open it with Google Colab.
2.  **Run the Setup Cells:** Execute the first two code cells under the heading **"Prepare python environment"**:
    * The first cell installs all necessary Python packages (e.g., `pydeseq2`, `mygene`, `pybiomart`, `rnanorm`).
    * The second cell attempts to mount Google Drive:
        ```python
        from google.colab import drive
        drive.flush_and_unmount()
        drive.mount("mnt")
        ```
        **Follow the prompts** to authorize Colab to access your Google Drive. This is necessary because the notebook will download the mouse reference GTF file to a specific path in your mounted drive (`/content/mnt/MyDrive/NASA/RR9/Mus_musculus.GRCm39.115.gtf.gz`).
3.  **Download GTF File:** The next cell will download the mouse genome annotation file (`Mus_musculus.GRCm39.115.gtf.gz`) and save it to the specified location on your Google Drive. This file is required for the **TPM normalization** step (`full_transform` function) using `rnanorm`.

### 2. Running the Code

The remainder of the notebook is structured into functions and their subsequent execution for different analyses:

* **Define data processing methods:** (Cells defining functions like `read_meta_data`, `filter_data`, `run_elasticnet`, etc.) **Run all these cells.**
* **REGRESSION MODELS:** (Cells defining models like ELASTICNET, RANDOM FOREST, etc.) **Run all these cells.**
* **DATASET 1: HNE ANALYSIS** (The analysis block for **Table 5** data). **Run this entire block.**
* **DATASET 2: TUNEL ANALYSIS** (The analysis block for **Table 7** data). **Run this entire block.**

Running the full notebook should take a few minutes, depending on network speed for data/file downloads and the complexity of the machine learning cross-validation.

---

##  Key Results and Reviewer Guide

The analysis is divided into two primary sections, each corresponding to a key result table in the associated manuscript.

### 1. HNE Analysis (Corresponds to Manuscript **Table 5**)

This section of the notebook performs a multi-model analysis (ElasticNet, Ridge, Random Forest, Perceptron) to find gene sets correlated with HNE (H&E staining) phenotype data.

* **Location in Notebook:** Under the heading **`# DATASET 1: HNE ANALYSIS`**
* **Target Gene List:** The final cell in this block generates the gene list for the regression model with the highest performance (e.g., the best ElasticNet or Random Forest).
* **Output:** The gene list for Table 5 is printed directly to the output of the final cell, typically under the heading:
    ```
    # Table 5 genes for MSigDB
    ```
    The output will look similar to this:
    ```
    Gene list for MSigDB (Table 5):
    ['Pcdhgc3', 'Prkcg', 'Bcar1', 'Mettl3', ...]
    ```

### 2. TUNEL Analysis (Corresponds to Manuscript **Table 7**)

This section of the notebook performs a multi-model analysis to find gene sets correlated with TUNEL (apoptosis) phenotype data.

* **Location in Notebook:** Under the heading **`# DATASET 2: TUNEL ANALYSIS`**
* **Target Gene List:** The final cell in this block generates the gene list for the highest performing regression model.
* **Output:** The gene list for Table 7 is printed directly to the output of the final cell, typically under the heading:
    ```
    # Table 7 genes for MSigDB
    ```
    The output will look similar to this:
    ```
    Gene list for MSigDB (Table 7):
    ['Acss3', 'Ptpn2', 'Fgf1', 'Tcf7l2', ...]
    ```

### Gene Ontology Analysis with MSigDB

To reproduce the Gene Ontology (GO) analysis of Tables 5 and 7:

1.  **Copy the Gene List:** Copy the full, comma-separated list of gene symbols (e.g., `['Pcdhgc3', 'Prkcg', 'Bcar1', ...]`) from the output of the relevant notebook cell (as described above).
2.  **Open MSigDB:** Navigate to the **Molecular Signatures Database (MSigDB)**:
    [https://www.gsea-msigdb.org/gsea/msigdb/](https://www.gsea-msigdb.org/gsea/msigdb/)
3.  **Use Gene Set Search:**
    * Click on the **"Search Gene Sets"** tab (or **"Search"** in the navigation bar).
    * Select **"Gene Set Search and Analysis"** (if not already selected).
    * Paste the copied gene list into the **"Enter a list of genes"** text area.
    * **Crucially, ensure the gene format is correct:** The notebook output uses **Mouse Gene Symbols** (e.g., *Pcdhgc3*). MSigDB will perform the conversion/mapping automatically.
    * Click **"Search"** or **"Analyze"** to view the over-represented Gene Ontology and pathway terms, matching the results summarized in Tables 5 and 7.

##  Visual Results

The notebook automatically generates and displays several diagnostic and results plots:

* **Mean-Variance Plot:** Generated for the initial RNA-Seq data to show data characteristics.
* **Phenotype Box Plots:** Generated by `run_plots()` to compare HNE/TUNEL phenotype values between spaceflight and ground control samples.
* **PCA Plot:** Generated by `run_plots()` to visualize the separation of ground control and spaceflight samples based on the processed gene expression data.
