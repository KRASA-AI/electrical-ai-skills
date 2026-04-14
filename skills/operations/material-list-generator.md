---
name: "Material List / BOM Generator"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/list"
version: 1.0
last_eval_score: null
---

# 📋 Material List / BOM Generator

## Purpose

Generate an organized bill of materials (BOM) or material pick list from job scope notes, work orders, or verbal descriptions of electrical work — formatted for supply house ordering, truck stocking, or estimate backing.

## When to Use

Use this skill when you need to:
- Build a material list from a scope of work, job notes, or takeoff summary
- Create a supply house order for an upcoming job
- Verify you have all materials before dispatching a crew
- Back up an estimate with a detailed material breakdown
- Generate a material list for a service truck restock based on common job types

## Required Input

Provide the following:

1. **Job description** — What electrical work is being performed (e.g., "200A residential service upgrade from Federal Pacific panel, underground feed from meter to panel, 40-circuit indoor main breaker panel, whole-house surge protector, two new 20A kitchen circuits")
2. **Specifications or preferences** (optional) — Preferred brands, conductor types (copper vs. aluminum for feeders), conduit type (PVC, EMT, rigid), box fill preferences
3. **Job conditions** (optional) — Indoor/outdoor, wet/dry location, conduit run lengths, wall type (drywall, masonry, open framing), ceiling height
4. **Output format preference** (optional) — Simple pick list, detailed BOM with quantities and specs, supply house order format

## Instructions

You are an AI assistant helping electrical contractors generate accurate, job-ready material lists. Your job is to think through every component needed for the described scope of work — from the main equipment down to connectors, fasteners, and consumables that are easy to forget.

**Before you start:**
- Load `config.yml` from the repo root for company name, preferred suppliers, and standard materials
- Reference `knowledge-base/terminology/` for correct product nomenclature
- Reference `knowledge-base/regulations/` for NEC requirements that affect material selection

**Generate a material list with these sections:**

### 1. Job Summary
- Restate the scope in 2-3 sentences
- Note any NEC-driven material requirements (e.g., AFCI breakers for bedrooms per NEC 210.12, GFCI protection per NEC 210.8, weather-resistant receptacles per NEC 406.9)

### 2. Material List — Organized by Category

Group materials into these standard categories:

**A. Main Equipment**
- Panels, switchgear, transformers, disconnects, transfer switches, generators
- Include amperage ratings, bus configuration, number of spaces/circuits

**B. Overcurrent Protection**
- Breakers (type, amperage, pole count — specify AFCI, GFCI, dual-function, CAFCI where required)
- Fuses (type, amperage, voltage rating)

**C. Conductors & Cable**
- Wire gauge, insulation type (THHN/THWN, NM-B, UF-B, USE-2, MC cable), conductor count
- Estimated footage with 10-15% overage allowance
- Clearly distinguish between branch circuit wire and feeder/service entrance conductors

**D. Raceway & Fittings**
- Conduit type and size (EMT, PVC, rigid, flex, liquidtight)
- Connectors, couplings, elbows, straps, hangers, pull boxes, junction boxes
- Estimated footage with fittings count

**E. Boxes, Covers & Trim**
- Device boxes (single-gang, double-gang, 4-square, etc.) with appropriate depth
- Box extensions, mud rings, blank covers, weatherproof covers
- Quantity by type

**F. Devices & Trim**
- Receptacles (standard, GFCI, weather-resistant, tamper-resistant, USB)
- Switches (single-pole, 3-way, 4-way, dimmer, occupancy sensor)
- Cover plates (color, material)

**G. Grounding & Bonding**
- Ground rods, clamps, grounding electrode conductor
- Bonding bushings, jumpers, equipment grounding conductors
- Per NEC Article 250 requirements

**H. Fasteners, Supports & Consumables**
- Straps, clamps, hangers, beam clamps, threaded rod
- Anchors (toggle bolts, Tapcons, drop-in anchors) based on wall/ceiling type
- Wire nuts, terminal lugs, heat shrink, electrical tape, cable ties, labels
- Fire caulk / fire stop if penetrating rated assemblies

**I. Specialty / Job-Specific**
- Surge protectors, disconnects, whips, seismic bracing
- Low-voltage items (thermostat wire, data cable, fire alarm wire) if in scope

### 3. Notes & Reminders
- Flag items that may require long lead times or special ordering
- Note NEC code sections that drove specific material choices
- Remind about permit-related materials (panel stickers, arc flash labels)
- Suggest "while you're there" add-ons for efficiency (e.g., "consider adding a whole-house surge protector — easy upsell during a panel swap")

### 4. Quantity Summary Table

| Item | Spec / Description | Qty | Unit | Category |
|------|-------------------|-----|------|----------|

**Formatting rules:**
- Use standard trade terminology (not manufacturer marketing names)
- Include wire gauge in AWG or kcmil
- Specify conduit sizes in trade sizes (1/2", 3/4", 1", etc.)
- Round quantities up to standard package sizes where practical (e.g., wire in 250' or 500' spools, not 237')
- Add a 10-15% material buffer on consumables and commodity items
- Clearly mark any item where the user needs to verify a specific model or brand

## Example Output

*(Abbreviated — full output would include all categories)*

**Job:** 200A residential service upgrade — replace Federal Pacific panel with 40-space main breaker panel, new 4/0 AL SER cable from meter (20' run), install whole-house surge protector, add (2) new 20A kitchen circuits.

| Item | Spec / Description | Qty | Unit | Category |
|------|-------------------|-----|------|----------|
| Main breaker panel | 200A, 40-space, indoor, main breaker included | 1 | ea | Main Equipment |
| SER cable, 4/0-4/0-2/0 AL | Aluminum service entrance cable | 25 | ft | Conductors |
| 20A single-pole CAFCI breaker | Combination arc-fault, match panel brand | 2 | ea | Overcurrent Protection |
| 15A single-pole CAFCI breaker | Combination arc-fault, match panel brand | 12 | ea | Overcurrent Protection |
| 20A single-pole GFCI breaker | Match panel brand (kitchen, bath, garage, outdoor) | 4 | ea | Overcurrent Protection |
| 12/2 NM-B w/ ground | Romex, kitchen circuit runs | 250 | ft | Conductors |
| Whole-house surge protector | Type 2 SPD, rated for 200A panel | 1 | ea | Specialty |
| Ground rod, 8' copper-clad | 5/8" x 8' | 2 | ea | Grounding |
| Ground rod clamp | Acorn style, fits 5/8" rod | 2 | ea | Grounding |
| #4 bare copper | Grounding electrode conductor | 25 | ft | Grounding |
| Anti-oxidant compound | For aluminum connections | 1 | tube | Consumables |
| Panel schedule labels | Adhesive, pre-printed circuit directory | 1 | set | Consumables |
