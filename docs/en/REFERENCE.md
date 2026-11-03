# Reference Information

> This document describes the sources and references used to develop the anvio-9-skills repository.

## 1. Primary Source

This skills repository was developed by studying the **anvi'o source code** and **official documentation** from the following reference project:

### Reference Project

- **Project:** anvi'o (anvio-9-skills-参考项目)
- **Official Website:** [https://anvio.org/](https://anvio.org/)
- **Official GitHub:** [https://github.com/merenlab/anvio](https://github.com/merenlab/anvio)
- **License:** GPL-2.0 (anvi'o software)

## 2. Source Code Components Referenced

The following components of the anvi'o source code were studied to create these skills:

### CLI Programs (`anvio/cli/`)

Anvi'o contains 150+ command-line programs implemented as Python modules. The skills reference the most commonly used programs. Key programs studied:

| Program | Skill Reference |
|---|---|
| `anvi-gen-contigs-database` | Skill 01 |
| `anvi-run-hmms` | Skill 01 |
| `anvi-run-ncbi-cogs` | Skill 01 |
| `anvi-run-kegg-kofams` | Skill 05 |
| `anvi-scan-trnas` | Skill 01 |
| `anvi-script-reformat-fasta` | Skill 01 |
| `anvi-init-bam` | Skill 02 |
| `anvi-profile` | Skill 02 |
| `anvi-merge` | Skill 02 |
| `anvi-cluster-contigs` | Skill 03 |
| `anvi-refine` | Skill 03 |
| `anvi-summarize` | Skill 03 |
| `anvi-compute-completeness` | Skill 03 |
| `anvi-import-collection` | Skill 03 |
| `anvi-rename-bins` | Skill 03 |
| `anvi-dereplicate-genomes` | Skill 03 |
| `anvi-gen-genomes-storage` | Skill 04 |
| `anvi-pan-genome` | Skill 04 |
| `anvi-display-pan` | Skill 04 |
| `anvi-compute-gene-cluster-homogeneity` | Skill 04 |
| `anvi-compute-functional-enrichment-in-pan` | Skill 04 |
| `anvi-estimate-metabolism` | Skill 05 |
| `anvi-setup-kegg-data` | Skill 05 |
| `anvi-setup-user-modules` | Skill 05 |
| `anvi-run-scg-taxonomy` | Skill 06 |
| `anvi-estimate-scg-taxonomy` | Skill 06 |
| `anvi-setup-scg-taxonomy` | Skill 06 |
| `anvi-run-trna-taxonomy` | Skill 06 |
| `anvi-estimate-trna-taxonomy` | Skill 06 |
| `anvi-interactive` | Skill 07 |
| `anvi-import-misc-data` | Skill 07 |
| `anvi-export-state` | Skill 07 |
| `anvi-run-workflow` | Skill 08 |

### Documentation Files (`anvio/docs/`)

Anvi'o maintains extensive markdown documentation for both programs and artifacts:

- **Program docs** (`docs/programs/`): 157 markdown files documenting each CLI program
- **Artifact docs** (`docs/artifacts/`): 141 markdown files documenting database types, file formats, and concepts

These documentation files were the primary reference for command syntax, parameters, and workflow descriptions.

### Workflows (`anvio/workflows/`)

Snakemake workflow definitions were studied for Skill 08:

| Workflow | Location |
|---|---|
| Contigs | `workflows/contigs/Snakefile` |
| Metagenomics | `workflows/metagenomics/` |
| Pangenomics | `workflows/pangenomics/` |
| Phylogenomics | `workflows/phylogenomics/` |
| tRNA-seq | `workflows/trnaseq/` |
| EcoPhylo | `workflows/ecophylo/` |
| SRA Download | `workflows/sra_download/` |

### Data Resources (`anvio/data/`)

Built-in HMM profiles referenced in the skills:

| HMM Source | Location | Description |
|---|---|---|
| Bacteria_71 | `data/hmm/Bacteria_71/` | 71 bacterial single-copy core genes |
| Archaea_76 | `data/hmm/Archaea_76/` | 76 archaeal single-copy core genes |
| Protista_83 | `data/hmm/Protista_83/` | 83 protistan single-copy core genes |
| Ribosomal_RNA_16S | `data/hmm/Ribosomal_RNA_16S/` | 16S ribosomal RNA |
| Ribosomal_RNA_18S | `data/hmm/Ribosomal_RNA_18S/` | 18S ribosomal RNA |

## 3. Official Anvi'o Resources

The following official anvi'o resources were also consulted:

| Resource | URL |
|---|---|
| Anvi'o main website | [https://anvio.org/](https://anvio.org/) |
| Anvi'o documentation | [https://anvio.org/help/](https://anvio.org/help/) |
| Interactive interface tutorial | [http://merenlab.org/tutorials/interactive-interface/](http://merenlab.org/tutorials/interactive-interface/) |
| Metagenomic workflow tutorial | [http://merenlab.org/2016/06/22/anvio-tutorial-v2/](http://merenlab.org/2016/06/22/anvio-tutorial-v2/) |
| Pangenomics tutorial | [http://merenlab.org/2016/11/08/pangenomics-v2](http://merenlab.org/2016/11/08/pangenomics-v2) |
| Snakemake workflows tutorial | [https://merenlab.org/2018/07/09/anvio-snakemake-workflows/](https://merenlab.org/2018/07/09/anvio-snakemake-workflows/) |
| SCG taxonomy documentation | [http://merenlab.org/2019/10/08/anvio-scg-taxonomy/](http://merenlab.org/2019/10/08/anvio-scg-taxonomy/) |
| Metabolism estimation tutorial | [https://anvio.org/tutorials/fmt-mag-metabolism/](https://anvio.org/tutorials/fmt-mag-metabolism/) |
| Additional data tables | [https://merenlab.org/2017/12/11/additional-data-tables/](https://merenlab.org/2017/12/11/additional-data-tables/) |
| Vocabulary page | [https://anvio.org/vocabulary/](https://anvio.org/vocabulary/) |
| Anvi'o GitHub repository | [https://github.com/merenlab/anvio](https://github.com/merenlab/anvio) |
| Anvi'o Discord community | [https://discord.gg/CFQeCwD8S5](https://discord.gg/CFQeCwD8S5) |

## 4. Tools and Algorithms Referenced

These skills reference external tools and algorithms that anvi'o integrates:

| Tool | Purpose | Reference |
|---|---|---|
| Pyrodigal / Pyrodigal-gv | Gene calling (ORF prediction) | [https://github.com/althonos/pyrodigal-gv](https://github.com/althonos/pyrodigal-gv) |
| HMMER | HMM profile searches | [http://hmmer.org/](http://hmmer.org/) |
| DIAMOND | Sequence similarity search | [https://github.com/bbuchfink/diamond](https://github.com/bbuchfink/diamond) |
| Bowtie2 | Read mapping | [https://bowtie-bio.sourceforge.net/bowtie2/](https://bowtie-bio.sourceforge.net/bowtie2/) |
| samtools | BAM file processing | [http://www.htslib.org/](http://www.htslib.org/) |
| MCL | Markov clustering for gene clusters | [http://micans.org/mcl/](http://micans.org/mcl/) |
| CONCOCT | Metagenomic binning | [https://github.com/BinPro/CONCOCT](https://github.com/BinPro/CONCOCT) |
| MetaBAT2 | Metagenomic binning | [https://bitbucket.org/berkeleylab/metabat](https://bitbucket.org/berkeleylab/metabat) |
| MaxBin2 | Metagenomic binning | [https://sourceforge.net/projects/maxbin2/](https://sourceforge.net/projects/maxbin2/) |
| DAS Tool | Binning integration | [https://github.com/cmks/DAS_Tool](https://github.com/cmks/DAS_Tool) |
| Snakemake | Workflow management | [https://snakemake.readthedocs.io/](https://snakemake.readthedocs.io/) |
| KEGG | Metabolic pathway database | [https://www.genome.jp/kegg/](https://www.genome.jp/kegg/) |
| GTDB | Genome taxonomy database | [https://gtdb.ecogenomic.org/](https://gtdb.ecogenomic.org/) |
| NCBI COGs | Clusters of Orthologous Groups | [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC102395/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC102395/) |
| SQLite | Database engine for anvi'o databases | [https://www.sqlite.org/](https://www.sqlite.org/) |

## 5. Key Publications

Anvi'o and its components are described in the following publications:

1. Eren et al. (2015). "Anvi'o: an advanced analysis and visualization platform for 'omics data." *PeerJ* 3:e1319. [doi:10.7717/peerj.1319](https://doi.org/10.7717/peerj.1319)
2. Eren et al. (2021). "Community-led, integrated, reproducible multi-omics with anvi'o." *Nature Microbiology* 6:3-4. [doi:10.1038/s41564-020-00834-3](https://doi.org/10.1038/s41564-020-00834-3)
3. Hyatt et al. (2010). "Prodigal: prokaryotic gene recognition and translation initiation site identification." *BMC Bioinformatics* 11:119. [doi:10.1186/1471-2105-11-119](https://doi.org/10.1186/1471-2105-11-119)
4. Camargo et al. (2023). "Identification of mobile genetic elements with geNomad." *Nature Biotechnology*. [doi:10.1038/s41587-023-01953-y](https://doi.org/10.1038/s41587-023-01953-y)

## 6. How These Skills Were Created

The skills in this repository were created through the following process:

1. **Source code analysis**: Systematically reviewed the anvi'o CLI module files (`anvio/cli/`) to understand available programs, parameters, and default behaviors.
2. **Documentation review**: Read the markdown documentation files (`docs/programs/` and `docs/artifacts/`) for each relevant program to understand usage patterns, requirements, and output formats.
3. **Workflow analysis**: Studied the Snakemake workflow definitions (`anvio/workflows/`) to understand how programs are chained together in automated pipelines.
4. **Synthesis and simplification**: Condensed the extensive documentation into focused, step-by-step guides that follow a consistent structure, with emphasis on practical usage and common pitfalls.

## 7. Disclaimer

- This skills repository is an **independent documentation project** and is not officially affiliated with the anvi'o development team.
- All command examples are based on anvi'o's CLI interface and are provided for educational purposes.
- Anvi'o is continuously evolving; some commands or parameters may change in future versions. Always consult the official documentation for the most up-to-date information.
- For bugs in anvi'o itself, please report them to the [official anvi'o GitHub repository](https://github.com/merenlab/anvio/issues).
