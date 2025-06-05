# Whole Genome Variant Analysis of Five *Mycobacterium tuberculosis* Clinical Isolates

**Prepared by:** Simon Mufara
**Date:** May 22, 2025
**Repository:** `variant-calling-project`

---

## 1. Objective

To identify and annotate SNPs and INDELs from five *Mycobacterium tuberculosis* whole-genome sequences using a reproducible WGS pipeline. The goal is to optimize this pipeline for future analyses and present actionable variant insights to the biology team.

---

## 2. Materials & Methods

| Step               | Tool                        | Purpose                                      |
| ------------------ | --------------------------- | -------------------------------------------- |
| Read QC & Trimming | `fastp`                     | Trims adapters and filters low-quality reads |
| Alignment          | `bwa mem`                   | Maps reads to H37Rv reference genome         |
| Post-alignment     | `samtools`                  | Sorts and indexes BAM files                  |
| Variant Calling    | `bcftools mpileup + call`   | Identifies SNPs and INDELs                   |
| Variant Annotation | `SnpEff`                    | Annotates variants using H37Rv genome        |
| Reporting          | `bcftools stats`, `MultiQC` | Summarizes metrics and QC reports            |

Scripts and workflow used are located in: `variant_calling_project/`

---

## 3. Sample Information

| Sample ID | Filename                | Status    |
| --------- | ----------------------- | --------- |
| Sample1   | `sample1_trimmed.fastq` | Processed |
| Sample2   | `sample2_trimmed.fastq` | Processed |
| Sample3   | `sample3_trimmed.fastq` | Processed |
| Sample4   | `sample4_trimmed.fastq` | Processed |
| Sample5   | `sample5_trimmed.fastq` | Processed |

---

## 4. Quality Control Results

Summarized using `fastp` and `MultiQC`:

| Sample  | Raw Reads | Trimmed Reads | % Retained |
| ------- | --------- | ------------- | ---------- |
| Sample1 | 1.2M      | 1.15M         | 95.8%      |
| Sample2 | 1.1M      | 1.06M         | 96.4%      |
| Sample3 | 1.3M      | 1.25M         | 96.2%      |
| Sample4 | 1.0M      | 958K          | 95.8%      |
| Sample5 | 1.2M      | 1.14M         | 95.0%      |

---

## 5. Variant Calling Summary (SNPs)

### Substitution Types

* A>C: 246,131
* A>G: 244,374 *(transition)*
* A>T: 128,229
* C>A: 245,955
* C>G: 467,556
* C>T: 245,462 *(transition)*
* G>A: 243,905 *(transition)*
* G>C: 468,137
* G>T: 244,971
* T>A: 128,753
* T>C: 244,884 *(transition)*
* T>G: 244,929

**Totals:**

* Transitions (Ti): 978,625
* Transversions (Tv): 2,174,661
* Total SNPs: 3,153,181
* **Ti/Tv Ratio:** 0.45

---

## 6. Variant Annotation Summary

Annotated with `SnpEff` using the H37Rv genome:

| Variant Type | Count   |
| ------------ | ------- |
| Synonymous   | 102,111 |
| Missense     | 84,217  |
| Nonsense     | 8,342   |
| Intergenic   | 135,204 |
| **Total**    | 329,874 |

**Key Observations:**

* Variants in *rpoB*, *katG*, and *gyrA* suggest potential drug resistance.
* *rpoB* mutations indicate possible rifampicin resistance.

---

## 7. Inter-sample Comparison

* **Shared SNPs across all samples:** 58,172
* **Sample-specific SNPs:** 4,000–7,800 per sample
* **Closest similarity:** Samples 1 and 3 (94% SNP overlap)

---

## 8. INDEL Analysis

![](stats/plots/counts_by_af.indels.png)

* INDELs plotted by allele frequency.
* The distribution shows a predominance of rare INDELs (AF < 0.2).

---

## 9. Conclusions & Recommendations

* Pipeline successfully identified and annotated both SNPs and INDELs.
* High-quality sequence data enabled consistent variant detection.
* Variants in drug-resistance genes support further phenotypic validation.
* Recommend genotype-phenotype correlation in follow-up studies.

---

## 10. Appendix

* **MultiQC Report:** `multiqc_report.html`
* **Annotated VCFs:** `annotated_vcfs/`
* **Raw VCFs:** `vcf_files/`
* **Pipeline Script:** `run_pipeline.sh`
