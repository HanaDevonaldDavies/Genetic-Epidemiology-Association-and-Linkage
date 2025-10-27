# Genetic-Epidemiology-Association-and-Linkage

This repository contains a complete pipeline for performing **Genome-Wide Association Study (GWAS) quality control (QC)** and **association analysis** using PLINK and R. The workflow is demonstrated on a simulated Alzheimer’s Disease (AD) case-control dataset (`denti`), following established best practices for GWAS.

---

## 📁 Project Overview

- **Objective**: To perform rigorous QC and association testing on a simulated GWAS dataset.
- **Phenotype**: Alzheimer’s Disease (binary case-control)
- **Tools Used**: PLINK v1.9, R (v4.3.1)
- **Input**: PLINK binary files (`.bed`, `.bim`, `.fam`), covariate file
- **Output**: Quality-controlled dataset, PCA results, Manhattan/QQ plots, association results

---

## 🧬 Dataset

- **Simulated Dataset**: `denti`
- **Sample Size**: 1,790 individuals (837 cases, 932 controls)
- **SNPs**: ~230,000 autosomal SNPs
- **Covariates**: Sex, age, top 10 principal components

> ⚠️ **Note**: This is a **dummy dataset** for educational purposes. Results are not biologically interpretable.

---

## 🔧 Pipeline Steps

### 1. Initial Sample QC
- Sex discordance check (`--check-sex`)
- Sample missingness filter (`--mind 0.05`)
- Heterozygosity outlier detection (`--het`)

### 2. SNP-Level QC
- SNP missingness filter (`--geno 0.05`)
- Hardy-Weinberg Equilibrium filter in controls (`--hwe 1e-6`)
- Minor Allele Frequency filter (`--maf 0.01`)

### 3. Population Structure & Relatedness
- LD pruning (`--indep-pairwise 50 5 0.2`)
- Exclusion of long-range LD regions
- PCA (two rounds) to correct for stratification
- IBD estimation to remove related individuals (`--genome`)

### 4. Association Testing
- Logistic regression with covariate adjustment
- Covariates: sex, age, top 10 PCs
- Significance thresholds:
  - Genome-wide: \( p < 5 \times 10^{-8} \)
  - Suggestive: \( p < 1 \times 10^{-6} \)

---

## 📊 Key Results

- Two suggestive SNPs identified:
  - `rs2304206` (Chr19, OR = 0.699, p = 1.77e-06)
  - `rs2304204` (Chr19, OR = 0.709, p = 4.37e-06)
- Genomic inflation factor (λ) = 1.0595 → minimal inflation
- No genome-wide significant hits
- Clean QQ and Manhattan plots
