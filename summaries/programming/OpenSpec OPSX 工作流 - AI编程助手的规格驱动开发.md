---
title: "OpenSpec OPSX 工作流 - AI编程助手的规格驱动开发"
source: "[[raw/articles/Fission-AIOpenSpec Spec-driven development (SDD) for AI coding assistants.md]]"
type: summary
created: 2026-04-19
updated: 2026-04-19
tags:
  - "AI开发工具"
  - "规格驱动开发"
  - "工作流自动化"
---

> 📄 原始文档：[[raw/articles/Fission-AIOpenSpec Spec-driven development (SDD) for AI coding assistants.md]]

---

# OpenSpec OPSX 工作流 - AI编程助手的规格驱动开发

## 概述

OpenSpec 的 OPSX 是一种**流畅、迭代的工作流**，用于 AI 编程助手中的规格驱动开发（SDD）。相比传统的线性阶段式流程，OPSX 提供了一种更灵活、更可定制的开发方式。

**项目亮点：** 支持 Claude Code、Cursor、Windsurf 等主流 AI 编码助手 | Schema 驱动的自定义工作流

---

## 传统工作流 vs OPSX

| 特性 | 传统工作流 | OPSX |
|------|----------|------|
| **结构** | 线性阶段：计划 → 实现 → 归档 | 流畅操作：随时执行任何操作 |
| **迭代** | 难以回退更新 | 随时更新任意制品 |
| **定制** | 固定结构 | Schema 驱动（自定义制品和依赖） |
| **模板** | TypeScript 硬编码 | 外部 YAML + Markdown |

**核心洞察：** 工作不是线性的，OPSX 停止假装它是。

---

## 核心理念

### 1. 操作而非阶段

- **Actions, not phases** — 创建、实现、更新、归档，随时执行
- **Dependencies are enablers** — 依赖关系展示可能性，而非强制下一步

### 2. 快速迭代

- 编辑模板 → 立即看到 AI 输出变化
- 增量测试 → 独立验证每个制品
- 无需等待新版本发布

### 3. 完全可定制

通过 Schema 管理命令创建自定义工作流：

```bash
# 从零开始创建新 schema
openspec schema init my-workflow

# 或基于现有 schema 派生
openspec schema fork spec-driven my-workflow

# 验证 schema 结构
openspec schema validate my-workflow
```

---

## 开发流程

### Phase 1: 探索与提案

```
/opsx:explore   # 思考想法，调查问题，澄清需求
/opsx:propose    # 创建变更并生成规划制品
```

### Phase 2: 迭代实现

```
/opsx:continue   # 创建下一个制品
/opsx:ff         # 快速创建所有规划制品
/opsx:apply      # 实现任务
```

### Phase 3: 完成

```
/opsx:archive    # 归档完成的变更
```

---

## OPSX 命令参考

| 命令 | 功能 |
|------|------|
| `/opsx:propose` | 创建变更并生成规划制品（默认快速路径） |
| `/opsx:explore` | 思考想法、调查问题、澄清需求 |
| `/opsx:new` | 启动新变更脚手架（扩展工作流） |
| `/opsx:continue` | 创建下一个制品（扩展工作流） |
| `/opsx:ff` | 快速创建所有规划制品（扩展工作流） |
| `/opsx:apply` | 实现任务，按需更新制品 |
| `/opsx:verify` | 验证实现是否符合制品（扩展工作流） |
| `/opsx:sync` | 同步增量规格到主分支（扩展工作流） |
| `/opsx:archive` | 归档完成的变更 |
| `/opsx:onboard` | 端到端变更的引导式演练 |

---

## 项目配置

在 `openspec/config.yaml` 中设置项目默认值和上下文：

```yaml
schema: spec-driven

context: |
  Tech stack: TypeScript, React, Node.js
  API conventions: RESTful, JSON responses
  Testing: Vitest for unit tests, Playwright for e2e

rules:
  proposal:
    - Include rollback plan
    - Identify affected teams
  specs:
    - Use Given/When/Then format for scenarios
```

### 配置字段

| 字段 | 类型 | 说明 |
|------|------|------|
| `schema` | string | 新变更的默认 schema |
| `context` | string | 注入所有制品的项目上下文 |
| `rules` | object | 按制品 ID 的规则 |

---

## Schema 架构

**spec-driven** (默认) 提供以下制品：

```
proposal ──→ specs ──→ design ──→ tasks
```

| 制品 ID | 说明 |
|---------|------|
| `proposal` | 变更提案 |
| `specs` | 规格说明 |
| `design` | 技术设计 |
| `tasks` | 实现任务 |

---

## 何时更新 vs 重新开始

| 测试 | 更新 | 新变更 |
|------|------|--------|
| **身份** | "同一件事，优化版" | "不同的工作" |
| **范围重叠** | >50% 重叠 | <50% 重叠 |
| **完成度** | 没有这些变更无法完成 | 可以先完成原工作 |
| **故事** | 更新链讲述连贯故事 | 补丁会使事情更混乱 |

**原则：** 更新保留上下文，新变更提供清晰度。

---

## 适用场景

- 需要规范开发流程的团队项目
- 追求规格与实现一致性的 AI 辅助开发
- 需要灵活迭代的开发工作流
- 自定义工作流程的实验性探索

---

**参考来源：** https://github.com/Fission-AI/OpenSpec/blob/main/docs/opsx.md
