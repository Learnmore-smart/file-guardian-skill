# File Guardian

A SOLO Skill that forces AI to write documentation before writing code. Replaces vague Plans with file-level constraints, eliminates AI hallucination through `.ai/` mirrored documentation.

---

## The Problem: Why Plans Aren't Enough

| Pain Point | Why Plans Fail | How File Guardian Fixes It |
|------------|---------------|---------------------------|
| AI breaks global styles when editing a Button | Plan only says "page has a button", doesn't constrain Button.tsx | **File-level doc** forces AI to read `.ai/src/components/Button.md`, knows it depends on theme.css |
| AI "fixes a bug" with hallucinated code | Plan was written at project start, AI ignores it during bug fixes | **HARD-GATE** forces documenting bug cause and fix plan before touching code |
| 3 months later, no idea why AI wrote code that way | Plan buried deep in folders, impossible to find | **Mirrored structure** — code path = doc path, always in sync |

**Plan is "upfront design". File Guardian is "continuous constraint".**

---

## File Guardian vs Plan/Spec

| Dimension | Plan/Spec | File Guardian |
|-----------|-----------|---------------|
| **Granularity** | Project / Feature level | **File level** — every .tsx/.ts/.css gets its own doc |
| **Location** | Separate docs/ folder | **Mirrored** — `src/components/Button.tsx` maps to `.ai/src/components/Button.md` |
| **Enforcement** | "Please refer to design doc" (ignorable) | **HARD-GATE** — AI cannot modify code without reading doc first |
| **Sync** | Written once at project start, never updated | **Forced sync** — doc must be updated before code changes |
| **Lifecycle** | Project kickoff only | **Full lifecycle** — enforced on every single change |

---

## Doc Location: `.ai/` Directory

```
Code:                            AI Docs:
src/                             .ai/src/
├── components/                  ├── components/
│   ├── Button.tsx       →       │   ├── Button.md
│   └── Card.tsx         →       │   └── Card.md
├── utils/                       ├── utils/
│   ├── format.ts        →       │   ├── format.md
│   └── validate.ts      →       │   └── validate.md
└── features/                    └── features/
    └── pricing/                     └── pricing/
        ├── Calculator.tsx  →          ├── Calculator.md
        └── utils.ts        →          └── utils.md

.ai/PROJECT_CONTEXT.md           (Global project context)
```

**Why `.ai/`**:
- Hidden directory (`.` prefix) — won't clutter your file explorer
- Unique name — won't conflict with `docs/`, `doc/`, `documentation/`
- Self-explanatory — clearly indicates AI constraint docs

---

## Two Initialization Modes

### Mode 1: Progressive (Default — for large codebases)

```
You: Fix Button.tsx
AI: Creates .ai/src/components/Button.md (only this one, ignores other files)
```

**Best for**: 200k+ line codebases. Zero overhead — docs created only for files you actually use.

### Mode 2: Full Initialization (User triggered)

**Trigger phrases**:
- "Initialize all AI docs"
- "Full init File Guardian"
- "Create doc mirror for all files"

```
You: Initialize all AI docs

AI: Detected 482 files, processing in 25 batches...
    [Batch 1/25] ✓
    [Batch 2/25] ✓
    ...
    All done!
```

**Best for**: When you want complete documentation upfront. Higher initial token cost, zero ongoing overhead.

---

## Installation

### 1. Install Skill

Copy `file-guardian/` to your SOLO skills directory:

```bash
# Typical paths
~/.solo/skills/file-guardian/
# or
/data/user/skills/file-guardian/
```

### 2. First Use

**Progressive (recommended)**:
```
You: Fix Button.tsx
AI: Auto-creates .ai/ directory and Button.md, then modifies code
```

**Full initialization**:
```
You: Initialize all AI docs
AI: Batch-creates .md docs for all files
```

### 3. Daily Usage

**Modify code**:
```
You: Add loading state to Button
AI: Read .ai/Button.md → Update doc → Show for confirmation → Modify code → Sync doc
```

**Fix bug**:
```
You: Button click not working
AI: Read doc → Record bug cause and fix plan in doc → You confirm → Fix → Sync
```

**Create new file**:
```
You: Create a price calculator
AI: Create .ai/PriceCalculator.md (design doc) → You confirm → Write code
```

---

## Doc Format

Each `.ai/{path}/{file}.md` contains:

```markdown
# FileName

## Purpose (one sentence)
"A price calculator that takes quantity and unit price, auto-calculates total with discounts"

## What It Does
Detailed feature description

## Public API
| Name | Type | Description |

## Dependencies
- Internal: ./path/to/file — why
- External: package-name — why

## Agent Decisions / Thoughts
- **{DATE} {AUTHOR}:** Record non-obvious reasoning, trade-offs, design choices, or mental models here to help future agents build upon this work without losing context.

## Important Notes / NEVER Change
- Critical constraint 1
- Critical constraint 2

## Bug Fixes
| Date | Bug | Cause | Fix |

## Change History
| Date | Change | Author |
```

---

## Core Principle

**NO DOC → NO WORK**

1. Check `.ai/` exists? → Create if not
2. Check file's `.md` exists? → Create FIRST if not
3. Read the doc
4. Update doc with planned changes
5. Get user confirmation
6. Modify code
7. Sync doc

**No exceptions.**

---

## File Structure

```
file-guardian/
├── SKILL.md                    # Main skill file
├── README.md                   # This file
├── rules/
│   ├── edge-cases.md          # 9 edge case handlers
│   └── initialization.md      # Initialization guide
└── templates/
    ├── file-doc.md            # Single file doc template
    └── project-context.md     # Project-level doc template
```

---

## Before vs After

**Before (Plan approach)**:
```
You: Fix the Button click bug
AI: OK (directly modifies code, might fix it, might break it)
```

**After (File Guardian)**:
```
You: Fix the Button click bug

AI:
[Reads .ai/src/components/Button.md]
"Per doc: Button uses theme.css, onClick must be bound to button element.

📄 Bug Fix Record:
- Problem: onClick bound to div
- Fix: Move to button element
- Impact: Current file only

Confirm to proceed?"

You: Confirm
AI: [Fix, verify, sync doc]
```

---

## Roadmap

- [ ] Auto-sync detection (warn when code changes without doc update)
- [ ] More languages (Python, Go, Rust)
- [ ] Doc change diff display
- [ ] VSCode extension

---

## License

MIT

---

---

# File Guardian（中文文档）

强制 AI 写代码前先写文档的 SOLO Skill。用文件级约束替代模糊的 Plan，通过 `.ai/` 镜像文档彻底解决 AI 幻觉问题。

---

## 核心问题：为什么 Plan 不够用？

| 被坑场景 | 为什么 Plan 没用 | File Guardian 怎么解决 |
|---------|-----------------|----------------------|
| 让 AI 改个 Button 组件，结果把全局样式搞崩了 | Plan 只描述"页面有按钮"，不约束 Button.tsx 具体怎么写 | **文件级文档**强制 AI 读 `.ai/src/components/Button.md`，知道它依赖 theme.css |
| AI 说"修复了 bug"，其实是幻觉编答案 | Plan 是项目初期写的，修 bug 时 AI 不会去看 | **HARD-GATE**强制修 bug 前先写修复方案到文档 |
| 三个月后看代码，不知道 AI 为什么这么写 | Plan 埋在文件夹深处，找不到了 | **镜像结构**，代码在哪文档就在哪，永远同步 |

**Plan 是"事前设计"，File Guardian 是"持续约束"。**

---

## 核心区别：File Guardian vs Plan/Spec

| 维度 | Plan/Spec | File Guardian |
|------|-----------|---------------|
| **粒度** | 项目级/功能级 | **文件级** — 每个 .tsx/.ts/.css 都有自己的文档 |
| **位置** | 单独的 docs/ 或根目录 | **镜像结构** — `src/components/Button.tsx` 对应 `.ai/src/components/Button.md` |
| **强制力** | "请参考设计文档"（可忽略） | **HARD-GATE** — 强制 AI 先读文档，不读不能改 |
| **同步** | 项目初期写一次，后期不同步 | **强制同步** — 改代码前必须先更新文档 |
| **适用阶段** | 项目启动期 | **全生命周期** — 每次修改都生效 |

---

## 文档位置：`.ai/` 目录

```
代码：src/components/Button.tsx
文档：.ai/src/components/Button.md

代码：src/utils/format.ts
文档：.ai/src/utils/format.md
```

**为什么 `.ai/`：**
- 隐藏目录（`.` 开头），不会和普通文件夹混淆
- 独特命名，不会和 `docs/` 冲突
- 明确表示这是 AI 约束文档

---

## 两种初始化方式

### 方式一：渐进式（默认，适合大项目）

```
你：帮我改 Button.tsx
AI：创建 .ai/src/components/Button.md（仅此一个，其他文件不处理）
```

**适合**：20万行代码大项目，用哪个创哪个，零 overhead。

### 方式二：全量初始化（用户触发）

**触发语**：
- "给我初始化所有 AI 文档"
- "全量初始化 File Guardian"
- "给所有文件创建文档镜像"

```
你：给我初始化所有 AI 文档

AI：扫描到 482 个文件，分 25 批处理...
     [第 1/25 批] ✓
     [第 2/25 批] ✓
     ...
     全部完成！
```

**适合**：你想一次性全搞定，愿意付 token 成本换完整文档。

---

## 安装使用

### 1. 安装 Skill

将 `file-guardian` 目录复制到你的 SOLO skills 目录：

```bash
# 通常路径
~/.solo/skills/file-guardian/
# 或
/data/user/skills/file-guardian/
```

### 2. 第一次使用

**渐进式（推荐）**：
```
你：帮我改 Button.tsx
AI：自动创建 .ai/ 目录和 Button.md，然后修改代码
```

**全量初始化**：
```
你：给我初始化所有 AI 文档
AI：批量创建所有文件的 .md 文档
```

### 3. 日常使用

**修改代码**：
```
你：给 Button 加 loading 状态
AI：读 .ai/Button.md → 更新文档 → 展示确认 → 改代码 → 同步文档
```

**修复 bug**：
```
你：Button 点击没反应
AI：读文档 → 在文档记录 bug 原因和修复方案 → 你确认 → 修复 → 同步
```

**创建新文件**：
```
你：创建价格计算器
AI：先创建 .ai/PriceCalculator.md（设计文档）→ 你确认 → 写代码
```

---

## 文档格式

每个 `.ai/{path}/{file}.md` 包含：

```markdown
# FileName

## Purpose（一句话人话）
"这个是价格计算器，用户输入数量和单价，自动计算总价"

## What It Does
详细功能描述

## Public API
| Name | Type | Description |

## Dependencies
- Internal: ./path/to/file — why
- External: package-name — why

## Agent Decisions / Thoughts
- **{DATE} {AUTHOR}:** 记录此处非显然的设计推导、权衡取舍、架构决策或心智模型，帮助未来的 AI Agent 理解上下文并在此基础上构建。

## Important Notes / NEVER Change
- 关键约束1
- 关键约束2

## Bug Fixes
| Date | Bug | Cause | Fix |

## Change History
| Date | Change | Author |
```

---

## 核心原则

**NO DOC → NO WORK**

1. 检查 `.ai/` 存在？不存在则创建
2. 检查文件对应的 `.md` 存在？不存在则先创建
3. 读取文档
4. 更新文档（记录计划变更）
5. 用户确认
6. 修改代码
7. 同步文档

**没有任何例外。**

---

## 文件结构

```
file-guardian/
├── SKILL.md                    # 主技能文件
├── README.md                   # 本文件
├── rules/
│   ├── edge-cases.md          # 9个边缘情况处理
│   └── initialization.md      # 初始化指南
└── templates/
    ├── file-doc.md            # 单文件文档模板
    └── project-context.md     # 项目级文档模板
```

---

## 效果对比

**以前（Plan 方式）**：
```
你：帮我修 Button 的点击 bug
AI：好的（直接改代码，可能修对，可能修错）
```

**现在（File Guardian）**：
```
你：帮我修 Button 的点击 bug

AI：
[读取 .ai/src/components/Button.md]
"根据文档，Button 使用 theme.css，onClick 必须绑定到 button 元素。

📄 Bug Fix 记录：
- 问题：onClick 绑定到了 div
- 修复：移到 button 元素
- 影响：仅当前文件

确认后实施修复。"

你：可以
AI：[修复，验证，同步文档]
```

---

## 后续优化

- [ ] 文档自动同步检测（代码改了提醒更新文档）
- [ ] 支持更多语言（Python、Go、Rust）
- [ ] 文档变更 diff 展示
- [ ] VSCode 插件支持

---

## License

MIT
