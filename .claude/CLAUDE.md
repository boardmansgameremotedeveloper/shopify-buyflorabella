# CLAUDE.md — Buy Flora Bella (Shopify Theme)

> **Authoritative source.**
> This document defines how Claude should operate within this repository.
> After editing this file, sync if needed across environments.

---

## Identity Labels

Identity labels are case-insensitive.

HUMAN OPERATOR: DXB
AI PLATFORM: CLAUDE
AI AGENT: CLAUDE, HENRY

## Project Overview

**shopify-buyflorabella** is a Shopify storefront built on the **Dawn theme**.

This repository contains:
- A **native Shopify theme (Liquid-based)** derived from Dawn
- Future customizations to support a **soil science / product-focused storefront**
- Planned **design and structural parity with an existing Hydrogen implementation**

---

## Primary Objectives (Completed)

### Phase 1 — Repository Understanding (Iteration #1)
Claude must:

1. **Scan the entire filesystem**
2. Identify and classify:
   - Shopify theme structure
   - Liquid templates, sections, snippets, assets
3. Build a **cache manifest** for tracked files
4. Establish a working mental model of:
   - Page structure
   - Layout hierarchy
   - Section composition

---

### Phase 2 — Theme Customization (Current effort)

Claude will:
- Create and modify `.liquid` files
- Add **custom sections**
- Extend templates beyond default Dawn capabilities
- Introduce reusable UI components

---

### Phase 3 — Hydrogen Parity Migration (Simultaneous Effort)

There exists a **separate Hydrogen codebase** which:
- Defines desired UX/UI and page structure

Claude will:
1. Analyze that external codebase
2. Extract:
   - Layout patterns
   - Component structure
   - Navigation model
3. Translate into Shopify-compatible constructs:
   - Templates (`templates/*.json`)
   - Sections (`sections/*.liquid`)
   - Snippets (`snippets/*.liquid`)

⚠️ Constraint:
Shopify themes **cannot replicate Hydrogen routing directly**

Therefore:
- New pages must be created via Shopify templates
- Logic must be adapted to Liquid limitations

---

## Stack

- Shopify Theme (Dawn base)
- Liquid templating
- JSON templates
- CSS / JS assets (Shopify standard pipeline)

---

## Key File Map (Expected)

Claude should verify and populate this mentally during scan:


/layout
/templates
/sections
/snippets
/assets
/config
/locales


---

## Rules

### General
- Do NOT assume React or Hydrogen runtime exists here
- Do NOT introduce unsupported Shopify features
- All work must remain **Shopify theme-compatible**

### Schema Name Character Limit

All Shopify section schema `"name"` values (and preset `"name"` values) **must be 25 characters or fewer**. This is a Shopify theme editor constraint. Count carefully before saving — names that exceed 25 chars will be truncated or rejected by Shopify.

---

### BirdLabs Custom Section Naming (legacy)

Custom sections created for this project (not Dawn originals) followed this convention:

- Be **prefixed with `birdlabs-`** in the filename (e.g. `birdlabs-header.liquid`, `birdlabs-announcement-bar.liquid`)
- Have their schema `"name"` field set to `"BirdLabs — <Section Name>"` so they appear clearly labeled in the Shopify theme editor (e.g. `"BirdLabs — Header"`)
- Use `"BirdLabs — <Section Name>"` as the preset `"name"` as well

This convention ensures BirdLabs custom sections are immediately distinguishable from Dawn base sections in both the codebase and the Shopify theme editor UI.

### File Handling
- Prefer modifying existing Dawn patterns rather than replacing wholesale
- Reuse snippets and sections when possible
- Keep logic simple—Liquid is limited

### Architecture
- Sections = primary building blocks
- Templates = page structure
- Snippets = reusable fragments

---

---

### BFBella Custom Section Naming (current)

Moving forward directive: custom sections created for this project (not Dawn originals) must:

- Be **prefixed with `bfbella-`** in the filename (e.g. `bfbella-header.liquid`, `bibfbella-announcement-bar.liquid`)
- Have their schema `"name"` field set to `"BFBella — <Section Name>"` so they appear clearly labeled in the Shopify theme editor (e.g. `"BFBella — Header"`)
- Use `"BFBella — <Section Name>"` as the preset `"name"` as well

This convention ensures BFBella custom sections are immediately distinguishable from Dawn base sections in both the codebase and the Shopify theme editor UI.

### File Handling
- Prefer modifying existing Dawn patterns rather than replacing wholesale
- Reuse snippets and sections when possible
- Keep logic simple—Liquid is limited

### Architecture
- Sections = primary building blocks
- Templates = page structure
- Snippets = reusable fragments


## Cache System

File: `.claude/cache/manifest.json`

### Initial Requirement (Iteration #1)

Claude must:

1. Walk the entire repository
2. For each relevant file:
   - Record:
     - path
     - mtime
     - size
3. Store in manifest

### Usage Rules

Before re-reading any file:
1. Run:

stat -c '%Y %s' <file>

2. Compare with manifest
3. If unchanged → use cached knowledge
4. If changed → re-read and update manifest

---

## User-Diff Protocol

Trigger words for manual file changes:

| Trigger | Behavior |
|--------|--------|
| `re-scan` | Re-check all tracked files, update cache, report changes |
| `re-scan <file>` | Re-check specific file(s), update cache |

On trigger:
- Always re-read changed files
- Report structural differences
- Confirm before proceeding

---

## Design Reference

Primary design intent will come from:


.claude/design.md


AND

External Hydrogen codebase (see design.md)

---

## Iteration Pattern

| File | Owner | Purpose |
|------|-------|---------|
| `claude_docs/tasks/TASK_TEMPLATE.md` | Reference only | Blank task template + state machine + naming conventions — documentation, never processed |
| `claude_docs/tasks/TASK_outcome.md` | Claude writes | Latest task result (overwritten each time) |
| `claude_docs/tasks/TASK_plan.md` | Claude writes | Pre-execution plan for current task |
| `claude_docs/DEVLOG.md` | Claude appends | Append-only session history — global log across all task types, stays at root |
| `claude_docs/DIAGNOSIS.md` | Claude writes | Active bug scratchpad — overwritten per issue, stays at root |
| `.claude/CLAUDE.md` | Both update | Project instructions (this file) — Claude may edit directly |
| `claude_docs/self_improve/LESSONS.md` | Claude writes | Staged lessons → promoted to CLAUDE.md (see below) |
| `claude_docs/issue_fixer/` | Claude writes | Bug diagnoses and fix records (see below) |
| `claude_docs/build_docs/` | Both | Phase DDs, plans, and outcomes |
| `claude_docs/exec_logs/` | Claude writes | Per-session command log (see below) |


---

---

## Issue Fixer (`issue_fixer/`)

Bug reports and fixes that are distinct from planned feature work live here. Treated
identically to `build_docs/` — each issue gets its own file, committed alongside the fix.

**File naming:** `issue_NNN_short_description.md`

**File structure:**
```
# Issue NNN — <title>
**Status:** OPEN | FIXED | WONT_FIX
**Date:** YYYY-MM-DD
**Symptom:** <what the user observes>

## Root Cause
<diagnosis>

## Fix
<what was changed and why>

## Files changed
- list
```

**DIAGNOSIS.md** (in `claude_docs/`) is the *active* single-issue scratchpad — always
overwritten with the current symptom being investigated. Resolved findings are moved
to a permanent `issue_NNN_*.md` file in `issue_fixer/`. DIAGNOSIS.md clears when
the issue is closed.

---


## Self-Improvement Protocol

Claude is authorized to evolve these instructions. No permission needed. This is not
a special mode — it is the default expectation.

### `self_improve/LESSONS.md` — the staging journal

Located at `claude_docs/self_improve/LESSONS.md`. Claude writes here during or after a
session when it observes something that should become a permanent instruction but
hasn't been validated yet.

**Write a lesson when:**
- You catch a bug that a better instruction would have prevented
- You discover an approach that contradicts what's currently written here
- A tool, pattern, or assumption turned out to be wrong in this project's context
- A class of failure (like a cookie attribute bug) was invisible to the test suite

**Entry format:**
```
## YYYY-MM-DD — <short title>
**Observed:** what happened, with enough detail to understand it cold
**Should change:** what the instruction should say
**Target:** which file to edit (this CLAUDE.md, global ~/.claude/CLAUDE.md, settings.json)
**Status:** STAGED
```

### Promotion: STAGED → CLAUDE.md

Promote a lesson by editing the target file directly and updating the lesson's
`Status` to `PROMOTED`. Promote when:
- The lesson was validated by the current session's outcome, OR
- It comes up in a second session without contradiction

### Human veto

The human may edit or delete any entry in LESSONS.md. If an entry is deleted, do not
re-add it. If an entry is marked `REJECTED`, leave it and do not act on it.

### Editing this file

Claude may edit `.claude/CLAUDE.md` directly. When doing so:
1. Make the change
2. Note it in `claude_docs/DEVLOG.md` (one line: what changed and why)
3. Commit it alongside the work that prompted the change — not as a separate commit

The goal is instructions that reflect how this project actually works, not how it was
imagined to work at setup time.

---

## Session Execution Log

During any `IN_PROGRESS` execution, Claude maintains a lightweight command log in `claude_docs/exec_logs/`.

**Log file naming:** `YYYYMMDD_HHMM.log` — one file per execution session, named from the timestamp when IN_PROGRESS begins.

**Entry format:**
```
[HH:MM:SS] SESSION_START — <task description>
[HH:MM:SS] CMD: <exact bash command>
[HH:MM:SS] DONE: <brief description of what completed>
[HH:MM:SS] ERROR: <what failed and why>
[HH:MM:SS] SESSION_END — <summary>
```

**Mechanics:** Prepend the log write to each Bash call so both happen in one tool invocation:
```bash
echo "[$(date '+%H:%M:%S')] CMD: actual_command" >> claude_docs/exec_logs/YYYYMMDD_HHMM.log && actual_command
```

**Rules:**
- Create `claude_docs/exec_logs/` if it does not exist.
- Write a `SESSION_START` entry before the first command of any `IN_PROGRESS` task; write `SESSION_END` after the last.
- Log file name is set once at session start — do not rename mid-session.
- Log writes are non-blocking: if the append fails, note the failure and continue.
- `claude_docs/exec_logs/` is gitignored — logs are local only.
- **Writes to `claude_docs/exec_logs/` are pre-authorized — always proceed without prompting for user confirmation.**
  (`.claude/settings.json` includes `claude_docs/exec_logs` in `additionalDirectories` and a Bash allow pattern for appends.)

**Purpose:** If a session is interrupted, the developer opens the latest log to see exactly what ran and decides whether to resume or abort — without reading a full transcript.

---

## Conversation History

**THIS IS NON-NEGOTIABLE. DO THIS FIRST. BEFORE ANY OTHER TOOL CALL.**

`claude_docs/semi_cache/ad_hoc_conversation.md` is a bash-history-style log of every user
message in this project. It is **never cleared** and **never compacted**.

**Format:**
```
[YYYY-MM-DD HH:MM] <user message verbatim>
```

Newest entries at the top. Oldest entries at the bottom.

**Rule — MANDATORY FIRST ACTION:** The very first thing Claude does upon receiving any user
message is to prepend it to `ad_hoc_conversation.md`. Not after research. Not after reading
files. FIRST. Use `sed -i` to prepend the timestamped message. This must happen even if
Claude is in the middle of another task, even after context compaction, even if the message
seems trivial.

```bash
# Prepend a new entry — run this AS THE FIRST BASH CALL of every response:
sed -i "0,/^---$/s|^---$|---\n\n[$(date '+%Y-%m-%d %H:%M')] <message verbatim here>|" \
  /var/www/html/pulsecomposer/dev/claude_docs/semi_cache/ad_hoc_conversation.md
```

Or use the Edit tool to insert at the top of the entries (after the `---` separator line).

**After context compaction:** The log will be missing entries from the compacted session.
Before resuming work, read the session summary and backfill any missing messages at the
correct timestamp positions (best-estimate times are acceptable).

**Writes to `claude_docs/semi_cache/` are pre-authorized — always proceed without prompting.**

**Workflow changes** — when Claude methodology changes in a session, append an entry to
`claude_docs/workflow_changes/BACKPORT_GUIDE.md` so sibling projects can catch up.

---

## Iteration Lifecycle — How Claude and Human Work Together

Refer to TASK_TEMPLATE.md as the authoritative workflow.

The goal is tight loops: short bursts of work, visible output, human decision, repeat.
Claude should never disappear for 10 minutes and reappear with a wall of changes.

### Before acting

For any task that touches more than two files or deploys to production, Claude states
**one sentence** before starting:

> "I'm going to fix X in file Y, then run tests, then deploy — tell me if you want
> a different scope."

This is not a plan document. It is a spoken sentence. The human can redirect or confirm
with one word.

### During execution

Claude gives **one-line progress updates** at meaningful transitions:
- "Tests passing, about to deploy."
- "Deploy failed at step 4 — investigating."
- "Found the root cause: [one sentence]. Fixing now."

No paragraphs. No recaps of what was just done. The human can see the tool calls.

### After completing

Claude ends with **what changed and what the human should check**:
- "Deployed. Run `tc_0008` in a browser tab to confirm login works end-to-end."
- "Fixed. The cookie is now Secure=True in prod. Check DevTools → Application → Cookies."

Not a summary of everything Claude did. The one thing the human needs to do next.

### When to stop and ask

Claude stops and asks instead of acting when:
- The scope of a fix would change more than 3 files unexpectedly
- A test fails for a reason that isn't obvious from the error
- A deploy step would affect production state in a non-reversible way
- The instruction is ambiguous between two significantly different implementations

Claude does NOT stop and ask for permission to:
- Write to `claude_docs/` files (exec logs, LESSONS.md, DEVLOG.md, `tasks/TASK_outcome.md`)
- Run read-only commands (grep, find, cat, git log)
- Run dev tests against the dev environment
- Make and commit changes to the dev branch

### Acceptance test as the handoff signal

When Claude believes a feature is working, the handoff is **not** "I think it's working."
The handoff is **tc_0008 passing** (or the relevant test case for the feature).

> "tc_0008 passes 20/20 in production. The login path is verified. Browser check:
> open an incognito window, enter the site password, log in, press F5 — you should
> stay logged in."

If a test case does not exist for the feature, Claude writes one before claiming done.

### Escalation: when Claude is wrong

When Claude delivers something that doesn't work (browser fails, user reports regression):
1. Claude states the gap in **one sentence**: "The automated test passes but the browser
   fails — this is almost always a cookie attribute or CORS issue."
2. Claude proposes **one diagnostic step**: "Check DevTools → Application → Cookies →
   confirm Secure is checked."
3. Claude waits for the human to confirm before assuming the cause and implementing a fix.

Claude does not implement three speculative fixes hoping one of them works.

---

## Iteration Workflow [ DxB to deprecate these instructions august 2026 ]

Documents live in:

.claude/claude_docs/


### Cache Creation
- Build `.claude/cache/manifest.json`
- Store mtime + size for each tracked file

### Structural Summary
Produce a concise internal model of:
- How pages are composed
- Key entry points (theme.liquid, index.json, etc.)
- Reusable components

---

## Future Work Preparation

Claude should be ready to:

- Create new `.liquid` sections
- Modify JSON templates
- Reconstruct Hydrogen layouts using Shopify primitives
- Introduce new page templates where needed

---

## Working With This Codebase

This is a **Shopify-first system**, not a React app.

All implementations must:
- Respect Shopify constraints
- Be deployable via Shopify CLI
- Avoid overengineering beyond Liquid capabilities

---

## End State of Iteration #1

Claude is considered ready when:

- Cache manifest exists and is accurate
- Theme structure is fully understood
- No unnecessary file re-reads occur
- Ready to begin targeted modifications in Iteration #2