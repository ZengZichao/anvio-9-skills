# 使用指南

> anvio-9-skills 仓库的完整使用指南。

## 1. 快速开始

### 1.1 前置要求

在使用这些技能文档之前，请确保已安装 anvi'o：

```bash
# 检查 anvi'o 安装
anvi-self-test --suite mini

# 检查版本
anvi-interactive --version
```

如果尚未安装 anvi'o，请参阅[官方安装指南](https://merenlab.org/software/anvio/)。

### 1.2 仓库结构

```
anvio-9-skills/
├── README.md                    # 英文 README
├── README_CN.md                 # 中文 README
├── CHANGELOG.md                 # 版本历史
├── LICENSE                      # MIT 许可证
├── docs/
│   ├── en/
│   │   ├── USAGE.md             # 英文使用指南
│   │   └── REFERENCE.md         # 英文参考信息
│   └── zh/
│       ├── USAGE_CN.md          # 本文（中文使用指南）
│       └── REFERENCE_CN.md      # 中文参考信息
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

### 1.3 阅读技能文档

每个技能文档（`SKILL.md`）遵循以下结构：

1. **头部** —— 版本、类别和难度等级
2. **概述** —— 技能涵盖内容的简要说明
3. **前置要求** —— 开始前需要准备什么
4. **关键概念** —— 重要术语表
5. **分步指南** —— 带代码块的编号步骤
6. **常见陷阱** —— 常见问题及解决方案
7. **快速参考** —— 精简的命令摘要，方便快速查阅
8. **输出产物** —— 生成的文件说明
9. **后续步骤** —— 相关技能的链接

## 2. 使用技能文档

### 2.1 选择起点

| 如果你想... | 从这个技能开始 |
|---|---|
| 处理原始组装 contigs | [01 Contigs 数据库构建](../skills/01-contigs-database-setup/SKILL.md) |
| 分析宏基因组样本 | [02 宏基因组分析](../skills/02-metagenomic-profiling/SKILL.md) |
| 从宏基因组中重建基因组 | [03 分箱与精修](../skills/03-binning-and-refinement/SKILL.md) |
| 比较多个基因组 | [04 泛基因组分析](../skills/04-pangenomics/SKILL.md) |
| 预测代谢能力 | [05 代谢估计](../skills/05-metabolism-estimation/SKILL.md) |
| 鉴定数据中的生物体 | [06 物种分类估计](../skills/06-taxonomy-estimation/SKILL.md) |
| 交互式可视化数据 | [07 交互式界面](../skills/07-interactive-interface/SKILL.md) |
| 自动化完整流程 | [08 Snakemake 工作流](../skills/08-snakemake-workflows/SKILL.md) |

### 2.2 复制命令

技能文档中的所有命令都可以直接复制粘贴到终端中。请注意：

- **占位符** 使用全大写字母（如 `MY_PROJECT`、`SAMPLE_NAME`）—— 请替换为你的实际值
- **文件路径** 如 `contigs.fa`、`contigs.db` —— 请根据实际文件位置调整
- **线程数** 如 `-T 6` —— 请根据你可用的 CPU 数量调整

### 2.3 完整宏基因组分析流程

以下是一个典型的端到端工作流，使用了多个技能：

```bash
# === 技能 01：创建 contigs 数据库 ===
anvi-gen-contigs-database -f assembly.fa -o contigs.db --project-name MY_PROJECT
anvi-run-hmms -c contigs.db -T 6
anvi-run-ncbi-cogs -c contigs.db -T 6

# === 技能 02：分析样本 ===
# （对每个样本）
bowtie2 --very-sensitive -x contigs_index -1 R1.fq.gz -2 R2.fq.gz -o sample.sam
samtools view -bS sample.sam > sample.raw.bam
anvi-init-bam -i sample.raw.bam -o sample.bam
anvi-profile -i sample.bam -c contigs.db -S SAMPLE_NAME -T 6

# 合并
anvi-merge */PROFILE.db -c contigs.db -o MERGED -S PROJECT

# === 技能 03：分箱 ===
anvi-cluster-contigs -p MERGED/PROFILE.db -c contigs.db -C COL --driver concoct -T 6
anvi-compute-completeness -c contigs.db -p MERGED/PROFILE.db -C COL

# === 技能 07：可视化和精修 ===
anvi-interactive -p MERGED/PROFILE.db -c contigs.db -C COL
anvi-refine -p MERGED/PROFILE.db -c contigs.db -C COL -b BIN_NAME

# === 技能 03：汇总 ===
anvi-summarize -c contigs.db -p MERGED/PROFILE.db -C COL -o SUMMARY

# === 技能 06：物种分类 ===
anvi-setup-scg-taxonomy
anvi-run-scg-taxonomy -c contigs.db -T 6
anvi-estimate-scg-taxonomy -c contigs.db -p MERGED/PROFILE.db -C COL -O taxonomy

# === 技能 05：代谢 ===
anvi-setup-kegg-data
anvi-run-kegg-kofams -c contigs.db -T 6
anvi-estimate-metabolism -c contigs.db -p MERGED/PROFILE.db -C COL -O metabolism
```

## 3. 新手建议

### 3.1 从小数据集开始

在完整数据集上运行之前，先用一小部分数据测试命令是否正常：
```bash
# 提取少量 contigs 用于测试
head -n 1000 contigs.fa > test_contigs.fa
```

### 3.2 使用多线程

大多数 anvi'o 程序支持 `-T` 参数指定线程数。根据系统资源使用尽可能多的线程：
```bash
anvi-run-hmms -c contigs.db -T 16  # 使用 16 个线程
```

### 3.3 保持数据库组织有序

```bash
mkdir -p project/{contigs_db,profiles,summaries}
```

### 3.4 查看程序帮助

每个 anvi'o 程序都有详尽的内置帮助：
```bash
anvi-gen-contigs-database -h     # 基本帮助
anvi-run-hmms -hh                # 扩展帮助（如果可用）
```

### 3.5 访问 anvi'o 社区

- [anvi'o 官方文档](https://anvio.org/)
- [anvi'o GitHub](https://github.com/merenlab/anvio)
- [anvi'o Discord 社区](https://discord.gg/CFQeCwD8S5)

## 4. 故障排除

### 常见问题

| 问题 | 解决方案 |
|---|---|
| "command not found: anvi-*" | 激活你的 anvi'o conda 环境 |
| 浏览器没有自动打开 | 使用 `--server-only` 并手动访问地址 |
| 聚类时内存不足 | 跳过层次聚类或减小数据集 |
| KEGG 版本不匹配 | 用匹配的 KEGG 数据重新运行 `anvi-run-kegg-kofams` |
| Bin 质量低 | 使用 `anvi-refine` 手动检查和精修 |

### 获取帮助

1. 查看相关技能文档的**常见陷阱**部分
2. 使用 `-h` 运行程序查看所有可用选项
3. 在 [anvi'o GitHub issues](https://github.com/merenlab/anvio/issues) 中搜索
4. 在 [anvi'o Discord](https://discord.gg/CFQeCwD8S5) 中提问
