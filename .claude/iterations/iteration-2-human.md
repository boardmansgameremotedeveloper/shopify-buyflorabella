# iteration-2-human.md — Hydrogen Decomposition & Shopify Mapping

## Objective

Analyze the existing **Hydrogen frontend implementation** and prepare a structured plan to replicate its **layout, components, and user experience** inside the Shopify Dawn theme using Liquid.

This is a **read + analyze + plan iteration**.
No destructive changes should be made to the Shopify theme yet.

---

## Source Codebase (Hydrogen)

Location:

```
/var/www/html/traceminerals_boardmansgame_com/frontend/hydrogen-frontend-v7/
```

This is a **React / Hydrogen storefront** that represents the desired final UX.

---

## Target Codebase (Shopify Theme)

* Dawn-based Shopify theme (current repository)
* Liquid + JSON templates + sections/snippets

---

## Tasks

### 1. Scan Hydrogen Codebase

Claude must recursively examine:

* `/routes`
* `/components`
* `/layouts`
* `/lib` (if present)
* any custom UI or utility folders

For each file:

* Identify purpose
* Identify whether it maps to:

  * page
  * layout
  * component
  * utility logic

---

### 2. Decompose Application Structure

Claude should extract:

#### A. Page-Level Structure

* List all routes/pages
* Identify:

  * homepage
  * product pages
  * collection pages
  * custom pages (marketing, landing, etc.)

#### B. Layout Patterns

* Global layout wrapper(s)
* Header / navigation structure
* Footer structure
* Shared layout containers

#### C. Component Inventory

Create a categorized list of components:

* Navigation components
* Product display components
* Marketing/hero sections
* Content blocks
* Interactive elements

---

### 3. Map Hydrogen → Shopify Architecture

For each identified element, determine:

| Hydrogen Concept | Shopify Equivalent               |
| ---------------- | -------------------------------- |
| Route            | Template (`templates/*.json`)    |
| Layout           | `layout/theme.liquid` + sections |
| Component        | Section or snippet               |
| Reusable UI      | Snippet                          |
| Page sections    | Section (`sections/*.liquid`)    |

---

### 4. Identify Gaps / Constraints

Claude must explicitly call out:

* Features that **cannot be directly implemented in Liquid**
* React-specific logic that requires simplification
* Client-side interactivity that may require:

  * minimal JS
  * or removal

---

### 5. Define Required Shopify Additions

Claude must produce a list of:

#### New Sections to Create

Examples:

* hero-banner.liquid
* product-feature-grid.liquid
* marketing-callout.liquid
* custom-navigation.liquid

#### New Templates

* custom landing pages
* marketing pages
* any route equivalents from Hydrogen

#### New Snippets

* reusable UI fragments
* cards, buttons, badges, etc.

---

### 6. Output Deliverable

Claude must create:

```
.claude/iterations/iteration-1-claude-to-execute.md
```

This document must include:

### A. System Overview

* High-level architecture of Hydrogen app

### B. Page Mapping

* Hydrogen routes → Shopify templates

### C. Component Mapping

* Hydrogen components → Shopify sections/snippets

### D. Section Build Plan

* List of sections to create
* Purpose of each
* Where used

### E. Execution Plan (for Iteration #2)

Step-by-step instructions:

1. Which files to create first
2. Which templates to modify
3. Order of implementation

---

## Rules

* DO NOT modify Shopify theme files yet
* DO NOT create `.liquid` files yet
* This is strictly analysis + planning
* Be explicit and structured—no vague summaries

---

## Success Criteria

Iteration is complete when:

* Hydrogen app is fully decomposed
* Shopify mapping is clearly defined
* Section/template/snippet plan exists
* Execution plan is ready for `go` in next iteration

---

## Trigger

After completing analysis:

* Write `iteration-1-claude-to-execute.md`
* Await the command: **"go"**
