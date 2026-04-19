# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在此代码库中工作时提供指导。

---

## 项目类型

这是一个**Obsidian 知识库**，用于个人知识管理，而非软件开发项目。没有构建、测试或代码检查命令。

---

## 知识库结构

- `index.md` — 知识库索引入口
- `raw/` — 待处理的原始材料（文章、原始笔记）
- `summaries/` — 按主题整理的处理后的知识摘要
- `.obsidian/` — Obsidian 知识库配置和插件数据
- `.claude/` — Claude Code 设置和会话数据
- `.claudian/` — Claudian 插件（Obsidian ↔ Claude 集成）设置

---

## 内容工作流

1. 将原始材料放入 `raw/<类别>/`
2. 处理/摘要化到 `summaries/<类别>/`
3. 在 `index.md` 中链接条目以便导航

---

## Obsidian 插件

- **Claudian** — 将 Claude AI 集成到 Obsidian 中处理笔记
- **Obsidian42-BRAT** — 管理测试版社区插件