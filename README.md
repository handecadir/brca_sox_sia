# TCGA-BRCA RNA-Seq Analysis Project

> **Note:** All analyses were performed using **RStudio’s Bioconductor Docker image**.  
> This means all data processing and analyses were conducted inside a **Docker container**, ensuring reproducibility and consistent environment setup.

This project focuses on **TCGA-BRCA (Breast Invasive Carcinoma)** RNA-Seq data, performing **differential gene expression (DGE) analysis**, **PCA**, **gene correlation**, **network analysis**, and **survival analysis**. The analyses specifically emphasize **SOX and Sialyltransferase genes**.  

The main goal is to identify **expression differences between tumor and normal tissues**, highlight critical genes, and examine the relationship between gene expression and patient survival.

---

## 📂 Content and Analysis Steps

### 1️⃣ Data Download and Preparation
- **Source:** GDC (Genomic Data Commons)  
- **Project:** TCGA-BRCA  
- **Data Type:** RNA-Seq, Gene Expression Quantification (STAR - Counts)  
- **Steps:**
  1. Query the project using `GDCquery()`.
  2. Download data via `GDCdownload()`.
  3. Load data into R using `GDCprepare()`.
  4. Clean Ensembl IDs and map them to gene symbols.

> This ensures analyses are performed using standardized gene names.

---

### 2️⃣ Normalization and PCA
- **Goal:** Explore sample variance and clustering patterns.
- **Method:**
  1. Convert counts to `edgeR::DGEList()`.
  2. Apply **TMM (Trimmed Mean of M-values) normalization**.
  3. Calculate CPM (Counts per Million).
  4. Filter low-expressed genes (genes with CPM > 1 in at least 10% of samples).
  5. Log2 transformation.
  6. PCA analysis and visualization of **Normal vs Tumor** samples.

---

### 3️⃣ Differential Gene Expression (DGE) Analysis
- **Goal:** Identify significant expression differences between normal and tumor tissues.
- **Method:**
  1. Select only **Normal and Tumor** samples (exclude metastatic samples).
  2. Compare groups using GLM-based **LRT (Likelihood Ratio Test)**.
  3. Visualize results with Volcano Plots.
  4. Highlight target genes (SOX and Sialyltransferase) separately.
- **Outputs:**
  - Full DGE results saved as Excel.
  - Target gene DGE results saved in a separate Excel file.

---

### 4️⃣ Boxplot Visualization
- **Goal:** Display expression distribution of selected genes across Normal and Tumor tissues.
- **Method:**
  - Separate panels for each gene.
  - Outliers and individual samples plotted as points.
  - Normal and Tumor tissues color-coded.

---

### 5️⃣ Survival Analysis (Kaplan-Meier)
- **Goal:** Explore relationship between gene expression and patient survival.
- **Method:**
  1. Use only tumor samples.
  2. Split expression into **High** and **Low** groups based on median.
  3. Fit Kaplan-Meier curves.
  4. Compute p-values using log-rank test.
- **Outputs:** Separate KM plots and risk tables for each gene.

---

### 6️⃣ Gene Correlation and Heatmap
- **Goal:** Assess correlations among selected genes.
- **Method:**
  1. Calculate Pearson correlation.
  2. Mark correlations with p < 0.05 with '*'.
  3. Generate heatmaps for both normal and tumor tissues.

---

### 7️⃣ Gene Network Analysis
- **Goal:** Visualize gene-gene interaction networks.
- **Method:**
  1. Select correlations with |r| > 0.5 and p < 0.05.
  2. Group genes into **SOX genes** and **Sialyltransferase genes**.
  3. Draw networks using `igraph`.
  4. Positive correlations in red, negative in blue.

---

## ⚙️ Usage
1. Install required R packages:

```r
BiocManager::install(c(
  "TCGAbiolinks", "org.Hs.eg.db", "edgeR", "ggplot2", "ggrepel", 
  "reshape2", "writexl", "survival", "survminer", "psych", "pheatmap", "igraph"
))
