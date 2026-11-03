# Skill: Metabolism Estimation

> **Version:** 0.1.0
> **Category:** Advanced Analysis
> **Difficulty:** Intermediate–Advanced

---

## Overview

Anvi'o can predict the metabolic capabilities of organisms based on their genetic content using KEGG Orthologs (KOs) and metabolic module definitions. This skill guides you through setting up KEGG data, annotating your contigs database, and estimating metabolic pathway completeness.

## Prerequisites

- A contigs database (`contigs.db`) — see [Contigs Database Setup](../01-contigs-database-setup/SKILL.md)
- KEGG data set up on your computer (one-time setup)
- Optional: a profile database and collection for bin-level metabolism estimation

## Key Concepts

| Concept | Description |
|---|---|
| **KEGG Modules** | Defined metabolic pathways composed of KEGG Orthologs (KOs). |
| **KOfam** | HMM profiles for KEGG Orthologs, used for functional annotation. |
| **Pathwise Completeness** | Maximum completeness across all possible paths through a module. |
| **Stepwise Completeness** | Percentage of top-level steps that are complete. |
| **Module Copy Number** | Estimated number of complete copies of a metabolic module. |

## Step-by-Step Guide

### Step 1: Set Up KEGG Data (One-Time)

```bash
anvi-setup-kegg-data
```

> **Note:** This downloads KEGG data from the KEGG FTP server, which requires a KEGG license. If you do not have KEGG access, you can still use user-defined metabolic modules (see Step 6).

### Step 2: Annotate Contigs Database with KOfams

```bash
anvi-run-kegg-kofams -c contigs.db \
                     -T 6
```

This searches for KOfam HMM hits and stores them in the contigs database under the source name `KOfam`.

**Key parameters:**
- `--kegg-data-dir`: Path to KEGG data (if non-default location)
- `--hmmer-program`: `hmmsearch` (default) or `hmmscan`
- `--include-nt-KOs`: Include KOs without bit-score thresholds
- `--drop-previous-kegg-annotations`: Remove previous KEGG annotations before re-annotating

### Step 3: Estimate Metabolism for a Single Genome

```bash
anvi-estimate-metabolism -c contigs.db \
                         -O metabolism_output
```

This produces the default "modules" mode output file: `metabolism_output_modules.txt`.

### Step 4: Estimate Metabolism for Bins

If you have bins stored in a collection:

```bash
anvi-estimate-metabolism -c contigs.db \
                         -p PROFILE.db \
                         -C MY_COLLECTION \
                         -O metabolism_output
```

For a specific bin:

```bash
anvi-estimate-metabolism -c contigs.db \
                         -p PROFILE.db \
                         -C MY_COLLECTION \
                         -b BIN_NAME \
                         -O metabolism_output
```

### Step 5: Estimate Metabolism for Multiple Genomes

#### Multiple External Genomes

```bash
anvi-estimate-metabolism -e external-genomes.txt \
                         -O multi_metabolism \
                         --matrix-format
```

#### Multiple Internal Genomes (Bins)

```bash
anvi-estimate-metabolism -i internal-genomes.txt \
                         -O multi_metabolism \
                         --matrix-format
```

### Step 6: Use User-Defined Metabolic Modules (Optional)

If you want to define your own metabolic pathways:

```bash
# Set up user modules
anvi-setup-user-modules -M /path/to/user/modules/

# Estimate with user modules only
anvi-estimate-metabolism -c contigs.db \
                         --user-modules /path/to/user/modules/ \
                         --only-user-modules \
                         -O user_metabolism

# Estimate with both KEGG and user modules
anvi-estimate-metabolism -c contigs.db \
                         --user-modules /path/to/user/modules/ \
                         -O combined_metabolism
```

### Step 7: Metagenome Mode

For unbinned metagenome assemblies:

```bash
anvi-estimate-metabolism -c contigs.db \
                         --metagenome-mode \
                         -O metagenome_metabolism
```

> In metagenome mode, metabolism is estimated per contig, which tends to underestimate module completeness because pathways are often split across contigs.

### Step 8: Pangenome Metabolism

Estimate metabolism for gene cluster bins in a pangenome:

```bash
anvi-estimate-metabolism --pan-db MY_PAN/PAN.db \
                         -g ALL-GENOMES.db \
                         -C MY_COLLECTION \
                         -O pan_metabolism
```

## Output Modes

Use `--output-modes` to specify which output files to generate:

| Mode | Description | Suffix |
|---|---|---|
| `modules` (default) | Module-level information | `_modules.txt` |
| `module_paths` | Information on each possible path in a module | `_module_paths.txt` |
| `module_steps` | Information on each top-level step | `_module_steps.txt` |
| `hits` | All enzyme annotations regardless of module membership | `_hits.txt` |
| `modules_custom` | Customizable columns | `_modules_custom.txt` |

Example:

```bash
anvi-estimate-metabolism -c contigs.db --output-modes modules,hits,module_paths -O output
```

## Key Parameters

| Parameter | Description | Default |
|---|---|---|
| `--module-completion-threshold` | Completeness threshold for "complete" modules | 0.75 |
| `--add-copy-number` | Include module copy number in output | off |
| `--add-coverage` | Include coverage/detection data (requires profile DB) | off |
| `--only-complete` | Only include modules meeting the threshold | off |
| `--include-zeros` | Include modules with 0% completeness | off |
| `--matrix-format` | Output as matrices (multi-mode only) | off |
| `--include-metadata` | Add metadata columns to matrix output | off |

## Common Pitfalls

1. **Version mismatch**: If you re-run `anvi-setup-kegg-data --reset`, your existing annotated databases will have mismatched KEGG versions. You must re-annotate them with `anvi-run-kegg-kofams`.
2. **Forgetting to run KOfam annotation**: `anvi-estimate-metabolism` requires KOfam annotations in the contigs database. Always run `anvi-run-kegg-kofams` first.
3. **Misinterpreting metagenome mode**: Metagenome mode estimates per-contig, which underestimates completeness. Consider binning first for more accurate results.
4. **Module completion threshold**: The default 75% is a heuristic. Adjust based on your needs, especially when comparing across samples.

## Quick Reference

```bash
# Setup (one-time)
anvi-setup-kegg-data

# Annotate
anvi-run-kegg-kofams -c contigs.db -T 6

# Estimate
anvi-estimate-metabolism -c contigs.db -O metabolism

# With bins
anvi-estimate-metabolism -c contigs.db -p PROFILE.db -C COL -O metabolism

# Multi-mode with matrices
anvi-estimate-metabolism -i internal-genomes.txt --matrix-format -O matrices
```

## Output Artifacts

| Artifact | Description |
|---|---|
| `*_modules.txt` | Module-level metabolism estimation (default output) |
| `*_hits.txt` | All enzyme annotations |
| `*_module_paths.txt` | Per-path information |
| `*_module_steps.txt` | Per-step information |
| Matrix files | When `--matrix-format` is used (completeness, presence/absence, hits) |

## Next Steps

After completing this skill, you may want to explore:
- [**Taxonomy Estimation**](../06-taxonomy-estimation/SKILL.md) — Assign taxonomy to your genomes/bins
- [**Pangenomics**](../04-pangenomics/SKILL.md) — Compare metabolic potential across genomes
