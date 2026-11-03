# Skill: Snakemake Workflows

> **Version:** 0.1.0
> **Category:** Automation
> **Difficulty:** Advanced

---

## Overview

Anvi'o provides Snakemake-based workflows that automate common multi-step analyses. These workflows handle dependencies, parallelization, and reproducibility. Available workflows include metagenomics, pangenomics, contigs processing, phylogenomics, and tRNA-seq.

## Prerequisites

- Snakemake installed
- Anvi'o installed and activated
- Appropriate input data for the chosen workflow

## Key Concepts

| Concept | Description |
|---|---|
| **Workflow** | A Snakemake pipeline defining a series of analysis steps. |
| **Workflow Config** | A JSON file specifying all parameters for a workflow. |
| **Snakefile** | The Snakemake rule file that defines the workflow logic. |
| **Rule** | A single step in the workflow (e.g., mapping, profiling). |

## Available Workflows

| Workflow | Description |
|---|---|
| `contigs` | Process contigs: gene calling, HMMs, functional annotations |
| `metagenomics` | Full metagenomic pipeline: QC, assembly, mapping, profiling, binning |
| `pangenomics` | Pangenomic analysis from genome FASTA files |
| `phylogenomics` | Phylogenomic tree construction |
| `trnaseq` | tRNA sequencing analysis |
| `ecophylo` | Ecological phylogenomics |
| `sra_download` | Download data from SRA |

## Step-by-Step Guide

### Step 1: Get the Default Configuration

```bash
anvi-run-workflow -w metagenomics \
                  --get-default-config metagenomics-config.json
```

This creates a JSON configuration file with all default parameters.

### Step 2: Check Workflow Dependencies

```bash
anvi-run-workflow -w metagenomics \
                  --list-dependencies
```

Ensure all required tools are installed before running.

### Step 3: Edit the Configuration

Open the JSON config file and modify parameters to match your project:

```json
{
  "samples": {
    "sample_1": ["path/to/R1.fq.gz", "path/to/R2.fq.gz"],
    "sample_2": ["path/to/R1.fq.gz", "path/to/R2.fq.gz"]
  },
  "contigs_db": {
    "contigs": "contigs.fa",
    "project_name": "MY_PROJECT"
  },
  "mapping": {
    "bowtie2": {
      "additional_params": "--very-sensitive"
    }
  }
}
```

### Step 4: Dry Run (Test Configuration)

```bash
anvi-run-workflow -w metagenomics \
                  -c metagenomics-config.json \
                  --dry-run
```

This shows what steps would be executed without actually running them.

### Step 5: Run the Workflow

```bash
anvi-run-workflow -w metagenomics \
                  -c metagenomics-config.json \
                  --save-workflow-graph
```

The `--save-workflow-graph` flag creates a visual representation (DAG) of the workflow.

### Step 6: Pass Additional Snakemake Parameters

```bash
anvi-run-workflow -w metagenomics \
                  -c metagenomics-config.json \
                  -A "--cores 16 --resources mem=64000"
```

The `-A` flag passes additional parameters directly to Snakemake.

## Workflow-Specific Configuration

### Metagenomics Workflow

The metagenomics workflow includes:
1. Read quality control (using illumina-utils)
2. Assembly (using MEGAHIT or metaSPAdes)
3. Contigs database creation
4. Read mapping (Bowtie2)
5. Profiling and merging
6. Binning (CONCOCT)
7. Summarization

### Pangenomics Workflow

```bash
anvi-run-workflow -w pangenomics \
                  --get-default-config pangenomics-config.json
```

The pangenomics workflow:
1. Creates contigs databases for each input genome
2. Generates a genomes storage database
3. Runs the pangenomic analysis
4. Computes gene cluster homogeneity

### Contigs Workflow

```bash
anvi-run-workflow -w contigs \
                  --get-default-config contigs-config.json
```

The contigs workflow:
1. Creates a contigs database
2. Runs HMMs
3. Annotates with COGs and/or KOfams
4. Scans for tRNAs

## Configuration File Structure

The configuration JSON typically includes sections for:

```json
{
  "samples": { },
  "contigs_db": { },
  "mapping": { },
  "profile_db": { },
  "binning": { },
  "summary": { }
}
```

Each section contains parameters specific to that stage of the workflow.

## Common Pitfalls

1. **Missing dependencies**: Always run `--list-dependencies` first. Missing tools will cause workflow failures.
2. **Incorrect sample paths**: Ensure all file paths in the config are correct and accessible.
3. **Insufficient resources**: Snakemake will try to run jobs in parallel. Set appropriate `--cores` and `--resources` limits.
4. **Config schema changes**: Workflow configurations may differ between anvi'o versions. Regenerate the default config when upgrading.

## Quick Reference

```bash
# Get default config
anvi-run-workflow -w metagenomics --get-default-config config.json

# Check dependencies
anvi-run-workflow -w metagenomics --list-dependencies

# Dry run
anvi-run-workflow -w metagenomics -c config.json --dry-run

# Full run
anvi-run-workflow -w metagenomics -c config.json --save-workflow-graph

# With Snakemake params
anvi-run-workflow -w metagenomics -c config.json -A "--cores 16"
```

## Output Artifacts

Output depends on the workflow but typically includes:

| Artifact | Description |
|---|---|
| `contigs.db` | Contigs database |
| `PROFILE.db` | Merged profile database |
| `SUMMARY/` | Summary directory |
| Workflow DAG | Visual graph of the workflow (with `--save-workflow-graph`) |

## Next Steps

After mastering workflows, you may want to:
- Customize workflow rules for your specific needs
- Explore the [anvi'o workflow documentation](https://merenlab.org/2018/07/09/anvio-snakemake-workflows/)
- Combine automated workflows with manual refinement using [Binning and Refinement](../03-binning-and-refinement/SKILL.md)
