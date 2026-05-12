---
name: file-guardian
description: >
  ALWAYS ACTIVE for any codebase. CRITICAL RULE: Before ANY code work, 
  AI MUST ensure .ai/ mirror exists and is up-to-date. If .ai/ missing → create it.
  If .ai/{file}.md missing → create it FIRST. Documentation must be updated BEFORE code 
  is touched. For bug fixes, AI MUST document the bug and fix plan in .ai/ FIRST, 
  get user confirmation, THEN fix code. This prevents hallucination by enforcing 
  documentation-first workflow.
---

# File Guardian — Documentation-First Guardian

## The Absolute Rule

**📋 NO DOC → NO WORK**

Before touching ANY code:
1. Check `.ai/` exists (create if not)
2. Check file-specific doc exists (create if not)
3. Read doc
4. Update doc with planned changes
5. Get user confirmation
6. NOW touch code

---

## Directory Structure: `.ai/` Mirror

```
Code:                           AI Docs:
src/                            .ai/src/
├── components/                 ├── components/
│   ├── Button.tsx      →       │   ├── Button.md
│   └── Card.tsx        →       │   └── Card.md
├── utils/                      ├── utils/
│   ├── format.ts       →       │   ├── format.md
│   └── validate.ts     →       │   └── validate.md
└── features/                   └── features/
    └── pricing/                    └── pricing/
        ├── Calculator.tsx →          ├── Calculator.md
        └── utils.ts       →          └── utils.md

.ai/PROJECT_CONTEXT.md          (全局项目上下文)
```

**Rule**: Replace extension with `.md`, preserve full path.

**Why `.ai/`**: Hidden directory, unique name, won't conflict with `docs/`.

---

## HARD-GATE — Non-Negotiable

<HARD-GATE>

### For ANY code operation (create, modify, delete, fix bug, refactor):

**STEP 1: Ensure .ai/ infrastructure**
```
if .ai/ directory does NOT exist:
  → Create .ai/
  → Create .ai/PROJECT_CONTEXT.md (from template)
  → Log: "Created .ai/ infrastructure"
```

**STEP 2: Ensure file-specific doc exists**
```
Code file: src/components/Button.tsx
Doc file:  .ai/src/components/Button.md

if doc file does NOT exist:
  → Create it FIRST (before ANY code work)
  → Fill with template
  → Document current state (if existing file) or planned design (if new)
  → Present to user
```

**STEP 3: Read and understand**
- Read the doc completely
- Note Purpose, Constraints, Dependencies
- Note "NEVER Change" items

**STEP 4: Update doc with plan**
- Document WHAT you're about to do
- Document WHY
- For bug fixes: document bug cause and fix strategy
- Add to Change History

**STEP 5: Present to user and get confirmation**
```
"I'm about to [action] [file]. Here's the updated documentation: [summary]

Confirm to proceed?"
```

**STEP 6: Execute code work**

**STEP 7: Sync docs**
- Update doc to match final code
- Update Change History

**NO EXCEPTIONS. EVER.**

</HARD-GATE>

---

## Initialization Modes

### Mode 1: Progressive (Default) — For Large Codebases

**Use when**: Large codebase, or don't want to process all files at once

**How it works**:
```
User: 帮我改 Button.tsx
    ↓
AI: 检查 .ai/ 存在？
    ↓ NO → 创建 .ai/ + PROJECT_CONTEXT.md
    ↓
AI: 检查 .ai/src/components/Button.md 存在？
    ↓ NO → 只创建 Button.md（不碰其他文件）
    ↓
AI: 继续正常流程
```

**Result**: 用哪个文件，就给哪个文件创建文档。未使用的文件无文档开销。

### Mode 2: Full Initialization — User Triggered

**Use when**: You want to create docs for ALL files at once

**Trigger phrases**:
- "给我初始化所有 AI 文档"
- "给整个项目创建 .ai/ 文档"
- "全量初始化 File Guardian"
- "给所有文件创建文档镜像"
- "初始化所有文档"
- "批量创建 AI 文档"

**How it works**:
```
User: 给我初始化所有 AI 文档

AI:
"我将为整个项目创建 .ai/ 文档镜像。

Detected:
- Total code files: ~500
- Estimated Token usage: High (analyzing each file and generating docs)
- Estimated time: May take several minutes

I'll process in batches (20 files per batch) to avoid timeout.

Confirm full initialization?"

User: 确认

AI:
"[Batch 1/25] Creating .ai/src/components/*.md... ✓
[Batch 2/25] Creating .ai/src/utils/*.md... ✓
...
[Batch 25/25] Creating complete ✓

All done!
- Created: 500 .md docs
- Location: .ai/ directory (mirrors code structure)
- You can now view AI docs for any file"
```

**Batch Processing for Large Projects**:
```
For 200k lines (~5000 files):
- Batch size: 20 files
- Delay between batches: 2 seconds
- Progress: Show batch number and current directory
- On timeout: Resume from last completed batch
```

---

## Critical Scenarios

### Scenario 1: New Feature (No Doc Exists) — MOST COMMON

```
User: 帮我做个价格计算器

AI:
"我来帮你做价格计算器。按照文档优先原则，我需要先创建设计文档。

[检查 .ai/ 存在，创建 .ai/src/components/PriceCalculator.md]

📄 **Design Doc: .ai/src/components/PriceCalculator.md**

# PriceCalculator

## Purpose（一句话人话）
这个是价格计算器，用户输入数量和单价，自动计算总价并显示折扣。

## What It Does
- 接收数量和单价输入
- 实时计算总价
- 应用折扣规则

## Public API
| Name | Type | Description |
|------|------|-------------|
| PriceCalculator | Component | 主组件 |

## Props
| Name | Type | Required | Description |
|------|------|----------|-------------|
| basePrice | number | Yes | 基础单价 |

## Dependencies
- Internal: src/utils/formatCurrency.ts

---

**Does this design match your expectation? Need any changes?**

User: 可以，但加上 quantity 参数

AI: 
"好的，我更新文档 [更新 Props 表格]
现在实现代码 [创建 PriceCalculator.tsx]
完成！文档和代码已同步。"
```

### Scenario 2: Bug Fix (Core Requirement)

```
User: Button 点击没反应，修一下

AI:
"我来修复 Button 的点击问题。按照文档优先原则，我需要先分析并记录修复方案。

🔍 **Problem Analysis**
[Read .ai/src/components/Button.md]
[Read src/components/Button.tsx]

📄 **Bug Fix Documentation Update**

Add to .ai/src/components/Button.md:

## Bug Fix: Click Not Responding

**Discovered**: 2024-01-15
**Description**: Button click doesn't trigger onClick callback
**Root Cause**: onClick bound to div instead of button element
**Fix Strategy**: Move onClick to button element
**Impact**: Only Button.tsx
**Test Plan**: Click button, verify callback triggered

---

**Confirm this fix plan, then I'll implement.**

User: 可以

AI:
"[Update docs]
[Implement fix]
[Verify]
Fix complete, docs synced."
```

**Key**: Bug fix = Document cause and plan → User confirm → Fix → Sync

### Scenario 3: Batch Operations (>3 files)

```
User: 帮我创建整个登录流程

AI:
"我来创建登录流程。Since multiple files involved, I use batch declaration mode:

📋 **Batch Design Doc**

Files involved:
1. **LoginForm.tsx** - Login form
   - Purpose: User inputs email and password
   - Props: onSubmit, loading, error
   
2. **useAuth.ts** - Auth hook
   - Purpose: Manage login state and token
   - Exports: useAuth, AuthProvider
   
3. **authService.ts** - API calls
   - Purpose: Call backend login API
   - Exports: login, logout, refreshToken

[Simplified design: Purpose + key API for each file]

**Confirm this overall design, then I'll:**
1. Create all docs
2. Show batch confirmation
3. Implement all code at once

Confirm this design direction?"

User: 可以，但 useAuth 应该叫 useAuthContext

AI:
"Received, updating design:
2. **useAuthContext.ts** - Auth hook
...

Now creating all docs and code."
[Batch create]
```

### Scenario 4: Delete/Rename

**Delete**:
```
User: 删除 Button 组件

AI:
"Confirm delete Button component?

⚠️ This will delete BOTH:
- src/components/Button.tsx
- .ai/src/components/Button.md

Confirm delete both files?"

User: 确认
AI: [Delete both files]
```

**Rename**:
```
User: 把 Button 改成 PrimaryButton

AI:
"Confirm rename?

Will execute:
- src/components/Button.tsx → PrimaryButton.tsx
- .ai/src/components/Button.md → PrimaryButton.md
- Update all import Button files
- Update doc references

Confirm?"
```

---

## Flow Diagram

```dot
digraph file_guardian {
  rankdir=TB;
  node [shape=box, fontname=sans-serif];

  start [label="AI wants to modify/create file" shape=oval];
  detect [label="Check .ai/ exists?"];
  no_ai [label="Create .ai/ + PROJECT_CONTEXT.md"];
  is_full [label="User requested\nfull init?"];
  batch [label="Batch process\nall files"];
  has_doc [label="Check .ai/{file}.md\nexists?"];
  create_doc [label="Create doc FIRST\n(before any code)"];
  read_doc [label="Read .ai/{file}.md"];
  update_doc [label="Update doc with\nplanned changes"];
  present [label="Present to user\nGet confirmation"];
  execute [label="Execute code work"];
  sync [label="Sync docs\nwith final code"];

  start -> detect;
  detect -> no_ai [label="No"];
  no_ai -> is_full;
  detect -> is_full [label="Yes"];
  is_full -> batch [label="Yes"];
  is_full -> has_doc [label="No"];
  batch -> sync;
  has_doc -> create_doc [label="No"];
  has_doc -> read_doc [label="Yes"];
  create_doc -> update_doc;
  read_doc -> update_doc;
  update_doc -> present;
  present -> execute [label="Confirmed"];
  present -> update_doc [label="Changes\nrequested"];
  execute -> sync;
}
```

---

## Documentation Format

### .ai/{path}/{file}.md Template

```markdown
# {FileName}

> Last updated: {DATE} | Protection: {STANDARD|PROTECTED|CRITICAL}

## Purpose（一句话人话）

{用人类语言描述，例如：
"这个是价格计算器，用户输入数量和单价，自动计算总价"}

## What It Does

{详细功能描述}

## Public API

| Name | Type | Description |
|------|------|-------------|
| {export} | {type} | {description} |

## Props / Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| {prop} | {type} | {Yes/No} | {description} |

## Dependencies

- **Internal:** `{./path}` — {why}
- **External:** `{package}` — {why}

## Important Notes / NEVER Change

- {约束1}
- {约束2}

## Bug Fixes

| Date | Bug | Cause | Fix |
|------|-----|-------|-----|
| {DATE} | {description} | {root cause} | {solution} |

## Change History

| Date | Change | Author |
|------|--------|--------|
| {DATE} | Created | {who} |
```

---

## Edge Cases (Blue Team Fixes)

See [rules/edge-cases.md](./rules/edge-cases.md) for detailed handling of:

1. **Missing .ai/** → Auto-create
2. **Batch operations** → Batch declaration mode
3. **Delete/Rename** → Sync both files
4. **Bug fixes** → Document cause + fix plan first
5. **Circular deps** → Depth limit 5
6. **User rejection** → Iterate and re-confirm
7. **External edits** → Warn on sync issues
8. **Large projects** → Lazy loading (on-demand docs)
9. **File boundaries** → Clear include/exclude list

---

## Why This Works

| Problem | Solution |
|---------|----------|
| AI hallucinates | Doc forces AI to think first |
| Code drift | Doc-code sync enforced |
| Bug fixes undocumented | Bug cause must be documented |
| User sees wrong result | User sees design before code |
| Maintainability | Human-readable constraints |

---

## Templates & Rules

- **File doc**: [templates/file-doc.md](./templates/file-doc.md)
- **Project context**: [templates/project-context.md](./templates/project-context.md)
- **Edge cases**: [rules/edge-cases.md](./rules/edge-cases.md)
- **Initialization**: [rules/initialization.md](./rules/initialization.md)
