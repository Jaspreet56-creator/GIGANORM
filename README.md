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

## Quick Start
Prepare:
```bash
srr_list.txt — one SRA/ENA accession per line (no headers)
coords.bed — regions to quantify (chr, start, end, region_name; tab or space delimited)
```
Run:
```bash
./giganorm srr_list.txt coords.bed /path/to/STARindex /path/to/output_dir 500 TPM
```
-All samples will be processed in parallel batches (max 500 at a time) [Please change this as per your available resources - SLURM capacity, storage etc]
-Per-sample metrics appear in /path/to/output_dir/TPM/SRRxxxxxx.TPM.txt
-A single merged matrix will be generated: /path/to/output_dir/TPM/combined_TPM_matrix.tsv
-All logs are in /path/to/output_dir/logs/

## To keep all intermediate files, add:
```bash
./giganorm --keep-intermediate srr_list.txt coords.bed /path/to/STARindex /path/to/output_dir 500 TPM
```
## Output
Per-sample metric files:
```bash
output_dir/TPM/SRR12345.TPM.txt
```
Combined matrix:
```bash
output_dir/TPM/combined_TPM_matrix.tsv
```
Logs:
```bash
output_dir/logs/
```
## Usage
Show help:
```bash
./giganorm --help

                                                ############################################################################
                                                #                                                                          #
                                                #     ██████╗ ██╗ ██████╗  █████╗ ███╗   ██╗ ██████╗ ██████╗ ███╗   ███╗   #
                                                #    ██╔════╝ ██║██╔════╝ ██╔══██╗████╗  ██║██╔═══██╗██╔══██╗████╗ ████║   #
                                                #    ██║  ███╗██║██║  ███╗███████║██╔██╗ ██║██║   ██║██████╔╝██╔████╔██║   #
                                                #    ██║   ██║██║██║   ██║██╔══██║██║╚██╗██║██║   ██║██╔══██╗██║╚██╔╝██║   #
                                                #    ╚██████╔╝██║╚██████╔╝██║  ██║██║ ╚████║╚██████╔╝██║  ██║██║ ╚═╝ ██║   #
                                                #     ╚═════╝ ╚═╝ ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═══╝ ╚═════╝ ╚═╝  ╚═╝╚═╝     ╚═╝   #
                                                #                                                                          #
                                                #           GIGANORM:   When your datasets are so big,                 #
                                                #                       only a pipeline with 'giga' energy can             #
                                                #                       chew through them (and burp up a matrix).          #
                                                #                                                                          #
                                                #                         "Go big, get normalized."                        #
                                                #                                                                          #
                                                ############################################################################


Usage:
  ./giganorm [--keep-intermediate] [--run-task] <srr_list.txt> <coords.bed> <STAR_index_dir> <output_dir> <batch_size> <metric>

Arguments:
  srr_list.txt     File with one SRR accession per line.
  coords.bed       BED file of regions; column 4 = region name (tab-delimited or space-delimited, will be fixed if needed).
  STAR_index_dir   Path to STAR genome index directory.
  output_dir       Root directory for outputs (fastq/, bam/, counts/, logs/, <metric>/).
  batch_size       Max number of concurrent tasks in the SLURM array. (Not needed in --run-task mode.)
  metric           TPM, RPKM, or FPKM (normalizes by uniquely mapped reads).

Options:
  -h, --help         Show this help message and exit.
  --keep, --keep-intermediate
                     Keep all intermediate files (FASTQ, BAM, counts, etc.) [default: delete].
  --run-task         Run a specific array task (internal).
```

## Mascot

      /^-----^\
     V  o o  V        “Woof! I sniffed out your counts.”
      |  Y  |         Norman the GigaDog approves this pipeline.
       \ Q /          All data normalized. Tail is wagging.
       / - \
       |    \
       |     \     "If your files are too big,
       || (___\        just giganorm it!"

## License
