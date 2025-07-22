                          ```text
                          ############################################################################
                          #                                                                          #
                          #     ██████╗ ██╗ ██████╗  █████╗ ███╗   ██╗ ██████╗ ██████╗ ███╗   ███╗   #
                          #    ██╔════╝ ██║██╔════╝ ██╔══██╗████╗  ██║██╔═══██╗██╔══██╗████╗ ████║   #
                          #    ██║  ███╗██║██║  ███╗███████║██╔██╗ ██║██║   ██║██████╔╝██╔████╔██║   #
                          #    ██║   ██║██║██║   ██║██╔══██║██║╚██╗██║██║   ██║██╔══██╗██║╚██╔╝██║   #
                          #    ╚██████╔╝██║╚██████╔╝██║  ██║██║ ╚████║╚██████╔╝██║  ██║██║ ╚═╝ ██║   #
                          #     ╚═════╝ ╚═╝ ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═══╝ ╚═════╝ ╚═╝  ╚═╝╚═╝     ╚═╝   #
                          #                                                                          #
                          #    GIGANORM:  When your datasets are so big, only 'giga' energy can      #
                          #               chew through them (and burp up a matrix).                  #
                          #                                                                          #
                          #                 "Go big, get normalized."                                #
                          ############################################################################
                          ```                                    

# giganorm 🐕‍🦺

**Effortlessly normalize gigantic (or tiny) NGS datasets — one pipeline, one matrix, no stress.**

---

## What is giganorm?

**giganorm** is a powerful, SLURM-ready pipeline for high-throughput calculation of normalized gene expression (TPM, RPKM, FPKM) over custom genomic regions, across hundreds or thousands of NGS samples.  
Automatically downloads, aligns, quantifies, and merges, so you get a single matrix in one go. Big data? Small data? No worries—giganorm has you covered.

---

## Features

- Parallelized download, alignment, and quantification (SLURM arrays)
- Automatic download (SRA with ENA fallback)
- Automatic BED formatting fixes (tabs/spaces)
- Skips samples and steps already completed—no recomputation
- Batch-limited job submission (no cluster overflow)
- All-in-one matrix output for any metric (TPM/RPKM/FPKM)
- Optionally keeps or cleans up all intermediate files
- Easy, robust, and fun (Norman the GigaDog says so!)

---

## Dependencies

| Dependency      | Description                                  | Website/Install           |
|-----------------|----------------------------------------------|---------------------------|
| **Bash**        | Shell interpreter (v4+ recommended)          | [bash](https://www.gnu.org/software/bash/)                |
| **SLURM**       | Workload manager for cluster arrays          | [slurm](https://slurm.schedmd.com/)                       |
| **STAR**        | RNA-seq aligner                              | [STAR](https://github.com/alexdobin/STAR)                 |
| **samtools**    | BAM file manipulation                        | [samtools](http://www.htslib.org/)                        |
| **bedtools**    | Genomic intervals utility                    | [bedtools](https://bedtools.readthedocs.io/en/latest/)     |
| **sra-tools**   | For `prefetch`, `fastq-dump`                 | [sra-tools](https://github.com/ncbi/sra-tools)            |
| **wget**        | Command-line downloader                      | [wget](https://www.gnu.org/software/wget/)                |
| **awk, grep**   | Standard text-processing tools               | Built-in to most Linux/Unix systems                       |

> ⚡️ **Tip:** Use [Conda](https://conda.io/) or your cluster modules to install everything.

---

## Installation

```bash
git clone https://github.com/YOUR_USERNAME/giganorm.git
cd giganorm
chmod +x giganorm
```

##Quick Start
Prepare:
```bash
srr_list.txt — one SRA/ENA accession per line (no headers)
coords.bed — regions to quantify (chr, start, end, region_name; tab or space delimited)
```
