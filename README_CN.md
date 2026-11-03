# anvio-9-skills

> **版本：** 0.1.0
> 一套旨在降低 anvi'o 使用门槛的技能文档集合。

[English](README.md) | [中文](README_CN.md)

---

## 这是什么？

**anvio-9-skills** 是一套分步骤的技能文档，旨在帮助新用户和中级用户快速上手 [anvi'o](https://anvio.org/) —— 一个开源的、社区驱动的微生物数据分析平台。每个技能文档聚焦于一个核心 anvi'o 工作流，提供从数据库创建到代谢估计、泛基因组分析等高级功能的实用指南。

本仓库**不包含** anvi'o 软件本身。它提供的是引导式文档，旨在简化 anvi'o 庞大的命令行工具集（150+ 个程序）和交互式界面的学习曲线。

## 技能一览

| 编号 | 技能 | 类别 | 描述 |
|---|---|---|---|
| 01 | [Contigs 数据库构建](skills/01-contigs-database-setup/SKILL.md) | 基础 | 创建和注释 contigs 数据库 —— 所有 anvi'o 工作流的基础 |
| 02 | [宏基因组分析](skills/02-metagenomic-profiling/SKILL.md) | 核心流程 | 比对 reads、分析样本、合并 profile 数据库 |
| 03 | [分箱与精修](skills/03-binning-and-refinement/SKILL.md) | 核心流程 | 将 contigs 聚类为 MAG、精修 bins、生成汇总报告 |
| 04 | [泛基因组分析](skills/04-pangenomics/SKILL.md) | 核心流程 | 比较多基因组的基因内容，识别核心基因和附属基因 |
| 05 | [代谢估计](skills/05-metabolism-estimation/SKILL.md) | 高级分析 | 利用 KEGG 模块预测代谢通路的完整性 |
| 06 | [物种分类估计](skills/06-taxonomy-estimation/SKILL.md) | 高级分析 | 使用单拷贝核心基因和 GTDB 估计基因组/bin 的分类 |
| 07 | [交互式界面](skills/07-interactive-interface/SKILL.md) | 可视化 | 使用 anvi'o 交互式界面进行数据探索和手动分箱 |
| 08 | [Snakemake 工作流](skills/08-snakemake-workflows/SKILL.md) | 自动化 | 使用 anvi'o 的 Snakemake 工作流自动化多步骤分析 |

## 推荐学习路线

```
01 Contigs DB → 02 分析 → 03 分箱 → 05 代谢
                  ↓           ↓
            07 界面      04 泛基因组
                  ↓           ↓
            06 分类      08 工作流
```

1. **从** [Contigs 数据库构建](skills/01-contigs-database-setup/SKILL.md) **开始** —— 每个工作流都需要 contigs 数据库。
2. **然后学习** [宏基因组分析](skills/02-metagenomic-profiling/SKILL.md) 和 [分箱与精修](skills/03-binning-and-refinement/SKILL.md)。
3. **探索** [交互式界面](skills/07-interactive-interface/SKILL.md) 来可视化你的数据。
4. **进阶到** [泛基因组分析](skills/04-pangenomics/SKILL.md)、[代谢估计](skills/05-metabolism-estimation/SKILL.md) 或 [物种分类估计](skills/06-taxonomy-estimation/SKILL.md)。
5. **自动化** 使用 [Snakemake 工作流](skills/08-snakemake-workflows/SKILL.md)。

## 文档

- [使用指南 (English)](docs/en/USAGE.md)
- [使用指南 (中文)](docs/zh/USAGE_CN.md)
- [参考信息 (English)](docs/en/REFERENCE.md)
- [参考信息 (中文)](docs/zh/REFERENCE_CN.md)

## 环境要求

- [anvi'o](https://merenlab.org/software/anvio/) v7 或更高版本（本技能集基于 anvi'o 9 开发）
- Python 3.8+
- 类 Unix 操作系统（推荐 Linux 或 macOS）
- 根据工作流需要的外部工具：Bowtie2、DIAMOND、Snakemake 等

## 如何使用这些技能文档

每个技能文档都是独立的，遵循一致的结构：

1. **概述** —— 技能涵盖的内容
2. **前置要求** —— 开始前需要准备什么
3. **关键概念** —— 应该了解的术语
4. **分步指南** —— 带命令示例的详细操作步骤
5. **常见陷阱** —— 应避免的错误
6. **快速参考** —— 精简的命令摘要
7. **后续步骤** —— 完成后可以做什么

按顺序阅读技能文档，或直接跳转到与当前任务相关的文档。所有命令都可以直接复制粘贴到终端中使用（请根据需要修改文件路径和名称）。

## 版本

**0.1.0** —— 初始版本。详见 [更新日志](CHANGELOG.md)。

## 许可证

本文档项目采用 [MIT 许可证](LICENSE)。

## 参考

本技能仓库通过研究 anvi'o 源代码和官方文档开发。详细来源信息请参阅 [参考信息](docs/zh/REFERENCE_CN.md)。

## 贡献

欢迎贡献！如果你发现错误或想添加新的技能文档：

1. Fork 本仓库
2. 创建功能分支（`git checkout -b new-skill`）
3. 提交你的更改
4. 发起 Pull Request

请遵循现有的技能文档结构以保持一致性。

## 致谢

- [anvi'o 开发团队](https://merenlab.org/) (Meren Lab) 创建和维护 anvi'o
- 所有贡献教程、博客文章和讨论的 anvi'o 社区成员
