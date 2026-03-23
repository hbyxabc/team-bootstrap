<div align="center">

# Team Bootstrap

**One command to set up a complete AI agent team for any project.**

**一条命令，为你的项目搭建完整的 AI Agent 团队。**

[English](#english) | [中文](#中文)

</div>

---

<a id="english"></a>

<details open>
<summary><h2>🇬🇧 English</h2></summary>

## What it does

`/team-bootstrap` creates a full AI agent team management system in your project:

- **4 Agent Roles** — Product Designer, CTO, Dev, Tester, each with defined responsibilities and tool permissions
- **10-Step Workflow** — From requirements to deployment, with verification gates at every step
- **Document Management** — Lifecycle rules, naming conventions, and living spec documents
- **Safety Hooks** — Automatic blocking of force-push, credential edits, and build verification before push
- **Session Continuity** — Handoff docs and memory system for cross-session context

## Quick Start

### Install

```bash
# From GitHub
npx skills add hbyxabc/team-bootstrap -s team-bootstrap -g -y

# Or copy into your project
cp -r team-bootstrap/ /your-project/.claude/skills/team-bootstrap/
```

### Use

Open Claude Code in your project directory, then:

```
/team-bootstrap MyProjectName
```

Claude will ask you a few questions (tech stack, build commands, design tool), then generate everything automatically.

## What gets created

```
your-project/
├── CLAUDE.md                          # Project contract (~150 lines)
├── .claude/
│   ├── agents/
│   │   ├── product-designer.md        # CPO + PM + Designer
│   │   ├── cto.md                     # Architecture + Deploy + Review
│   │   ├── dev.md                     # Full-stack development
│   │   └── tester.md                  # Independent testing (read-only)
│   ├── rules/                         # (empty, add your own)
│   └── settings.json                  # Protective hooks
├── docs/
│   ├── board.md                       # Task board
│   ├── handoff.md                     # Session handoff
│   ├── team-workflow.md               # 10-step dev process
│   ├── doc-management.md              # Document lifecycle system
│   ├── product/
│   │   ├── decisions.md               # Decision record
│   │   ├── modules/                   # [Spec] docs per module
│   │   ├── requirements/              # [Requirement] docs per change
│   │   └── archive/
│   ├── design/                        # (if design tool selected)
│   │   ├── deliverables/
│   │   └── archive/
│   └── technical/
│       ├── deliverables/              # [Plan] and [Review] docs
│       └── archive/
```

## The 4 Roles

| Role | Model | What they do | What they can't do |
| --- | --- | --- | --- |
| **Product Designer** | Opus | Requirements, product logic, UX copy, visual design | Write code, deploy |
| **CTO** | Opus | Architecture review, code review, deployment, tech plans | Product decisions, visual design |
| **Dev** | Sonnet | Full-stack development, local testing, bug fixes | Modify design files, push to production |
| **Tester** | Sonnet | Independent testing, requirement verification | Modify any files (read-only) |

## The 10-Step Workflow

```
[1] Requirements → [2] Design → [3] Tech Review → [4] Development →
[5] Testing → [6] Code Review → [7] Acceptance → [8] Deploy →
[9] Production Verify → [10] Archive & Learn
```

- Bug fixes use a **fast track**: [1] → [4] → [6] → [8] → [9]
- Founder involvement at only 2 points: after [2] and at [7]

## Safety Hooks

Automatically enforced via `.claude/settings.json`:

| Hook | What it blocks |
| --- | --- |
| Force-push guard | `git push --force` or `-f` |
| No-verify guard | `--no-verify` flag |
| Build gate | Push without passing frontend build + backend check |
| File protection | Editing `.env`, credentials, or design files without permission |
| Type checker | Runs tsc/py_compile after every edit (catches errors immediately) |

## Design Tool Support

During setup, you can choose:

- **Pencil** (.pen files) — Auto-configures Pencil MCP. No API key needed.
- **Figma** — Guides you through getting a Figma API key and configures the connection.
- **None** — Skips design tool setup. Design-related sections are removed.

## After Setup

1. **Review CLAUDE.md** — Customize the ALWAYS/NEVER rules for your project
2. **Add code rules** — Create `.claude/rules/backend.md`, `frontend.md` etc. with language-specific conventions
3. **Create spec docs** — For each existing module, create a `[Spec]` doc in `docs/product/modules/`
4. **Try a small feature** — Run one feature through the full 10-step workflow to validate

## Key Concepts

### Spec Documents

Each module has one living, complete spec document. It's not a summary — it's the full product definition. When a feature ships, requirement details merge into the spec verbatim. Read one spec, understand the entire module.

### Session Continuity

At session start: read `board.md` → `handoff.md` → `memory/`

At session end: update `board.md` + `handoff.md` → commit + push

### Document Lifecycle

```
draft → review → approved → implemented → archived
```

Requirement docs get archived after their content merges into the spec. Review docs get archived 7 days after conclusions land in code/rules.

## Requirements

- [Claude Code](https://claude.com/claude-code) CLI
- Node.js (for npx)
- Git

## License

MIT

</details>

---

<a id="中文"></a>

<details>
<summary><h2>🇨🇳 中文</h2></summary>

## 它做什么

`/team-bootstrap` 在你的项目中创建一套完整的 AI Agent 团队管理系统：

- **4 个 Agent 角色** — 产品设计师、CTO、开发、测试，各有明确职责和工具权限
- **10 步开发流程** — 从需求到部署，每一步都有验证关卡
- **文档管理** — 生命周期规则、命名规范、活文档（规格文档）体系
- **安全钩子** — 自动拦截强制推送、凭证编辑，推送前强制构建验证
- **会话接续** — 交接文档和记忆系统，确保跨会话上下文不丢失

## 快速开始

### 安装

```bash
# 从 GitHub 安装
npx skills add hbyxabc/team-bootstrap -s team-bootstrap -g -y

# 或者直接复制到项目
cp -r team-bootstrap/ /your-project/.claude/skills/team-bootstrap/
```

### 使用

在项目目录打开 Claude Code，然后输入：

```
/team-bootstrap 我的项目名
```

Claude 会问你几个问题（技术栈、构建命令、设计工具），然后自动生成所有内容。

## 生成的文件结构

```
your-project/
├── CLAUDE.md                          # 项目契约（约 150 行）
├── .claude/
│   ├── agents/
│   │   ├── product-designer.md        # 产品设计师（CPO + PM + 设计师）
│   │   ├── cto.md                     # CTO（架构 + 部署 + 评审）
│   │   ├── dev.md                     # 全栈开发
│   │   └── tester.md                  # 独立测试（只读）
│   ├── rules/                         # （空目录，添加你自己的规则）
│   └── settings.json                  # 安全钩子
├── docs/
│   ├── board.md                       # 任务看板
│   ├── handoff.md                     # 会话交接
│   ├── team-workflow.md               # 10 步开发流程
│   ├── doc-management.md              # 文档生命周期系统
│   ├── product/
│   │   ├── decisions.md               # 决策记录
│   │   ├── modules/                   # 各模块的 [规格] 文档
│   │   ├── requirements/              # 各变更的 [需求] 文档
│   │   └── archive/
│   ├── design/                        # （选择了设计工具时生成）
│   │   ├── deliverables/
│   │   └── archive/
│   └── technical/
│       ├── deliverables/              # [方案] 和 [评审] 文档
│       └── archive/
```

## 4 个角色

| 角色 | 模型 | 职责 | 不能做 |
| --- | --- | --- | --- |
| **产品设计师** | Opus | 需求、产品逻辑、UX 文案、视觉设计 | 写代码、部署 |
| **CTO** | Opus | 架构审查、代码审查、部署、技术方案 | 产品决策、视觉设计 |
| **Dev** | Sonnet | 全栈开发、本地测试、Bug 修复 | 修改设计文件、推送到生产环境 |
| **Tester** | Sonnet | 独立测试、需求验证 | 修改任何文件（只读权限） |

## 10 步工作流

```
[1] 需求 → [2] 设计 → [3] 技术评审 → [4] 开发 →
[5] 测试 → [6] 代码审查 → [7] 验收 → [8] 部署 →
[9] 生产验证 → [10] 归档与总结
```

- Bug 修复走**快速通道**：[1] → [4] → [6] → [8] → [9]
- 创始人只需在 2 个节点介入：[2] 之后和 [7] 验收时

## 安全钩子

通过 `.claude/settings.json` 自动执行：

| 钩子 | 拦截内容 |
| --- | --- |
| 强制推送防护 | `git push --force` 或 `-f` |
| 跳过验证防护 | `--no-verify` 标志 |
| 构建门禁 | 未通过前端构建 + 后端检查就推送 |
| 文件保护 | 未经批准编辑 `.env`、凭证或设计文件 |
| 类型检查 | 每次编辑后运行 tsc/py_compile（立即捕获错误） |

## 设计工具支持

在初始化时，你可以选择：

- **Pencil**（.pen 文件）— 自动配置 Pencil MCP，无需 API Key
- **Figma** — 引导你获取 Figma API Key 并配置连接
- **无** — 跳过设计工具配置，移除设计相关章节

## 初始化之后

1. **检查 CLAUDE.md** — 根据你的项目自定义 ALWAYS/NEVER 规则
2. **添加代码规则** — 在 `.claude/rules/` 下创建 `backend.md`、`frontend.md` 等语言级规范
3. **创建规格文档** — 为已有模块在 `docs/product/modules/` 创建 [规格] 文档
4. **试跑一个小功能** — 让一个功能走完整的 10 步流程来验证

## 核心概念

### 规格文档（[规格]）

每个模块有一份活的、完整的规格文档。它不是摘要——而是完整的产品定义。功能上线后，需求细节会逐字合并进规格文档。读一份规格，就能理解整个模块。

### 会话接续

会话开始时：读 `board.md` → `handoff.md` → `memory/`

会话结束时：更新 `board.md` + `handoff.md` → commit + push

### 文档生命周期

```
草稿 → 评审 → 已批准 → 已实现 → 已归档
```

需求文档在内容合并进规格后归档。评审文档在结论落地到代码/规则 7 天后归档。

## 环境要求

- [Claude Code](https://claude.com/claude-code) CLI
- Node.js（用于 npx）
- Git

## 许可证

MIT

</details>
