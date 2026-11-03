# Skill: Interactive Interface

> **Version:** 0.1.0
> **Category:** Visualization
> **Difficulty:** Beginner–Intermediate

---

## Overview

The anvi'o interactive interface is a powerful web-based visualization tool that allows you to explore your data through concentric circle displays, rectangular trees, and more. It supports manual binning, data exploration, pangenome visualization, and real-time analysis.

## Prerequisites

- Anvi'o installed with browser access
- A profile database (for metagenomic data) or pan database (for pangenomic data)
- A contigs database

## Key Concepts

| Concept | Description |
|---|---|
| **Items** | The entities displayed in the central tree (contigs, splits, bins, or gene clusters). |
| **Layers** | Data tracks displayed as concentric circles around the central tree. |
| **View** | A specific data view (e.g., mean coverage, detection) for the display. |
| **State** | A saved set of visualization settings (layer order, colors, view, etc.). |
| **Collection** | A set of named bins that can be displayed and managed in the interface. |
| **Draw** | The button that renders the display after changing settings. |

## Step-by-Step Guide

### Step 1: Launch on a Profile Database

The most common use — visualizing metagenomic data:

```bash
anvi-interactive -p PROFILE.db \
                -c contigs.db
```

Your default browser should open automatically. If not, check the terminal for the URL.

### Step 2: Collection Mode — Visualize Bins

Display bins instead of individual contigs:

```bash
anvi-interactive -p PROFILE.db \
                -c contigs.db \
                -C MY_COLLECTION
```

In this mode, each item in the central tree is a bin, and layers show aggregate statistics for each bin.

### Step 3: Gene Mode — Visualize Genes

Display individual genes within a bin:

```bash
anvi-interactive -p PROFILE.db \
                -c contigs.db \
                -C MY_COLLECTION \
                -b BIN_NAME \
                --gene-mode
```

### Step 4: Refine a Bin

Open a specific bin for interactive refinement:

```bash
anvi-refine -p PROFILE.db \
            -c contigs.db \
            -C MY_COLLECTION \
            -b BIN_NAME
```

This opens the interactive interface focused on contigs in the specified bin, allowing you to:
- Remove contigs that don't belong
- Split a bin into multiple bins
- Select contigs and create new bins

### Step 5: Manual Mode — Visualize Any Tabular Data

```bash
anvi-interactive --manual-mode
```

Provide TAB-delimited data files:
- A data table (rows = items, columns = layers)
- Optionally, a NEWICK tree file

### Step 6: Display a Pangenome

```bash
anvi-display-pan -g ALL-GENOMES.db \
                -p MY_PAN/PAN.db \
                --title "My Pangenome"
```

### Step 7: Network Configuration

If the browser doesn't open or you're on a remote server:

```bash
anvi-interactive -p PROFILE.db \
                -c contigs.db \
                --ip-address 127.0.0.1 \
                --port-number 8901 \
                --server-only
```

Then open your browser to `http://127.0.0.1:8901`.

### Step 8: Password Protection

```bash
anvi-interactive -p PROFILE.db \
                -c contigs.db \
                --password-protected
```

### Step 9: Extend with Additional Data

Import miscellaneous data to add layers to your display:

```bash
# Item-level data (data about each contig/bin)
anvi-import-misc-data additional_data.txt \
                      -p PROFILE.db \
                      --target-data-table items

# Layer-level data (data about each sample)
anvi-import-misc-data sample_metadata.txt \
                      -p PROFILE.db \
                      --target-data-table layers
```

### Step 10: Save and Load States

Save your visualization settings as a named state:

In the interface, use the "State" panel to save your current settings. To load a saved state from the command line:

```bash
anvi-interactive -p PROFILE.db \
                -c contigs.db \
                --state-autoload MY_STATE_NAME
```

### Step 11: Export a State

Export a state for sharing or version control:

```bash
anvi-export-state -p PROFILE.db \
                  --state-name MY_STATE \
                  -o state.json
```

## Interactive Interface Controls

| Control | Description |
|---|---|
| **Draw** | Renders the display with current settings |
| **State Panel** | Save/load visualization settings |
| **Layer Panel** | Configure layer order, colors, and types |
| **Bin Panel** | Create, rename, and manage bins |
| **Search** | Find specific items by name |
| **Mouse wheel** | Zoom in/out |
| **Click** | Select an item |
| **Shift+Click** | Select multiple items |

## Useful Queries

Check what's available in your database:

```bash
# List available views
anvi-interactive -p PROFILE.db -c contigs.db --show-views

# List saved states
anvi-interactive -p PROFILE.db -c contigs.db --show-states

# List collections
anvi-interactive -p PROFILE.db -c contigs.db --list-collections
```

## Common Pitfalls

1. **Not clicking "Draw"**: After changing settings, you must click "Draw" to see the updated display.
2. **No hierarchical clustering**: If you have >25,000 splits and didn't use `--enforce-hierarchical-clustering`, there's no central tree. You can still use `anvi-import-items-order` to provide an external tree.
3. **Browser doesn't open**: Use `--server-only` with explicit `--ip-address` and `--port-number`, then manually navigate to the URL.
4. **Large datasets**: The interface may be slow with very large numbers of items. Consider using `anvi-split` to create smaller, manageable profiles.

## Quick Reference

```bash
# Basic display
anvi-interactive -p PROFILE.db -c contigs.db

# Collection mode
anvi-interactive -p PROFILE.db -c contigs.db -C COL

# Gene mode
anvi-interactive -p PROFILE.db -c contigs.db -C COL -b BIN --gene-mode

# Refine
anvi-refine -p PROFILE.db -c contigs.db -C COL -b BIN

# Pangenome
anvi-display-pan -g GENOMES.db -p PAN/PAN.db

# Remote server
anvi-interactive -p PROFILE.db -c contigs.db --server-only --ip-address 0.0.0.0 --port-number 8080
```

## Next Steps

After mastering the interactive interface, explore:
- [**Binning and Refinement**](../03-binning-and-refinement/SKILL.md) — Use the interface for manual binning
- [**Snakemake Workflows**](../08-snakemake-workflows/SKILL.md) — Automate the full pipeline
