---
name: "Inspection Report Generator"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/report"
version: 1.1
last_eval_score: 9.40
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
- Load `config.yml` from the repo root for:
  - `company.legal_name`, `company.license_number`, `company.contact`
  - `default_jurisdiction.nec_cycle` (the cycle the firm normally works under) and `default_jurisdiction.citation_format` (full §X.Y.Z(letter)(number) vs. abbreviated; cycle-prefixed vs. unprefixed)
  - `default_jurisdiction.state_amendments` (e.g., MA 527 CMR 12, WA WAC 296-46B, OR OAR 918-300)
  - `inspectors[]` — list of staff inspectors (name, license, signature line) for the report header
- **Confirm the AHJ-adopted NEC cycle for THIS property** — the firm's default cycle and the AHJ's cycle are not always the same. Ask the user (or pull from the work order) which cycle applies; do not assume the firm's default.
- Reference `knowledge-base/regulations/nec-2026-key-changes.md` to identify which findings are 2026-only and need cycle-aware gating
- Reference `knowledge-base/regulations/` for NEC article citations
- Reference `knowledge-base/terminology/` for standard trade terminology

### NEC Cycle Check (run BEFORE writing any flag)

The skill **never** flags a finding as a 🟡 Code Violation against an NEC section the AHJ has not adopted. The mismatch between the firm's default cycle and the AHJ's cycle is the most common source of false 🟡 flags in inspection reports — reports get corrected by the AHJ, the firm looks unprepared, and the customer loses confidence. Apply this gate to every finding before it becomes a flag:

**Cycle-sensitive provisions to gate:**

| Provision | 2017 | 2020 | 2023 | 2026 |
|-----------|------|------|------|------|
| §210.8(F) outdoor branch GFCI | — | Required (with exceptions) | Required | Required (carve-outs added) |
| §210.12 AFCI extension to all dwelling 120 V | Limited rooms | Most habitable | Most habitable | Most habitable |
| §210.12(D) DFCI (dual-function leakage detection) | — | — | — | **2026 only** |
| §230.85 Emergency disconnect, dwelling services | — | Required ≤ 400 A | Required ≤ 400 A | Required all sizes (§230.85(B)(2) outdoor) |
| §240.67 / §240.87 ARMS / energy-reducing maintenance switching | ≥ 1,200 A (§240.87) | ≥ 1,200 A | ≥ 1,200 A | **2026 lowers threshold** to certain ≥ 800 A scenarios; verify exact text |
| §220 → §120 Article migration | §220 | §220 | §220 | **§120 (renumbered)** |
| §230.67 SPD on dwelling services | — | Required | Required | Required |
| §706 Energy Storage Systems | §706 (limited) | §706 expanded | §706 expanded | §706 expanded with battery-management provisions |
| §625.42(B) EV EVEMS / managed-load | Limited | Permitted | Permitted | **Expanded — load-management presumption** |

**Severity gating rule:** If a finding implicates a 2026-only provision and the AHJ is on 2020 / 2023, the finding is auto-downgraded from 🟡 Code Violation to 🔵 Recommendation, with a footnote: "*This finding would be a 🟡 Code Violation under NEC 2026 §X. The [AHJ] has adopted NEC [cycle]; surfaced as a recommendation only.*" The skill never produces a 🟡 against an unadopted code section.

**State-amendment overlay:** Where the state has adopted amendments that supersede the base NEC (MA 527 CMR 12, WA WAC 296-46B, OR OAR 918-300, NJ NJAC 5:23-3.16, etc.), apply the amendment language and cite both. Pull the amendment list from `default_jurisdiction.state_amendments`.

### Panel-History Flag Library

When the inspection encounters a known-problematic panel manufacturer, surface it with the right severity and language. **Flag, do not assert "must replace."** The contractor's job in the report is to inform — replacement is the property owner's decision after consultation with their insurer, the AHJ, and a licensed contractor.

| Panel | Period | Issue | Severity | Language |
|-------|--------|-------|----------|----------|
| Federal Pacific Stab-Lok | 1950s–1980s | Documented breaker non-trip failures; CPSC investigation closed 1983 inconclusive but field-failure rate elevated | 🟡 Recommendation (insurance carriers often decline coverage; some carriers require replacement) | "FPE Stab-Lok panel installed. Independent field-failure data and several insurance carriers' underwriting positions warrant evaluation. Owner should consult their property insurer regarding coverage implications. Replacement assessment recommended." |
| Zinsco / Sylvania | 1950s–1970s | Aluminum bus corrosion; breaker-to-bus failure | 🟡 Recommendation | "Zinsco panel installed. Aluminum-bus corrosion is documented in this manufacturer line and may compromise breaker connection integrity. Replacement assessment recommended." |
| Pushmatic (Bulldog) | 1960s–1970s | Breakers no longer manufactured; difficulty sourcing replacements; some breakers fail to trip on overload | 🔵 Recommendation | "Pushmatic panel installed. Replacement breakers are scarce. Servicing future faults will require panel-level work." |
| Challenger pre-1988 | 1980s | HACR breaker recalled by CPSC for thermal failure | 🟡 Recommendation | "Challenger panel pre-1988 installed; CPSC-recalled HACR breakers may be present. Identification and replacement of any recalled breakers recommended." |
| GE original 1980s mini-breakers | 1980s | Some half-size breakers had documented bus-stab failures | 🔵 Recommendation | "GE half-size breakers in legacy panel. Tighten and torque to manufacturer spec; assess for arcing signatures." |

**Never assert:** "this panel must be replaced," "your insurance will be cancelled," "this is a fire hazard." All three create liability and may be inaccurate.

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
- **Cite the cycle the AHJ has adopted, not the cycle the firm normally works under.** Pull `default_jurisdiction.citation_format` from config — some AHJs read full §X.Y.Z(letter)(number); others read abbreviated. Some AHJs prefix with the cycle ("NEC 2023 §210.12(A)"); others drop the prefix.
- Cross-reference the state amendment where applicable (e.g., "NEC 2023 §210.12(A); MA 527 CMR 12 amendment retains").
- Adjust technical language for the audience — homeowner reports should explain terms; GC/inspector reports can use trade shorthand
- Use severity color codes consistently
- If the user provides rough notes, ask clarifying questions only if critical information is missing (location, what they saw). Otherwise, make reasonable inferences and note assumptions.

### Photo Reference Log standardization

Every photo referenced in a finding must:

- Have a stable identifier ("PHOTO-001," "PHOTO-002," etc., used in the finding's location field)
- Have a one-line description: WHAT, WHERE, WHEN (e.g., "PHOTO-014: Main panel interior — double-tap on Sq.D HOM 20A breaker — 2026-04-27 11:42")
- Note any EXIF preservation (orientation, timestamp, GPS where available — preservation matters for legal review)
- Use lowercase / numeric naming with dash separators when filed (PHOTO-014.jpg) — easier to grep and reference
- Be re-linked from the Findings table by ID (column header "Photo Ref")

If photos are not yet uploaded into the report, list them as "PENDING" and produce the report with placeholders so the field tech can attach later without re-running the skill.

## Worked Examples

### Example 1 — Rough-In Inspection, Residential Addition, Austin TX (NEC 2020)

**Property:** 4127 Live Oak Ln, Austin TX. Single-family residential addition: 380 sf primary-suite expansion, new bath, new walk-in closet. Panel: existing 200 A Square D QO main, 30/40 spaces, 14 spaces available. AHJ: City of Austin Development Services. **AHJ-adopted NEC cycle: 2020** (Texas does not have a statewide adoption; cities adopt independently — Austin is on 2020 as of 2026-04-27).

**Field Notes (raw):**
- All NM-B 12/2 staples within 12" of boxes ✓
- Bedroom AFCI on new circuit ✓
- New bath GFCI/AFCI dual-function ✓
- Closet light is a 65W LED — 12" clearance to top shelf, 4" sidewall — within NEC 410.16 ✓
- Outdoor receptacle on rear patio — single-use GFCI; no DFCI installed — Note for AHJ
- Service emergency disconnect: existing 200 A panel is the disco, mounted indoor in the garage (panel was installed 2018 under NEC 2014); new addition does not change service
- New bath fan / light on a 15A AFCI — bath fan rating verified
- Two new exterior wall sconces at rear patio — both AFCI'd

**Generated Report (abbreviated):**

**Report:** Electrical Rough-In Inspection — 4127 Live Oak Ln, Austin TX
**Inspector:** Marisol Vega, License TX-EC-19847 (Krasa Electric, LLC, License TX-EC-9921)
**Date:** 2026-04-27
**Type:** Rough-in inspection (residential addition)
**NEC cycle:** Adopted by City of Austin = NEC 2020. Cited herein.
**Report #:** INS-20260427-014

**Executive Summary:** Rough-in is substantially compliant with NEC 2020 as adopted by City of Austin. Two findings flagged: (1) outdoor receptacle without DFCI, surfaced as a 🔵 Recommendation rather than 🟡 (DFCI is a 2026-only provision; not adopted in Austin); (2) one open junction box flagged 🔴 as safety hazard. Total: 1 🔴, 0 🟡, 2 🔵. Recommend correction of safety hazard before drywall.

| # | Location | Finding | Severity | Code Ref | Action | Photo |
|---|----------|---------|----------|----------|--------|-------|
| 1 | Primary closet ceiling | Open NM cable splice in junction box; cover not yet installed | 🔴 Safety Hazard | NEC 2020 §314.25(A) | Install cover plate before close-up | PHOTO-007 |
| 2 | Rear patio receptacle | GFCI installed; DFCI not installed | 🔵 Recommendation | NEC 2020 §210.8(F); footnote: DFCI per NEC 2026 §210.12(D) — 2026-only provision; Austin has not adopted NEC 2026; surfaced as recommendation only | Optional: install dual-function leakage detection device when AHJ adopts 2026 | PHOTO-013 |
| 3 | Service entrance | Existing 200 A main is service disco; per NEC 2014 install (compliant at install date); §230.85(B) emergency-disco-outdoor would apply if service is replaced under NEC 2020+ | 🔵 Recommendation | NEC 2020 §230.85(B) | Defer; no service replacement in current scope | — |

**Note:** Finding #2 demonstrates the cycle-aware severity gate. Under NEC 2026 §210.12(D), DFCI on outdoor branch circuits is a code requirement. Austin has not adopted NEC 2026 as of the inspection date. The finding is surfaced as a 🔵 Recommendation, not a 🟡 Code Violation. Calling it a 🟡 here would have produced an AHJ pushback at sign-off.

---

### Example 2 — Final Inspection, Commercial TI, Boston MA (NEC 2023)

**Property:** 75 Federal St, Suite 402, Boston MA. 2,840 sf law-firm tenant fit-out: 32 receptacles, conference-room AV, server closet with 4-ton cooling, dedicated 30 A 240 V for an under-counter coffee/espresso line, ATS for the building's life-safety branch (existing). AHJ: Boston Inspectional Services Department. **AHJ-adopted NEC cycle: 2023** (MA adopts NEC 2023 with 527 CMR 12 amendments).

**Field Notes (raw):**
- All AFCI / GFCI per code ✓
- Server closet GFCI-outlets per kitchen-equivalent rule ✓
- Receptacle on upper-cabinet face for coffee-line is at 38" — review height vs. ADA reach
- Working clearance to TI panel measured 30" — 6" short of §110.26(A)(1) for 480 V — wait, panel is 208/120 V — re-check; 30" is min for 208 V ✓
- Server closet receptacles: dedicated 20 A AFCI, but circuit feeding two server racks — measure 13.6 A continuous — within §210.20 80% rule ✓
- AV plate in conference room: 2 receptacles + HDMI plate; load-balanced ✓
- ATS for life-safety: not in scope of TI; existing
- New emergency lighting in egress corridor: 90-min battery backup verified ✓

**Generated Report (abbreviated):**

**Report:** Final Electrical Inspection — 75 Federal St Ste 402, Boston MA
**Inspector:** Patrick O'Donnell, License MA-A-21885 (Krasa Electric, LLC, License MA-A-7142)
**Date:** 2026-04-27
**Type:** Final inspection (commercial TI)
**NEC cycle:** Adopted by MA = NEC 2023, with 527 CMR 12 amendments.
**Report #:** INS-20260427-015

**Executive Summary:** Final inspection passes with 1 🟡 Code Violation correction-before-occupancy and 2 🔵 Recommendations. The TI panel passes working-clearance check. The coffee-line receptacle height does not meet ADA 4 reach range and is flagged for relocation.

| # | Location | Finding | Severity | Code Ref | Action | Photo |
|---|----------|---------|----------|----------|--------|-------|
| 1 | Kitchenette receptacle | Upper-cabinet receptacle face at 38" AFF; ADA reach max is 48" forward / 54" side — 38" within reach but **not** within "operable parts" 15"–48" zone for kitchen counter receptacle per ADA 308; not strictly NEC, but 527 CMR 12 cross-references state-accessibility code | 🟡 Code Violation (state amendment) | 527 CMR 12 amendment cross-reference; 521 CMR (MA accessibility) §17.6 | Relocate to 44" AFF or relocate to face of base cabinet at 18" AFF | PHOTO-006 |
| 2 | Server closet | One 20 A AFCI feeds two server racks; load 13.6 A continuous (under 80% threshold) but operating headroom is thin | 🔵 Recommendation | NEC 2023 §210.20 | Consider splitting onto a second 20 A circuit to add headroom | PHOTO-009 |
| 3 | Egress corridor emergency lighting | 90-minute battery backup verified; recommend annual functional test logged | 🔵 Recommendation | NEC 2023 §700.3(B); MA 527 CMR 12 amendment | Add annual test entry to PM matrix | PHOTO-012 |

**State amendment cross-reference:** MA 527 CMR 12 retains NEC 2023 §210.8 GFCI provisions and adds the 521 CMR accessibility cross-reference for kitchen receptacles in commercial spaces. Cited per `default_jurisdiction.citation_format`.

---

### Example 3 — Insurance Pre-Purchase Evaluation, 1962 Ranch, Cleveland OH (NEC 2017)

**Property:** 18421 Lake Shore Blvd, Cleveland OH. 1,640 sf 1962 ranch on slab; pre-purchase insurance evaluation requested by buyer's insurance carrier. AHJ: City of Cleveland Department of Building & Housing. **AHJ-adopted NEC cycle: 2017** (Ohio adopts NEC 2017 statewide; Cleveland on 2017).

**Field Notes (raw):**
- Service panel: Federal Pacific Stab-Lok 100 A, 20-space — original to home
- Branch circuits: aluminum 12 AWG and 14 AWG single-strand on most circuits; copper on kitchen-remodel-era circuits added ~1985
- Most receptacles two-prong ungrounded; six receptacles grounded type (kitchen / 1985 remodel zone) but 2 of those test as "open ground" with circuit-tester
- No GFCI in kitchen, bath, garage, outdoor — original wiring throughout most of house
- Bathroom: one ungrounded receptacle 14" from bathtub — no GFCI
- Bonding: water-pipe bond present but corroded; appears intact
- Service-entrance: weatherhead sealant deteriorated; mast plumb
- No AFCI anywhere
- Smoke alarms: battery-only, four units, ages unknown
- Bedroom #2: ceiling-fan box rated for fan support ✓ (recent replacement)

**Generated Report (abbreviated):**

**Report:** Pre-Purchase Electrical Evaluation — 18421 Lake Shore Blvd, Cleveland OH
**Inspector:** James Whitaker, License OH-EC-15992 (Krasa Electric, LLC, License OH-EC-4218)
**Date:** 2026-04-27
**Type:** Pre-purchase insurance evaluation (residential)
**NEC cycle:** Cleveland adopts NEC 2017. Cited herein. *House was built under NEC 1959 and most original wiring is grandfathered; this report identifies non-compliance with current code as a material decision point for the buyer and insurer, not as a citation against the seller.*
**Report #:** INS-20260427-016

**Executive Summary:** This 1962 home retains original branch-circuit wiring on most circuits, an FPE Stab-Lok service panel, predominantly ungrounded two-prong receptacles, and no GFCI / AFCI protection except where added in the 1985 kitchen remodel. The home is grandfathered to its 1959 NEC; however, several conditions warrant insurance-carrier review and prospective remediation. **Insurance-carrier note:** Several major homeowner-insurance carriers decline or load on FPE Stab-Lok panels and aluminum branch wiring; the buyer should confirm coverage availability with their carrier before close.

Total: 3 🟡 Code Violations (current code; grandfather analysis applies), 5 🔵 Recommendations.

| # | Location | Finding | Severity | Code Ref | Action | Photo |
|---|----------|---------|----------|----------|--------|-------|
| 1 | Service panel | Federal Pacific Stab-Lok 100 A panel, original installation. Field-failure data and several insurance carriers' underwriting positions warrant evaluation. | 🟡 Recommendation (Panel-History Flag Library; not a current-NEC violation but material to insurance) | — | Owner should consult their property insurer regarding coverage. Replacement assessment recommended. **The skill does NOT assert "must replace" or "insurance will be cancelled."** | PHOTO-001 |
| 2 | Branch circuits | Aluminum single-strand 12 AWG and 14 AWG branch wiring on most pre-1985 circuits | 🟡 Recommendation | — (not a current-NEC violation; grandfathered) | CO/ALR-listed devices recommended at every termination; AlumiConn or Copalum repair where retrofitting; insurance-carrier review | PHOTO-003 |
| 3 | Kitchen, bath, garage, outdoor receptacles | No GFCI protection on receptacles per current NEC 2017 §210.8(A) | 🟡 Code Violation (current code; grandfathered, but flagged for insurance and buyer) | NEC 2017 §210.8(A) | Install GFCI receptacles or GFCI breakers on affected circuits as part of remediation | PHOTO-005, 006, 007 |
| 4 | Bath receptacle | Receptacle 14" from bathtub edge; ungrounded; no GFCI | 🟡 Code Violation | NEC 2017 §210.8(A)(1); §406.4(D) for grounding-type retrofit | Replace with GFCI receptacle (NEC 2017 §406.4(D)(2)(b) permits GFCI on ungrounded circuit with "No Equipment Ground" sticker as compliance path), and GFCI-protect | PHOTO-008 |
| 5 | Receptacles tested | 2 of 6 grounded-type receptacles in 1985 kitchen-remodel zone test as "open ground" | 🟡 Code Violation | NEC 2017 §250.114; §250.130(C) | Identify and correct grounding path; re-bond or run new equipment-grounding conductor | PHOTO-006 |
| 6 | Smoke alarms | Battery-only smoke alarms; ages unknown | 🔵 Recommendation | OH state code (not NEC) | Replace with hardwired interconnected dual-sensor alarms with battery backup | PHOTO-011 |
| 7 | Service mast | Weatherhead sealant deteriorated; minor water tracking | 🔵 Recommendation | — | Re-seal weatherhead | PHOTO-002 |

**Buyer / insurer guidance block:**
This home's electrical system is grandfathered to its construction-era code. The findings above identify non-compliance with current code (NEC 2017 as adopted by City of Cleveland) and material insurance-underwriting concerns. Several major homeowner-insurance carriers decline or load on properties with FPE Stab-Lok panels and aluminum branch wiring. The buyer should confirm coverage availability with their intended carrier before close. A scope and budget for remediation can be prepared on request — typical remediation (panel replacement, GFCI / AFCI retrofit, alumin um-conductor termination remediation, bath grounding) on a home this size runs $9,000–$16,000.

**Disclaimer:** Visual inspection only. Non-destructive. Based on conditions observed at time of inspection. Does not guarantee absence of concealed defects. Does not constitute legal or insurance advice. Buyer should consult AHJ for permit-related questions and their insurance carrier regarding coverage.
