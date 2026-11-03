# Skill: Metagenomic Profiling

> **Version:** 0.1.0
> **Category:** Core Workflow
> **Difficulty:** Beginner–Intermediate

---

## Overview

Metagenomic profiling in anvi'o involves mapping short reads from one or more samples back to a contigs database, then computing coverage, detection, and sequence variability (SNVs, SCVs, SAAVs). This skill walks through the complete profiling workflow from BAM file creation to merged profile databases.

## Prerequisites

- A contigs database (`contigs.db`) — see [Contigs Database Setup](../01-contigs-database-setup/SKILL.md)
- Short reads (FASTQ) for one or more samples
- A read mapper installed (Bowtie2 recommended)

## Key Concepts

| Concept | Description |
|---|---|
| **BAM File** | Binary Alignment/Map file containing read alignments to contigs. |
| **Single Profile DB** | Profile database for a single sample, created from one BAM file. |
| **Profile DB (Merged)** | Combined profile database from multiple single profiles. Contains cross-sample statistics. |
| **Coverage** | Average number of reads covering each nucleotide position. |
| **Detection** | Proportion of nucleotide positions with at least one read mapped. |
| **SNV** | Single-nucleotide variant — a position where reads differ from the reference. |

## Step-by-Step Guide

### Step 1: Map Reads to Contigs

Use Bowtie2 to map your short reads to the contigs FASTA:

```bash
# Build the index (one-time)
bowtie2-build contigs.fa contigs_index

# Map reads for each sample
bowtie2 --very-sensitive -x contigs_index \
        -1 sample_R1.fq.gz \
        -2 sample_R2.fq.gz \
        -o sample.sam
```

### Step 2: Convert and Index BAM File

Anvi'o requires an indexed, sorted BAM file:

```bash
samtools view -bS sample.sam > sample.raw.bam
anvi-init-bam -i sample.raw.bam -o sample.bam
```

> `anvi-init-bam` ensures the BAM file is properly sorted and reheadered for anvi'o compatibility.

### Step 3: Profile a Single Sample

Create a single-profile database for each sample:

```bash
anvi-profile -i sample.bam \
             -c contigs.db \
             -S SAMPLE_NAME \
             -T 6
```

**Key parameters:**
- `-i`: Input BAM file
- `-c`: Contigs database
- `-S`: Sample name (must be unique across all samples)
- `-T`: Number of threads
- `--min-contig-length`: Minimum contig length to profile (default: 1000)
- `--min-percent-identity`: Minimum read identity (default: 0)
- `--min-coverage-for-variability`: Minimum coverage to report variants (default: 10)
- `--profile-SCVs`: Also profile single-codon variants (adds runtime)

### Step 4: Merge Multiple Single Profiles

If you have multiple samples, merge their single-profile databases:

```bash
anvi-merge */PROFILE.db \
           -c contigs.db \
           -o MERGED_PROFILE \
           -S MERGED_PROJECT \
           --enforce-hierarchical-clustering
```

> If you have more than 25,000 splits, hierarchical clustering is skipped by default to avoid excessive memory usage. Use `--enforce-hierarchical-clustering` to force it (ensure you have enough RAM).

**Clustering parameters:**
- `--distance`: Distance metric (default: `euclidean`)
- `--linkage`: Linkage method (default: `ward`)

### Step 5: Profile with a Blank Profile (No Mapping Data)

If you lack mapping data but still want profile database functionality:

```bash
anvi-profile -c contigs.db \
             --blank-profile \
             -S BLANK_SAMPLE
```

## Understanding Profile Output

The merged profile database contains:

| Data | Description |
|---|---|
| Per-split coverage | Mean coverage and detection for each split in each sample |
| Variability tables | SNV, SCV, and SAAV positions across samples |
| Hierarchical clustering | Dendrogram of splits based on coverage patterns (if computed) |
| Items order | The order of splits for display in the interactive interface |

## Common Pitfalls

1. **Inconsistent parameters across samples**: If you plan to merge, keep profiling parameters (`--min-contig-length`, `--min-percent-identity`, etc.) consistent across all samples.
2. **Missing sample names**: Always provide `-S` with a unique name; otherwise anvi'o will auto-generate one that may not be meaningful.
3. **Forgetting `anvi-init-bam`**: Raw BAM files from some mappers may not be properly formatted for anvi'o. Always run `anvi-init-bam`.
4. **Large datasets with clustering**: Hierarchical clustering on >25,000 splits requires significant RAM. Consider skipping it or splitting your dataset.

## Quick Reference

```bash
# Map reads
bowtie2 --very-sensitive -x contigs_index -1 R1.fq.gz -2 R2.fq.gz -o sample.sam
samtools view -bS sample.sam > sample.raw.bam
anvi-init-bam -i sample.raw.bam -o sample.bam

# Profile
anvi-profile -i sample.bam -c contigs.db -S SAMPLE_NAME -T 6

# Merge
anvi-merge */PROFILE.db -c contigs.db -o MERGED -S PROJECT
```

## Output Artifacts

| Artifact | Description |
|---|---|
| `PROFILE.db` | Single-sample or merged profile database |
| `AUXILIARY-DATA.db` | Auxiliary data for variability visualization |
| `SAMPLES-MERGED` | Directory containing the merged profile |

## Next Steps

After completing this skill, proceed to:
- [**Binning and Refinement**](../03-binning-and-refinement/SKILL.md) — Cluster contigs into bins (MAGs)
- [**Interactive Interface**](../07-interactive-interface/SKILL.md) — Visualize your profile data
