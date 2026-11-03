# Skill: Binning and Refinement

> **Version:** 0.1.0
> **Category:** Core Workflow
> **Difficulty:** Intermediate

---

## Overview

Binning is the process of grouping contigs from a metagenome into "bins" that ideally represent individual genomes (MAGs — Metagenome-Assembled Genomes). Anvi'o supports multiple binning algorithms, manual refinement through the interactive interface, and comprehensive summary generation.

## Prerequisites

- A merged profile database (`PROFILE.db`) — see [Metagenomic Profiling](../02-metagenomic-profiling/SKILL.md)
- A contigs database (`contigs.db`)
- At least one binning algorithm installed (CONCOCT, MetaBAT2, MaxBin2, or DAS Tool)

## Key Concepts

| Concept | Description |
|---|---|
| **Collection** | A named set of bins stored in a profile database. |
| **Bin** | A group of contigs that are hypothesized to belong to the same population/genome. |
| **MAG** | Metagenome-Assembled Genome — a bin that meets quality criteria. |
| **Completeness** | Estimated percentage of expected single-copy core genes present in a bin. |
| **Redundancy** | Estimated percentage of duplicated single-copy core genes in a bin (contamination indicator). |

## Step-by-Step Guide

### Step 1: Automated Binning

Run an automated binning algorithm on your merged profile:

```bash
anvi-cluster-contigs -p PROFILE.db \
                     -c contigs.db \
                     -C MY_COLLECTION \
                     --driver concoct \
                     -T 6
```

**Available drivers:**
- `concoct` — CONCOCT binning algorithm
- `metabat2` — MetaBAT2 binning algorithm
- `maxbin2` — MaxBin2 binning algorithm
- `dastool` — DAS Tool (integrates results from multiple binners)
- `binsanity` — BinSanity binning algorithm

### Step 2: Check Bin Quality

Assess the completeness and redundancy of your bins:

```bash
anvi-compute-completeness -c contigs.db \
                          -p PROFILE.db \
                          -C MY_COLLECTION
```

This uses single-copy core gene HMMs (Bacteria_71, Archaea_76, Protista_83) to estimate genome completeness and contamination.

### Step 3: Refine Bins Manually

Open the interactive interface to refine specific bins:

```bash
anvi-refine -p PROFILE.db \
            -c contigs.db \
            -C MY_COLLECTION \
            -b BIN_NAME
```

In the interactive interface you can:
- Remove contigs with anomalous coverage or taxonomy
- Split bins that appear to contain multiple populations
- Merge contigs from different bins

### Step 4: Import External Binning Results

If you have binning results from an external tool, import them as a collection:

```bash
anvi-import-collection bins.txt \
                       -p PROFILE.db \
                       -c contigs.db \
                       -C EXTERNAL_COLLECTION \
                       --contigs-mode
```

The `bins.txt` file should be a TAB-delimited file with two columns: `contig_name` and `bin_name`.

### Step 5: Generate a Summary

Create a comprehensive summary of your collection:

```bash
anvi-summarize -c contigs.db \
               -p PROFILE.db \
               -C MY_COLLECTION \
               -o SUMMARY \
               --init-gene-coverages
```

The output `SUMMARY` directory contains:
- `index.html` — Interactive HTML summary
- `bins_summary.txt` — Per-bin statistics (length, GC, completeness, redundancy)
- `bin_by_bin/` — Per-bin FASTA files, HMM hits, coverage stats
- `bins_across_samples/` — Cross-sample matrices

### Step 6: Rename Bins (Optional)

Give your bins more meaningful names:

```bash
anvi-rename-bins -p PROFILE.db \
                 -c contigs.db \
                 -C MY_COLLECTION \
                 --prefix MAG_ \
                 --report-file renaming_report.txt
```

### Step 7: Dereplicate Genomes (Optional)

If you have multiple bins from different samples, dereplicate them at a given ANI threshold:

```bash
anvi-dereplicate-genomes -i internal-genomes.txt \
                         -o DEREPLICATED \
                         --ani 95
```

## Quality Assessment Guidelines

| Metric | Good | Questionable | Poor |
|---|---|---|---|
| Completeness | >70% | 50–70% | <50% |
| Redundancy | <5% | 5–10% | >10% |

## Common Pitfalls

1. **Not refining automated bins**: Automated binning is a starting point. Always visually inspect and refine bins for best results.
2. **Ignoring redundancy**: High redundancy (>10%) indicates contamination. Refine to split mixed bins.
3. **Wrong contigs mode**: When importing external collections, remember to use `--contigs-mode` if your file uses contig names rather than split names.
4. **Forgetting to summarize**: `anvi-summarize` provides essential quality metrics and exported files for downstream analysis.

## Quick Reference

```bash
# Bin
anvi-cluster-contigs -p PROFILE.db -c contigs.db -C COL --driver concoct -T 6

# Check quality
anvi-compute-completeness -c contigs.db -p PROFILE.db -C COL

# Refine
anvi-refine -p PROFILE.db -c contigs.db -C COL -b BIN_NAME

# Summarize
anvi-summarize -c contigs.db -p PROFILE.db -C COL -o SUMMARY
```

## Output Artifacts

| Artifact | Description |
|---|---|
| Collection | Stored internally in the profile database |
| `SUMMARY/` | Directory with comprehensive bin summaries and exported files |
| `bins_summary.txt` | Table of per-bin statistics |
| Per-bin FASTA | Contig sequences for each bin in `SUMMARY/bin_by_bin/` |

## Next Steps

After completing this skill, proceed to:
- [**Pangenomics**](../04-pangenomics/SKILL.md) — Compare genomes using your MAGs
- [**Taxonomy Estimation**](../06-taxonomy-estimation/SKILL.md) — Assign taxonomy to your bins
- [**Metabolism Estimation**](../05-metabolism-estimation/SKILL.md) — Predict metabolic capabilities
