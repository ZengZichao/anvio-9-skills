# 参考信息

> 本文档描述了开发 anvio-9-skills 仓库所使用的来源和参考资料。

## 1. 主要来源

本技能仓库通过研究 **anvi'o 源代码**和**官方文档**开发，参考项目如下：

### 参考项目

- **项目：** anvi'o (anvio-9-skills-参考项目)
- **官方网站：** [https://anvio.org/](https://anvio.org/)
- **官方 GitHub：** [https://github.com/merenlab/anvio](https://github.com/merenlab/anvio)
- **许可证：** GPL-2.0（anvi'o 软件）

## 2. 参考的源代码组件

以下 anvi'o 源代码组件被研究用于创建这些技能文档：

### 命令行程序 (`anvio/cli/`)

Anvi'o 包含 150 多个命令行程序，以 Python 模块形式实现。技能文档引用了最常用的程序。主要研究的程序：

| 程序 | 对应技能 |
|---|---|
| `anvi-gen-contigs-database` | 技能 01 |
| `anvi-run-hmms` | 技能 01 |
| `anvi-run-ncbi-cogs` | 技能 01 |
| `anvi-run-kegg-kofams` | 技能 05 |
| `anvi-scan-trnas` | 技能 01 |
| `anvi-script-reformat-fasta` | 技能 01 |
| `anvi-init-bam` | 技能 02 |
| `anvi-profile` | 技能 02 |
| `anvi-merge` | 技能 02 |
| `anvi-cluster-contigs` | 技能 03 |
| `anvi-refine` | 技能 03 |
| `anvi-summarize` | 技能 03 |
| `anvi-compute-completeness` | 技能 03 |
| `anvi-import-collection` | 技能 03 |
| `anvi-rename-bins` | 技能 03 |
| `anvi-dereplicate-genomes` | 技能 03 |
| `anvi-gen-genomes-storage` | 技能 04 |
| `anvi-pan-genome` | 技能 04 |
| `anvi-display-pan` | 技能 04 |
| `anvi-compute-gene-cluster-homogeneity` | 技能 04 |
| `anvi-compute-functional-enrichment-in-pan` | 技能 04 |
| `anvi-estimate-metabolism` | 技能 05 |
| `anvi-setup-kegg-data` | 技能 05 |
| `anvi-setup-user-modules` | 技能 05 |
| `anvi-run-scg-taxonomy` | 技能 06 |
| `anvi-estimate-scg-taxonomy` | 技能 06 |
| `anvi-setup-scg-taxonomy` | 技能 06 |
| `anvi-run-trna-taxonomy` | 技能 06 |
| `anvi-estimate-trna-taxonomy` | 技能 06 |
| `anvi-interactive` | 技能 07 |
| `anvi-import-misc-data` | 技能 07 |
| `anvi-export-state` | 技能 07 |
| `anvi-run-workflow` | 技能 08 |

### 文档文件 (`anvio/docs/`)

Anvi'o 为程序和产物维护了详尽的 Markdown 文档：

- **程序文档** (`docs/programs/`): 157 个 Markdown 文件，记录每个命令行程序
- **产物文档** (`docs/artifacts/`): 141 个 Markdown 文件，记录数据库类型、文件格式和概念

这些文档文件是命令语法、参数和工作流描述的主要参考来源。

### 工作流 (`anvio/workflows/`)

Snakemake 工作流定义被研究用于技能 08：

| 工作流 | 位置 |
|---|---|
| Contigs | `workflows/contigs/Snakefile` |
| 宏基因组 | `workflows/metagenomics/` |
| 泛基因组 | `workflows/pangenomics/` |
| 系统发育 | `workflows/phylogenomics/` |
| tRNA-seq | `workflows/trnaseq/` |
| EcoPhylo | `workflows/ecophylo/` |
| SRA 下载 | `workflows/sra_download/` |

### 数据资源 (`anvio/data/`)

技能文档中引用的内置 HMM 配置：

| HMM 来源 | 位置 | 描述 |
|---|---|---|
| Bacteria_71 | `data/hmm/Bacteria_71/` | 71 个细菌单拷贝核心基因 |
| Archaea_76 | `data/hmm/Archaea_76/` | 76 个古菌单拷贝核心基因 |
| Protista_83 | `data/hmm/Protista_83/` | 83 个原生生物单拷贝核心基因 |
| Ribosomal_RNA_16S | `data/hmm/Ribosomal_RNA_16S/` | 16S 核糖体 RNA |
| Ribosomal_RNA_18S | `data/hmm/Ribosomal_RNA_18S/` | 18S 核糖体 RNA |

## 3. 官方 Anvi'o 资源

以下官方 anvi'o 资源也被参考：

| 资源 | URL |
|---|---|
| Anvi'o 主网站 | [https://anvio.org/](https://anvio.org/) |
| Anvi'o 文档 | [https://anvio.org/help/](https://anvio.org/help/) |
| 交互式界面教程 | [http://merenlab.org/tutorials/interactive-interface/](http://merenlab.org/tutorials/interactive-interface/) |
| 宏基因组工作流教程 | [http://merenlab.org/2016/06/22/anvio-tutorial-v2/](http://merenlab.org/2016/06/22/anvio-tutorial-v2/) |
| 泛基因组教程 | [http://merenlab.org/2016/11/08/pangenomics-v2](http://merenlab.org/2016/11/08/pangenomics-v2) |
| Snakemake 工作流教程 | [https://merenlab.org/2018/07/09/anvio-snakemake-workflows/](https://merenlab.org/2018/07/09/anvio-snakemake-workflows/) |
| SCG 物种分类文档 | [http://merenlab.org/2019/10/08/anvio-scg-taxonomy/](http://merenlab.org/2019/10/08/anvio-scg-taxonomy/) |
| 代谢估计教程 | [https://anvio.org/tutorials/fmt-mag-metabolism/](https://anvio.org/tutorials/fmt-mag-metabolism/) |
| 附加数据表 | [https://merenlab.org/2017/12/11/additional-data-tables/](https://merenlab.org/2017/12/11/additional-data-tables/) |
| 术语表 | [https://anvio.org/vocabulary/](https://anvio.org/vocabulary/) |
| Anvi'o GitHub 仓库 | [https://github.com/merenlab/anvio](https://github.com/merenlab/anvio) |
| Anvi'o Discord 社区 | [https://discord.gg/CFQeCwD8S5](https://discord.gg/CFQeCwD8S5) |

## 4. 引用的工具和算法

这些技能文档引用了 anvi'o 集成的外部工具和算法：

| 工具 | 用途 | 参考 |
|---|---|---|
| Pyrodigal / Pyrodigal-gv | 基因预测（ORF 识别） | [https://github.com/althonos/pyrodigal-gv](https://github.com/althonos/pyrodigal-gv) |
| HMMER | HMM 配置搜索 | [http://hmmer.org/](http://hmmer.org/) |
| DIAMOND | 序列相似性搜索 | [https://github.com/bbuchfink/diamond](https://github.com/bbuchfink/diamond) |
| Bowtie2 | Read 比对 | [https://bowtie-bio.sourceforge.net/bowtie2/](https://bowtie-bio.sourceforge.net/bowtie2/) |
| samtools | BAM 文件处理 | [http://www.htslib.org/](http://www.htslib.org/) |
| MCL | 基因聚类的马尔可夫聚类 | [http://micans.org/mcl/](http://micans.org/mcl/) |
| CONCOCT | 宏基因组分箱 | [https://github.com/BinPro/CONCOCT](https://github.com/BinPro/CONCOCT) |
| MetaBAT2 | 宏基因组分箱 | [https://bitbucket.org/berkeleylab/metabat](https://bitbucket.org/berkeleylab/metabat) |
| MaxBin2 | 宏基因组分箱 | [https://sourceforge.net/projects/maxbin2/](https://sourceforge.net/projects/maxbin2/) |
| DAS Tool | 分箱整合 | [https://github.com/cmks/DAS_Tool](https://github.com/cmks/DAS_Tool) |
| Snakemake | 工作流管理 | [https://snakemake.readthedocs.io/](https://snakemake.readthedocs.io/) |
| KEGG | 代谢通路数据库 | [https://www.genome.jp/kegg/](https://www.genome.jp/kegg/) |
| GTDB | 基因组分类数据库 | [https://gtdb.ecogenomic.org/](https://gtdb.ecogenomic.org/) |
| NCBI COGs | 直系同源群 | [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC102395/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC102395/) |
| SQLite | Anvi'o 数据库引擎 | [https://www.sqlite.org/](https://www.sqlite.org/) |

## 5. 关键文献

Anvi'o 及其组件在以下文献中描述：

1. Eren et al. (2015). "Anvi'o: an advanced analysis and visualization platform for 'omics data." *PeerJ* 3:e1319. [doi:10.7717/peerj.1319](https://doi.org/10.7717/peerj.1319)
2. Eren et al. (2021). "Community-led, integrated, reproducible multi-omics with anvi'o." *Nature Microbiology* 6:3-4. [doi:10.1038/s41564-020-00834-3](https://doi.org/10.1038/s41564-020-00834-3)
3. Hyatt et al. (2010). "Prodigal: prokaryotic gene recognition and translation initiation site identification." *BMC Bioinformatics* 11:119. [doi:10.1186/1471-2105-11-119](https://doi.org/10.1186/1471-2105-11-119)
4. Camargo et al. (2023). "Identification of mobile genetic elements with geNomad." *Nature Biotechnology*. [doi:10.1038/s41587-023-01953-y](https://doi.org/10.1038/s41587-023-01953-y)

## 6. 技能文档的开发过程

本仓库中的技能文档通过以下过程创建：

1. **源代码分析**：系统地审查了 anvi'o 命令行模块文件（`anvio/cli/`），了解可用程序、参数和默认行为。
2. **文档审查**：阅读了每个相关程序的 Markdown 文档文件（`docs/programs/` 和 `docs/artifacts/`），了解使用模式、要求和输出格式。
3. **工作流分析**：研究了 Snakemake 工作流定义（`anvio/workflows/`），了解程序如何在自动化流程中链接。
4. **综合与简化**：将大量文档压缩为聚焦的、分步骤的指南，遵循一致的结构，强调实际使用和常见陷阱。

## 7. 免责声明

- 本技能仓库是一个**独立的文档项目**，与 anvi'o 开发团队没有官方附属关系。
- 所有命令示例基于 anvi'o 的命令行界面，仅供教育目的。
- Anvi'o 在持续演进；某些命令或参数可能在将来版本中发生变化。请始终查阅官方文档获取最新信息。
- 对于 anvi'o 本身的 bug，请向[官方 anvi'o GitHub 仓库](https://github.com/merenlab/anvio/issues)报告。
