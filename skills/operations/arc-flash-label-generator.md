---
name: "Arc Flash Label Generator"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~20 min/label set"
version: 1.1
last_eval_score: 9.60
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
5. **Company info / footer block** — Company name, license number, assessor name/title, re-assessment-due date (typical: 5 years from assessment, or on material change). Pull from `config.yml` if available — the Labeling Preferences block below handles defaults.
6. **Jurisdiction and adoption year** — State/AHJ and which NEC cycle they have currently adopted (2020, 2023, or 2026). This changes the wording and whether the assessment-date line is mandatory.
7. **Label size & printer** *(optional if `config.yml.labeling` is populated)* — Target label dimensions (common: 4×6, 5×7, 3.5×5, 2×4 inches) and the printer / material. Defaults to `config.yml.labeling.default_stock_size` and `config.yml.labeling.default_printer` if not provided.
8. **Language(s)** *(optional if `config.yml.labeling.dual_language` is set)* — English only, or dual-language. Defaults to `config.yml.labeling.dual_language`; falls back to English-only if not configured.

## Instructions

You are an AI assistant helping an electrical contractor draft NEC 2026 §110.16-compliant arc flash hazard labels from an assessment already performed by a qualified person. You are not performing the arc flash risk assessment. You are formatting the results, checking them for obvious completeness problems, and producing print-ready label text.

**Before you start — Config-Driven Labeling Preferences Block:**

Load `config.yml` from the repo root and resolve the following labeling keys IN ORDER. If a key is missing, use the listed fallback and surface a one-line Internal Note recommending the user populate it.

| Config key | What it controls | Default if missing |
|---|---|---|
| `company.name` | "Installed by" footer line | "[Company Name]" placeholder + flag |
| `company.license_no` | "Installed by" footer line | "[License #]" placeholder + flag |
| `labeling.default_printer` | Label printer (Brady BMP71 / Panduit LS9EQ / Seton thermal / other) | "[Printer]" placeholder + flag |
| `labeling.default_stock_size` | Default label dimensions if intake field 7 not provided | Prompt user for label size |
| `labeling.standard_stock_spec` | Full material spec string (e.g., "Brady B-534 UV-resistant vinyl" / "Panduit C-Series laminated polyester" / "Seton #60280 laminated polyester") | "[Stock Spec]" placeholder + flag |
| `labeling.label_frame_style` | ANSI Z535.4 signal-word panel framing vs. IEEE 1584-2018 simplified label framing | `ANSI-Z535.4` (default; renders the full signal-word header + body + footer panel) |
| `labeling.dual_language` | Whether the firm always produces dual-language labels on multilingual job sites | `false` (English only unless intake field 8 overrides) |
| `labeling.assessor_name` | Default assessor name/title for the "Assessor" block | Prompt user |
| `labeling.reassess_interval_years` | Default re-assessment interval (typically 5) | 5 |

Apply resolved labeling preferences to every label draft. If intake fields 7 or 8 are explicitly provided, they override the config defaults. Never invent a label size, stock spec, or language choice — if both config and intake are silent on a field, flag it before drafting.

**Before you start — IEEE 1584-2018 Calculation-Method Gating Block:**

When the assessment method is an incident-energy study, capture the following before building label text. If the user did not provide them, ask before proceeding — the choice of IEEE 1584 edition, working distance, and clearing-time assumption affects the accuracy of the label's incident energy and PPE-category claims.

| Field | Why it matters |
|---|---|
| **IEEE 1584 edition used** (2002 or 2018) | The 2018 empirically-derived multi-bus-gap model produces different IE values than the 2002 simplified model for the same system. A label printed from a 2002 study re-used on a system now modeled under 2018 (or vice versa) is an audit failure. If the study was done under 2002 and the AHJ or site owner requires 2018, flag the label set "re-assessment required" before printing. |
| **Working distance assumed** | The label must state the working distance the IE value was calculated at (typically 18 in for LV, 36 in for MV 1–15 kV, 36–48 in for HV >15 kV). A label that says "7.2 cal/cm²" without a working distance is non-compliant. |
| **Voltage class** | LV (≤1000 V) / MV (>1000 V–15 kV) / HV (>15 kV). Shock-protection approach boundaries differ materially: LV limited approach 42 in, restricted 12 in; MV 4160 V limited 36 in, restricted 4 in; 13.8 kV limited 26 in, restricted 2 in. The label's shock-protection block must use the voltage-class-correct boundaries. |
| **Clearing-time assumption** | 15-cycle (OSHA 1910.333 / 1926.403) or 30-cycle (NFPA 70E 2024 §130.5 default). Some studies use a 15-cycle assumption for additional conservatism; the label's PPE category should reflect the clearing time the study actually used, not an assumed default. |
| **Two-event method used?** | IEEE 1584-2018 allows a two-event analysis for systems with backup protection (main breaker + upstream bus protective relay). If used, the label's IE value is the higher of the two events. Flag if the study uses two-event so the O&M summary captures it. |

Surface the IEEE 1584 gating results in the Internal Notes block, including the edition, working distance, voltage class, clearing-time basis, and whether two-event was used.

**Before you start — NEC cycle and knowledge-base references:**

- Reference `knowledge-base/regulations/nec-2026-key-changes.md` for the summary of §110.16(B) and (C), including the assessment-date requirement and the removal of the 1,000 A threshold for non-dwelling service/feeder equipment.
- Reference `knowledge-base/terminology/` for standard arc flash vocabulary.
- If the adopted NEC cycle is 2020 or 2023, use the threshold and wording rules that applied at that adoption — **do not** front-run the 2026 wording on a permit drawing or label for a jurisdiction that has not yet adopted. Flag the mismatch in your output. The assessment-date line is mandatory under NEC 2026 §110.16(B)/(C); it is *recommended but not mandated* under NEC 2023; it is *not required* under NEC 2020 (though it is best practice). State which applies in the label's Internal Notes.

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

## Worked Examples

---

### Example 1 — Commercial multi-equipment label set (480Y/277V, IE study, Oregon NEC 2026)

**Inputs:**

- Method: Incident-energy analysis (SKM PowerTools, firm: Northgate Engineering, PE stamp on file). IEEE 1584-2018 model. Working distance: 18 in. LV ≤1000 V. Clearing time: 30-cycle (NFPA 70E 2024 §130.5 default). Two-event method: not used.
- Assessment date: 2026-03-22. Re-assess by: 2031-03-22 or on material change.
- Jurisdiction: Oregon, NEC 2026 adopted 2026-04-01. Assessment-date line mandatory.
- Equipment:
  1. SWBD-1 (MDP, service equipment), Mechanical Rm. 120, 480Y/277V, IE = 6.8 cal/cm² @ 18 in, AFB = 45 in, PPE Cat 2.
  2. MCC-A (motor control center, feeder-supplied), Process Rm. 210, 480V, IE = 12.2 cal/cm² @ 18 in, AFB = 72 in, PPE Cat 3.
  3. Panel LP-2B (branch panel, feeder-supplied, 208Y/120V), Office 245, IE = 1.1 cal/cm² @ 18 in, AFB = 19 in, PPE Cat 1.
- Config: company = Torres Electric, license = EC-00842, `labeling.default_printer` = Brady BMP71, `labeling.standard_stock_spec` = Brady B-534 UV-resistant vinyl, `labeling.label_frame_style` = ANSI-Z535.4, `labeling.dual_language` = false.
- Label size: Brady 5×7 (overrides config default of 4×6).
- Language: English (matches config default).
- Assessor: J. Ramirez, PE (Northgate Engineering). Study ref: NG-2026-0311.

**Output:**

#### A. Label draft table

| Equipment | Label text block | Stock | Notes |
|---|---|---|---|
| **SWBD-1 — Mechanical Rm. 120** | ⚠ WARNING — Arc Flash and Shock Hazard — Appropriate PPE Required<br>Equipment: SWBD-1 — Mechanical Rm. 120<br>Nominal System Voltage: 480Y/277 V<br>Arc Flash Boundary: 45 in (3′-9″)<br>Incident Energy at 18 in: 6.8 cal/cm² — Min. PPE: Category 2<br>Limited Approach: 42 in — Restricted Approach: 12 in — Glove Class: 00<br>Risk Assessment Completed: 2026-03-22 — Re-assess by: 2031-03-22 or on material change<br>Assessor: J. Ramirez, PE (Northgate Engineering) — Method: IE study (SKM, IEEE 1584-2018) — Study Ref: NG-2026-0311<br>Installed by: Torres Electric — License #EC-00842 | Brady B-534 5×7 | Service equipment under NEC 2026 §110.16(B). Full-detail label required; generic sticker not acceptable. Assessment-date line mandatory (NEC 2026 adopted 2026-04-01). |
| **MCC-A — Process Rm. 210** | ⚠ WARNING — Arc Flash and Shock Hazard — Appropriate PPE Required<br>Equipment: MCC-A — Process Rm. 210<br>Nominal System Voltage: 480 V<br>Arc Flash Boundary: 72 in (6′-0″)<br>Incident Energy at 18 in: 12.2 cal/cm² — Min. PPE: Category 3<br>Limited Approach: 42 in — Restricted Approach: 12 in — Glove Class: 00<br>Risk Assessment Completed: 2026-03-22 — Re-assess by: 2031-03-22 or on material change<br>Assessor: J. Ramirez, PE (Northgate Engineering) — Method: IE study (SKM, IEEE 1584-2018) — Study Ref: NG-2026-0311<br>Installed by: Torres Electric — License #EC-00842 | Brady B-534 5×7 | Feeder-supplied equipment under §110.16(C). PPE Cat 3 requires arc-rated face shield + balaclava or arc-rated flash suit hood; arc-rated hood ≥25 cal/cm² recommended at 12.2 cal/cm² IE. |
| **LP-2B — Office 245** | ⚠ WARNING — Arc Flash and Shock Hazard — Appropriate PPE Required<br>Equipment: LP-2B — Office 245<br>Nominal System Voltage: 208Y/120 V<br>Arc Flash Boundary: 19 in (1′-7″)<br>Incident Energy at 18 in: 1.1 cal/cm² — Min. PPE: Category 1<br>Limited Approach: 42 in — Restricted Approach: 12 in — Glove Class: 00<br>Risk Assessment Completed: 2026-03-22 — Re-assess by: 2031-03-22 or on material change<br>Assessor: J. Ramirez, PE (Northgate Engineering) — Method: IE study (SKM, IEEE 1584-2018) — Study Ref: NG-2026-0311<br>Installed by: Torres Electric — License #EC-00842 | Brady B-534 5×7 | Feeder-supplied branch panel under §110.16(C). Low energy (PPE Cat 1) but assessment-date line still required — the most common NEC 2026 compliance gap on low-energy panels. |

#### B. Out-of-scope / flag list
None. All three pieces of equipment fall under NEC 2026 §110.16(B) or (C). No 110.16(A)-only equipment submitted.

#### C. Field-assessment gap list
None. All three labels have IE, working distance, boundary, PPE category, assessor, study reference, and assessment date.

#### D. O&M summary
Arc flash risk assessment for the SWBD-1 / MCC-A / LP-2B equipment set was performed by Northgate Engineering (J. Ramirez, PE) on 2026-03-22 using the incident-energy analysis method in SKM PowerTools (IEEE 1584-2018 model; study ref NG-2026-0311; 30-cycle clearing-time basis per NFPA 70E 2024 §130.5; two-event method not used). Labels conform to NEC 2026 §110.16(B)/(C) including the assessment-date requirement (adopted Oregon 2026-04-01). Per NFPA 70E, re-assessment is required on or before 2031-03-22, or sooner upon any material change to the upstream overcurrent protection, transformer, or conductor configuration. Brady B-534 UV-resistant vinyl label stock on Brady BMP71 (Torres Electric standard). Study file and PPE selection guide included in the O&M binder under Tab 8.

**Internal Notes:**
- IEEE 1584-2018 edition confirmed by study author; 30-cycle clearing-time basis per NFPA 70E 2024 §130.5; two-event method not used.
- Brady 5×7 override accepted; config default was 4×6. All three labels fit comfortably on 5×7 at 8 content lines each.
- `labeling.assessor_name` not in config — populated from intake (J. Ramirez, PE). Recommend adding to config for future label sets from the same study.
- Oregon adopted NEC 2026 on 2026-04-01 (verified via KB). Assessment-date line is mandatory.

---

### Example 2 — Residential 200 A service with detached garage (PPE-table method, NEC 2026, Cleveland OH)

**Inputs:**

- Method: PPE category table per NFPA 70E Table 130.7(C)(15)(a) — selected by Northgate Engineering for the detached garage MDO-1 (no IE study on file for this residential site).
- Assessment date: 2026-02-14. Re-assess by: 2031-02-14 or on material change.
- Jurisdiction: Cleveland OH, NEC 2026 adopted 2026-01-01. Assessment-date line mandatory.
- Equipment submitted:
  1. MSE-1 (200 A main service equipment), main dwelling interior utility room — 1-family dwelling.
  2. MDO-1 (200 A outdoor main disconnect, detached garage / accessory structure), 120/240V single-phase, PPE Cat 1, NFPA 70E Table 130.7(C)(15)(a) Row: "Panelboards or other equipment ≤240 V (does not include voltage testing)." Task: routine opening of the deadfront cover.
- Config: company = Torres Electric, license = EC-00842, `labeling.default_printer` = Brady BMP71, `labeling.standard_stock_spec` = Brady B-534 UV-resistant vinyl, `labeling.label_frame_style` = ANSI-Z535.4, `labeling.dual_language` = false.
- Label size: Brady 4×6 (config default).
- Language: English (config default).
- Assessor: T. Nguyen, PE (Northgate Engineering). Study ref: NG-2026-0218.

**Output:**

#### A. Label draft table

| Equipment | Label text block | Stock | Notes |
|---|---|---|---|
| **MDO-1 — Detached Garage** | ⚠ WARNING — Arc Flash and Shock Hazard — Appropriate PPE Required<br>Equipment: MDO-1 — Detached Garage<br>Nominal System Voltage: 120/240 V Single-Phase<br>Arc Flash Boundary: per NFPA 70E Table 130.7(C)(15)(a) for panelboard task<br>PPE Category: 1 — per NFPA 70E Table 130.7(C)(15)(a), Row: "Panelboards or other equipment ≤240 V." Field calculation of incident energy was not performed.<br>Limited Approach: 42 in — Restricted Approach: 12 in — Glove Class: 00<br>Risk Assessment Completed: 2026-02-14 — Re-assess by: 2031-02-14 or on material change<br>Assessor: T. Nguyen, PE (Northgate Engineering) — Method: PPE table — Study Ref: NG-2026-0218<br>Installed by: Torres Electric — License #EC-00842 | Brady B-534 4×6 | Feeder-supplied accessory structure main disconnect — non-dwelling occupancy, §110.16(C) applies. PPE Cat 1 per table method. |

#### B. Out-of-scope / flag list

| Equipment | Reason | Applicable marking |
|---|---|---|
| MSE-1 — Main dwelling interior utility room | **1-family dwelling service equipment.** NEC 2026 §110.16 exempts 1- and 2-family dwelling units. The main dwelling's interior service equipment does NOT require a full-detail §110.16(B)/(C) label. A general-purpose §110.16(A) "WARNING: Arc Flash and Shock Hazard" marking is still permitted and recommended. | §110.16(A) general marking (optional but recommended) |

> **Key distinction — detached garage vs. main dwelling:** The detached garage (MDO-1) is a *separate structure* that functions as a non-dwelling accessory; NEC 2026 does not exempt it from §110.16(B)/(C) even though it is on a 1-family residential property. The main dwelling's service entrance (MSE-1) is in the dwelling itself and is exempt. Labeling only MDO-1 is the correct scope — over-labeling MSE-1 with a full-detail label invites AHJ questions and is not required.

#### C. Field-assessment gap list
None. MDO-1 has PPE category, table row, assessor, study reference, and assessment date.

#### D. O&M summary
Arc flash risk assessment for the MDO-1 detached garage main disconnect was performed by Northgate Engineering (T. Nguyen, PE) on 2026-02-14 using the PPE category table method (NFPA 70E Table 130.7(C)(15)(a); study ref NG-2026-0218). Field calculation of incident energy was not performed; PPE Cat 1 was selected from the table for routine panelboard deadfront work at ≤240 V. Label conforms to NEC 2026 §110.16(C) including the assessment-date requirement (Cleveland OH adopted NEC 2026 on 2026-01-01). The main dwelling service equipment (MSE-1) is exempt from §110.16(B)/(C) as a 1-family dwelling service entrance; a general §110.16(A) marking is applied per Torres Electric standard practice. Per NFPA 70E, re-assessment is required on or before 2031-02-14, or sooner upon any material change to the upstream overcurrent protection or conductor configuration.

**Internal Notes:**
- NEC 2026 cycle confirmed for Cleveland OH (adopted 2026-01-01, KB verified). Assessment-date mandatory.
- PPE-table method is appropriate for this small residential accessory structure — a full IE study is not required and would not provide meaningful differentiation at this energy level.
- Main dwelling MSE-1 correctly scoped out — dwelling exemption applies. If the future buyer of this property converts the home to a multi-unit or changes the occupancy classification, re-evaluate §110.16(B)/(C) applicability on MSE-1 at that time.
- `labeling.assessor_name` not in config — populated from intake (T. Nguyen, PE, Northgate Engineering).

---

### Example 3 — Industrial 4160 V MCC (medium-voltage, ETAP IE study, Tacoma WA NEC 2020, dual-language)

**Inputs:**

- Method: Incident-energy analysis (ETAP, firm: Pacific Arc Consulting, PE stamp on file). IEEE 1584-2018 model. Working distance: 36 in (MV standard). MV voltage class (4160 V). Clearing time: 30-cycle (NFPA 70E 2024 §130.5). Two-event method: used (primary relay + upstream bus protective relay); label reflects the higher event.
- Assessment date: 2026-01-20. Re-assess by: 2031-01-20 or on material change.
- Jurisdiction: Tacoma WA, NEC 2020 adopted. Assessment-date line: not mandated under NEC 2020, but recommended (best practice) and required by the site owner's facility safety program.
- Equipment:
  1. MCC-EAST (4160 V motor control center, feeder-supplied from 13.8/4.16 kV transformer T-2), Process Room 110, IE = 31.4 cal/cm² @ 36 in, AFB = 144 in (12 ft), PPE Cat 4 (arc-rated flash suit ≥40 cal/cm² required).
- Config: company = Bristow Electric Co., license = WA-EC-0031892, `labeling.default_printer` = Panduit LS9EQ, `labeling.standard_stock_spec` = Panduit C-Series laminated polyester, `labeling.label_frame_style` = ANSI-Z535.4, `labeling.dual_language` = true (site labor profile; English + Spanish).
- Label size: Panduit 5×7 (matches config default for industrial stock).
- Language: English + Spanish (config dual_language = true).
- Assessor: R. Okwu, PE (Pacific Arc Consulting). Study ref: PAC-2026-0120.

**Output:**

#### A. Label draft table

| Equipment | Label text block (English / Spanish) | Stock | Notes |
|---|---|---|---|
| **MCC-EAST — Process Room 110** | **[English]** ⚠ WARNING — Arc Flash and Shock Hazard — Appropriate PPE Required / Equipment: MCC-EAST — Process Rm. 110 / Nominal System Voltage: 4160 V / Arc Flash Boundary: 144 in (12′-0″) / Incident Energy at 36 in: 31.4 cal/cm² — Min. PPE: Category 4 (Arc-Rated Flash Suit ≥40 cal/cm² Required) / Limited Approach: 36 in — Restricted Approach: 4 in — Glove Class: 2 / Risk Assessment Completed: 2026-01-20 — Re-assess by: 2031-01-20 or on material change / Assessor: R. Okwu, PE (Pacific Arc Consulting) — Method: IE study (ETAP, IEEE 1584-2018, two-event) — Study Ref: PAC-2026-0120 / Installed by: Bristow Electric Co. — License #WA-EC-0031892 **[Spanish]** ⚠ ADVERTENCIA — Peligro de Destello de Arco Eléctrico y Descarga — Se Requiere EPP Apropiado / Límite de Destello de Arco: 144 in (12′-0″) / Energía Incidente a 36 in: 31.4 cal/cm² — EPP Mínimo: Categoría 4 (Traje Ignífugo ≥40 cal/cm² Requerido) / Evaluación Completada: 2026-01-20 — Próxima Evaluación: 2031-01-20 o ante cambio de material | Panduit C-Series 5×7 | MV feeder-supplied equipment, §110.16(C). PPE Cat 4: full arc-rated flash suit ≥40 cal/cm², arc-rated balaclava, face shield rated ≥40 cal/cm², Class 2 voltage-rated gloves (≥17 kV), dielectric footwear, hearing protection. Arc-flash boundary 12 ft — barricade required before any approach. MV shock-protection boundaries applied (4160 V): limited 36 in, restricted 4 in vs. LV values of 42 in / 12 in. |

#### B. Out-of-scope / flag list
None. MCC-EAST is feeder-supplied industrial equipment under §110.16(C). No dwelling-exempt or non-covered equipment submitted.

#### C. Field-assessment gap list
None. MCC-EAST has IE, working distance, boundary, PPE category, IEEE 1584 edition, two-event confirmation, assessor, study reference, and assessment date.

#### D. O&M summary
Arc flash risk assessment for the MCC-EAST 4160 V motor control center was performed by Pacific Arc Consulting (R. Okwu, PE) on 2026-01-20 using the incident-energy analysis method in ETAP (IEEE 1584-2018 model, two-event analysis; study ref PAC-2026-0120; 30-cycle clearing-time basis per NFPA 70E 2024 §130.5). Incident energy at 36 in is 31.4 cal/cm² (the higher of the two events). PPE Category 4 is required for any task within the arc-flash boundary. The arc-flash boundary of 144 in (12 ft) requires a physical barricade before any qualified person approaches the MCC. Labels are bilingual (English + Spanish) per site labor profile, printed on Panduit C-Series laminated polyester using the Panduit LS9EQ (Bristow Electric Co. standard). Per NFPA 70E, re-assessment is required on or before 2031-01-20, or sooner upon any material change to the 13.8/4.16 kV transformer T-2, the upstream relay coordination, or the MCC bus configuration. NEC 2020 is the adopted cycle at Tacoma WA; the assessment-date line is not mandated under NEC 2020 but is required by the site owner's facility safety program and is included on all labels.

**Internal Notes:**
- IEEE 1584-2018 edition and two-event method confirmed by study author (PAC-2026-0120). The higher event value (31.4 cal/cm²) is used on the label. The lower event value (18.8 cal/cm²) applies under normal relay coordination and is documented in the study file; the label uses the conservative two-event value per site owner's requirement.
- NEC 2020 cycle confirmed for Tacoma WA (KB verified). Assessment-date line included per site-owner facility safety program (not yet mandated by NEC 2020 but required by the site owner). Document the owner requirement in the O&M package so the future facility manager understands why the date appears on a 2020-cycle label.
- Dual-language: English header / full body + Spanish header + key safety lines (arc-flash boundary, IE, PPE category, assessment date). Numeric values non-translated. Panduit 5×7 stock holds all content at legible font size.
- MV shock-protection boundaries applied (4160 V per NFPA 70E Table 130.4(D)(a)): Limited 36 in, Restricted 4 in, Prohibited 1 in. Do NOT use the LV 480 V boundaries (42 in / 12 in) on this label.

---

_Disclaimer: This output is a formatting aid. The incident energy, arc flash boundary, and PPE category values must come from a qualified person's arc flash risk assessment per NFPA 70E. This skill does not calculate those values. Verify the label text against the governing NEC cycle adopted by your AHJ before printing._
