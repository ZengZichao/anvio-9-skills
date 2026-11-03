# anvio-9-skills

> **Version:** 0.1.0
> A curated set of skills to lower the barrier to entry for anvi'o users.

[English](README.md) | [中文](README_CN.md)

---

## What is this?

**anvio-9-skills** is a collection of step-by-step skill documents designed to help new and intermediate users get started with [anvi'o](https://anvio.org/) — an open-source, community-driven platform for microbiological data analysis. Each skill provides a focused, practical walkthrough of a core anvi'o workflow, from database creation to advanced analyses like metabolism estimation and pangenomics.

This repository does **not** contain anvi'o software itself. It provides guided documentation that simplifies the learning curve of anvi'o's extensive CLI toolkit (150+ programs) and interactive interface.

## Skills Overview

| # | Skill | Category | Description |
|---|---|---|---|
| 01 | [Contigs Database Setup](skills/01-contigs-database-setup/SKILL.md) | Foundational | Create and annotate a contigs database — the foundation of all anvi'o workflows |
| 02 | [Metagenomic Profiling](skills/02-metagenomic-profiling/SKILL.md) | Core Workflow | Map reads, profile samples, and merge profile databases |
| 03 | [Binning and Refinement](skills/03-binning-and-refinement/SKILL.md) | Core Workflow | Cluster contigs into MAGs, refine bins, and generate summaries |
| 04 | [Pangenomics](skills/04-pangenomics/SKILL.md) | Core Workflow | Compare gene content across genomes to identify core and accessory genes |
| 05 | [Metabolism Estimation](skills/05-metabolism-estimation/SKILL.md) | Advanced Analysis | Predict metabolic pathway completeness using KEGG modules |
| 06 | [Taxonomy Estimation](skills/06-taxonomy-estimation/SKILL.md) | Advanced Analysis | Estimate taxonomy of genomes and bins using SCGs and GTDB |
| 07 | [Interactive Interface](skills/07-interactive-interface/SKILL.md) | Visualization | Use the anvi'o interactive interface for data exploration and manual binning |
| 08 | [Snakemake Workflows](skills/08-snakemake-workflows/SKILL.md) | Automation | Automate multi-step analyses with anvi'o's Snakemake workflows |

## Recommended Learning Path

```
01 Contigs DB → 02 Profiling → 03 Binning → 05 Metabolism
                     ↓              ↓
               07 Interface    04 Pangenomics
                     ↓              ↓
               06 Taxonomy    08 Workflows
```

1. **Start with** [Contigs Database Setup](skills/01-contigs-database-setup/SKILL.md) — every workflow needs a contigs database.
2. **Then learn** [Metagenomic Profiling](skills/02-metagenomic-profiling/SKILL.md) and [Binning and Refinement](skills/03-binning-and-refinement/SKILL.md).
3. **Explore** the [Interactive Interface](skills/07-interactive-interface/SKILL.md) to visualize your data.
4. **Advance to** [Pangenomics](skills/04-pangenomics/SKILL.md), [Metabolism Estimation](skills/05-metabolism-estimation/SKILL.md), or [Taxonomy Estimation](skills/06-taxonomy-estimation/SKILL.md).
5. **Automate** with [Snakemake Workflows](skills/08-snakemake-workflows/SKILL.md).

## Documentation

- [Usage Guide (English)](docs/en/USAGE.md)
- [使用指南 (中文)](docs/zh/USAGE_CN.md)
- [Reference Information](docs/en/REFERENCE.md)
- [参考信息 (中文)](docs/zh/REFERENCE_CN.md)

## Requirements

- [anvi'o](https://merenlab.org/software/anvio/) v7 or later (this skill set was developed against anvi'o 9)
- Python 3.8+
- A Unix-like operating system (Linux or macOS recommended)
- External tools depending on the workflow: Bowtie2, DIAMOND, Snakemake, etc.

## How to Use These Skills

Each skill document is self-contained and follows a consistent structure:

1. **Overview** — what the skill covers
2. **Prerequisites** — what you need before starting
3. **Key Concepts** — terminology you should know
4. **Step-by-Step Guide** — detailed walkthrough with command examples
5. **Common Pitfalls** — mistakes to avoid
6. **Quick Reference** — condensed command summary
7. **Next Steps** — where to go from here

Read the skills in order, or jump directly to the one relevant to your current task. All commands can be copied and pasted directly into your terminal (modify file paths and names as needed).

## Version

**0.1.0** — Initial release. See [CHANGELOG](CHANGELOG.md) for details.

## License

This documentation project is licensed under the [MIT License](LICENSE).

## Reference

This skills repository was developed by studying the anvi'o source code and official documentation. For detailed attribution, please see [Reference Information](docs/en/REFERENCE.md).

## Contributing

Contributions are welcome! If you find an error or want to add a new skill:

1. Fork this repository
2. Create a feature branch (`git checkout -b new-skill`)
3. Commit your changes
4. Open a pull request

Please follow the existing skill document structure for consistency.

## Acknowledgments

- The [anvi'o development team](https://merenlab.org/) at the Meren Lab for creating and maintaining anvi'o
- All anvi'o community members who have contributed tutorials, blog posts, and discussions
