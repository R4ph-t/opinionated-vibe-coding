---
name: ovc-file-split
description: Find oversized files in the codebase, recommend how to decompose them, and perform the refactoring. Identifies files with too many responsibilities, vague names, or excessive length and splits them into focused modules. Use when the user asks to break down large files, split a module, reduce file size, or decompose code. Also trigger on "file too big", "split this file", "break down files", "decompose module", "extract from file".
metadata:
  author: r4ph-t
  version: "1.0"
license: MIT
---

# OVC File Split

Scan the codebase for oversized files, recommend how to decompose them, then perform the refactoring. Large files are a common symptom of AI-assisted development, where agents add functionality to whichever file is already open rather than creating focused modules.

## What counts as too large

- **Over ~300 lines**: deserves scrutiny. Read the file and assess whether it has a single clear responsibility.
- **Over ~500 lines**: almost always needs splitting regardless of language.

Thresholds are role-aware. A 400-line test file exercising one module may be fine. A 400-line route handler mixing validation, business logic, and response formatting is not.

## Signs a file needs splitting

- **Multiple responsibilities**: route definitions + business logic + validation + helpers in one file.
- **Vague filename**: `utils.ts`, `helpers.py`, `misc.go`, `common.rb` -- if you can't name it precisely, it bundles unrelated things.
- **Excessive region comments**: `// --- Auth ---`, `# ===== Validation =====` -- these are section boundaries begging to become files.
- **Scroll depth**: if you need to search to navigate the file, it's too long.

## Phase 1: Analysis

### 1. Scan the project tree

Identify files above the threshold. Exclude vendored code, generated files, lock files, and test fixtures.

### 2. Read each candidate

For each oversized file, identify:
- Distinct responsibilities or logical sections
- Which functions/classes/types belong together
- What the natural module boundaries are

### 3. Produce a split report

For each candidate file, output:

```
### [file path] ([line count] lines)

**Responsibilities found:**
1. [Responsibility A] (lines X-Y)
2. [Responsibility B] (lines X-Y)
3. [Responsibility C] (lines X-Y)

**Proposed split:**
- `[new-file-a]` ← [what moves here]
- `[new-file-b]` ← [what moves here]
- `[original-file]` ← [what stays, now a thin orchestrator or re-exports]

**New import graph:**
[Which new files import from which, and how the original file's exports change]
```

Present the full report and wait for the user to approve, adjust, or reject each proposed split before proceeding.

## Phase 2: Refactoring

For each approved split:

### 1. Create the new files

Move the extracted code into the new target files. Add the necessary imports to each new file.

### 2. Update the original file

Replace extracted code with imports from the new modules. If the original file served as a public API surface, keep re-exports so that external consumers are unaffected.

### 3. Update all importers

Find every file that imports symbols from the original file. Update import paths to point to the new locations where needed. If the original file re-exports everything, importers may not need changes -- but verify.

### 4. Verify

- Run the linter and type checker to confirm nothing broke.
- Run existing tests if available.
- If anything fails, fix it before moving to the next file.

### 5. Commit

Suggest a commit after each logical split using Conventional Commits format:

```
refactor: extract [responsibility] from [original file] into [new file(s)]
```

## Guard rails

- **No API changes**: never rename, remove, or change the signature of public exports during a split. Only move code.
- **Preserve tests**: do not modify test assertions. Update import paths only.
- **No circular dependencies**: if the proposed split would create a circular import, flag it and suggest an alternative decomposition before proceeding.
- **Prefer fewer files**: when in doubt, prefer fewer, slightly larger files over many tiny ones. The goal is focused modules, not one-function-per-file.
- **Respect existing patterns**: if the codebase already has a convention for file organization (e.g., `controllers/`, `services/`, `models/`), place new files accordingly rather than inventing a new structure.
