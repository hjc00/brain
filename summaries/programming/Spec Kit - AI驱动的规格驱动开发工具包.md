---
title: "Spec Kit - AI驱动的规格驱动开发工具包"
source: "https://speckit.org/"
type: summary
created: 2026-04-18
tags:
  - "AI开发工具"
  - "规格驱动开发"
  - "工作流自动化"
---

> 📄 原始文档：[[raw/articles/Spec Kit - AI-Powered Specification-Driven Development Toolkit]]

---

# Spec Kit - AI驱动的规格驱动开发工具包

## 概述

Spec Kit 是一款革新性的 AI 驱动规格驱动开发（Spec-Driven Development）工具包，通过将规格文档转化为可执行代码，显著提升软件开发效率。

**数据亮点：** 28K+ GitHub Stars | 11+ AI 代理支持 | 2.3K+ Forks | MIT 开源协议

---

## 核心理念

| 理念 | 说明 |
|------|------|
| 🎯 **关注"什么"** | 定义要构建什么及原因，而非技术实现细节 |
| 🤖 **AI 驱动** | 利用 AI 代理自动将规格转化为可工作代码 |
| 🚀 **更快交付** | 消除规格与实现之间的鸿沟，缩短开发周期 |

---

## 开发流程

### Phase 1: 基础阶段

1. **Constitution** - 建立项目原则和开发指南
2. **Specification** - 清晰定义需求
3. **Clarification** - 澄清模糊之处

### Phase 2: 实现阶段

1. **Planning** - 选择技术栈和架构
2. **Tasks** - 分解为可执行任务
3. **Implementation** - 生成可工作代码

---

## 斜杠命令参考

| 命令 | 功能 |
|------|------|
| `/constitution` | 创建项目治理原则和开发规范 |
| `/specify` | 定义要构建的内容（需求和用户故事） |
| `/clarify` | 通过结构化提问澄清不明确区域 |
| `/plan` | 创建技术实现计划（技术栈、架构） |
| `/tasks` | 生成可操作的任务列表 |
| `/analyze` | 跨制品一致性和覆盖率分析 |
| `/implement` | 执行所有任务，构建功能代码 |

---

## 快速开始

```bash
# 1. 安装
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# 2. 初始化项目
specify init my-project --ai claude

# 3. 创建规格
/specify Build a task management app with user authentication...

# 4. 生成和实现
/plan Use React with TypeScript, Node.js backend, PostgreSQL
/tasks
/implement
```

---

## 支持的 AI 代理

支持 11+ 主流 AI 编码助手，包括：Claude、Copilot、Cursor、Gemini 等。

---

## 适用场景

- 需要明确需求定义的团队项目
- 希望规范开发流程的中大型项目
- 需要 AI 辅助代码生成的开发工作流
- 追求高质量文档与实现一致性的项目

---

**参考来源：** https://speckit.org/
