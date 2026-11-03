# Skill: Taxonomy Estimation

> **Version:** 0.1.0
> **Category:** Advanced Analysis
> **Difficulty:** Intermediate

---

## Overview

Anvi'o can estimate the taxonomy of genomes, bins, and metagenomes using single-copy core genes (SCGs) compared against the GTDB (Genome Taxonomy Database). This skill covers the setup, annotation, and taxonomy estimation workflow.

## Prerequisites

- A contigs database (`contigs.db`) — see [Contigs Database Setup](../01-contigs-database-setup/SKILL.md)
- HMM searches already run on the contigs database (`anvi-run-hmms`)
- SCG taxonomy data set up on your computer (one-time setup)

## Key Concepts

| Concept | Description |
|---|---|
| **SCG** | Single-copy core gene — genes expected to appear once per genome. |
| **GTDB** | Genome Taxonomy Database — the reference database used for taxonomy assignment. |
| **SCG Taxonomy** | Taxonomy estimated by comparing SCG sequences to GTDB reference genomes. |
| **tRNA Taxonomy** | Taxonomy estimated using tRNA sequences (alternative approach). |

## Step-by-Step Guide

### Step 1: Set Up SCG Taxonomy Database (One-Time)

```bash
anvi-setup-scg-taxonomy
```

This downloads and prepares the GTDB-based SCG search databases.

### Step 2: Annotate Contigs Database with SCG Taxonomy

```bash
anvi-run-scg-taxonomy -c contigs.db \
                      -T 6
```

> This step requires that `anvi-run-hmms` has already been run on the contigs database, as SCG taxonomy relies on the HMM hits for single-copy core genes.

### Step 3: Estimate Taxonomy for a Single Genome

```bash
anvi-estimate-scg-taxonomy -c contigs.db \
                           -O taxonomy_output
```

This provides the best taxonomy estimate for your genome based on SCG hits.

To see detailed per-gene hits:

```bash
anvi-estimate-scg-taxonomy -c contigs.db \
                           --debug
```

### Step 4: Estimate Taxonomy for Bins

```bash
anvi-estimate-scg-taxonomy -c contigs.db \
                           -p PROFILE.db \
                           -C MY_COLLECTION \
                           -O taxonomy_output
```

For a specific bin:

```bash
anvi-estimate-scg-taxonomy -c contigs.db \
                           -p PROFILE.db \
                           -C MY_COLLECTION \
                           -b BIN_NAME \
                           -O taxonomy_output
```

### Step 5: Estimate Taxonomy in Metagenome Mode

For an unbinned metagenome:

```bash
anvi-estimate-scg-taxonomy -c contigs.db \
                           --metagenome-mode \
                           -O metagenome_taxonomy
```

### Step 6: Include Relative Abundance Across Samples

If you have a profile database with multiple samples:

```bash
anvi-estimate-scg-taxonomy -c contigs.db \
                           --metagenome-mode \
                           -p PROFILE.db \
                           --compute-scg-coverages \
                           -O taxonomy_with_abundance
```

This adds per-sample coverage columns showing the relative abundance of each taxon.

### Step 7: Multiple Genomes at Once

```bash
# External genomes
anvi-estimate-scg-taxonomy -e external-genomes.txt \
                           -O multi_taxonomy

# Internal genomes (bins)
anvi-estimate-scg-taxonomy -i internal-genomes.txt \
                           -O multi_taxonomy
```

### Step 8: tRNA Taxonomy (Alternative)

Anvi'o also supports taxonomy estimation based on tRNA sequences:

```bash
# One-time setup
anvi-setup-trna-taxonomy

# Annotate
anvi-run-trna-taxonomy -c contigs.db

# Estimate
anvi-estimate-trna-taxonomy -c contigs.db \
                            -O trna_taxonomy
```

## Output Format

The output file contains taxonomy levels from domain to species:

| Column | Description |
|---|---|
| `taxon` | Full taxonomy string |
| `t_domain` | Domain-level taxonomy |
| `t_phylum` | Phylum-level taxonomy |
| `t_class` | Class-level taxonomy |
| `t_order` | Order-level taxonomy |
| `t_family` | Family-level taxonomy |
| `t_genus` | Genus-level taxonomy |
| `t_species` | Species-level taxonomy |
| `confidence` | Confidence score |
| `bin_name` | (When running on bins) Name of the bin |
| `gene_name` | SCG used for this estimate |

## Common Pitfalls

1. **Forgetting to run HMMs first**: SCG taxonomy depends on HMM hits. Always run `anvi-run-hmms` before `anvi-run-scg-taxonomy`.
2. **Low-completeness genomes**: SCG taxonomy works poorly for low-completeness bins (<50%), viral genomes, or eukaryotic genomes.
3. **Version mismatch**: If you re-run `anvi-setup-scg-taxonomy`, existing annotated databases may have mismatched versions. Re-annotate them.
4. **Expecting species-level resolution**: SCG taxonomy often cannot resolve to species level. Interpret results at the genus or family level when confidence is low.

## Quick Reference

```bash
# Setup (one-time)
anvi-setup-scg-taxonomy

# Annotate
anvi-run-scg-taxonomy -c contigs.db -T 6

# Estimate
anvi-estimate-scg-taxonomy -c contigs.db -O taxonomy

# For bins
anvi-estimate-scg-taxonomy -c contigs.db -p PROFILE.db -C COL -O taxonomy
```

## Output Artifacts

| Artifact | Description |
|---|---|
| `*_taxonomy.txt` | Tab-delimited taxonomy estimation results |
| SCG annotations | Stored internally in the contigs database |

## Next Steps

After completing this skill, you may want to explore:
- [**Binning and Refinement**](../03-binning-and-refinement/SKILL.md) — Create bins for taxonomy estimation
- [**Metabolism Estimation**](../05-metabolism-estimation/SKILL.md) — Connect taxonomy with metabolic potential
