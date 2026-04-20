---
name: process-article
description: |
  处理 Obsidian vault 中变动的文章（新增/修改/删除），同步更新总结和索引，最后推送到 GitHub。
  
  当用户：
  - 将新文章放入 raw/ 目录
  - 说"同步文章"、"处理文章变动"、"sync articles"
  - 要求"处理文章"、"同步到 git"、"sync and push"
  - 在 current_note 中打开 raw/ 目录下的文件
  
  使用此 skill 进行增量同步：
  1. 检测 raw/ 目录变动（新增/修改/删除）
  2. 新增 → 创建总结 + 更新 index
  3. 修改 → 更新总结 + 更新 index
  4. 删除 → 删除总结 + 从 index 移除
  5. 提交并推送到 GitHub
---

# Process Article Skill - 增量同步版

自动检测 `raw/` 目录的变动，同步 `summaries/` 和 `index.md`，最后推送到 GitHub。

## 工作流程

```
┌─────────────────────────────────────────────────────────────┐
│  检测 raw/ 目录变动（对比 summaries/ 的 source 映射）        │
│                          ↓                                   │
│  ┌─────────┬─────────┬─────────┐                            │
│  │ 新增文章 │ 修改文章 │ 删除文章 │                            │
│  └────┬────┴────┬────┴────┬────┘                            │
│       ↓        ↓        ↓                                  │
│  创建总结  更新总结  删除总结                                  │
│       ↓        ↓        ↓                                  │
│  更新 index.md（同步链接）                                   │
│                          ↓                                   │
│  Git: add → commit → push                                   │
└─────────────────────────────────────────────────────────────┘
```

## 检测变动

### 步骤 1: 获取当前文件列表

```bash
# 列出 raw/ 下所有 .md 文件
find raw/ -name "*.md" -type f

# 列出 summaries/ 下所有总结文件
find summaries/ -name "*.md" -type f
```

### 步骤 2: 读取 summaries 文件的 source 字段

每个总结文件 frontmatter 中的 `source` 字段指向原始文件：

```yaml
---
title: "..."
source: "原始链接或 [[raw/path/to/file.md]]"
type: summary
---
```

### 步骤 3: 分类变动

| 变动类型 | 判断条件 |
|---------|---------|
| **新增** | raw/ 中的文件没有对应的 summary |
| **修改** | 文件时间戳晚于对应的 summary 更新时间 |
| **删除** | summary 的 source 指向的文件已不存在 |

## 处理新增文章

### 1. 读取原始文档

读取 `raw/<category>/<file>.md`，提取 frontmatter 和内容。

### 2. 确定分类

根据文件路径确定 summary 的分类目录：

| 原始路径 | 总结分类 |
|---------|---------|
| `raw/articles/xxx.md` | `summaries/` 或按主题细分 |
| `raw/notes/xxx.md` | `summaries/notes/` |
| `raw/books/xxx.md` | `summaries/books/` |

### 3. 创建总结文档

在 `summaries/<category>/` 下创建总结文件。

**文件命名规则：**
- 如果原文件是英文，翻译标题：`Spec Kit.md` → `Spec Kit - AI驱动的规格驱动开发工具包.md`
- 如果原文件是中文，保留标题：`设计模式.md` → `设计模式 - 创建型模式.md`

**文档结构：**

```markdown
---
title: "文档标题"
source: "[[raw/<category>/<原始文件名>]]"
type: summary
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags:
  - "标签1"
---

> 📄 原始文档：[[raw/<category>/<原始文件名>]]

---

# 文档标题

## 概述

一句话介绍文章主题...

## 核心概念

| 项目 | 说明 |
|------|------|
| ... | ... |

## [其他章节]

根据内容类型添加：工具类→安装/使用；教程类→步骤/注意事项；概念类→定义/原理

## 参考来源

- [链接名称](URL)
```

## 处理修改文章

### 1. 识别修改的文件

比较 raw/ 文件的修改时间和对应 summary 的 `updated` 字段。

### 2. 更新总结

读取更新后的原始文档，重新生成总结内容。

**保留原有的 `created` 字段，只更新 `updated` 字段：**

```yaml
---
created: 2026-04-18  # 保留原值
updated: 2026-04-18  # 更新为当前日期
---
```

## 处理删除文章

### 1. 识别已删除的文件

检查 summaries/ 中所有文件的 source 字段，指向不存在的 raw/ 文件。

### 2. 删除总结文件

删除对应的 `summaries/<category>/<file>.md`。

### 3. 从 index.md 移除链接

从 `index.md` 中找到并删除该总结的 wikilink。

## 更新 index.md

### 读取当前 index

```bash
cat index.md
```

### 查找对应分类

根据总结的主题，在 index.md 中找到或创建对应的分类部分。

### 添加/更新链接

```markdown
### 子分类

- [[summaries/<category>/<文档名>]] - 简短描述
```

### 删除链接

找到并移除对应的 wikilink 行。

## Git 推送

### 步骤 1: 添加文件

```bash
git add raw/ summaries/ index.md
```

### 步骤 2: 生成提交信息

根据变动类型生成描述性提交信息：

| 变动类型 | 提交信息格式 |
|---------|-------------|
| 新增 | `feat: 添加 "<文件名>" 的总结` |
| 修改 | `fix: 更新 "<文件名>" 的总结` |
| 删除 | `fix: 删除 "<文件名>" 的总结` |
| 混合 | `chore: 同步文章变动 (add: X, update: Y, delete: Z)` |

### 步骤 3: 提交

```bash
git commit -m "<提交信息>"
```

### 步骤 4: 推送

```bash
git push
```

## 错误处理

### raw/ 目录不存在

跳过同步步骤，提示用户：

```
未找到 raw/ 目录。请先将原始文章放入 raw/ 目录。
```

### summaries/ 目录不存在

自动创建所需的分类目录：

```bash
mkdir -p summaries/programming summaries/notes
```

### index.md 不存在

创建基础 index.md：

```markdown
---
type: knowledge-index
version: 1.0
created: YYYY-MM-DD
---

# 知识库索引

> 原始材料存于 `raw/`，知识概要存于 `summaries/`。
```

### Git 推送失败

检查网络连接和认证状态，提示用户重新认证或重试。

## 执行清单

同步完成后确认：

- [ ] 新增文章已创建总结
- [ ] 修改文章已更新总结
- [ ] 删除文章已清理总结
- [ ] index.md 已同步更新
- [ ] 所有变动已 git add
- [ ] 已生成描述性提交信息
- [ ] 已推送到 GitHub
