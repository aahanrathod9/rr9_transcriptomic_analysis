# rr9_transcriptomic_analysis
This repo analyzes NASA OSD-255 mouse retina RNA-Seq data to identify gene signatures correlated with spaceflight-induced oxidative stress (HNE) and apoptosis (TUNEL). It uses machine learning models to link gene expression to phenotypic damage, resulting in key gene lists for MSigDB analysis (Tables 5 &amp; 7).

# Transcriptomic Analysis of Spaceflight Effects on Mouse Retina

This repository contains the Jupyter Notebook used for the transcriptomic analysis of mouse retinal tissue following spaceflight (NASA Open Science Data Repository, OSD-255).

The notebook includes code blocks of data loading, filtering, differential gene expression analysis (DGEA), and various machine learning regression tasks (ElasticNet, Random Forest, Perceptron, etc.) to link gene expression with phenotypic outcomes (HNE and TUNEL staining) in the retina.

## Instructions
1. Open rr9_final_notebook.ipynb in Google Drive
2. Run all cells in notebook (Google Drive will prompt a login in order to mount)
    * Colab will install dependencies, mount Google Drive, download the GTF file automatically, and execute all analysis steps.

Running the full notebook should take a few minutes, depending on network speed for data/file downloads and the amount of tests ran.

---

##  Key Results and Reviewer Guide

Near the end of the notebook, after all cells have been executed, the script prints a summary block containing four sets:

```
union: {...}
intersection: {...}
only hne: {...}
only tunel: {...}
```

The gene sets corresponding to the manuscript are:

- 'only hne' - the final HNE gene list used in Table 5  
- 'only tunel' - the final TUNEL gene list used in Table 7

These sets represent the union of genes identified across the majority of high-performing models for each phenotype. These lists of genes will be used to compare with MSigDB in the next section.

## Checking the Gene Lists in MSigDB (for Tables 5 and 7 of paper)

The notebook prints Python-style gene sets:
```
{'Anp32a', 'Arl6ip1', 'Atf4', ...}
```
MSigDB cannot parse braces or quotes, so the lists must be cleaned before analysis.

### 1. Extract the Gene List

After running the notebook, locate the printed sets:
* only hne (Table 5)
* only tunel (Table 7)

Copy the entire set.

### 2. Clean the Formatting

Remove:
* { and }
* '
* commas

and convert the list to space-separated format.

```
Anp32a Arl6ip1 Atf4 B2m Chgb ...
```

### 3. Open MSigDB (Mouse Collections)
    1. Go to https://www.gsea-msigdb.org/gsea/msigdb
    2. In left sidebar, under Mouse Collections (in green) click "Investigate"
<img width="988" height="1138" alt="Screenshot 2025-12-15 at 4 51 19 PM" src="https://github.com/user-attachments/assets/503f95e8-a46c-4abe-9d9d-b1a39bc47ea2" />

### 4. Run GO Overlap Analysis
	1. Paste clean space-seperated gene list into input box
	2. Under Compute Overlaps select *GO: Gene Ontology* and it should check all of the mouse collections under the GO category.
	3. Click compute overlaps at the bottom and compare results.
<img width="826" height="701" alt="Screenshot 2025-12-15 at 6 49 20 PM" src="https://github.com/user-attachments/assets/2fb2543f-59cc-4402-b2c3-43fe6e9c7a81" />
