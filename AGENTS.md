# Bible Wiki — Schema

## 概述

本 Wiki 是一个关于**圣经真理**的持久化、LLM 维护的知识库。
LLM 负责编写和维护所有 Wiki 页面。用户负责策展来源、引导分析和提出问题。
默认使用**中文**输出所有内容。

## 目录结构

```
bible_wiki/
├── raw/                    # 不可变的源文档
│   ├── assets/            # 下载的图片和媒体
│   └── {目录号}{名称}/    # 例如: 68Christ/, 73FaithHopeLove/
│       ├── *.htm          # ccbiblestudy HTML 文件
│       └── *.pdf          # PDF 文件
├── wiki/                   # LLM 生成的 Wiki 页面
│   ├── index.md           # 内容目录（每次操作后更新）
│   ├── log.md             # 时间顺序操作日志
│   ├── overview.md        # Wiki 高层综述
│   ├── sources/           # 源摘要页面
│   ├── entities/          # 实体页面（人物、地点、组织等）
│   ├── concepts/          # 概念/主题页面
│   └── analyses/          # 归档的查询结果、对比分析、深度探讨
├── AGENTS.md              # 本 Schema 文件
└── .gitignore
```

## 页面分类

- **来源** (`wiki/sources/`)：每个摄入源一页。摘要、关键论点、元数据。
- **实体** (`wiki/entities/`)：每个实体一页（人物、组织、地点等）。跨来源汇聚信息。
- **概念** (`wiki/concepts/`)：每个主题/概念一页。跨来源综合理解。
- **分析** (`wiki/analyses/`)：归档的查询结果——对比、深度探讨、时间线等。
- **综述** (`wiki/overview.md`)：整个 Wiki 的高层综合。在重要摄入后更新。

## 约定

### 文件名
- 使用 kebab-case：`ji-du-de-shen-xing.md`、`bao-luo.md`
- 源摘要加日期前缀：`2026-05-04-68Christ-summary.md`

### 前置数据
每个 Wiki 页面必须包含 YAML 前置数据：
```yaml
---
title: 页面标题
tags: [标签1, 标签2]
date_created: YYYY-MM-DD
date_updated: YYYY-MM-DD
sources: [source1.md, source2.md]
---
```

### 交叉引用
- 使用 `[[wikilinks]]` 作内部链接（Obsidian 兼容）
- 使用相对路径引用来源：`[来源](../../raw/68Christ/102S.htm)`

### 索引格式
index.md 中每个条目：
```markdown
- [[page-name]] — 一行摘要（N 个来源）
```

### 日志格式
log.md 中每个条目：
```markdown
## [YYYY-MM-DD] 操作 | 标题
简要描述执行了什么操作。
创建/更新的页面：[[page1]]、[[page2]]、...
```

操作类型：`摄入`、`查询`、`检查`、`更新`、`初始化`

## 工作流

### 摄入工作流
1. 读取 raw/ 中的源文件（Big5 编码，HTML 实体需解码）
2. 在 wiki/sources/ 创建源摘要
3. 识别实体和概念
4. 创建或更新实体/概念页面
5. 添加交叉引用
6. 更新 index.md
7. 追加 log.md

### 查询工作流
1. 读取 index.md
2. 读取相关页面
3. 综合回答
4. 可选择将回答归档到 wiki/analyses/
5. 如归档则更新 index.md 和 log.md

### 检查工作流
1. 扫描矛盾、孤立页、过时内容
2. 报告发现
3. 应用批准的修复
4. 追加 log.md

## 领域专属备注

### ccbiblestudy.net 文档特点
- **HTML 文件**：Big5 编码，HTML 实体需 `html.unescape()` 解码
- **MS Word HTML**：含 `o:`、`w:` 命名空间标签，需先清理
- **成对文件**：`.htm`(简体) + `.htmS`(和合本)
- **PDF 文件**：检查 `%PDF-1.x` 头部

### 圣经分类
- 旧约 / 新约
- 主题分类：神学、人物、教会、历史、末世

## Obsidian 连接

- 将 `/root/bible_wiki/` 作为 vault 打开
- 安装 Obsidian Web Clipper 快速添加网页来源
- 使用图谱视图可视化 Wiki 结构
