<p align="center">
  <img src="giganorm.svg" alt="GIGANORM Logo" width="400">
</p>                              

# giganorm 🐕‍🦺

**Effortlessly normalize gigantic (or tiny) NGS datasets — one pipeline, one matrix, no stress.**

---

## What is giganorm?

**giganorm** is a powerful, SLURM-optimized pipeline designed for high-throughput, region-based gene expression quantification (TPM, RPKM, or FPKM) across hundreds or thousands of NGS samples, or just a few—no project is too big or too small. From automatic SRA/ENA data retrieval and robust download fallback, through STAR-based alignment and bedtools quantification over any custom BED-defined regions, giganorm automates every step and logs all actions for reproducibility. Its smart, job-scaled SLURM integration handles massive datasets efficiently, reusing partial results and keeping your cluster happy, while user-friendly features like optional intermediate file cleanup, auto-conversion of BED formats, and clear error handling make it as stress-free as possible. The pipeline ends by merging all per-sample results into a single, ready-to-analyze matrix—no extra scripts or manual merging required—so you can focus on your science instead of your workflow. Whether you're quantifying genes, exons, enhancers, APA events, or any user-defined features, giganorm ensures you get accurate, normalized counts and a mascot-powered stamp of approval every run. If you want reliable, reproducible, and scalable quantification—giganorm has you covered.

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
| **STAR**        | RNA-seq aligner                              | [STAR](https://github.com/alexdobin/STAR) [1]             |
| **samtools**    | BAM file manipulation                        | [samtools](http://www.htslib.org/) [2]                    |
| **bedtools**    | Genomic intervals utility                    | [bedtools](https://bedtools.readthedocs.io/en/latest/)[3] |
| **sra-tools**   | For `prefetch`, `fastq-dump`                 | [sra-tools](https://github.com/ncbi/sra-tools) [4]        |
| **wget**        | Command-line downloader                      | [wget](https://www.gnu.org/software/wget/)                |
| **WebProxy**    | proxy server with web-based configuration    | [WebProxy](https://github.com/bp2008/WebProxy)[5]         |
| **awk, grep**   | Standard text-processing tools               | Built-in to most Linux/Unix systems                       |

> ⚡️ **Tip:** Use [Conda](https://conda.io/) or your cluster modules to install everything.

---

## Installation

```bash
git clone https://github.com/Jaspreet56-creator/GIGANORM.git
cd giganorm
chmod +x giganorm
```
---
## Prerequisites

**Make sure the INDEX file is already generated for your target species using STAR**

IF Not, please use the following command to do so:
```bash
STAR --runThreadN 4 --runMode genomeGenerate --genomeDir /path/to/STARindex --genomeFastaFiles /path/to/genome_fasta_file.fa --sjdbGTFfile /path/to/genome_annotation_gtf.gtf
```

## Quick Start
Prepare:
```bash
example_srr_list.txt — one SRA/ENA accession per line (no headers) [PLEASE INCLUDE ANY CHARACTER IN LAST LINE OF THIS FILE]
example_coords.bed — regions to quantify (chr, start, end, region_name; tab or space delimited)
```
Run:
```bash
./giganorm example_srr_list.txt example_coords.bed /path/to/STARindex /path/to/output_dir 500 TPM
```
All samples will be processed in parallel batches (3 in this toy example) [Please change this as per your available resources - SLURM capacity, storage etc]

Per-sample metrics appear in /path/to/output_dir/TPM/SRRxxxxxx.TPM.txt

A single merged matrix will be generated: /path/to/output_dir/TPM/combined_TPM_matrix.tsv

All logs are in /path/to/output_dir/logs/

---

## To keep all intermediate files, add:
```bash
./giganorm --keep-intermediate example_srr_list.txt example_coords.bed /path/to/STARindex /path/to/output_dir 500 TPM
```
---

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
---

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
                        #           GIGANORM:   When your datasets are so big,                     #
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
---

## Mascot

      /^-----^\
     V  o o  V        “Woof! I sniffed out your counts.”
      |  Y  |         Norman the GigaDog approves this pipeline.
       \ Q /          All data normalized. Tail is wagging.
       / - \
       |    \
       |     \           "If your files are too big,
       || (___\              just giganorm it!"

---

## License
MIT License

Copyright (c) 2025 jaspreetthind

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

## References
1. Dobin A, Davis CA, Schlesinger F, Drenkow J, Zaleski C, Jha S, Batut P, Chaisson M, Gingeras TR. STAR: ultrafast universal RNA-seq aligner.
   Bioinformatics. 2013 Jan 1;29(1):15-21. doi: 10.1093/bioinformatics/bts635. Epub 2012 Oct 25. PMID: 23104886; PMCID: PMC3530905.
2. Li H, Handsaker B, Wysoker A, Fennell T, Ruan J, Homer N, Marth G, Abecasis G, Durbin R; 1000 Genome Project Data Processing Subgroup. The Sequence Alignment/Map format and SAMtools.
   Bioinformatics. 2009 Aug 15;25(16):2078-9. doi: 10.1093/bioinformatics/btp352. Epub 2009 Jun 8. PMID: 19505943; PMCID: PMC2723002.
3. Quinlan AR, Hall IM. BEDTools: a flexible suite of utilities for comparing genomic features.
   Bioinformatics. 2010 Mar 15;26(6):841-2. doi: 10.1093/bioinformatics/btq033. Epub 2010 Jan 28. PMID: 20110278; PMCID: PMC2832824.
4. NCBI SRA Toolkit Development Team (2024). SRA Toolkit. National Center for Biotechnology Information.
   GitHub Repository. https://github.com/ncbi/sra-tools
5. bp2008. (2024). WebProxy.
   GitHub. https://github.com/bp2008/WebProxy
