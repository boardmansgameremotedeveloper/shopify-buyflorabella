# CLAUDE.md — Buy Flora Bella (Shopify Theme)

> **Authoritative source.**
> This document defines how Claude should operate within this repository.
> After editing this file, sync if needed across environments.

---

## Project Overview

**Buy Flora Bella** is a Shopify storefront built on the **Dawn theme**.

This repository contains:
- A **native Shopify theme (Liquid-based)** derived from Dawn
- Future customizations to support a **soil science / product-focused storefront**
- Planned **design and structural parity with an existing Hydrogen implementation**

---

## Primary Objectives (Current Phase)

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

### Phase 2 — Theme Customization (Upcoming)

Claude will:
- Create and modify `.liquid` files
- Add **custom sections**
- Extend templates beyond default Dawn capabilities
- Introduce reusable UI components

---

### Phase 3 — Hydrogen Parity Migration (Planned)

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

### BirdLabs Custom Section Naming

All custom sections created for this project (not Dawn originals) must:

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

External Hydrogen codebase (to be introduced)

---

## Iteration Workflow

Documents live in:


.claude/iterations/


### Flow


iteration-N-human.md
↓
iteration-N-claude-questions.md (optional)
↓
iteration-N-claude-to-execute.md
↓
"go"
↓
[execution]
↓
iteration-N-outcome.md  ← REQUIRED after every iteration


### Outcome Document (Required)

After every iteration completes, Claude MUST create `.claude/iterations/iteration-N-outcome.md` documenting:
- What was done (file-by-file summary)
- Any decisions made or constraints discovered
- Known issues or follow-up items

---

### Trigger Phrases

| Phrase | Action |
|--------|--------|
| `read iteration-N-human.md` | Analyze requirements |
| `go` | Execute current plan |
| `prepare for iteration N` | Create new iteration stub |

---

## Iteration #1 — REQUIRED TASK

Claude must perform the following BEFORE any development work:

### 1. Filesystem Scan
- Enumerate all files
- Identify Shopify theme structure

### 2. Classification
Group files into:
- layout
- templates
- sections
- snippets
- assets
- config

### 3. Cache Creation
- Build `.claude/cache/manifest.json`
- Store mtime + size for each tracked file

### 4. Structural Summary
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