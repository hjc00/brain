---
title: "OpenSpec - AI编程助手的规格驱动开发框架"
source: "https://github.com/Fission-AI/OpenSpec"
type: summary
created: 2026-04-19
updated: 2026-04-19
tags:
  - "AI开发工具"
  - "规格驱动开发"
  - "工作流自动化"
---

> 📄 原始文档：https://github.com/Fission-AI/OpenSpec/tree/main/docs

---

# OpenSpec - AI编程助手的规格驱动开发框架

## 概述

OpenSpec 是一个轻量级的规格管理系统，帮助开发者和 AI 编程助手在写代码之前就"要做什么"达成一致。

**核心理念：** 流畅迭代、简单实用、棕地优先（兼容现有代码库）

---

## 核心概念

### 文件结构

```
openspec/
├── specs/           # 规格的来源：描述系统当前行为
└── changes/        # 提议的变更：在独立文件夹中管理
```

### 规格格式

使用 **Requirements + Scenarios** 结构：

- **GIVEN** - 前置条件
- **WHEN** - 触发动作
- **THEN** - 预期结果

**关键词：** SHALL / MUST / SHOULD（遵循 RFC 2119）

### 变更制品（Delta Specs）

每个变更文件夹包含：

| 制品 | 说明 |
|------|------|
| `proposal.md` | 意图、范围和方法 |
| `specs/` | 增量规格（ADDED/MODIFIED/REMOVED） |
| `design.md` | 技术架构决策 |
| `tasks.md` | 实现检查清单 |

---

## 安装

### 环境要求

- **Node.js 20.19.0+**

### 安装方式

```bash
# npm
npm install -g @fission-ai/openspec@latest

# pnpm
pnpm add -g @fission-ai/openspec@latest

# yarn
yarn global add @fission-ai/openspec@latest

# bun
bun add -g @fission-ai/openspec@latest
```

### Nix 安装

```bash
# 直接运行
nix run github:Fission-AI/OpenSpec -- init

# 安装到用户配置
nix profile install github:Fission-AI/OpenSpec
```

### 验证

```bash
openspec --version
```

---

## 工作流程（OPSX）

**核心原则：** 操作而非阶段

### 默认工作流（Core Profile）

```
/opsx:propose → /opsx:apply → /opsx:archive
```

### 扩展工作流

| 命令 | 功能 |
|------|------|
| `/opsx:explore` | 探索想法，调查问题 |
| `/opsx:new` | 启动新变更脚手架 |
| `/opsx:continue` | 创建下一个制品 |
| `/opsx:ff` | 快速创建所有制品 |
| `/opsx:apply` | 实现任务 |
| `/opsx:verify` | 验证实现符合制品 |
| `/opsx:archive` | 归档完成的变更 |

### 工作流模式

**快速功能：**
```
/opsx:new → /opsx:ff → /opsx:apply → /opsx:verify → /opsx:archive
```

**探索模式：**
```
/opsx:explore → /opsx:new → /opsx:continue → ... → /opsx:apply
```

---

## CLI 命令

```bash
# 初始化
openspec init                    # 初始化项目配置
openspec update                  # 重新生成配置

# 浏览
openspec list                    # 列出变更/规格
openspec view                    # 交互式仪表板
openspec show <item>            # 显示详情

# 生命周期
openspec archive <change>        # 归档变更

# 工作流
openspec status --change <name> # 查看制品状态
openspec instructions <artifact># 获取 AI 指导

# Schema 管理
openspec schemas                 # 列出可用 schema
openspec schema validate          # 验证 schema
openspec schema fork <source> <target>  # 派生 schema
```

---

## 自定义配置

### 项目配置（config.yaml）

```yaml
schema: spec-driven

context: |
  Tech stack: TypeScript, React
  API: RESTful, JSON
  Testing: Vitest

rules:
  proposal:
    - Include rollback plan
  specs:
    - Use Given/When/Then format
```

### Schema 派生

```bash
# 从现有 schema 派生
openspec schema fork spec-driven my-workflow

# 从零开始创建
openspec schema init name --artifacts "a,b,c"
```

### Schema 优先级

1. CLI 参数（`--schema`）
2. 变更元数据（`.openspec.yaml`）
3. 项目配置（`openspec/config.yaml`）
4. 默认值（`spec-driven`）

---

## 多语言支持

在 `config.yaml` 中配置语言：

```yaml
context: |
  Idioma: 简体中文
  语言：中文
```

支持：葡萄牙语、西班牙语、中文、日语、法语、德语等。

---

## 支持的 AI 工具

| 工具 | 命令格式 |
|------|----------|
| Claude Code | `/opsx:propose` |
| Cursor | `/opsx-propose` |
| Windsurf | `/opsx-propose` |
| GitHub Copilot | 自定义 |
| Amazon Q Developer | 自定义 |
| Cline | 自定义 |
| Gemini CLI | 自定义 |
| Continue | 自定义 |

### 工具配置

```bash
# 初始化所有工具
openspec init

# 指定工具
openspec init --tools claude,cursor

# 所有工具
openspec init --tools all

# 跳过
openspec init --tools none
```

---

## 迁移指南（从旧版）

**主要变化：**

| 方面 | 旧版 | OPSX |
|------|------|------|
| 命令 | `/openspec:proposal` | `/opsx:propose` |
| 工作流 | 创建所有制品 | 可增量或一次性创建 |
| 回退 | 笨拙 | 自然，随时更新 |
| 配置 | `CLAUDE.md` 标记 | `openspec/config.yaml` |

**迁移命令：**

```bash
openspec init          # 交互式
openspec update         # 迁移并刷新
openspec init --force   # 非交互式 / CI
```

---

## 适用场景

- 需要在 AI 辅助开发中保持规格一致性的团队
- 追求规格与实现可追溯性的项目
- 需要灵活迭代的开发工作流
- 兼容现有代码库的棕地开发

---

**参考来源：** https://github.com/Fission-AI/OpenSpec
