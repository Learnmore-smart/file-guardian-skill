# Edge Cases & Blue Team Fixes

## 1. Missing .ai/ Directory (CRITICAL)

**Problem**: `.ai/` directory doesn't exist

**Fix**:
```
Check if .ai/ exists?
  ↓ NO
Create .ai/ directory
Create .ai/PROJECT_CONTEXT.md (from template)
Continue normal flow
```

**Enforcement**: AI must auto-create directory structure, cannot give up.

---

## 2. Batch Operations (CRITICAL)

**Problem**: User requests multiple related files

**Fix - Batch Declaration Mode**:

```
User: 帮我创建整个登录流程

AI: 
"I'll create the login flow. Since multiple files involved, I use batch declaration mode:

📋 **Batch Design Doc**

Files involved:
1. LoginForm.tsx - Login form component
2. useAuth.ts - Auth logic hook
3. authService.ts - API calls
4. authUtils.ts - Utility functions

[Provide simplified Purpose + API design for each]

Confirm this overall design, then I'll:
1. Create all docs
2. Batch show for your confirmation
3. Implement all code at once"
```

**Rule**: 
- Single request involving >3 files → Use batch mode
- Confirm overall design first, then detail each file

---

## 3. Delete/Rename (CRITICAL)

**Problem**: Code changed but .ai/ docs not synced

**Fix - Sync Detection**:

```
Before each operation:
1. Check code file exists but doc doesn't → Create doc
2. Check code file doesn't exist but doc does → Ask to delete doc
3. Check code file mtime > doc mtime → Warn possible desync
```

**Delete Flow**:
```
User: 删除 Button 组件

AI:
"Confirm delete Button component?

⚠️ This will delete BOTH:
- src/components/Button.tsx
- .ai/src/components/Button.md

Confirm delete both files?"
```

**Rename Flow**:
```
User: Rename Button to PrimaryButton

AI:
"Confirm rename?

Will execute:
- src/components/Button.tsx → PrimaryButton.tsx
- .ai/src/components/Button.md → PrimaryButton.md
- Update all import Button files
- Update doc dependencies

Confirm?"
```

---

## 4. Bug Fix Scenario (CRITICAL - Core Requirement)

**Problem**: AI directly changes code when fixing bugs

**Fix - Bug Fix Documentation First**:

```
User: Button click not working, fix it

AI:
"I'll fix the Button click issue. Following documentation-first principle, I need to analyze and record the fix plan first.

🔍 **Problem Analysis**
[Read .ai/src/components/Button.md]
[Read src/components/Button.tsx]

📄 **Bug Fix Documentation Update**

Add to .ai/src/components/Button.md:

## Bug Fix: Click Not Responding

**Discovered**: {DATE}
**Description**: Button click doesn't trigger onClick callback
**Root Cause**: {analyzed cause}
**Fix Strategy**: {specific fix steps}
**Impact**: {affected files}
**Test Plan**: {how to verify}

---

**Confirm this fix plan, then I'll implement.**

User: 可以

AI:
"[Update docs]
[Implement fix]
[Verify]
Fix complete, docs synced."
```

**Enforcement**: 
- Bug fix = Document cause and plan → User confirm → Code fix → Sync
- Must record bug cause in doc to prevent recurrence

---

## 5. Circular Dependencies (MEDIUM)

**Problem**: A→B→C→A, reading docs infinite loop

**Fix - Dependency Depth Limit**:

```
When reading docs, maintain visited stack:
- Visited files list
- Max depth: 5 levels
- Detect cycle → Log warning, stop deeper

Example:
Reading Button.md → depends on useTheme
Reading useTheme.md → depends on ThemeProvider
Reading ThemeProvider.md → depends on Button (cycle!)
→ Stop, log: "Circular dependency detected: Button ↔ ThemeProvider"
```

---

## 6. User Rejection (MEDIUM)

**Problem**: User says "不对" (not right)

**Fix - Iteration Flow**:

```
AI: Present doc
User: 不对，不是这样做的

AI:
"Understood. Please tell me:
1. What's wrong? (Purpose / API / Dependencies / Other)
2. How should it be?"

User: Should use callback instead of return value

AI:
"Received, I update doc:"
[Update doc]
"Now correct?"

User: 对
AI: [Implement code]
```

**Rule**: Rejection → Ask specific issue → Update → Re-confirm

---

## 7. External Modifications (MEDIUM)

**Problem**: User edited code in VSCode without updating .ai/

**Fix - Sync Warning**:

```
When AI reads doc:
Check code file mtime > doc file mtime?
  ↓ YES
Warn user:
"⚠️ Detected code file newer than doc (possible external edit)

Suggested actions:
1. I'll read latest code and update doc
2. Then continue current task

Proceed?"
```

---

## 8. Large Projects (MEDIUM)

**Problem**: 1000+ files, initialization too slow

**Fix - Lazy Loading Mode**:

```
Don't scan all files at once!

On initialization:
- Only create PROJECT_CONTEXT.md
- Create file docs on-demand

When need to operate a file:
- Check if it has doc
- No → Create
- Yes → Read

Progressive improvement, not one-time scan
```

---

## 9. Non-Code File Boundaries (LOW)

**Problem**: JSON/YAML/config files need docs?

**Fix - Clear Boundaries**:

**Need docs**:
- .js, .ts, .jsx, .tsx
- .vue, .svelte
- .css, .scss, .less
- .py, .go, .rs, .java

**Don't need docs**:
- package.json, tsconfig.json (config)
- .md files themselves
- Test data files
- Static assets

**Optional**:
- Complex config files can have docs if manually added
