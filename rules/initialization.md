# Initialization Guide

## Directory Name: `.ai/`

**Why `.ai/`:**
- `.` prefix = hidden directory (won't clutter file explorer)
- `ai` = clearly indicates AI-related documentation
- Unique = won't conflict with common names like `docs/`, `doc/`, `documentation/`
- Short = easy to type

**Structure:**
```
Code: src/components/Button.tsx
Doc:  .ai/src/components/Button.md

Code: src/utils/format.ts
Doc:  .ai/src/utils/format.md
```

---

## Two Initialization Modes

### Mode 1: Progressive (Default)

**Trigger**: Any code request

**Behavior**:
- Only create docs for files being used
- Other files remain without docs
- Zero overhead for unused files

**Example:**
```
User: 帮我改 Button.tsx
AI: Create .ai/src/components/Button.md (only this one)
```

**Best for**: Large codebases (200k+ lines), or when you want minimal initial setup.

---

### Mode 2: Full Initialization

**Trigger phrases** (say any of these):
- "给我初始化所有 AI 文档"
- "给整个项目创建 .ai/ 文档"
- "全量初始化 File Guardian"
- "给所有文件创建文档镜像"
- "初始化所有文档"
- "批量创建 AI 文档"

**Behavior**:
- Scan entire codebase
- Create .md for ALL code files
- Batch processing to avoid timeout

**Process for large projects:**
```
1. Scan and count files
2. Show estimate: "~500 files, high token usage"
3. Get user confirmation
4. Process in batches (20 files per batch)
5. Show progress: "[12/25] Creating docs for utils/..."
6. Complete summary
```

**Best for**: When you want complete documentation upfront, and are willing to pay the token cost.

---

## First Time Usage Guide

### Scenario A: Large Project (200k lines), Want Progressive

```
You: 帮我修复 src/components/Button.tsx 的点击问题

AI:
"I'll fix Button. Detected first-time use of File Guardian:

[Create .ai/ directory]
[Create .ai/PROJECT_CONTEXT.md]
[Create .ai/src/components/Button.md]

✓ Infrastructure created (Button only)

Now reading Button doc and analyzing fix plan..."
```

**Result**: Only Button has doc. Other files get docs when used.

---

### Scenario B: Large Project, Want Full Init

```
You: 给我初始化所有 AI 文档

AI:
"I'll create .ai/ docs for entire project.

Scan result:
- Total code files: 482
- Estimated Token usage: High
- Batches: 25 (20 files per batch)

This will consume significant Tokens, but then all files have complete docs.

Confirm full initialization?"

You: 确认

AI:
"[Batch 1/25] Processing src/components/... ✓
[Batch 2/25] Processing src/utils/... ✓
...
[Batch 25/25] Complete ✓

🎉 Full initialization complete!
- Created: 482 AI docs
- Location: .ai/ directory
- All code files now have corresponding .md docs"
```

**Result**: Every file has its doc. Higher initial cost, zero ongoing overhead.

---

### Scenario C: Small Project, Want Full Init

```
You: Create all docs for this project

AI:
"Scan complete: 32 code files.
Starting AI doc creation..."

[Create all 32 docs at once]

"✓ All complete!"
```

---

## Daily Workflow Examples

### Modify Code

```
You: Add loading state to Button

AI:
"[Read .ai/src/components/Button.md]
[Update doc: add loading prop]

📄 Doc update:
- Props added: loading?: boolean
- Description: Show loading state, disable click

Confirm then implement code changes."

You: 确认

AI: [Modify Button.tsx] [Sync doc]
```

### Fix Bug

```
You: Button click not working

AI:
"[Read .ai/src/components/Button.md to understand past agent thoughts, change history, and bug fixes]
[Analyze code]

📄 Bug Fix record:
Problem: onClick not properly bound
Cause: Event handler on div instead of button
Fix: Move onClick to button element
Test: Click should trigger callback

Confirm this fix plan?"

You: 确认

AI: [Implement fix] [Update doc Bug Fixes table]
```

### Create New File

```
You: Create price calculator

AI:
"I'll create design doc first:"

[Create .ai/src/components/PriceCalculator.md]

📄 Design preview:
Purpose: Price calculator, input quantity and price, auto-calculate total
Props: basePrice, quantity, discount?
Dependencies: formatCurrency util

Confirm this design?"

You: Add onCalculate callback

AI: [Update doc] [Create PriceCalculator.tsx] [Sync]
```

---

## Checking Documentation Status

```
You: Which files have AI docs?

AI:
"Documentation coverage:

Documented (12):
- src/components/Button.md
- src/components/Card.md
- ...

Not documented (45):
- src/utils/validate.ts
- src/features/auth/Login.tsx
- ...

Suggestion: Create docs for src/features/ files?"
```

---

## Migration from docs/ to .ai/

If you previously used `docs/`:

```
You: I used docs/ before, now want .ai/

AI:
"I'll help you migrate:

1. Create .ai/ directory
2. Copy docs/src/**/* to .ai/src/**/*
3. Update PROJECT_CONTEXT.md path references
4. Optional: Delete old docs/ directory

Confirm migration?"
```
