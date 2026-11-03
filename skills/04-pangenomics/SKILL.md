# Skill: Pangenomics

> **Version:** 0.1.0
> **Category:** Core Workflow
> **Difficulty:** Intermediate

---

## Overview

Pangenomics in anvi'o compares the gene content across multiple genomes to identify core (shared) and accessory (variable) gene clusters. The workflow uses DIAMOND (or BLAST) for sequence similarity search, MCL for clustering, and the anvi'o interactive interface for visualization and analysis.

## Prerequisites

- Contigs databases for each genome — see [Contigs Database Setup](../01-contigs-database-setup/SKILL.md)
- Genomes of interest (either as external FASTA/contigs DBs or as internal bins from a metagenomic workflow)
- DIAMOND installed (default search engine)

## Key Concepts

| Concept | Description |
|---|---|
| **Genomes Storage DB** | A database storing gene information for all genomes in a pangenomic analysis. |
| **Pan DB** | A database storing gene cluster analysis results for a pangenome. |
| **Gene Cluster** | A group of homologous genes across genomes, identified by sequence similarity and MCL clustering. |
| **Core Genes** | Gene clusters present in all genomes. |
| **Accessory Genes** | Gene clusters present in only a subset of genomes. |
| **Singletons** | Gene clusters found in only one genome. |
| **External Genomes** | Genomes stored as individual contigs databases (e.g., downloaded from NCBI). |
| **Internal Genomes** | Bins within a profile database (e.g., MAGs from a metagenomic workflow). |

## Step-by-Step Guide

### Step 1: Prepare Genome Input Files

Create an input file listing your genomes.

#### For External Genomes

Create an `external-genomes.txt` file (TAB-delimited):

```
name	contigs_db_path
Genome_1	/path/to/genome_1.db
Genome_2	/path/to/genome_2.db
Genome_3	/path/to/genome_3.db
```

> Each genome must already have a contigs database. If not, create one:
> ```bash
> anvi-gen-contigs-database -f genome_1.fa -o genome_1.db --project-name Genome_1
> ```

#### For Internal Genomes (Bins/MAGs)

Create an `internal-genomes.txt` file (TAB-delimited):

```
name	bin_id	collection_id	profile_db_path	contigs_db_path
MAG_1	Bin_1	MY_COLLECTION	/path/to/PROFILE.db	/path/to/contigs.db
MAG_2	Bin_2	MY_COLLECTION	/path/to/PROFILE.db	/path/to/contigs.db
```

#### Mixed (Both Types)

You can include both external and internal genomes in the same pangenomic analysis.

### Step 2: Generate a Genomes Storage Database

```bash
# From external genomes only
anvi-gen-genomes-storage -e external-genomes.txt \
                         -o ALL_GENOMES-GENOMES.db

# From internal genomes only
anvi-gen-genomes-storage -i internal-genomes.txt \
                         -o ALL_GENOMES-GENOMES.db

# From both
anvi-gen-genomes-storage -e external-genomes.txt \
                         -i internal-genomes.txt \
                         -o ALL_GENOMES-GENOMES.db
```

> The output filename **must** end with `-GENOMES.db`.

### Step 3: Run the Pangenomic Analysis

```bash
anvi-pan-genome -g ALL_GENOMES-GENOMES.db \
                --project-name MY_PANGENOME \
                --num-threads 6
```

**Key parameters:**
- `-g`: Genomes storage database
- `--project-name`: Name for the pangenome project
- `--num-threads`: Number of threads for DIAMOND search
- `--minbit`: Minimum fraction of the shorter gene's length for a hit to be considered (default: 0.5)
- `--mcl-inflation`: MCL inflation parameter (default: 6.0; higher = more granular clusters)
- `--min-occurrence`: Minimum number of genomes a gene cluster must appear in to be kept (default: 1)
- `--use-ncbi-blast`: Use NCBI BLAST instead of DIAMOND (slower but more sensitive)

### Step 4: Visualize the Pangenome

Open the interactive interface to explore your pangenome:

```bash
anvi-display-pan -g ALL_GENOMES-GENOMES.db \
                -p MY_PANGENOME/PAN.db \
                --title "My Pangenome"
```

In the interface:
- Each **item** is a gene cluster
- Each **layer** is a genome
- Use the view "gene_cluster_presence_absence" to see which genomes contain each cluster
- Create collections to group gene clusters into functional categories

### Step 5: Compute Gene Cluster Homogeneity (Optional)

Assess the sequence homogeneity within gene clusters:

```bash
anvi-compute-gene-cluster-homogeneity -p MY_PANGENOME/PAN.db \
                                       -g ALL_GENOMES-GENOMES.db \
                                       -o HOMOGENEITY
```

### Step 6: Compute Functional Enrichment (Optional)

Identify functions enriched in specific genome groups:

```bash
anvi-compute-functional-enrichment-in-pan -p MY_PANGENOME/PAN.db \
                                           -g ALL_GENOMES-GENOMES.db \
                                           -o functional-enrichment.txt
```

### Step 7: Summarize the Pangenome

```bash
anvi-summarize -g ALL_GENOMES-GENOMES.db \
               -p MY_PANGENOME/PAN.db \
               -C MY_COLLECTION \
               -o PANGENOME_SUMMARY
```

## Input File Formats

### external-genomes.txt

| Column | Description |
|---|---|
| `name` | Unique genome name |
| `contigs_db_path` | Path to the contigs database |

### internal-genomes.txt

| Column | Description |
|---|---|
| `name` | Unique genome name |
| `bin_id` | Bin name within the collection |
| `collection_id` | Collection name in the profile database |
| `profile_db_path` | Path to the profile database |
| `contigs_db_path` | Path to the contigs database |

## Common Pitfalls

1. **Inconsistent annotations**: Annotate all contigs databases with the same functional sources (COGs, KOfams) **before** creating the genomes storage. Annotations cannot be added after the pan database is created.
2. **Genome name conflicts**: Use unique, descriptive names. Special characters in names can cause issues.
3. **Too few or too many genomes**: For meaningful pangenomics, use at least 3 genomes. Very large sets (>100 genomes) may require significant computational resources.
4. **Minbit threshold too low**: A low `--minbit` value includes weak hits, creating overly broad gene clusters. The default 0.5 is usually appropriate.

## Quick Reference

```bash
# Prepare genomes
anvi-gen-genomes-storage -e external-genomes.txt -o ALL-GENOMES.db

# Run pangenome
anvi-pan-genome -g ALL-GENOMES.db --project-name MY_PAN -T 6

# Visualize
anvi-display-pan -g ALL-GENOMES.db -p MY_PAN/PAN.db

# Summarize
anvi-summarize -g ALL-GENOMES.db -p MY_PAN/PAN.db -C COL -o SUMMARY
```

## Output Artifacts

| Artifact | Description |
|---|---|
| `*-GENOMES.db` | Genomes storage database |
| `PAN.db` | Pangenome database with gene cluster analysis |
| `SUMMARY/` | Summary directory with gene cluster and genome information |

## Next Steps

After completing this skill, proceed to:
- [**Interactive Interface**](../07-interactive-interface/SKILL.md) — Deeper exploration of pangenome visualization
- [**Metabolism Estimation**](../05-metabolism-estimation/SKILL.md) — Estimate metabolic potential of gene cluster bins
