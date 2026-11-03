# Skill: Contigs Database Setup

> **Version:** 0.1.0
> **Category:** Foundational
> **Difficulty:** Beginner

---

## Overview

The anvi'o contigs database (`contigs.db`) is the cornerstone of virtually every anvi'o workflow. It stores all information related to your sequences: gene calls, k-mer frequencies, functional annotations, and more. This skill guides you through creating a contigs database from a FASTA file and enriching it with annotations.

## Prerequisites

- Anvi'o installed and activated (`anvi-self-test` should pass)
- An assembly FASTA file (contigs) ready for analysis
- Basic familiarity with the command line

## Key Concepts

| Concept | Description |
|---|---|
| **Contigs FASTA** | A FASTA file containing assembled contig sequences. Must be properly formatted before use. |
| **Contigs DB** | An SQLite-based database storing gene calls, k-mer frequencies, and annotations for your contigs. |
| **Split** | Anvi'o soft-splits long contigs (>20,000 bp by default) into smaller units called "splits" for efficient processing. |
| **Gene Calling** | Identification of open reading frames (ORFs) using Pyrodigal by default. |

## Step-by-Step Guide

### Step 1: Prepare Your Contigs FASTA

Ensure your FASTA file is properly formatted. Contig names should be simple (no spaces or special characters). If needed, use `anvi-script-reformat-fasta`:

```bash
anvi-script-reformat-fasta -i raw_assembly.fa \
                           -o contigs.fa \
                           --prefix CONTIG \
                           --min-length 1000 \
                           --simplify-names
```

### Step 2: Generate the Contigs Database

Create a contigs database from your FASTA file:

```bash
anvi-gen-contigs-database -f contigs.fa \
                          --project-name MY_PROJECT \
                          -o contigs.db \
                          -T 4
```

**Key parameters:**
- `-f`: Input FASTA file (supports `.gz` compressed files)
- `--project-name`: A unique name for this database (important for downstream analyses)
- `-o`: Output database path
- `-T`: Number of threads (multithreading speeds up the process)
- `--kmer-size`: K-mer size for frequency computation (default: 4, max: 5 without SQLite recompilation)
- `--split-length`: Soft-split length for long contigs (default: 20,000 bp)

### Step 3: Run HMM Searches for Single-Copy Core Genes

Annotate your contigs database with HMM hits for single-copy core genes (useful for completeness/redundancy checks and taxonomy):

```bash
anvi-run-hmms -c contigs.db \
              --num-threads 6
```

This searches for Bacteria_71, Archaea_76, Protista_83, and ribosomal RNA HMM profiles by default.

To run a specific HMM source only:

```bash
anvi-run-hmms -c contigs.db \
              -I Bacteria_71
```

### Step 4: Annotate with Functional Databases (Optional but Recommended)

#### NCBI COGs

```bash
# One-time setup (skip if already done)
anvi-setup-ncbi-cogs

# Run COG annotation
anvi-run-ncbi-cogs -c contigs.db \
                   -T 6
```

#### KEGG KOfams (for metabolism estimation)

```bash
# One-time setup (requires KEGG access)
anvi-setup-kegg-data

# Run KOfam annotation
anvi-run-kegg-kofams -c contigs.db \
                     -T 6
```

### Step 5: Scan for tRNAs (Optional)

```bash
anvi-scan-trnas -c contigs.db
```

### Step 6: Check Database Status

Verify your database contents:

```bash
anvi-db-info -c contigs.db
```

You can also display contigs statistics:

```bash
anvi-display-contigs-stats -c contigs.db
```

## Common Pitfalls

1. **Non-unique project names**: Always assign a unique `--project-name`. Duplicate names cause issues in multi-sample analyses.
2. **Improperly formatted FASTA**: Contig names with spaces or special characters can cause errors downstream.
3. **Forgetting to run HMMs**: Many downstream analyses (completeness estimation, taxonomy) depend on HMM hits being present in the contigs database.
4. **Insufficient threads**: Gene calling and HMM searches are CPU-intensive. Use `-T` to multithread when possible.

## Quick Reference

```bash
# Minimal workflow
anvi-script-reformat-fasta -i raw.fa -o contigs.fa --simplify-names
anvi-gen-contigs-database -f contigs.fa -o contigs.db --project-name MY_PROJECT
anvi-run-hmms -c contigs.db --num-threads 6
anvi-run-ncbi-cogs -c contigs.db -T 6
```

## Output Artifacts

| Artifact | Description |
|---|---|
| `contigs.db` | The contigs database containing all sequence-level information |
| HMM hits | Stored internally in the contigs database |
| Functional annotations | Stored internally in the contigs database gene_functions table |

## Next Steps

After completing this skill, proceed to:
- [**Metagenomic Profiling**](../02-metagenomic-profiling/SKILL.md) — Profile your contigs against read mappings
- [**Interactive Interface**](../07-interactive-interface/SKILL.md) — Visualize your contigs database
- [**Metabolism Estimation**](../05-metabolism-estimation/SKILL.md) — Estimate metabolic potential (if KEGG annotations are present)
