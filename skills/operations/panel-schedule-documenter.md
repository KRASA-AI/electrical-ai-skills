---
name: "Panel Schedule Documenter"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/panel"
version: 2.1
last_eval_score: 8.50
---

# 🔌 Panel Schedule Documenter

## Purpose

Create or update a formatted panel schedule from as-built notes, photos, or verbal descriptions — producing a clean, NEC **408.4**-compliant circuit directory ready for the panel door or close-out package, plus the required cross-reference to the **arc-flash warning label** (NEC 2026 §110.16(B)/(C)) on every commercial / industrial panel that's likely to be examined or serviced while energized.

The skill produces three deliverables per run: a printable directory in the standard two-column (single-phase) or three-column (three-phase) layout, a flagged-issues list (oversized OCPD, missing AFCI/GFCI/dual-function protection where current code requires it, double-tapped lugs, mis-balanced phases, MWBC handle-tie omissions), and a short **arc-flash / labeling cross-reference** that tells the user whether this panel triggers a label per the AHJ's adopted NEC cycle and hands off to `arc-flash-label-generator.md` for the actual label.

## When to Use

Use this skill when you need to:
- Document an existing panel after a service call, troubleshooting visit, or inspection
- Create an as-built panel schedule after roughing in a new panel
- Update a panel schedule after adding or modifying circuits (CO, sub, retrofit)
- Convert messy handwritten panel notes or a phone photo of a panel door into a clean digital directory
- Prepare panel documentation for a final inspection, lender close-out, or owner O&M package
- Cross-check a commercial panel for the NEC 2026 §110.16 arc-flash labeling requirement before pulling final inspection
- Generate a directory that can be printed, laminated, and applied to the panel door per NEC §408.4

## Required Input

Provide the following:

1. **Panel info** — Designation (e.g., "Panel A," "MDP," "LP-2"), manufacturer / catalog # if known, main breaker (or MLO + upstream OCPD), bus rating, voltage system (120/240V 1Φ / 208Y/120V 3Φ / 480Y/277V 3Φ / 480V Δ), AIC rating, location, fed-from reference
2. **Circuit data** — For each circuit: slot number, breaker size (A), poles (1P / 2P / 3P), breaker type (standard / AFCI / GFCI / AF/GF dual-function / SWD / HACR / GFPE), circuit description / what it feeds
3. **Source format** — Handwritten notes / phone photo of the door / verbal walk-through / existing schedule needing update / blueprint legend
4. **Jurisdiction & adopted NEC cycle** — State and city / county. Required to determine whether NEC 2023 or NEC 2026 governs AFCI/GFCI extents and §110.16 arc-flash labeling
5. **Occupancy class** — Dwelling / non-dwelling (commercial / industrial). Drives §110.16 label trigger
6. **Any unknowns** — Circuits the field tech couldn't trace, spare breakers, spaces, "do not use" tags

## Instructions

You are an AI assistant helping a licensed electrician produce a clean, code-aligned panel directory and the cross-checks that go with it. Your job is to take messy real-world input and return a printable directory plus the issue list and labeling cross-reference.

**Before you start:**

- Load `config.yml` from the repo root for company name, license #, default jurisdiction, default voltage assumptions, and printed-directory header preferences
- Reference `knowledge-base/terminology/` for correct circuit-description conventions and standardized abbreviations
- Reference `knowledge-base/regulations/nec-2026-key-changes.md` for §110.16(B)/(C) arc-flash labeling expansion (the 2026 cycle removed the 1,000 A trigger for non-dwellings) and any §210.8 / §210.12 deltas on AFCI/GFCI scope
- If the user did not state the AHJ-adopted cycle, run the **NEC Cycle Check** below before flagging "missing AFCI" or "missing arc-flash label"

**NEC Cycle Check (always run first):**

```
1. Did the user state the adopted NEC cycle? → Use it.
2. If not, did the user provide a state and city/county? → Default to the
   most recent AHJ-confirmed cycle for that jurisdiction. State your
   assumption explicitly in the Notes section.
3. If neither, ask once: "Which NEC cycle does this AHJ enforce?" If the
   user declines, default to NEC 2023 and label assumptions plainly.
4. Do not flag a circuit as "missing AFCI" or "missing arc-flash label"
   based on a 2026-only provision in a jurisdiction still on 2020 or 2023.
```

**Process:**

1. **Parse the input.** Match each circuit to its position, size, poles, type, and description. Treat illegible / ambiguous entries as `UNKNOWN — NEEDS IDENTIFICATION`. Do not invent a circuit description.
2. **Organize into the standard layout.**
   - *Single-phase 120/240V:* two-column layout, odd slots left, even slots right, phase A on odd-1 / even-2 / etc., phase B on odd-3 / even-4 / etc., alternating per NEMA standard
   - *Three-phase 208Y/120V or 480Y/277V:* three-column layout with phase A/B/C tagged, or two-column with explicit phase column. Confirm the panel's actual phasing pattern (most modern panelboards alternate A-B-C-A-B-C down the bus)
3. **For each circuit, include:** slot #, breaker A, poles, breaker type (AFCI / GFCI / AF/GF / SWD / HACR / GFPE / standard), description using standard conventions, and connected load in VA / kVA when sufficient data exists
4. **Mark conventions:**
   - `SPACE` for empty bus positions
   - `SPARE [size]A` for an installed but unused breaker
   - `MWBC w/ Ckt #X` to denote multiwire branch circuits sharing a neutral and require a handle tie per NEC §210.4(B)
   - `DEDICATED` for single-load circuits (DW, disposal, microwave, well pump, A/C condenser, range, dryer, EV)
   - `(GFP)` on a feeder OCPD where Ground-Fault Protection of Equipment is required (480Y/277V services ≥ 1,000 A per §230.95)
5. **Calculate phase balance** when sufficient data is provided. For three-phase panels, list connected load on each phase and flag imbalances >15% for review.
6. **Flag issues** in a separate `Issues / Follow-up` block — never silently insert a "needs AFCI" note in the directory itself. Common flags:
   - **Double-tapped lugs** noted in the field (most residential breakers are listed for one conductor; some Cutler-Hammer CH and Square D QO are listed for two — check the breaker label)
   - **OCPD oversized for the conductor** described (e.g., 30 A breaker on #14 AWG)
   - **Missing AFCI** on dwelling-unit branch circuits where the cycle in force requires it (NEC 2023 §210.12 — kitchens, family rooms, bedrooms, dining rooms, libraries, sunrooms, hallways, closets, laundry; NEC 2017 / 2020 had a narrower set)
   - **Missing GFCI** on dwelling kitchen, bath, garage, outdoor, basement (NEC 2023 §210.8(A) expanded scope); commercial GFCI per §210.8(B)
   - **Missing dual-function on bedroom outlets** in 2023+ jurisdictions
   - **MWBC without handle tie** (§210.4(B))
   - **Phase imbalance** > 15% on three-phase commercial / industrial panels
   - **Bonding jumper / EGC absent or undersized** (verify in the field, not from the directory alone)
   - **Aluminum branch wiring** (1965–1973 vintage — recommend AlumiConn / COPALUM follow-up)
   - **Federal Pacific Stab-Lok, Zinsco, Pushmatic, ITE Pushmatic, Challenger** panels — note the field-history concern, do not assert "must replace"
7. **Cross-reference the arc-flash labeling requirement** for non-dwelling panels:
   - In **NEC 2026** AHJs: §110.16(B)/(C) — arc-flash labels required on **all non-dwelling distribution equipment** likely to be examined / serviced / maintained while energized, regardless of ampere rating, with required content including assessment date, voltage, AFB, incident energy or PPE category, and equipment ID. Hand off to `operations/arc-flash-label-generator.md` for the label package.
   - In **NEC 2023** AHJs: §110.16(B) — labels required on service equipment ≥ 1,200 A in non-dwelling occupancies (1,000 A under 2020).
   - In **NEC 2020 / 2017** AHJs: thresholds vary; cite the cycle in force and recommend label regardless if the panel is ≥ 480 V or service equipment, per common 70E practice.
8. **Calculate the total connected load** if the user supplied per-circuit VA. State demand factors if applicable (Article 220 for NEC 2017–2023, Article 120 for NEC 2026 dwellings). For commercial calcs, hand off to `operations/nec-2026-load-calculation-helper.md`.
9. **Standard circuit description conventions:**
   - Use room / area names: "Kitchen Countertop Recepts," "Master Bed Lts & Recepts"
   - Specify dedicated circuits: "DW (Dedicated)," "Microwave (Dedicated)," "Range (Dedicated)"
   - Note 240 V loads clearly: "Electric Range 2P-40A," "A/C Condenser 2P-30A," "EV Charger 2P-50A"
   - Industry abbreviations where standard: HVAC / WH (water heater) / DW / DISP / GFI / AF / DF / HR (handrail) / SUB (sub-panel) / EV
   - For commercial: include room number, panel fed-from reference, connected load in VA/kVA
10. Append the **footer block** with company name, license #, documented-by, date, AHJ cycle assumed, and a note about §408.4 directory durability.

**Output format:**

```
═════════════════════════════════════════════════════════
PANEL DIRECTORY — [Panel Designation]
[Company Name from config]   License # [from config]
═════════════════════════════════════════════════════════

Location:        [Physical location]
Manufacturer:    [Mfr / Catalog #]
Voltage System:  [120/240V 1Φ / 208Y/120V 3Φ / 480Y/277V 3Φ]
Bus Rating:      [A]         Main Breaker: [A or "MLO"]
AIC Rating:      [kA]        Fed From:     [source]
Occupancy:       [Dwelling / Non-Dwelling — Commercial / Industrial]
NEC Cycle:       [2017 / 2020 / 2023 / 2026]   AHJ: [city/county/state]

CIRCUIT DIRECTORY

| Ckt# | A  | P | Type | Description              |  | Description              | Type | P | A  | Ckt# |
|------|----|---|------|--------------------------|--|--------------------------|------|---|----|------|
|  1   | 20 | 1 | AFCI | Kitchen Lights           |  | Kitchen Counter Recepts  | DF   | 1 | 20 |  2   |
|  3   | 20 | 1 | DF   | Master Bed Lts & Recepts |  | Bedroom 2 Lts & Recepts  | DF   | 1 | 20 |  4   |
| ...  |    |   |      |                          |  |                          |      |   |    | ...  |

Spaces:  [count]
Spares:  [count and sizes]
MWBCs:   [list any multiwire branch circuits — Ckts X/Y, X/Y/Z]

ISSUES / FOLLOW-UP
- [Flag 1] — [NEC reference if applicable]
- [Flag 2] — [field action recommended]

ARC-FLASH LABELING (non-dwelling panels)
- NEC cycle in force: [2017 / 2020 / 2023 / 2026]
- Label required:    [YES — hand off to arc-flash-label-generator.md / NO — under threshold for cycle / N/A — dwelling]
- Required content:  [voltage / AFB / incident energy or PPE cat / assessment date / equipment ID]

§408.4 NOTE
Circuit directories must legibly identify the purpose or use of each
circuit. Labels must be durable for the environment. Transfer this
directory to the panel-door label or laminated card.

Documented by:  [Tech name]
Date:           [Today's date]
NEC cycle assumed: [cycle] — verify against the AHJ
```

After the directory, include a short **Internal Notes** block listing:
- Any circuit you tagged `UNKNOWN — NEEDS IDENTIFICATION` and the recommended trace method (continuity, plug-in tracer, voltage-drop method)
- Any assumption made when input was ambiguous
- A pointer to `operations/arc-flash-label-generator.md` if the panel triggers a label
- A pointer to `operations/nec-2026-load-calculation-helper.md` if a connected-load calc was attempted
- Any panel-history concern (FPE Stab-Lok, Zinsco, Pushmatic, Challenger) flagged for the customer-facing summary

## What NOT to do

- Do **not** invent circuit descriptions. `UNKNOWN — NEEDS IDENTIFICATION` is the right answer for any circuit the field tech couldn't trace.
- Do **not** flag "missing AFCI / arc-flash label" using a 2026-only rule in a 2020 / 2023 AHJ. Run the cycle check first.
- Do **not** silently rewrite the customer's labels to be "more accurate" — preserve their wording in the directory and put corrections in the Issues block.
- Do **not** assert a panel "must be replaced" because of brand history. Flag the field concern; replacement is the customer's call with the EC.
- Do **not** combine the directory with the arc-flash label content in one block — those are two deliverables, and the label has its own format requirements (handed off to `arc-flash-label-generator.md`).
- Do **not** put company branding inside the directory grid — the directory must remain legible and durable per §408.4. Branding goes in the header block.

## Worked Example 1 — Residential 200 A Service Upgrade, Portland OR (NEC 2026)

**User input:**
> "Documenting the new panel from a 100 A → 200 A upgrade I just finished. Square D QO HOM3060L200PCAFVP, 200 A main, 120/240V single-phase, indoor garage wall, fed from new outdoor disconnect (Siemens MC2040B1200ESC). Portland, OR (city is on NEC 2026 since Oct 2025).
>
> Circuits as installed:
>   1 — 20A 1P AFCI Kitchen lights
>   2 — 20A 1P DF Kitchen counter receps left
>   3 — 20A 1P DF Kitchen counter receps right (small appliance ckt)
>   4 — 20A 1P DF Dishwasher dedicated
>   5,7 — 50A 2P Range
>   6 — 20A 1P DF Disposal
>   8 — 20A 1P DF Microwave dedicated
>   9,11 — 30A 2P Dryer
>   10 — 20A 1P DF Laundry
>   12 — 15A 1P AFCI MBR lts/recepts
>   13 — 15A 1P AFCI BR2 lts/recepts
>   14 — 15A 1P AFCI BR3 lts/recepts
>   15 — 20A 1P GFCI Bath 1 recepts
>   16 — 20A 1P GFCI Bath 2 recepts
>   17,19,21 — 50A 2P + 50A 2P EV charger (Wallbox Pulsar Plus 48A) — actually 17,19 only, 2P-50A, dedicated, EVEMS-managed
>   18 — 20A 1P DF Hallway lts/recepts
>   20 — 20A 1P GFCI Garage recepts
>   22 — 20A 1P DF Living room lts/recepts
>   23,25 — 30A 2P A/C condenser
>   24 — 20A 1P DF Family room lts/recepts
>   26 — 20A 1P GFCI Outdoor recepts
>   27 — 15A 1P DF Smoke/CO interconnect ckt
>   28 — Spare 20A
>   29,30 — Space
>
> Field notes: ground rod system replaced (2 rods 6 ft apart per NEC 250.53), water bonding to copper main, gas bond at meter, neutral bonded at outdoor disconnect only (not at indoor panel — sub-panel rule). Customer is Sarah Chen, 1422 Maple St."

**Output:**

> ═════════════════════════════════════════════════════════
> PANEL DIRECTORY — Panel A (Main Indoor)
> Torres Electric    License # EC-00842
> ═════════════════════════════════════════════════════════
>
> Location:        Garage interior wall, north
> Manufacturer:    Square D QO HOM3060L200PCAFVP
> Voltage System:  120/240V 1Φ
> Bus Rating:      200 A         Main Breaker: 200 A (sub of outdoor disco)
> AIC Rating:      22 kA         Fed From:     Outdoor disconnect (Siemens MC2040B1200ESC) at meter pedestal
> Occupancy:       Dwelling
> NEC Cycle:       2026         AHJ: City of Portland, OR (effective Oct 2025)
>
> CIRCUIT DIRECTORY
>
> | Ckt# | A  | P | Type | Description              |  | Description              | Type | P | A  | Ckt# |
> |------|----|---|------|--------------------------|--|--------------------------|------|---|----|------|
> |  1   | 20 | 1 | AFCI | Kitchen Lights           |  | Kitchen Counter L (SAC)  | DF   | 1 | 20 |  2   |
> |  3   | 20 | 1 | DF   | Kitchen Counter R (SAC)  |  | DW (Dedicated)           | DF   | 1 | 20 |  4   |
> |  5   | 50 | 2 | std  | Range (Dedicated)        |  | Disposal (Dedicated)     | DF   | 1 | 20 |  6   |
> |  7   |    |   |      | (Range, lower pole)      |  | Microwave (Dedicated)    | DF   | 1 | 20 |  8   |
> |  9   | 30 | 2 | std  | Dryer (Dedicated)        |  | Laundry Recepts          | DF   | 1 | 20 | 10   |
> | 11   |    |   |      | (Dryer, lower pole)      |  | MBR Lts & Recepts        | AFCI | 1 | 15 | 12   |
> | 13   | 15 | 1 | AFCI | BR2 Lts & Recepts        |  | BR3 Lts & Recepts        | AFCI | 1 | 15 | 14   |
> | 15   | 20 | 1 | GFCI | Bath 1 Recepts           |  | Bath 2 Recepts           | GFCI | 1 | 20 | 16   |
> | 17   | 50 | 2 | std  | EV Charger (EVEMS-managed) |  | Hallway Lts & Recepts  | DF   | 1 | 20 | 18   |
> | 19   |    |   |      | (EV, lower pole)         |  | Garage Recepts           | GFCI | 1 | 20 | 20   |
> | 21   | —  | — | —    | SPACE                    |  | LR Lts & Recepts         | DF   | 1 | 20 | 22   |
> | 23   | 30 | 2 | HACR | A/C Condenser (Ded.)     |  | FR Lts & Recepts         | DF   | 1 | 20 | 24   |
> | 25   |    |   |      | (A/C, lower pole)        |  | Outdoor Recepts          | GFCI | 1 | 20 | 26   |
> | 27   | 15 | 1 | DF   | Smoke/CO Interconnect    |  | SPARE 20A                | std  | 1 | 20 | 28   |
> | 29   | —  | — | —    | SPACE                    |  | SPACE                    | —    | — | —  | 30   |
>
> Spaces:  3 (slots 21, 29, 30)
> Spares:  1 × 20A (slot 28)
> MWBCs:   None
>
> ISSUES / FOLLOW-UP
> - **EV charger (Ckts 17/19)** is wired as EVEMS-managed (Wallbox Pulsar Plus 48 A on a 50 A breaker). Confirm the EVEMS firmware is set to limit at ≤80% of breaker rating per **NEC 2026 §625.42 / §750**. EVEMS labeling required at the panel and at the EVSE per **§625.42(C)**.
> - **Smoke/CO interconnect (Ckt 27)** is on a dual-function breaker. Code permits but check that the AFCI doesn't nuisance-trip on the alarm transformer; document for the homeowner.
> - **Bonding** — confirmed at outdoor disconnect (service-entrance neutral-to-EGC bond). Indoor panel correctly **isolated** (4-wire feeder per §250.32 sub-panel rules). Verify visually before close-out.
> - **Grounding** — two rods 6 ft apart, supplemental electrode system per §250.53(A)(3). Document on rough-in card; no further action.
>
> ARC-FLASH LABELING (non-dwelling panels)
> - NEC cycle in force: 2026
> - Label required:    **N/A — Dwelling occupancy.** §110.16 dwelling-unit-services labeling requirements do apply for this 200 A service equipment per §110.16(A) — confirm a permanent warning label was installed at the **outdoor disconnect** (the service equipment), not at this indoor sub-panel.
> - If the homeowner later adds a small commercial use (home business, ADU rental), revisit the §110.16 requirement.
>
> §408.4 NOTE
> Circuit directories must legibly identify the purpose or use of each
> circuit. Labels must be durable for the environment. Transfer this
> directory to the panel-door label or laminated card.
>
> Documented by:  Mike Torres
> Date:           2026-04-25
> NEC cycle assumed: 2026 (confirmed — Portland adopted Oct 2025)
>
> ---
>
> **Internal Notes:**
> - All circuits identified by the installing tech; no UNKNOWN entries this run.
> - EV charger is EVEMS-managed at 48 A continuous on a 50 A OCPD. The §120.82(D) load calc was run separately — see the load-calc helper output filed in the project folder. Service is correctly sized at 200 A.
> - For the §110.16 service-equipment label at the outdoor disconnect: hand off to `operations/arc-flash-label-generator.md`. The outdoor disconnect is the service equipment; the indoor panel is downstream.
> - Federal Pacific / Zinsco / Challenger / Pushmatic — N/A; this is a brand-new Square D QO panel.
> - Customer plain-English directory (homeowner version): hand off to `customer-service/customer-explanation-generator.md` if the homeowner wants a non-trade version for their records.

## Worked Example 2 — Commercial 480Y/277V 1,200 A Switchboard, Existing Building, Phoenix AZ

**User input:**
> "Documenting an existing MDP at a 14,800 SF medical-office TI we just took over. The schedule on the inside cover is half-illegible. Phoenix is on NEC 2017 with local amendments for permit purposes (verified with City of Phoenix Development Services electrical bulletin 2025-08). Existing: GE switchboard cat # AKD-8 lineup, 480Y/277V 3-phase 4-wire, 1,200 A bus, 1,200 A main breaker (GE TPS, magnetic only — adjustable trip). AIC 65 kA. Fed from utility 75 kVA pad-mount. Two distribution sections plus a transformer section.
>
> Section A (1,200A bus, 480Y/277V):
>   1 — 100A 3P MCC-1 (manufacturing area, decommissioned 2018, breaker on but no load — likely SPARE)
>   2 — 100A 3P RTU-1 (rooftop unit, north zone)
>   3 — 60A 3P RTU-2 (rooftop unit, south zone)
>   4 — 50A 3P EWH (electric water heater, central core)
>   5 — 100A 3P Sub-panel LP-1 (lighting and 277V loads, north)
>   6 — 100A 3P Sub-panel LP-2 (lighting and 277V loads, south)
>   7 — 60A 3P UNKNOWN — labeled 'TENANT' but no termination at any tenant equipment we can find
>   8 — 30A 3P UNKNOWN — half-erased label
>   9 — 200A 3P (Step-down xfmr T1: 480Δ → 208Y/120V 75 kVA) feeding Sub-panel HP-1 / receptacle panel
>  10 — 60A 3P SPARE (per existing schedule)
>  11 — Space
>  12 — Space
>
> Customer wants a clean directory and any §110.16 / 70E concerns flagged before final inspection. Three pre-existing labels on the panel: 'DANGER — ARC FLASH HAZARD,' 'EQUIPMENT RATED 65 kAIC,' 'FAULT STUDY 2018 — REPLACE 2023.' Customer is Mesa Medical Park."

**Output:**

> ═════════════════════════════════════════════════════════
> PANEL DIRECTORY — MDP (Section A)
> Sonoran Electric Co.    License # AZ ROC-289044
> ═════════════════════════════════════════════════════════
>
> Location:        Main electrical room, NW corner, ground floor
> Manufacturer:    GE AKD-8 switchboard lineup (legacy)
> Voltage System:  480Y/277V 3Φ 4-wire
> Bus Rating:      1,200 A         Main Breaker: 1,200 A (GE TPS, magnetic-only adjustable)
> AIC Rating:      65 kA           Fed From:     Utility 75 kVA pad-mount transformer
> Occupancy:       Non-Dwelling — Commercial (Medical Office TI)
> NEC Cycle:       2017 (City of Phoenix per Bulletin 2025-08)   AHJ: City of Phoenix Development Services
>
> CIRCUIT DIRECTORY (Section A — distribution feeders only; sub-panels documented separately)
>
> | Ckt# | A    | P | Description                                       | Notes                                  |
> |------|------|---|---------------------------------------------------|----------------------------------------|
> |  1   | 100  | 3 | MCC-1 (decommissioned 2018) — verify SPARE        | Field-trace before re-energizing       |
> |  2   | 100  | 3 | RTU-1 — North Zone                                | HACR rated; verify nameplate           |
> |  3   |  60  | 3 | RTU-2 — South Zone                                | HACR rated; verify nameplate           |
> |  4   |  50  | 3 | EWH — Central Core Mechanical Room                | Confirm 480V 3Φ EWH still in service   |
> |  5   | 100  | 3 | Sub-panel LP-1 — North 277V Lighting & Loads      | Schedule LP-1 documented separately    |
> |  6   | 100  | 3 | Sub-panel LP-2 — South 277V Lighting & Loads      | Schedule LP-2 documented separately    |
> |  7   |  60  | 3 | UNKNOWN — NEEDS IDENTIFICATION                    | Labeled "TENANT" but no termination    |
> |  8   |  30  | 3 | UNKNOWN — NEEDS IDENTIFICATION                    | Label erased; trace before re-energize |
> |  9   | 200  | 3 | Step-down Xfmr T1 (480Δ→208Y/120V, 75 kVA)        | Feeds Sub-panel HP-1 (receptacles)     |
> | 10   |  60  | 3 | SPARE                                             | Per existing schedule                  |
> | 11   | —    | — | SPACE                                             |                                        |
> | 12   | —    | — | SPACE                                             |                                        |
>
> Spaces:  2 (slots 11, 12)
> Spares:  1 × 60A 3P (slot 10)
> Unknown: 2 × 3P (slots 7, 8) — TRACE REQUIRED before final inspection
>
> ISSUES / FOLLOW-UP
> - **Slot 1 (MCC-1, decommissioned 2018):** Breaker is on but feeder is reportedly de-energized at the dead MCC. Lock out, trace conductors, and either (a) remove and re-tag as SPARE per §408.4 or (b) fully demolish per §110.27. Do not leave a "decommissioned" feeder energized at the line side of an unlabeled OCPD.
> - **Slots 7 & 8 (UNKNOWN):** Two 3-pole feeders without verifiable termination. Trace with the lineup de-energized before final inspection. NEC §408.4(A) requires the directory to "clearly and specifically" identify the purpose of each circuit — `UNKNOWN — NEEDS IDENTIFICATION` is acceptable as a *temporary* state but not as a final close-out condition.
> - **Phase balance:** Connected-load not provided in intake; cannot calculate. Recommend running a 1-week kW logger on the secondary side of the main and on each feeder to confirm phase balance before recommending any new tenant load.
> - **Existing arc-flash label "FAULT STUDY 2018 — REPLACE 2023":** The 2018 study is now ~8 years old. NFPA 70E recommends review every 5 years or whenever the system changes. The TI scope likely qualifies as a system change. **Recommend a refreshed incident-energy study** before re-energizing the lineup with new tenant loads. The pre-existing label cannot be relied on for current PPE selection.
> - **GE AKD-8 lineup parts availability:** GE AKD-8 is an end-of-life product line. Replacement breakers are increasingly third-party rebuilt. Document a renewal-parts plan with the owner.
> - **Phoenix is on NEC 2017** for permit purposes — do NOT add a §230.85 emergency disconnect or a §110.16(B) 2026-cycle assessment-date label on this drawing without written confirmation from City of Phoenix Development Services that the project is being reviewed under a newer cycle.
>
> ARC-FLASH LABELING (non-dwelling panels)
> - NEC cycle in force: 2017
> - Label required:    **YES under §110.16(B) — service equipment ≥ 1,200 A in non-dwelling occupancy.** A label exists ("DANGER — ARC FLASH HAZARD") but content compliance is unverified and the underlying study is 8 years old.
> - Required content under 2017: voltage, AFB, incident energy or PPE category, equipment ID. Assessment-date is best-practice but not strictly required until the AHJ adopts the 2026 cycle.
> - Hand off to `operations/arc-flash-label-generator.md` once the refreshed study is complete.
>
> §408.4 NOTE
> Circuit directories must legibly identify the purpose or use of each
> circuit. Labels must be durable for the environment. The current MDP
> directory does NOT meet §408.4(A) until slots 1, 7, and 8 are resolved.
>
> Documented by:  Daniela Reyes (Sonoran Electric, Foreman)
> Date:           2026-04-25
> NEC cycle assumed: 2017 (Phoenix Bulletin 2025-08)
>
> ---
>
> **Internal Notes:**
> - Three circuits flagged for trace before final inspection (slots 1, 7, 8). Plan a 4-hour LOTO window with the property manager.
> - Refreshed incident-energy study is the highest-priority follow-up. Pre-final-inspection deliverable. Hand off to `arc-flash-label-generator.md` once the study lands.
> - Sub-panel LP-1, LP-2, and HP-1 directories should be documented separately on individual `panel-schedule-documenter.md` runs.
> - The 2018 study is a real liability risk — communicate to Mesa Medical Park in writing (use `customer-service/customer-explanation-generator.md` if a non-technical version is needed for their facilities team).
> - GE AKD-8 renewal parts: brief the customer on switchboard end-of-life realities. Not a code violation today, but a 5–10-year capital plan item.
> - No EV / EVEMS / BESS scope in this lineup; §625 / §706 not applicable.

## Worked Example 3 — Sub-panel Update After Adding a Heat-Pump Water Heater (NEC 2023, MA)

**User input:**
> "Just added a 30A 2P circuit for a Rheem Performance Platinum 50 gal heat-pump water heater on an existing residential sub-panel. Boston, MA — MA is on NEC 2023 with 527 CMR 12 amendments. The sub-panel was last documented 2021. Sub-panel is Cutler-Hammer BR series, 100A MLO, 120/240V 1Φ, 24-circuit. Fed from a 100A 2P breaker in the main panel. The HPWH circuit is now in slots 21,23 (next to a SPARE that was there). Updated directory should reflect.
>
> Existing 2021 schedule (from photo):
>   1 — 20A 1P AFCI Living rm/dining lts
>   2 — 20A 1P GFCI Kitchen counter L
>   3 — 20A 1P AFCI Bedroom 1 lts/rec
>   4 — 20A 1P GFCI Kitchen counter R
>   5 — 20A 1P AFCI Bedroom 2 lts/rec
>   6 — 20A 1P AFCI DW
>   7 — 20A 1P AFCI Bath lts (no recepts on this ckt)
>   8 — 20A 1P GFCI Bath recepts
>   9,11 — 30A 2P Dryer
>  10 — 20A 1P AFCI Hall lts/rec
>  12 — 15A 1P SPARE
>  13,15 — 30A 2P A/C condenser
>  14 — 20A 1P GFCI Garage recepts
>  17 — 20A 1P GFCI Outdoor recepts
>  16,18 — 30A 2P SPARE
>  19 — 20A 1P AFCI Smoke/CO
>  20 — 20A 1P GFCI Bsmt recepts
>  21,23 — SPACE — now 30A 2P HPWH (NEW)
>  22 — 20A 1P AFCI Bsmt lts
>  24 — Space"

**Output:**

> ═════════════════════════════════════════════════════════
> PANEL DIRECTORY — Sub-panel SP-1 (Updated)
> [Company from config]    License # [from config]
> ═════════════════════════════════════════════════════════
>
> Location:        Basement utility room (sub of main panel in garage)
> Manufacturer:    Cutler-Hammer BR series, 100A MLO, 24-ckt
> Voltage System:  120/240V 1Φ
> Bus Rating:      100 A         Main Breaker: MLO (fed from 100A 2P in main panel)
> AIC Rating:      10 kA         Fed From:     Main Panel A, Ckts 17/19 (100A 2P)
> Occupancy:       Dwelling
> NEC Cycle:       2023 (MA statewide; 527 CMR 12 supplemental rules)   AHJ: City of Boston ISD
>
> CIRCUIT DIRECTORY
>
> | Ckt# | A  | P | Type | Description              |  | Description              | Type | P | A  | Ckt# |
> |------|----|---|------|--------------------------|--|--------------------------|------|---|----|------|
> |  1   | 20 | 1 | AFCI | LR/DR Lts                |  | Kitchen Counter L (SAC)  | GFCI | 1 | 20 |  2   |
> |  3   | 20 | 1 | AFCI | BR1 Lts & Recepts        |  | Kitchen Counter R (SAC)  | GFCI | 1 | 20 |  4   |
> |  5   | 20 | 1 | AFCI | BR2 Lts & Recepts        |  | DW (Dedicated)           | AFCI | 1 | 20 |  6   |
> |  7   | 20 | 1 | AFCI | Bath Lts                 |  | Bath Recepts             | GFCI | 1 | 20 |  8   |
> |  9   | 30 | 2 | std  | Dryer (Dedicated)        |  | Hall Lts & Recepts       | AFCI | 1 | 20 | 10   |
> | 11   |    |   |      | (Dryer, lower pole)      |  | SPARE 15A                | std  | 1 | 15 | 12   |
> | 13   | 30 | 2 | HACR | A/C Condenser (Ded.)     |  | Garage Recepts           | GFCI | 1 | 20 | 14   |
> | 15   |    |   |      | (A/C, lower pole)        |  | SPARE 30A 2P             | std  | 2 | 30 | 16   |
> | 17   | 20 | 1 | GFCI | Outdoor Recepts          |  | (SPARE, lower pole)      |      |   |    | 18   |
> | 19   | 20 | 1 | AFCI | Smoke/CO Interconnect    |  | Bsmt Recepts             | GFCI | 1 | 20 | 20   |
> | 21   | 30 | 2 | std  | HPWH (Dedicated, NEW 4/26) |  | Bsmt Lts                 | AFCI | 1 | 20 | 22   |
> | 23   |    |   |      | (HPWH, lower pole)       |  | SPACE                    | —    | — | —  | 24   |
>
> Spaces:  1 (slot 24)
> Spares:  1 × 15A 1P (slot 12), 1 × 30A 2P (slots 16/18)
> MWBCs:   None
>
> ISSUES / FOLLOW-UP
> - **HPWH circuit (Ckts 21/23):** Confirmed 30 A 2P standard breaker on #10 AWG copper. Rheem Performance Platinum 50 gal nameplate is 30 A max — verify against the actual unit nameplate before final inspection.
> - **Sub-panel ground-bus / neutral-bus separation:** Confirm neutral is **isolated** from EGC at the sub-panel (4-wire feeder per §250.32). MA 527 CMR 12 supplemental rules echo this; some 1980s-era homes have the bond strap still in place from when the sub was installed as a main — verify visually.
> - **Sub-panel feed sized 100 A** but adding a 30 A 2P HPWH increases connected load. Current panel is approaching capacity. Run a load calc against the sub-feed (recommend `operations/nec-2026-load-calculation-helper.md` set to 2023-cycle Article 220 method) to verify the existing 100 A 2P upstream OCPD is still appropriate.
> - **AFCI on bedroom outlets** — already in place per the existing 2021 schedule; no AFCI gap.
> - **GFCI scope** — basement receptacles, bath, garage, outdoor, kitchen all GFCI per existing schedule. Code-current under NEC 2023.
>
> ARC-FLASH LABELING (non-dwelling panels)
> - NEC cycle in force: 2023
> - Label required:    **N/A — Dwelling occupancy.** §110.16(A) general warning labels at service equipment apply at the main panel upstream; this is a downstream sub-panel.
>
> §408.4 NOTE
> Circuit directories must legibly identify the purpose or use of each
> circuit. Labels must be durable for the environment. Print the updated
> directory and replace the 2021 directory inside the panel door.
>
> Documented by:  [Tech name]
> Date:           2026-04-25
> NEC cycle assumed: 2023 (MA statewide)
>
> ---
>
> **Internal Notes:**
> - Run the load calc before close-out — adding a 30 A 2P HPWH circuit on top of an existing 100 A sub may put the sub-feed near capacity, depending on heating equipment, range type, and existing demand.
> - MA 527 CMR 12 has supplemental dwelling-unit rules — verify nothing in this panel runs against an active state amendment (e.g., MA still requires AFCI on bedroom circuits via state amendment regardless of NEC; this sub already has it).
> - 2021 directory should be removed and replaced; do not stack two directories.
> - HPWH circuit description set to "(Dedicated, NEW 4/26)" — homeowner can ID the new circuit at a glance for warranty / service purposes.
> - No EV / EVSE / BESS scope; not applicable.

---

**Skill outputs are intended as documentation aids. The licensed electrician is responsible for verifying each circuit, the directory's legibility on the panel door, and the panel's compliance with the AHJ's adopted NEC cycle.**
