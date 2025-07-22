# giganorm 🐕‍🦺

**Normalize gigantic (or tiny) NGS datasets, fast. One pipeline, one matrix, no stress.**

---

## What is it?

`giganorm` is a Slurm-ready, SRA-to-matrix pipeline for generating TPM/RPKM/FPKM quantifications on custom regions across hundreds (or thousands!) of samples, in parallel. Fast, fun, and robust—just like our mascot, Norman the GigaDog.

## Features

- Automatic download (SRA/ENA fallback)
- Auto-fixes BED formatting
- Handles hundreds of samples with batch limiting (Slurm arrays)
- Reuses partial results—never recomputes what you’ve already done!
- Produces a single, ready-to-analyze expression matrix
- Easy cleanup, funny mascot

---

## Installation

1. Clone the repo:
    ```bash
    git clone https://github.com/YOUR_USERNAME/giganorm.git
    cd giganorm
    ```
2. Make the script executable:
    ```bash
    chmod +x giganorm
    ```

3. **Dependencies:**  
    - Bash, SLURM, STAR, samtools, bedtools, fastq-dump/prefetch, wget, awk  
    - (See [dependencies](#dependencies) section below)

---

## Quick Start

Prepare:
- `srr_list.txt` — one SRR/sample per line
- `coords.bed` — regions to quantify (BED, can be space- or tab-delimited)

**Example:**
```bash
./giganorm srr_list.txt coords.bed /path/to/STARindex /path/to/output_dir 10 TPM
