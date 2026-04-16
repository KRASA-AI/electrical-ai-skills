---
name: "Arc Flash Label Generator"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~20 min/label set"
version: 1.0
last_eval_score: null
---

# ⚡ Arc Flash Label Generator

## Purpose

Generate NEC 2026 §110.16-compliant arc flash hazard labels for service equipment, feeders, and other covered gear. Under the 2026 NEC, generic "WARNING — Arc Flash Hazard" stickers are no longer acceptable for most commercial and industrial installations: each label must carry specific information tied to an incident-energy analysis or PPE category table, and every label must be durable, field-applied, and dated to the assessment — not the print date.

This skill turns a short intake (equipment list + study data or PPE-table choice) into a set of label drafts ready to send to a sign printer or label-maker. It also produces a labeling summary a project manager can attach to the O&M package, and it flags equipment that looks out of scope or under-studied before you print.

This is a drafting and check-your-work skill, not a substitute for a qualified person's arc flash risk assessment under NFPA 70E.

## When to Use

Use this skill when you need to:

- Produce labels for new service equipment, switchboards, panelboards, industrial control panels, motor control centers, meter sockets, or feeder-supplied equipment in non-dwelling occupancies
- Re-label equipment during a service upgrade, retrofit, or after a 5-year (or material-change) re-assessment required by NFPA 70E
- Translate incident-energy study output (from SKM, EasyPower, ETAP, Arcad, etc.) into the exact wording required by NEC 110.16(B) / (C)
- Label equipment where the owner has chosen the PPE category table method from NFPA 70E Table 130.7(C)(15)(a) or (b) instead of a full incident-energy analysis
- Generate an "assessment date included" block so the label complies with NEC 2026's new date-of-assessment requirement
- Spot-check an existing label set before handing off to QA or the AHJ
- Produce bilingual (English + Spanish, or English + French) labels for multi-language job sites

Do not use this skill to bypass a qualified person's assessment. The incident energy and boundary values must come from a study or table selection that a qualified person has signed off on. This skill formats and checks; it does not calculate.

## Required Input

Provide the following:

1. **Assessment method** — Incident-energy analysis (with the engineering firm's name and study date) **or** PPE category table method per NFPA 70E Table 130.7(C)(15). If IE study, the software package is helpful to know.
2. **Assessment date** — The date the arc flash risk assessment was completed. This is the field NEC 2026 now requires on the label (no longer just the print date).
3. **Equipment list** — For each piece of equipment: designation (e.g., "SWBD-1", "MCC-A", "LP-2B"), location (building/room), nominal system voltage, and whether it is service equipment, feeder-supplied equipment, or other covered gear. A panel schedule or one-line excerpt works.
4. **Per-equipment study data** — For each piece: available incident energy (cal/cm²) at the working distance, working distance (inches), arc flash boundary (inches or feet), and minimum arc-rated PPE category or site-specific PPE description. If PPE-table method, the PPE category (1–4) and the task-vs-equipment row used.
5. **Company info / footer block** — Company name, license number, assessor name/title, re-assessment-due date (typical: 5 years from assessment, or on material change). Pull from `config.yml` if available.
6. **Jurisdiction and adoption year** — State/AHJ and which NEC cycle they have currently adopted (2020, 2023, or 2026). This changes the wording and whether the assessment-date line is mandatory.
7. **Label size & printer** — Target label dimensions (common: 4×6, 5×7, 3.5×5, 2×4 inches) and the printer / material (Brady, Seton, Panduit thermal-transfer vinyl, laminated polyester, etc.). Different stock sizes pull different field layouts.
8. **Language(s)** — English only, or dual-language. Default: English.

## Instructions

You are an AI assistant helping an electrical contractor draft NEC 2026 §110.16-compliant arc flash hazard labels from an assessment already performed by a qualified person. You are not performing the arc flash risk assessment. You are formatting the results, checking them for obvious completeness problems, and producing print-ready label text.

**Before you start:**

- Load `config.yml` from the repo root for the company's legal name, license number, and default assessor block.
- Reference `knowledge-base/regulations/nec-2026-key-changes.md` for the summary of §110.16(B) and (C), including the assessment-date requirement and the removal of the 1,000 A threshold for non-dwelling service/feeder equipment.
- Reference `knowledge-base/terminology/` for standard arc flash vocabulary.
- If the adopted NEC cycle is 2020 or 2023, use the threshold and wording rules that applied at that adoption — **do not** front-run the 2026 wording on a permit drawing or label for a jurisdiction that has not yet adopted. Flag the mismatch in your output.

**Process:**

1. Read the full intake. If any required field is missing, list them and stop. Do **not** invent incident energy, boundary, or PPE values.
2. For each piece of equipment, decide which NEC section and method applies:
   - **110.16(B) — Service equipment, 2026 NEC, non-dwelling:** full detail label (nominal voltage, arc flash boundary, incident energy OR required PPE level, date of assessment).
   - **110.16(C) — Other equipment (feeders, panelboards, switchboards, IC panels, MCCs) likely to require examination while energized, 2026 NEC, non-dwelling:** full detail label, same content.
   - **110.16(A) — General marking "WARNING: arc flash and shock hazard":** only applies where (B)/(C) do not.
3. Build the per-label content block:
   - **Header:** "⚠ WARNING — Arc Flash and Shock Hazard — Appropriate PPE Required"
   - **Equipment ID line:** "Equipment: {designation} — {location}"
   - **Voltage line:** "Nominal System Voltage: {volts} V"
   - **Arc flash boundary line:** "Arc Flash Boundary: {distance} in ({feet}' {inches}\")"
   - **Energy/PPE line (pick one style, based on assessment method):**
     - IE method: "Incident Energy at {working-distance} in: {cal/cm²} cal/cm² — Minimum PPE: {category or site-specific description}"
     - PPE-table method: "PPE Category: {1–4} — per NFPA 70E Table 130.7(C)(15)({a or b}), {task/equipment row}. Field calculation of incident energy was **not** performed."
   - **Shock protection block:** "Limited Approach: {in} — Restricted Approach: {in} — Glove Class: {0/00/1/2/3/4}"
   - **Assessment date line:** "Risk Assessment Completed: {YYYY-MM-DD}  |  Re-assess by: {YYYY-MM-DD or 'on material change'}"
   - **Assessor block:** "Assessor: {name, title}  |  Method: {IE study / PPE table}  |  Study Ref: {study ID or software}"
   - **Owner / installer block:** "Installed by: {company name, license #}"
4. Format for the specified label size. If the chosen stock cannot hold all required fields legibly (rough rule: ≤ 8 lines on a 4×6, ≤ 6 lines on 3.5×5, ≤ 4 lines on 2×4), flag it and suggest the next size up or a two-panel label layout.
5. If dual-language was requested, render the warning header, boundary, PPE line, and assessment-date line in both languages; keep numeric values non-translated.
6. Produce four deliverables in this order:
   - **A. Label draft table** — one row per piece of equipment, columns: designation, label text block, recommended stock size, special notes.
   - **B. Out-of-scope / flag list** — equipment the intake included that does not require a 110.16(B)/(C) label (e.g., residential service for a 1–2 family dwelling; general-purpose 120 V receptacle circuit in a non-covered location). For those, note why and whether a 110.16(A) general marking still applies.
   - **C. Field-assessment gap list** — any equipment where required data is missing (no IE value, no boundary, stale assessment date, no assessor name). The user must go back to the study author for these.
   - **D. O&M summary paragraph** — 1–2 paragraph plain-English summary the PM can drop into the project closeout package, listing the assessment method, assessor, date, software (if IE), and re-assessment interval.
7. End the response with a disclaimer block: "This output is a formatting aid. The incident energy, arc flash boundary, and PPE category values must come from a qualified person's arc flash risk assessment per NFPA 70E. This skill does not calculate those values. Verify the label text against the governing NEC cycle adopted by your AHJ before printing."

**What to flag aggressively:**

- Assessment dates older than 5 years (NFPA 70E re-assessment interval is now codified at 5 years or upon material change).
- Equipment labeled "service equipment" but at a 1- or 2-family dwelling — NEC 2026 §110.16 still exempts dwellings, and over-labeling invites callbacks.
- Incident-energy values rounded to whole numbers with no working-distance stated.
- PPE-table method applied to equipment whose parameters fall outside the table's assumed short-circuit current / clearing time bounds.
- Any label that lacks the assessment date under NEC 2026 — this is the single most common compliance gap reported by AHJs.

## Example Output

**Inputs (abbreviated):**

- Method: Incident-energy analysis (SKM, firm: Northgate Engineering, PE stamp on file), assessment date 2026-03-22.
- Jurisdiction: Oregon, 2026 NEC adopted 2026-04-01.
- Equipment:
  1. SWBD-1 (MDP, service equipment), Mechanical Rm. 120, 480Y/277V, IE = 6.8 cal/cm² @ 18", AFB = 45", PPE Cat 2.
  2. MCC-A (motor control center, feeder-supplied), Process Rm. 210, 480V, IE = 12.2 cal/cm² @ 18", AFB = 72", PPE Cat 3.
  3. Panel LP-2B (branch panel, feeder-supplied, 208Y/120V), Office 245, IE = 1.1 cal/cm² @ 18", AFB = 19", PPE Cat 1.
- Label stock: Brady 5×7 thermal-transfer polyester, English only.
- Assessor: J. Ramirez, PE.
- Re-assess by: 2031-03-22 or on material change.

**Output:**

### A. Label draft table

| Equipment | Label text block | Stock | Notes |
|---|---|---|---|
| **SWBD-1 — Mechanical Rm. 120** | ⚠ WARNING — Arc Flash and Shock Hazard — Appropriate PPE Required<br>Equipment: SWBD-1 — Mechanical Rm. 120<br>Nominal System Voltage: 480Y/277 V<br>Arc Flash Boundary: 45 in (3'-9")<br>Incident Energy at 18 in: 6.8 cal/cm² — Min. PPE: Category 2<br>Limited Approach: 42 in — Restricted Approach: 12 in — Glove Class: 00<br>Risk Assessment Completed: 2026-03-22 — Re-assess by: 2031-03-22 or on material change<br>Assessor: J. Ramirez, PE — Method: IE study (SKM) — Study Ref: NG-2026-0311<br>Installed by: {company name, license #} | 5×7 | Service equipment under 110.16(B), 2026 NEC. Full detail required; generic sticker not acceptable. |
| **MCC-A — Process Rm. 210** | (same structure; 480V; AFB 72 in; IE 12.2 cal/cm² @ 18 in; PPE Cat 3) | 5×7 | Feeder-supplied equipment under 110.16(C). PPE Cat 3 triggers arc-rated face shield + balaclava or flash suit hood. |
| **LP-2B — Office 245** | (same structure; 208Y/120V; AFB 19 in; IE 1.1 cal/cm² @ 18 in; PPE Cat 1) | 5×7 | Feeder-supplied branch panel under 110.16(C). Low energy but assessment-date line still required. |

### B. Out-of-scope / flag list
None in this set. All three pieces of equipment fall under NEC 2026 §110.16(B) or (C) and require full-detail labels. No 110.16(A)-only equipment was submitted.

### C. Field-assessment gap list
None. Each piece has IE, working distance, boundary, PPE category, assessor, and assessment date.

### D. O&M summary
Arc flash risk assessment for the SWBD-1 / MCC-A / LP-2B equipment set was performed by Northgate Engineering (J. Ramirez, PE) on 2026-03-22 using the incident-energy analysis method in SKM PowerTools (study ref NG-2026-0311). Labels conform to NEC 2026 §110.16(B)/(C) including the new assessment-date requirement. Per NFPA 70E, re-assessment is required on or before 2031-03-22, or sooner upon any material change to the upstream overcurrent protection, transformer, or conductor configuration. The study file and field PPE selection guide have been included in the O&M binder under Tab 8.

---

_Disclaimer: This output is a formatting aid. The incident energy, arc flash boundary, and PPE category values must come from a qualified person's arc flash risk assessment per NFPA 70E. This skill does not calculate those values. Verify the label text against the governing NEC cycle adopted by your AHJ before printing._
