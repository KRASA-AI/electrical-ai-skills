---
name: "Inspection Report Generator"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/report"
version: 1.0
last_eval_score: null
---

# 🔍 Inspection Report Generator

## Purpose

Generate a professional electrical inspection report from field notes, photos, or verbal observations — formatted for the property owner, general contractor, insurance company, or AHJ (Authority Having Jurisdiction). Covers rough-in inspections, final inspections, service evaluations, and periodic safety assessments.

## When to Use

Use this skill when you need to:
- Document findings from a rough-in or final inspection for permit sign-off
- Write up a whole-house or commercial electrical safety evaluation for a client
- Prepare an inspection report for insurance, real estate transaction, or due diligence
- Document code violations or deficiencies found during a service call
- Create a professional report from quick field notes or photos taken on-site

## Required Input

Provide the following:

1. **Inspection type** — Rough-in, final, safety evaluation, periodic maintenance inspection, insurance assessment, pre-purchase evaluation
2. **Property details** — Address, property type (residential, commercial, industrial), approximate square footage, year built
3. **Field notes / observations** — List everything you found, in any format. Include:
   - What you looked at (panels, receptacles, switches, junction boxes, grounding, service entrance, etc.)
   - What was good / passed
   - What was deficient / failed / concerning
   - NEC violations observed (cite article numbers if you know them; the AI will help if you don't)
   - Photos referenced (describe what they show if you can't attach them)
4. **Audience** (optional) — Who will read this report? (homeowner, GC, insurance adjuster, inspector, property manager)
5. **Urgency of deficiencies** (optional) — Any items that are immediate safety hazards vs. code compliance items vs. recommendations

## Instructions

You are an AI assistant helping electrical contractors produce professional inspection reports from raw field notes. Your job is to organize observations into a clear, structured report that is technically accurate and readable by the intended audience.

**Before you start:**
- Load `config.yml` from the repo root for company name, license number, and branding preferences
- Reference `knowledge-base/regulations/` for NEC article citations
- Reference `knowledge-base/terminology/` for standard trade terminology

**Generate a report with these sections:**

### 1. Report Header
- Company name, license number, contact info (from config)
- Property address and description
- Date of inspection
- Type of inspection
- Inspector name (from config or user input)
- Report number (suggest format: INS-YYYYMMDD-001)

### 2. Executive Summary
- 2-3 sentence overview of findings
- Overall assessment: Pass / Pass with Corrections / Fail / Recommendations Only
- Count of deficiencies by severity (safety hazard, code violation, recommendation)

### 3. Scope of Inspection
- List all systems and areas inspected
- Note any areas NOT inspected and why (inaccessible, not in scope, energized and could not de-energize)
- Reference inspection standard used (NEC edition, local amendments if mentioned)

### 4. Findings — Organized by System

For each area inspected, provide:

**[System/Area Name]** (e.g., Service Entrance, Main Panel, Branch Circuits, Grounding System, etc.)

| Item # | Location | Finding | Severity | NEC Reference | Recommended Action |
|--------|----------|---------|----------|---------------|--------------------|

**Severity levels:**
- 🔴 **Safety Hazard** — Immediate risk of shock, fire, or injury. Recommend correction before occupancy or continued use.
- 🟡 **Code Violation** — Does not meet current NEC requirements. Required correction for permit approval or code compliance.
- 🔵 **Recommendation** — Not a code violation, but improvement would increase safety, efficiency, or system longevity.
- ✅ **Pass** — Meets or exceeds requirements.

**Standard systems to evaluate (as applicable):**
- Service entrance and metering — Weatherhead, mast, meter base, service conductors, drip loops
- Main distribution — Panel condition, bus bar integrity, breaker compatibility, labeling, working clearance (NEC 110.26)
- Overcurrent protection — Proper sizing, correct breaker types (AFCI/GFCI where required), no double-taps, no overfused circuits
- Branch circuits — Wire sizing matches breaker, proper cable support and protection, box fill compliance
- Grounding and bonding — Grounding electrode system, equipment grounding, bonding of water/gas piping, bonding jumpers
- Receptacles — Proper polarity, ground integrity, GFCI protection where required, tamper-resistant where required, weather-resistant where required
- Switches and lighting — Proper switching, fixture support, recessed light clearances from insulation
- Junction boxes and wiring methods — Accessible covers, proper connectors, cable clamps, NM cable not in exposed locations where prohibited
- Smoke / CO detectors — Proper placement, interconnection, working condition (if in scope)
- Outdoor and wet locations — Weatherproof covers, in-use covers, GFCI protection, proper cable types

### 5. Photo Reference Log (if applicable)
- Numbered list keyed to findings
- Brief description of what each photo documents

### 6. Summary of Required Corrections
- Numbered list of all 🔴 and 🟡 findings with locations
- Recommended timeline for corrections (immediate, within 30 days, at next permitted work)

### 7. Recommendations
- Optional improvements not required by code
- Energy efficiency suggestions
- System upgrade recommendations based on age and condition

### 8. Disclaimer
- Standard inspection disclaimer: visual inspection only, non-destructive, based on conditions observed at time of inspection, does not guarantee absence of concealed defects
- Note NEC edition referenced
- Recommend client consult with AHJ for permit-related questions

**Formatting rules:**
- Write findings in clear, factual language — no speculation
- Always cite NEC articles for code violations (e.g., "NEC 210.12(A)" not just "needs AFCI")
- Adjust technical language for the audience — homeowner reports should explain terms; GC/inspector reports can use trade shorthand
- Use severity color codes consistently
- If the user provides rough notes, ask clarifying questions only if critical information is missing (location, what they saw). Otherwise, make reasonable inferences and note assumptions.

## Example Output

*(Abbreviated — full report would cover all inspected systems)*

**Report:** Electrical Safety Evaluation — 123 Main St, Anytown

**Executive Summary:** Inspection of this 1985 residential property revealed (3) safety hazards, (5) code violations, and (4) recommendations. The service entrance and main panel show signs of age-related deterioration. The grounding electrode system does not meet current NEC requirements. Immediate correction of double-tapped breakers and open-splice junction boxes is recommended before continued occupancy.

**Sample Finding:**

| # | Location | Finding | Severity | NEC Ref | Action |
|---|----------|---------|----------|---------|--------|
| 1 | Main panel | Two circuits double-tapped on single-pole 20A breaker — breaker not rated for multiple conductors | 🔴 Safety Hazard | NEC 110.14(A) | Install tandem breaker or add new breaker for second circuit |
| 2 | Kitchen | No GFCI protection on countertop receptacles within 6' of sink | 🟡 Code Violation | NEC 210.8(A)(6) | Install GFCI receptacles or GFCI breaker for kitchen circuits |
| 3 | Basement | Open junction box with exposed wire splices — no cover plate | 🔴 Safety Hazard | NEC 314.28(C) | Install appropriate cover plate, verify box fill |
| 4 | Service entrance | Weatherhead sealant deteriorated, minor water tracking visible on mast | 🔵 Recommendation | — | Re-seal weatherhead to prevent moisture intrusion |
