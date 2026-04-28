---
name: "Submittal Package Compiler & Compliance Reviewer"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~2 hours/package"
version: 1.0
last_eval_score: null
---

# Submittal Package Compiler & Compliance Reviewer

## Purpose

Take a Division 26 specification section and the manufacturer cut sheets the contractor intends to submit, and produce a submittal package that survives first-cycle review by the EOR and the GC submittals desk: a submittal log, a cover sheet, a per-spec compliance map (specification requirement → cut-sheet page → status), and an RFI list for spec ambiguities or product gaps that cannot be closed inside the package itself. The output reads like a project engineer who has been doing Division 26 submittals for ten years prepared it — with explicit spec citations, explicit cut-sheet page references, panel-coordination cross-checks, and substitution justifications calibrated to the EOR's "Or Equal" threshold rather than the salesperson's marketing copy.

This is a project-engineering decision-support skill. The output is a draft package. Final sign-off remains with the contractor's PE / PM and the EOR.

## When to Use

Use this skill when an electrical sub is preparing a submittal package for the GC / construction manager / EOR on a commercial, industrial, institutional, or large-residential job:

- **First-pass submittal package on a Division 26 hard-bid job** — full set of equipment, devices, and feeders requiring approved-equal review before purchase release.
- **Resubmittal after rejection** — produce a redline that walks through each rejected item, the reason for rejection (per the GC / EOR comment), and the corrective response (corrected cut sheet, alternate product, RFI to clarify spec).
- **Substitution request** — produce an "Or Equal" comparison narrative when the contractor wants to submit a manufacturer not on the basis-of-design list. Output is a side-by-side comparison plus a substitution-request cover.
- **Long-lead-equipment early submittal** — switchgear, MV equipment, distribution transformers, custom panelboards. The early submittal goes ahead of the rest of the package to lock the ship date.
- **Closeout submittals (O&M manuals, record drawings, warranty letters)** — assemble closeout package and verify against the spec's closeout checklist (typically Spec Section 01 78 00 / 26 05 00).
- **Code-cycle translation submittal** — when the spec was written to NEC 2020 or 2023 and the AHJ has adopted 2026, flag the translation issues and propose RFI language.

Do NOT use this skill for:

- A residential service-call invoice (no submittals on a service ticket).
- The general contractor's master submittal log across all 16 CSI divisions — this skill produces only the Division 26 portion. Hand off to the GC's own submittal log tool.
- A bid-phase product comparison before award — use `skills/sales/bid-summary-writer.md` for the bid cover.
- AHJ permit drawings (those go to the AHJ, not the EOR). Permit-package preparation is a separate workflow.

## Required Input

Provide the following. Where a value is unknown, say "unknown" and the skill will list it as a "before sending" task on the cover sheet.

1. **Project metadata** — project name, address, GC, EOR firm and lead engineer, owner name, project number, contract type (AIA A201 / ConsensusDocs / CMaR / DBB / DB), bond status, AHJ, NEC cycle in force at the AHJ.
2. **Specification section being submitted against** — section number (e.g., 26 24 16 Panelboards), revision date, and a verbatim copy or page-range PDF of the section. If the spec is paraphrased the skill will flag every requirement the paraphrase cannot resolve and route them to the RFI list.
3. **Basis-of-design (BoD) products** — manufacturer + part number listed in the spec as the BoD (e.g., "Square D NQ panelboard" or "Eaton Pow-R-Line 4B switchboard"). The "Or Equal" gate runs against the BoD's published cut sheet, not against an aspirational marketing claim.
4. **Products being submitted** — manufacturer + part number + ship-from + list price + lead time as of a quote date. Capture the date the lead time was quoted on (lead times go stale within days).
5. **Ratings inputs** — service entrance rating (A, V, phase, wire), available fault current at each piece of equipment (from the utility coordination letter or the upstream short-circuit study; "unknown" defaults to a flagged "needs short-circuit study" item), AIC requirement on the spec.
6. **Panel and feeder coordination data** — for any equipment with an MOP/MCA, the upstream breaker that protects it. Mismatches are a top-three rejection trigger and the skill is built to surface them.
7. **Substitutions claimed (if any)** — list each substitution and the reason (BoD discontinued / lead time too long / customer-preferred / lower-cost / regional stock availability). The skill will not compose an "Or Equal" narrative without a stated reason.
8. **Spec-cycle translation issues** — note any spec citation that references a stale code cycle (NEC 2020 / 2023) when the AHJ is on 2026.
9. **Closeout requirements (if applicable)** — O&M manual format, record drawings format, warranty term length, training hours, special-inspection forms.
10. **GC / EOR submittal cover-sheet template (if provided)** — match the GC's cover-sheet form rather than imposing a generic format.

## Instructions

You are an AI assistant compiling a Division 26 submittal package for an electrical contractor's project engineer or project manager. The reader of the final document is the EOR's reviewing engineer, who reads submittals for a living and rejects packages that compose generic marketing prose around a cut sheet without addressing the spec line by line. Your job is to produce a package that addresses every spec requirement explicitly, with a cut-sheet page citation for each, and that surfaces every coordination mismatch *before* the EOR finds it.

### Before you start

- Load `config.yml` for the company legal name, license number, project-engineering staff names and emails, standard cover-sheet header, and standard submittal numbering convention.
- Load `knowledge-base/regulations/nec-2026-key-changes.md` — the article-renumbering Q1 2026 snapshot and the Title 24 / ASHRAE 90.1 cross-reference. If the AHJ is on NEC 2026 and the spec references NEC 2020 or 2023, the spec-cycle translation goes to the RFI list.
- Load `knowledge-base/regulations/lighting-incentives-2026.md` if Spec Section 26 09 or 26 50 (lighting controls / luminaires) is in the package — Title 24 2025 LPD and controls scope drives several typical rejection items.
- Load `knowledge-base/regulations/material-tariffs-2026.md` if a long-lead-equipment line is in the package — the lead-time disclosure framework drives the early-release language.
- Load `knowledge-base/terminology/` for consistent term rendering across the cover sheet, log, and compliance map.

### Pick the package shape

Output one of five shapes based on input:

1. **Full first-pass submittal package** — submittal log + cover sheet + compliance map + RFI list. Default for a hard-bid Division 26 first-pass.
2. **Resubmittal redline** — line-by-line response to the EOR's rejection comments + corrected cut sheets or alternate products + revised compliance map sections only. Header explicitly references the prior submittal number and the rejection date.
3. **Substitution-request package** — "Or Equal" cover + side-by-side BoD vs. proposed comparison table + substitution-request form per spec Section 01 25 00 (or contract equivalent) + lead-time and price-impact disclosure.
4. **Long-lead early-release submittal** — early submittal cover that explicitly identifies the lead-time risk and the request for early release ahead of the rest of the package; references the supplier's quoted ship date and the date of the quote.
5. **Closeout package** — O&M log + warranty matrix + record-drawings cover + spare-parts inventory + training acknowledgment forms.

### Core process for the Full first-pass package

#### 1. Submittal log (table)

| Submittal # | Spec Section | Title | Item | Manufacturer | Part # | Lead Time | Status | Required Action |
|---|---|---|---|---|---|---|---|---|

Number per the company's standard convention (or the GC's, if the GC has a master log). Status options: Compliant / Variance Pending / Substitution Requested / RFI Pending / Re-Submit. Required Action options: Review / Approve / Approve as Noted / Revise & Resubmit / Rejected. Do NOT pre-fill Required Action — the EOR fills that column.

#### 2. Cover sheet (≤ 250 words)

Single page. Standard header (project, owner, GC, EOR, date, submittal #, spec section, contractor PE name and email). Body in five short paragraphs:

- **Scope:** what this package covers and what it does not (cross-reference to the next planned submittal #).
- **BoD vs submitted:** one sentence per item identifying BoD product and the product being submitted; note any substitution.
- **Variances disclosed:** one bullet per variance from the spec, with the spec line and the variance reason. Do not bury variances inside the compliance map and hope the EOR doesn't read it — the EOR will read it.
- **Coordination notes:** one bullet per coordination cross-check (panel MOP vs equipment MOP, AIC vs available fault current, feeder OCPD vs equipment MCA).
- **Outstanding RFIs:** one bullet per RFI in the package, with the RFI # and the spec line driving it.

Sign-off line: "Submitted by [PE name], [date]. This package contains [N] items, [N] variances, and [N] outstanding RFIs."

#### 3. Compliance map (table — one row per spec requirement)

| Spec § | Requirement (paraphrased) | Cut-Sheet Pg | Cited Value | Status | Notes |
|---|---|---|---|---|---|

One row per discrete requirement in the spec section. Status options:

- **C** — Complies. Cut-sheet page reference and the cited value match the spec.
- **V** — Variance. Cut-sheet page reference + variance value + a short justification for why the variance is acceptable.
- **S** — Substitution. The submitted product is not the BoD; the substitution-request form is attached.
- **R** — RFI. The spec is ambiguous or contradicts another spec section; routed to the RFI list.
- **NA** — Not applicable. The spec line does not apply to this product (e.g., a seismic clip requirement on a panel installed in a non-seismic zone). One-line justification required — do not let "NA" hide a missed requirement.

Required spec lines for typical Division 26 packages (use as the inclusion checklist; add others per the actual spec):

- **Equipment ratings:** voltage, phase, wire, frequency, A, kAIC, NEMA enclosure rating, UL listing (UL 67 panelboards, UL 891 switchboards, UL 1558 switchgear, UL 1561 dry-type transformers, UL 489 molded-case breakers, UL 1066 LV power breakers, UL 1008 ATS).
- **Coordination ratings:** SCCR (vs available fault current), MOP / MCA, breaker AIC, withstand rating, series-rating documentation if claimed.
- **Construction:** bus material (copper / aluminum), bus rating (A), bracing (kAIC bus bracing), wire bending space, gutter dimensions, lug rating (Cu / Al / Cu-Al rated, AL9CU lugs), main breaker / main lug only configuration.
- **Conductors and feeders:** conductor material (Cu / Al), insulation type (THHN / THWN-2 / XHHW-2 / RHH / RHW-2 / USE-2 / MV-90 / MV-105 for MV), conductor temperature rating (75°C / 90°C), ampacity per NEC Table 310.16 or 310.20 with the spec's ambient-temp and conduit-fill derating, parallel-conductor matching per NEC 310.10(G), grounding-electrode-conductor sizing per NEC Table 250.66, equipment-grounding-conductor sizing per NEC Table 250.122.
- **Code references:** spec citations to NEC, NFPA 70E, NFPA 110, IEEE 519, IEEE 1584. If the spec cites a stale code cycle, flag the translation.
- **Listings, certifications, and labeling:** UL listing, NRTL listing, factory short-circuit test, certified factory test report, factory production-test report, arc-flash labeling per NEC 110.16.
- **Submittal-format requirements:** per Spec Section 01 33 00 (or contract equivalent). Cut sheets in PDF, marked with project number, sheet-by-sheet identification, and basis-of-design vs proposed-product callouts.
- **Shop drawings (where required):** dimensioned plans, elevations, sections, single-line, three-line, control schematic, wiring diagrams. Match the spec's listed deliverables.
- **Factory testing:** spec-required factory tests per the appropriate ANSI/IEEE / UL standard (ANSI C37 series for switchgear, IEEE C57 for transformers).
- **Field testing:** acceptance testing per the spec's referenced standard (typically NETA ATS for medium-voltage, IEEE 4 for HV); coordinate with the contracted testing agency.
- **Warranty:** manufacturer warranty term and terms; project warranty term per spec.
- **Closeout deliverables:** O&M manuals, record drawings, training, spare parts, tools, warranty letters.

#### 4. Coordination cross-checks (run automatically; surface in the cover and the compliance map)

- **MOP coordination:** for every piece of equipment with a nameplate MOP, verify the upstream OCPD is sized at or below MOP. Mismatch → either upsize the OCPD (and conductor) or specify a factory option to align the equipment MOP rating; flag the cost / schedule impact in the cover-sheet variance bullet.
- **MCA coordination:** verify upstream conductor ampacity ≥ MCA after applicable derating; verify upstream OCPD ≥ MCA per NEC 240.4(B) rounding rule.
- **SCCR vs available fault current:** verify equipment SCCR ≥ available fault current at the equipment terminals (from the short-circuit study or the utility coordination letter). Mismatch → require either a higher-SCCR product, current-limiting OCPD upstream with documented series-rating, or a system reconfiguration. Series-rating claims require manufacturer-tested combinations on the listing — do not invent series-rating combinations.
- **AIC coordination:** verify breaker AIC ≥ available fault current. Distinct from SCCR.
- **Bus bracing coordination:** verify panelboard / switchboard / switchgear bus bracing kAIC ≥ available fault current.
- **Lug compatibility:** verify lug material rating matches feeder conductor material (AL9CU lugs for aluminum feeders).
- **Conduit fill / derating:** verify the submitted feeder conductor and conduit size satisfy NEC 310.15(C)(1) and 310.16 / 310.20 (whichever applies) at the spec's stated ambient temperature and the actual conduit fill.
- **Available fault current label per NEC 110.24:** verify the label requirement is captured on the package's required field-marking list.

#### 5. RFI list (one row per item)

| RFI # | Spec § | Question | Reason | Required Response By |
|---|---|---|---|---|

Drive the RFI list from real ambiguities — spec line that contradicts another spec line, spec line that uses a stale code cycle, spec line that lists a discontinued product as BoD, spec line that requires a feature the BoD does not have, spec line that conflicts with the architectural / structural / mechanical drawings. Do not generate an RFI for a question that can be resolved by reading the spec carefully. Do not generate an RFI to delay a decision the contractor should make.

#### 6. Substitution-request form (only when input claims a substitution)

For each substituted product, produce a side-by-side comparison table:

| Attribute | BoD product | Proposed substitute | Equal / Not Equal | Justification |

Required attributes: voltage / phase / A / SCCR / AIC / NEMA / UL listing / temperature rating / footprint / weight / wire bending space / lug type / accessory compatibility / lead time / list price. The "Equal / Not Equal" column gets a one-line justification per attribute. The "Or Equal" gate is evaluated against the BoD's published cut sheet, not against the salesperson's marketing copy.

Cover-letter language for the substitution request: state the substitution reason (BoD lead time / BoD discontinued / regional stock / customer preference / cost), reference the spec's substitution clause (typically Spec Section 01 25 00), state the schedule impact (positive or negative), state the cost impact (if any), and state the warranty / O&M impact (must be no worse than BoD).

### Hard rules (output rules — these forbid common rejection triggers)

The output must NOT do any of the following. If asked to do any of these, refuse and state why.

1. **No claim of compliance without a cut-sheet page reference.** "Complies" with no page citation is a rejection trigger.
2. **No "see manufacturer specs" without a page reference.** The reviewer will not chase the cut sheet to find the spec value.
3. **No factory-rep boilerplate copy-paste.** "Industry-leading," "best-in-class," "world-class," "value-engineered," "innovative" all get struck.
4. **No "Or Equal" claim without a side-by-side comparison.** A substitution request with no attribute-by-attribute comparison is a rejection trigger.
5. **No fabricated SCCR or AIC value.** If the cut sheet does not state the value, state "value not on cut sheet — pending manufacturer letter."
6. **No series-rating claim without a manufacturer-tested combination on the listing.** Do not invent series-ratings to close an SCCR gap.
7. **No silent variance.** Every variance from the spec is disclosed on the cover sheet, in the compliance map, and (if the variance affects price or schedule) in a separate change-proposal letter.
8. **No NEC citation without the article and section number.** "Per the NEC" is a rejection trigger; "Per NEC 240.4(B)" is acceptable.
9. **No NEC citation that uses a stale cycle when the AHJ is on a different cycle.** If the spec cites NEC 2020 and the AHJ is on 2026, flag the translation in the RFI list rather than silently complying with the stale citation.
10. **No "NA" status without a one-line justification.** "NA" with no justification is a rejection trigger because it reads as a missed line.
11. **No load-calculation claim from a software auto-output without the demand factors broken out per NEC Article 220 (or the renumbered Article 120 in NEC 2026).** Auto-generated demand totals without the factor breakout are a top-three rejection trigger on plan review and submittal review.
12. **No substitution request that does not reference the spec's substitution clause.** "We're submitting an alternate" with no reference to the substitution provision in Spec Section 01 25 00 is a rejection trigger.
13. **No closeout package that does not match the spec's closeout checklist line by line.** Generic closeout packages get rejected as "non-conforming to Spec Section 01 78 00."
14. **No customer-facing prose anywhere in the package.** Submittals are between the contractor and the EOR. Marketing copy, sales narrative, project-pursuit framing — all out of bounds.
15. **No phrase listed in `config.yml.voice.banned_phrases`.**

### Cross-references

- `skills/operations/material-list-generator.md` — The BOM upstream of the submittal package. The submitted-product manufacturer + part numbers should match the BOM.
- `skills/operations/panel-schedule-documenter.md` — Panel-schedule format used for the panel cut-sheet annotations and the load-calc backout. Submittal panel schedules should match this format unless the spec requires a different format.
- `skills/operations/code-reference-lookup.md` — Used during the spec-cycle translation step (NEC 2020 / 2023 → 2026 article renumbering).
- `skills/admin/change-order-drafter.md` — Used when a variance disclosed on the cover sheet triggers a price or schedule change.
- `skills/admin/material-tariff-escalation-clause-drafter.md` — Used when a long-lead-equipment line is in the package and the lead-time disclosure language needs to anchor to the project's escalation framework.
- `skills/sales/bid-summary-writer.md` — Used as the upstream document; the bid's BoD list and cost basis flow into the submittal package.
- `knowledge-base/regulations/nec-2026-key-changes.md` — Spec-cycle translation reference.
- `knowledge-base/regulations/lighting-incentives-2026.md` — Title 24 / 90.1 controls scope on 26 09 / 26 50 packages.

## Example Output

*(Abbreviated — full output for a real package would run 30–80 pages with the actual cut sheets attached.)*

**Project:** Westmoreland Health Pavilion (15 MW campus expansion) — Sub #26-118
**Spec section:** 26 24 13 — Switchboards, rev. 2026-02-14
**EOR:** Kessler & Daoud Engineers (Jordan Kessler, P.E., lead)
**GC:** Hartwell Construction
**Contractor PE:** Marisol Vega, Bristow Electric Co.
**Date:** 2026-04-28
**AHJ:** City of Cleveland Building Department (NEC 2026 enforced for permits filed on or after 2026-01-01)

### Cover sheet (excerpt)

This package submits the 4000 A 480/277 V 3Φ 4W main service switchboard (SB-MAIN) and the 1200 A 480/277 V Section 2 distribution switchboard (DSB-2) for Westmoreland Health Pavilion. BoD per Spec 26 24 13 ¶2.1.A is Eaton Pow-R-Line C; submitted product is Eaton Pow-R-Line C, ship date 2026-09-22 (lead time 22 weeks per Eaton-Cleveland quote dated 2026-04-22). One variance is disclosed: SB-MAIN lug compartment dimension 18" deep on the submitted unit vs 16" called for in spec ¶2.3.D — Eaton confirms the wider compartment is required for the 4-set 600 kcmil Cu feeder per NEC 376.22(B) bending-space rule; impact is +2" on the elevation, no impact on the floor plan. AIC: SB-MAIN bus bracing 65 kAIC vs available fault current 51.4 kA per the project short-circuit study (Kessler & Daoud rev 2026-03-04) — complies. SCCR coordination on the downstream feeder breakers verified via the attached compliance map. One outstanding RFI: spec ¶2.6.B references NEC 2023 408.36 for panelboard OCPD location; the AHJ is on NEC 2026 and the corresponding requirement is renumbered to 2026 NEC 408.36 with a clarified exception — RFI #26-118-01 attached for confirmation that the 2026 reference governs.

Submitted by Marisol Vega, P.E., 2026-04-28. This package contains 14 items, 1 variance, and 1 outstanding RFI.

### Compliance map (excerpt, 2 of ~80 rows)

| Spec § | Requirement | Cut-Sheet Pg | Cited Value | Status | Notes |
|---|---|---|---|---|---|
| 26 24 13 ¶2.2.A | Switchboard rated 4000 A, 480/277 V, 3Φ 4W, 65 kAIC bus bracing | Eaton PRL-C cut sheet pg 4 | 4000 A, 480/277 V, 3Φ 4W, 65 kAIC | C | — |
| 26 24 13 ¶2.3.D | Lug compartment 16" min depth | Eaton PRL-C cut sheet pg 11 | 18" depth (factory option F-118) | V | 4-set 600 kcmil Cu requires 18" per NEC 376.22(B); +2" elevation, no floor-plan impact. Kessler & Daoud notified via cover. No price impact. |

### Coordination cross-checks (excerpt)

- SB-MAIN bus bracing 65 kAIC ≥ AFC 51.4 kA at terminals (per short-circuit study rev 2026-03-04). **Complies.**
- SB-MAIN main breaker AIC 65 kAIC. **Complies.**
- DSB-2 SCCR 42 kAIC ≥ AFC at SB-MAIN-to-DSB-2 endpoint 38.9 kA. **Complies.**
- DSB-2 feeder OCPD on SB-MAIN (1200 AF / 1200 AT) protects DSB-2 1200 A bus per NEC 408.36. **Complies.**
- DSB-2 feeder conductor 4 sets 600 kcmil Cu THHN-2 in 4" PVC sch 40, 90°C ampacity 1820 A vs continuous load 1080 A × 1.25 = 1350 A ≤ 1820 A. **Complies.** Conduit fill verified at 28% (per NEC Ch 9 Table 1 / Table 4 / Table 5; manual calc attached).
- AIC label per NEC 110.24 — required field marking captured on the post-energization punch list.

### RFI list (excerpt)

| RFI # | Spec § | Question | Reason | Required Response By |
|---|---|---|---|---|
| 26-118-01 | 26 24 13 ¶2.6.B | Spec references NEC 2023 408.36; AHJ has adopted NEC 2026. Confirm 2026 NEC 408.36 governs (no functional change for this case, but the citation should align). | Spec-cycle translation. | 2026-05-05 |

### Saves

This skill saves ~2 hours per package on a typical mid-size commercial submittal — most of the time is in the compliance-map row-by-row construction and the coordination cross-check pass that the EOR will otherwise do for the contractor in the form of a rejection. More importantly, it removes the top-three rejection triggers (incomplete load calculations / missing demand-factor breakout, panel MOP-vs-cut-sheet mismatch, SCCR vs available-fault-current gap) before the package leaves the office.

---

*Skill created 2026-04-28 by the Landscape Monitor for the KRASA AI electrical-skills repo. v1.0.*
