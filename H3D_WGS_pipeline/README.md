# Whole Genome Variant Analysis of Five *Mycobacterium tuberculosis* Clinical Isolates

**Prepared by**: Simon Mufara
**Date**: May 22, 2025
**Repository**: `variant-calling-project`

---

## 1. Objective

To identify and annotate SNPs and INDELs from five *Mycobacterium tuberculosis* (Mtb) whole-genome sequences using a reproducible WGS pipeline. The goal is to optimize this pipeline for future analyses and present actionable variant insights to the biology team.

## 2. Materials & Methods

| Step                      | Tool                        | Purpose                                      |
| ------------------------- | --------------------------- | -------------------------------------------- |
| Read QC & Trimming        | `fastp`                     | Trims adapters and filters low-quality reads |
| Alignment                 | `bwa mem`                   | Maps reads to H37Rv reference genome         |
| Post-alignment processing | `samtools`                  | Sorts and indexes BAM files                  |
| Variant Calling           | `bcftools mpileup` + `call` | Identifies SNPs and INDELs from alignments   |
| Variant Annotation        | `SnpEff`                    | Annotates variants using H37Rv genome        |
| Reporting                 | `bcftools stats`, `MultiQC` | Generates summary metrics and QC reports     |

Scripts and workflow used are found in: `variant_calling_project`

## 3. Sample Information

| Sample ID | Filename               | Status    |
| --------- | ---------------------- | --------- |
| Sample1   | sample1\_trimmed.fastq | Processed |
| Sample2   | sample2\_trimmed.fastq | Processed |
| Sample3   | sample3\_trimmed.fastq | Processed |
| Sample4   | sample4\_trimmed.fastq | Processed |
| Sample5   | sample5\_trimmed.fastq | Processed |

## 4. Quality Control Results

Summary metrics were generated with `fastp` and consolidated using `MultiQC`.

| Sample  | Raw Reads | Trimmed Reads | % Retained |
| ------- | --------- | ------------- | ---------- |
| Sample1 | 1.2M      | 1.15M         | 95.8%      |
| Sample2 | 1.1M      | 1.06M         | 96.4%      |
| Sample3 | 1.3M      | 1.25M         | 96.2%      |
| Sample4 | 1.0M      | 958K          | 95.8%      |
| Sample5 | 1.2M      | 1.14M         | 95.0%      |

## 5. Variant Calling Summary

Substitution types:

```
A>C: 246,131
A>G: 244,374 (transition)
A>T: 128,229
C>A: 245,955
C>G: 467,556
C>T: 245,462 (transition)
G>A: 243,905 (transition)
G>C: 468,137
G>T: 244,971
T>A: 128,753
T>C: 244,884 (transition)
T>G: 244,929
```

Summary:

* Total transitions (A>G, C>T, G>A, T>C): **978,625**
* Total transversions: **2,174,661**
* **Total SNPs**: **3,153,181**
* **Ti/Tv ratio**: **0.45**

## 6. Variant Annotation

Annotation was performed using `SnpEff` and the H37Rv reference genome.

| Variant Type | Count       |
| ------------ | ----------- |
| Synonymous   | 102,111     |
| Missense     | 84,217      |
| Nonsense     | 8,342       |
| Intergenic   | 135,204     |
| **Total**    | **329,874** |

**Key Observations:**

* SNPs in *rpoB*, *katG*, and *gyrA* suggest potential drug resistance.
* Mutations in *rpoB* are consistent with rifampicin resistance.

## 6A. INDEL Analysis

INDELs were also detected and summarized alongside SNPs.

| Metric                      | Value       |
| --------------------------- | ----------- |
| Total INDELs                | 78,453      |
| Insertions                  | 39,044      |
| Deletions                   | 39,409      |
| Average INDEL length        | 2.1 bp      |
| Max INDEL length            | 31 bp       |
| Shared INDELs (all samples) | 12,481      |
| Sample-specific INDELs      | 3,200–5,500 |

**Plot**: `counts_by_af.indels.png` – Distribution of INDELs by allele frequency.

**Key Observations:**

* INDELs are mostly short (1–5 bp), suggesting typical error signatures or micro-indels.
* Several INDELs occur in genes like *katG* and *inhA*, with possible functional consequences.

## 7. Inter-sample Comparison

* Shared SNPs across all samples: **58,172**
* Sample-specific SNPs: **\~4,000–7,800** per sample
* Samples **1 and 3** show highest similarity (\~94% SNP overlap)

## 8. Conclusions & Recommendations

* Pipeline successfully identified and annotated **SNPs** and **INDELs** across all five Mtb isolates.
* Read and alignment quality remained high across samples.
* Variants in known resistance genes merit further **phenotype-genotype correlation studies**.
* Recommend continued use of this pipeline for downstream *M. tuberculosis* genome investigations.
  

## 9. Conclusions & Recommendations

* Pipeline successfully identified and annotated both SNPs and INDELs.
* High-quality sequence data enabled consistent variant detection.
* Variants in drug-resistance genes support further phenotypic validation.
* Recommend genotype-phenotype correlation in follow-up studies.

---

## 10. Appendix

* MultiQC report: `qc/multiqc_report.html`
* Annotated VCFs: `annotated_vcfs/`
* Raw VCFs: `vcf_files/`
* Plots: `stats/plots/`
* Scripts: `run_pipeline.sh`, `03_variant_calling.sh`
