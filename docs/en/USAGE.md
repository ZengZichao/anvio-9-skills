# Usage Guide

> Complete guide to using the anvio-9-skills repository.

## 1. Getting Started

### 1.1 Prerequisites

Before using these skills, ensure you have anvi'o installed:

```bash
# Check anvi'o installation
anvi-self-test --suite mini

# Check version
anvi-interactive --version
```

If anvi'o is not installed, follow the [official installation instructions](https://merenlab.org/software/anvio/).

### 1.2 Repository Structure

```
anvio-9-skills/
├── README.md                    # English README
├── README_CN.md                 # Chinese README
├── CHANGELOG.md                 # Version history
├── LICENSE                      # MIT License
├── docs/
│   ├── en/
│   │   ├── USAGE.md             # This file
│   │   └── REFERENCE.md         # Reference information
│   └── zh/
│       ├── USAGE_CN.md          # Chinese usage guide
│       └── REFERENCE_CN.md      # Chinese reference info
├── skills/
│   ├── 01-contigs-database-setup/
│   │   └── SKILL.md
│   ├── 02-metagenomic-profiling/
│   │   └── SKILL.md
│   ├── 03-binning-and-refinement/
│   │   └── SKILL.md
│   ├── 04-pangenomics/
│   │   └── SKILL.md
│   ├── 05-metabolism-estimation/
│   │   └── SKILL.md
│   ├── 06-taxonomy-estimation/
│   │   └── SKILL.md
│   ├── 07-interactive-interface/
│   │   └── SKILL.md
│   └── 08-snakemake-workflows/
│       └── SKILL.md
└── examples/
```

### 1.3 Reading a Skill Document

Each skill document (`SKILL.md`) follows this structure:

1. **Header** — version, category, and difficulty level
2. **Overview** — brief description of what the skill covers
3. **Prerequisites** — what you need before starting
4. **Key Concepts** — a table of important terminology
5. **Step-by-Step Guide** — numbered steps with code blocks
6. **Common Pitfalls** — frequently encountered issues
7. **Quick Reference** — condensed command summary for quick lookup
8. **Output Artifacts** — description of files produced
9. **Next Steps** — links to related skills

## 2. Using the Skills

### 2.1 Choose Your Starting Point

| If you want to... | Start with Skill |
|---|---|
| Process raw assembly contigs | [01 Contigs Database Setup](../skills/01-contigs-database-setup/SKILL.md) |
| Analyze metagenomic samples | [02 Metagenomic Profiling](../skills/02-metagenomic-profiling/SKILL.md) |
| Reconstruct genomes from metagenomes | [03 Binning and Refinement](../skills/03-binning-and-refinement/SKILL.md) |
| Compare multiple genomes | [04 Pangenomics](../skills/04-pangenomics/SKILL.md) |
| Predict metabolic capabilities | [05 Metabolism Estimation](../skills/05-metabolism-estimation/SKILL.md) |
| Identify organisms in your data | [06 Taxonomy Estimation](../skills/06-taxonomy-estimation/SKILL.md) |
| Visualize data interactively | [07 Interactive Interface](../skills/07-interactive-interface/SKILL.md) |
| Automate a full pipeline | [08 Snakemake Workflows](../skills/08-snakemake-workflows/SKILL.md) |

### 2.2 Copying Commands

All commands in the skill documents are designed to be copy-pasted into your terminal. Look for:

- **Placeholders** in ALL_CAPS (e.g., `MY_PROJECT`, `SAMPLE_NAME`) — replace with your actual values
- **File paths** like `contigs.fa`, `contigs.db` — adjust to match your actual file locations
- **Thread counts** like `-T 6` — adjust based on your available CPUs

### 2.3 Working Through a Complete Metagenomic Analysis

Here's a typical end-to-end workflow using multiple skills:

```bash
# === Skill 01: Create contigs database ===
anvi-gen-contigs-database -f assembly.fa -o contigs.db --project-name MY_PROJECT
anvi-run-hmms -c contigs.db -T 6
anvi-run-ncbi-cogs -c contigs.db -T 6

# === Skill 02: Profile samples ===
# (For each sample)
bowtie2 --very-sensitive -x contigs_index -1 R1.fq.gz -2 R2.fq.gz -o sample.sam
samtools view -bS sample.sam > sample.raw.bam
anvi-init-bam -i sample.raw.bam -o sample.bam
anvi-profile -i sample.bam -c contigs.db -S SAMPLE_NAME -T 6

# Merge
anvi-merge */PROFILE.db -c contigs.db -o MERGED -S PROJECT

# === Skill 03: Binning ===
anvi-cluster-contigs -p MERGED/PROFILE.db -c contigs.db -C COL --driver concoct -T 6
anvi-compute-completeness -c contigs.db -p MERGED/PROFILE.db -C COL

# === Skill 07: Visualize and refine ===
anvi-interactive -p MERGED/PROFILE.db -c contigs.db -C COL
anvi-refine -p MERGED/PROFILE.db -c contigs.db -C COL -b BIN_NAME

# === Skill 03: Summarize ===
anvi-summarize -c contigs.db -p MERGED/PROFILE.db -C COL -o SUMMARY

# === Skill 06: Taxonomy ===
anvi-setup-scg-taxonomy
anvi-run-scg-taxonomy -c contigs.db -T 6
anvi-estimate-scg-taxonomy -c contigs.db -p MERGED/PROFILE.db -C COL -O taxonomy

# === Skill 05: Metabolism ===
anvi-setup-kegg-data
anvi-run-kegg-kofams -c contigs.db -T 6
anvi-estimate-metabolism -c contigs.db -p MERGED/PROFILE.db -C COL -O metabolism
```

## 3. Tips for New Users

### 3.1 Start Small

Before running on your full dataset, test with a small subset to verify commands work:
```bash
# Extract a few contigs for testing
head -n 1000 contigs.fa > test_contigs.fa
```

### 3.2 Use Multithreading

Most anvi'o programs accept `-T` for thread count. Use as many threads as your system allows:
```bash
anvi-run-hmms -c contigs.db -T 16  # Use 16 threads
```

### 3.3 Keep Your Databases Organized

```bash
mkdir -p project/{contigs_db,profiles,summaries}
```

### 3.4 Check Program Help

Every anvi'o program has extensive built-in help:
```bash
anvi-gen-contigs-database -h     # Basic help
anvi-run-hmms -hh                # Extended help (if available)
```

### 3.5 Visit the Anvi'o Community

- [anvi'o documentation](https://anvio.org/)
- [anvi'o GitHub](https://github.com/merenlab/anvio)
- [anvi'o Discord](https://discord.gg/CFQeCwD8S5)

## 4. Troubleshooting

### Common Issues

| Issue | Solution |
|---|---|
| "command not found: anvi-*" | Activate your anvi'o conda environment |
| Browser doesn't open | Use `--server-only` and navigate manually |
| Out of memory during clustering | Skip hierarchical clustering or reduce dataset size |
| KEGG version mismatch | Re-run `anvi-run-kegg-kofams` with matching KEGG data |
| Bin quality is low | Use `anvi-refine` to manually inspect and refine |

### Getting Help

1. Check the **Common Pitfalls** section of the relevant skill document
2. Run the program with `-h` to see all available options
3. Search the [anvi'o GitHub issues](https://github.com/merenlab/anvio/issues)
4. Ask on the [anvi'o Discord](https://discord.gg/CFQeCwD8S5)
