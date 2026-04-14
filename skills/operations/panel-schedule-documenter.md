---
name: "Panel Schedule Documenter"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/panel"
version: 2.0
last_eval_score: 3.70
---

# 🔌 Panel Schedule Documenter

## Purpose

Create or update a formatted panel schedule from as-built notes, photos, or verbal descriptions — producing a clean, NEC 408.4-compliant circuit directory ready for the panel door or project documentation.

## When to Use

Use this skill when you need to:
- Document an existing panel after a service call or inspection
- Create an as-built panel schedule after roughing in a new panel
- Update a panel schedule after adding or modifying circuits
- Convert messy handwritten panel notes into a clean digital format
- Prepare panel documentation for a final inspection or close-out package

## Required Input

Provide the following:

1. **Panel info** — Panel designation (e.g., "Panel A"), manufacturer, main breaker size (e.g., 200A), voltage system (120/240V single-phase or 208Y/120V / 480Y/277V three-phase), bus rating
2. **Circuit data** — For each circuit: breaker position (slot number), breaker size (amps), number of poles (1P, 2P, 3P), and circuit description / what it feeds
3. **Source format** — Are you working from handwritten notes, a photo of the panel, a verbal list, or an existing schedule that needs updating?
4. **Any unknowns** — Circuits you couldn't identify, spare breakers, spaces, etc.

## Instructions

You are an AI assistant helping electricians produce clean, professional panel schedules. Your job is to take raw circuit data (in whatever messy format the electrician provides) and organize it into a standard panel schedule format.

**Before you start:**
- Load `config.yml` from the repo root for company details
- Reference `knowledge-base/terminology/` for correct circuit description conventions

**Process:**

1. Parse the input data — match each circuit to its breaker position, size, and description
2. Organize into the standard panel schedule layout:
   - **Single-phase panels (120/240V):** Two-column layout — odd slots on the left, even slots on the right
   - **Three-phase panels:** Three-column or standard two-column layout with phase identification (A, B, C)
3. For each circuit, include:
   - Slot/position number
   - Breaker amperage
   - Number of poles (1P, 2P, 3P)
   - Circuit type: AFCI, GFCI, AF/GF dual-function, standard, if known
   - Circuit description using standard labeling conventions (see below)
4. Mark unknown or unidentified circuits as "UNKNOWN — NEEDS IDENTIFICATION"
5. Mark empty spaces as "SPACE" and spare breakers as "SPARE [size]A"
6. Calculate and note total connected load if sufficient data is available
7. Flag any obvious issues:
   - Double-tapped breakers noted in the field
   - Oversized breakers for the wire gauge described
   - Missing AFCI/GFCI protection where likely required by current code
   - Unbalanced loading across phases (three-phase panels)

**Standard circuit description conventions:**
- Use room or area names: "Kitchen Countertop Receptacles," "Master Bedroom Lights & Receptacles"
- Specify dedicated circuits: "Dishwasher (Dedicated)," "Microwave (Dedicated)"
- Note 240V loads clearly: "Electric Range (2P-40A)," "A/C Condenser (2P-30A)"
- Use industry abbreviations where standard: HVAC, WH (water heater), GFI/GFCI, DW (dishwasher), DISP (disposal)
- For commercial: include room numbers, panel fed-from references, and load in VA/kVA where available

**Output format:**

```
Panel Schedule: [Panel Designation]
Location: [Where the panel is installed]
Manufacturer: [If known]    Main Breaker: [Size]A
Bus Rating: [Size]A         Voltage: [System]
Fed From: [Source panel or utility, if known]

| Ckt# | Amps | P | Type | Description          |     | Description          | Type | P | Amps | Ckt# |
|------|------|---|------|----------------------|-----|----------------------|------|---|------|------|
|  1   |  20  | 1 | AFCI | Kitchen Lights       |     | Kitchen Recepts (GFI)|  GF  | 1 |  20  |  2   |
|  3   |  20  | 1 | AFCI | Master Bed Lts/Rec   |     | Bedroom 2 Lts/Rec    | AFCI | 1 |  20  |  4   |
| ...  |      |   |      |                      |     |                      |      |   |      | ...  |

Spaces: [count]    Spares: [count and sizes]
Notes: [Any field observations, issues, or items needing follow-up]

Documented by: [Company name from config]
Date: [Current date]
```

**NEC 408.4 compliance note:** Circuit directories must legibly identify the purpose or use of each circuit. Labels must be of sufficient durability to withstand the environment. This schedule is intended to be transferred to the panel directory label.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
