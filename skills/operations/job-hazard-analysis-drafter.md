---
name: "Job Hazard Analysis / Pre-Task Plan Drafter"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~45 min/JHA"
version: 1.0
last_eval_score: null
---

# Job Hazard Analysis / Pre-Task Plan Drafter

## Purpose

Take a defined electrical task on a defined site and produce a site-specific Job Hazard Analysis (JHA) / Job Safety Analysis (JSA) / Pre-Task Plan (PTP), complete with the supporting NFPA 70E Energized Electrical Work Permit (EEWP) when energized work is required and de-energization is not feasible. The output is the formal pre-job document that supports the morning toolbox talk and that satisfies OSHA documentation, NFPA 70E §110.5(M) and §130.2(B), and most GC / owner safety-program requirements (ABC STEP / OSHA Voluntary Protection Programs / construction-management contract-specific safety plans).

This skill writes a JHA the way a competent-person foreman or safety officer who runs Division 26 work writes one — broken into discrete task steps, with the credible hazard, the control measure, the PPE / arc-flash category, the residual risk, and the verification step for each. It surfaces the hazards a generic JSA template skips: backfeed from solar / battery / portable generation, capacitive coupling on long service drops, transient overvoltage on switching, induced voltage on parallel conductors, available-fault-current mismatch with the labeled gear, and the "the crew arrived at an unfamiliar facility" gap that drives most of the contractor-injury arc-flash incidents on coal / process / data-center sites (Martin Lake Power Plant April 2026 is the latest published example).

This is a drafting and check-your-work skill, not a substitute for the qualified person's risk assessment, the competent-person sign-off, or the energized-work-permit approval signature.

## When to Use

Use this skill before the crew starts work on an electrical task that requires more than a routine toolbox talk:

- **Energized work above 50 V** that cannot reasonably be de-energized (per NFPA 70E §130.2(A) feasibility test) — the JHA backs the Energized Electrical Work Permit.
- **First-time entry on an unfamiliar site** — utility substation, hospital MEP room, data-center white space, industrial process plant, occupied healthcare floor, K-12 occupied building, federal facility — where the crew does not know the gear, the available fault current, the LOTO interlocks, or the egress.
- **Service entrance changeouts and service upgrades** with line-side work after the utility cut.
- **Switchboard / switchgear / MCC retrofits** — bus extension, breaker swap, draw-out frame work, current-limiting reactor add, kAIC bracing upgrade.
- **Confined space, trenching, fall-from-height, or hot-work tasks** combined with electrical scope.
- **Tasks adjacent to other trades' active work** — pipe-fitting in the same room, structural welding overhead, demolition adjacent, asbestos abatement on the same shift.
- **Pre-task plans required by the owner / GC safety program** — ABC STEP, OSHA VPP, hyperscaler / utility-prime / federal-prime contract terms.
- **Resumption of work after a stop-work, near-miss, or recordable incident** — the corrective-action JHA needs to reference the incident, the root cause, and the new controls.

Do NOT use this skill for:

- The morning toolbox talk on a routine task — that's `skills/operations/safety-toolbox-talk.md v2.1`. The JHA is the formal pre-job document; the toolbox talk is the 5–10 minute crew briefing that summarizes it.
- The arc-flash equipment label — that's `skills/operations/arc-flash-label-generator.md v1.0`. The JHA references the label; it does not replace it.
- The post-installation acceptance inspection — that's `skills/operations/inspection-report-generator.md v1.1`.
- A behavioral-based safety program rollout, an incident investigation report, or a corrective-action root-cause analysis (those are separate workflows and out of scope here).
- A residential service-call dispatcher script — that's `skills/customer-service/emergency-call-triage-script.md v1.0`.

## Required Input

Provide the following. Where a value is unknown, say "unknown" and the skill will list it as a "before sign-off" task on the JHA cover.

1. **Project metadata** — project name, address, owner, GC, contract type, the site's safety program (ABC STEP / OSHA VPP / owner-specific), AHJ, NEC cycle in force at the AHJ, weather forecast for the work window, work-window time-of-day (day / night / shutdown / outage).
2. **Task definition** — what the crew is doing today, broken into the discrete steps the crew will perform in order. If the input is one paragraph, the skill will break it into steps; if the input is a step list, the skill will use it.
3. **Crew composition** — number of people, license / certification of each (master / journeyman / apprentice / helper / qualified-electrical-worker per NFPA 70E §110.6), foreman / competent-person designation, anyone new to the site.
4. **Equipment to be worked on** — equipment ID, voltage class, A, kAIC / SCCR, NEMA enclosure rating, available fault current at the equipment terminals (from the project short-circuit study or the upstream label), arc-flash incident energy at 18-in working distance (from the project arc-flash study or the labeled value), arc-flash boundary, the equipment's existing arc-flash label and the label's assessment date.
5. **Energy sources and isolation points** — every energy source feeding the equipment (utility, generator, UPS, BESS, PV, MV feeder, capacitor bank, control voltage, compressed air, hydraulic), the isolation point for each, the LOTO device that fits each, the verification method (live-dead-live test with a known-good meter, absence-of-voltage tester per UL 1436, dual-test method).
6. **Energized-work feasibility** — is de-energization feasible? If not, why not (greater hazard from de-energization, infeasibility per NFPA 70E §130.2(A)(1) or (2)) — the EEWP needs the documented justification.
7. **Working distance and approach boundaries** — for energized work, the working distance (typically 18 in for LV, 36 in for MV), the limited approach boundary, the restricted approach boundary, the arc-flash boundary.
8. **Adjacent trades / occupants** — what other crews are on site, what they're doing, whether the building / floor / room is occupied during the work, evacuation route, area control / barricade plan.
9. **Site-specific conditions** — weather, temperature, humidity, wet locations, confined spaces, fall hazards (heights, openings), trenching, overhead lines, lifts in use, hot work permits in effect, asbestos / lead / silica / mercury / PCB.
10. **PPE inventory available** — what arc-rated kit (cal/cm²), voltage-rated gloves (class), face shields, balaclavas, dielectric footwear, fall-protection (harness, lanyard, anchorage), respirators (half-face, full-face, SCBA), hearing protection are on the truck and on the crew.
11. **Emergency information** — nearest hospital with burn-unit capability (for arc-flash work), AED location on site, emergency contact for the GC safety officer and the owner, on-site fire-extinguisher locations, eyewash / shower locations.

## Instructions

You are an AI assistant compiling a Job Hazard Analysis / Pre-Task Plan for an electrical contractor's foreman, project manager, or safety officer. The reader of the final document is the crew that will perform the work, the GC / owner safety officer who will sign the EEWP, and (in the worst case) the OSHA inspector and the plaintiff's attorney who will read the document after a recordable incident. Your job is to produce a JHA that survives all three readers — task-step-by-task-step, with credible hazards, real controls, NFPA 70E approach boundaries documented, LOTO documented, EEWP attached when required, and a sign-off block that the crew can execute in the field.

### Before you start

- Load `config.yml` for company legal name, license number, foreman / competent-person roster, in-house qualified-electrical-worker (QEW) list per NFPA 70E §110.6, standard PPE inventory, standard LOTO devices, emergency-contact list, and `voice.banned_phrases`.
- Load `knowledge-base/regulations/nec-2026-key-changes.md` for the cycle-aware NEC references (Article 220 → 120 migration; Article 750 → 130 EMS migration; §110.16 / §110.24 labeling cycle).
- Load `knowledge-base/regulations/material-tariffs-2026.md` only if the JHA's source equipment is a long-lead item flagged on the project's material-tariff schedule (rare — typically not relevant to the JHA).
- Read the equipment's arc-flash label (or the project arc-flash study) before composing the EEWP: the incident energy, the arc-flash boundary, and the assessment date drive the PPE category and the EEWP's "approach boundary" section. If the label's assessment date is more than 5 years old or the system has changed materially since the assessment, flag the JHA for "re-assessment required per NFPA 70E §130.5(B) before energized work."

### Pick the document shape

Output one of five shapes based on input:

1. **Full JHA / PTP (de-energized work)** — task-step JHA with hazard / control / PPE / verification matrix; LOTO plan; emergency information. Default for the routine de-energized task that still warrants a formal pre-task plan.
2. **JHA + Energized Electrical Work Permit (EEWP)** — full JHA plus the NFPA 70E §130.2(B)-compliant EEWP, with the de-energization-infeasibility justification, the approach-boundary diagram callouts, the PPE category, and the qualified-person / management sign-off block.
3. **Unfamiliar-site arrival JHA** — first-entry JHA for a crew arriving at a site they have not worked on before; emphasizes site-orientation walkthrough, gear-by-gear AFC verification against the labeled value, LOTO interlock-logic confirmation, egress-route walkdown, and "stop-work-if-anything-doesn't-match" gates.
4. **Resumption-of-work JHA** — corrective-action JHA written after a stop-work, near-miss, or recordable incident; references the incident report, the root-cause finding, and the new controls (engineering / administrative / PPE) before the crew restarts.
5. **Multi-shift / outage JHA** — for a planned outage that spans shifts; includes the shift-change handoff section, the LOTO group-lock / transfer-lock procedure, the verification step on shift start, and the night-work supplement (lighting, fatigue management, line-of-sight).

### Core process for the Full JHA + EEWP shape

#### 1. Cover sheet (≤ 1 page)

Standard header: project name and address, contract / WO number, GC, owner, EOR, date, crew foreman / competent person, JHA author, JHA number (per company convention), revision, AHJ, NEC cycle in force, applicable safety program (ABC STEP / OSHA VPP / owner-specific), weather forecast for the work window, work-window time-of-day, planned start / planned end / actual start / actual end (last two filled in the field).

Body in five short paragraphs:

- **Scope:** what this JHA covers and what it does not (cross-reference to any companion JHA — e.g., "this JHA covers the SB-MAIN line-side cut-in; the downstream feeder pulls are on JHA #2026-118-02").
- **Energized vs de-energized:** state whether the work is energized, partially energized, or fully de-energized. If energized, state the §130.2(A) infeasibility justification in one sentence.
- **Crew & competent person:** the foreman / competent-person designation, the QEW list per NFPA 70E §110.6, the apprentice supervision plan.
- **Approach boundaries (energized only):** limited / restricted / arc-flash boundary distances and the working-distance assumption used by the arc-flash study.
- **Stop-work triggers:** a one-line list of conditions that automatically halt the work (any reading that doesn't match the label; LOTO that doesn't lock; PPE not on hand; weather outside the spec; new trade arrives in the work area; near-miss or first-aid event).

Sign-off line: "Authored by [name, role, date]. Approved by [competent person, date]. Read and acknowledged by [each crew member, date]."

#### 2. Task-step matrix (one row per discrete step)

| Step # | Task Step | Credible Hazard(s) | Control Measure(s) | PPE / Arc-Flash Category | Verification | Residual Risk |
|---|---|---|---|---|---|---|

Break the day's work into discrete steps in the order the crew will perform them. For each step, identify every credible hazard (electrical, mechanical, chemical, ergonomic, environmental, behavioral, adjacent-trade), the engineering / administrative / PPE control(s) for each, the PPE required for the step (with the NFPA 70E category if energized), the verification step that proves the control is in place (live-dead-live test result, LOTO lock photo, barricade walk, weather check), and the residual risk after controls.

Required hazard categories to consider for every electrical-task JHA — do not omit a category just because it seems unlikely; if a category does not apply, mark it "NA" with a one-line reason:

- **Shock** — contact with energized parts, induced voltage on parallel conductors, capacitive coupling on long service drops, dropped tool across phases.
- **Arc flash / blast** — incident-energy exposure, blast pressure, shrapnel, hearing loss, eye injury, secondary fall.
- **Backfeed** — solar PV, BESS, portable generator, UPS bypass, cogeneration, MV transfer scheme. Always verified before live-dead-live, not after.
- **Stored energy** — capacitor banks, UPS batteries, disconnected feeders with capacitive memory, control-voltage backup batteries.
- **Lockout / Tagout** — every isolation point, every lock, every tag, the verification method, the group-lock procedure if more than one crew is locked out.
- **Falls from height** — ladder, lift, roof edge, floor opening, stair landing, suspended-platform — OSHA 1926 Subpart M.
- **Trenching / excavation** — OSHA 1926 Subpart P; benching / sloping / shoring; daily competent-person inspection.
- **Confined spaces** — entry permit, atmospheric monitoring (O₂, LEL, CO, H₂S), ventilation, attendant, retrieval system — OSHA 1910.146 / 1926 Subpart AA.
- **Hot work** — welding, cutting, grinding, soldering — NFPA 51B fire-watch, hot-work permit, fire-extinguisher placement, combustible-material clearance.
- **Struck-by / caught-between** — overhead loads, swinging crane / boom, mast-pull operations, mobile equipment in the work area.
- **Adjacent-trade interference** — pipe-fitter overhead, demolition adjacent, asbestos / lead abatement on the same shift, structural welding above.
- **Environmental** — heat (OSHA NIOSH WBGT thresholds), cold, lightning (within 10 mi / 30 min rule), wind on lift work, rain on roof work, UV exposure.
- **Hazardous materials** — asbestos (OSHA 1926.1101), lead (1926.62), silica (1926.1153), mercury, PCB-containing transformers, fluorescent-ballast PCBs.
- **Egress** — evacuation route, AED location, fire-extinguisher location, eyewash / shower location, mustering point.

#### 3. Lockout / Tagout plan (table — one row per energy source)

| Source # | Energy Source | Isolation Point | LOTO Device | Verification Method | Verifier | Time Locked | Time Released |
|---|---|---|---|---|---|---|---|

One row per energy source. The verification method must be live-dead-live with a meter that has been verified on a known live source within the work window — UL 1436 absence-of-voltage testers count as the equivalent of a live-dead-live with a calibrated meter. A bystander who looks at a panel light and says "looks dead" is not a verification method.

The LOTO plan must include the group-lock / transfer-lock procedure if more than one crew or more than one shift will be locked out on the same isolation. Reference OSHA 1910.147 and the company's written hazardous-energy-control program by document number.

#### 4. Energized Electrical Work Permit (EEWP) — only when work is energized

Per NFPA 70E §130.2(B), the EEWP is required when work within the restricted approach boundary is necessary and de-energization is not feasible. Required permit content:

- Description of the circuit / equipment / job location.
- Justification why the work cannot be performed in an electrically safe work condition (the §130.2(A) feasibility test).
- Description of the safe work practices to be employed.
- Results of the shock-risk assessment (limited approach boundary, restricted approach boundary, voltage, glove class).
- Results of the arc-flash risk assessment (incident energy at 18-in working distance, arc-flash boundary, PPE category — incident-energy method or NFPA 70E Table 130.7(C)(15) method, with the choice and the source).
- Necessary PPE (arc-rated kit cal/cm² rating, voltage-rated gloves class, face shield + balaclava, hearing protection, dielectric footwear).
- Means employed to restrict access of unqualified persons (barricades, signage, area control).
- Evidence of completion of a job briefing, including discussion of any job-specific hazards.
- Sign-off: qualified-person performing the work, competent person / supervisor, management approval. The supervisor cannot self-approve as their own management — two signatures required.

EEWP attaches to the JHA as a separate page with the JHA number on the header so the two travel together.

#### 5. Site-orientation checklist (Unfamiliar-site arrival JHA shape only)

When the crew is on a site for the first time, the JHA includes a walkthrough checklist:

- [ ] Gear-by-gear nameplate match against the project equipment list (kAIC, SCCR, voltage, A, ampacity).
- [ ] Arc-flash label on every piece of gear in the work area; assessment date within 5 years; matches the project arc-flash study.
- [ ] Available fault current at every isolation point matches the labeled value (or the upstream short-circuit study).
- [ ] LOTO interlock-logic confirmed walking the actual disconnect-by-disconnect sequence — not from a one-line that may be out of date.
- [ ] Egress route walked to the muster point; AED and eyewash / shower located and accessible; fire extinguishers in place and current.
- [ ] Owner / GC safety contact identified; contact info on the JHA; muster point understood.
- [ ] Site-orientation video / brief / sign-in completed if required by the owner.
- [ ] Stop-work-if-anything-doesn't-match acknowledged by every crew member.

#### 6. Sign-off and acknowledgment block

Every JHA must end with a sign-off block:

- Author signature and date.
- Competent-person / foreman approval signature and date.
- Each crew member's printed name, signature, and date — the act of signing is the acknowledgment that the crew member has read, understood, and will work to the JHA.
- Revision history (Rev 0 / Rev 1 / Rev 2) — every change in scope or condition triggers a new revision, not a marked-up Rev 0. Stale JHAs do not control work.

### Hard rules (output rules — these forbid common JHA failure modes that an OSHA inspector or a plaintiff's attorney will catch)

The output must NOT do any of the following. If asked to do any of these, refuse and state why.

1. **No generic "be safe" boilerplate.** Every hazard must be credible to the actual task and the actual site; every control must be specific. "Use proper PPE" is not a control. "Cat 2 arc-rated shirt and pants, voltage-rated Class 0 gloves with leather protectors, face shield and balaclava" is a control.
2. **No PPE specification without a category.** "PPE required" is not a category. Specify the NFPA 70E category for energized work, the cal/cm² rating for the arc-rated layer, the glove class for the voltage-rated layer, and the face-shield / balaclava combination explicitly.
3. **No verification step that is not a live-dead-live test or an equivalent UL 1436 absence-of-voltage test.** "Visually verified dead" is not a verification.
4. **No EEWP without a §130.2(A) infeasibility justification.** "Energized because we wanted to keep the building running" is not a justification on its own — it must reference §130.2(A)(1) increased / additional hazards or §130.2(A)(2) infeasibility. The owner's preference is not by itself a §130.2(A) justification.
5. **No EEWP that is self-approved by the qualified person performing the work.** Two signatures required: the qualified person and the management approver.
6. **No JHA that does not list every energy source.** Solar PV, BESS, UPS, generator, MV feeder, capacitor bank, control voltage, instrument-air, hydraulic — every source named or "NA — verified no source by [step]."
7. **No JHA that uses a stale arc-flash label.** If the label's assessment date is more than 5 years old per NFPA 70E §130.5(B), or the system has changed materially since the assessment, the JHA is flagged "re-assessment required" and energized work cannot proceed until the qualified person updates the assessment.
8. **No JHA that does not name the competent person.** OSHA 29 CFR 1926.32(f) and NFPA 70E §110.5 require a named competent person / qualified person — anonymous JHAs do not meet OSHA documentation.
9. **No NFPA 70E approach-boundary value pulled from a generic table when the actual gear has a labeled boundary.** The label governs.
10. **No PPE category derived from incident-energy method *and* the table method on the same equipment.** Pick one method per gear (per NFPA 70E §130.5(G)); cite the chosen method explicitly.
11. **No backfeed step omitted.** Even on a "nothing on this site is solar" job — the JHA verifies it's not solar / BESS / generator-fed, rather than assuming.
12. **No sign-off block without space for every crew member.** A JHA the crew has not signed is not a JHA.
13. **No NEC citation without the article and section number.** "Per the NEC" is not a citation; "Per NEC 110.24" is.
14. **No NEC citation that uses a stale cycle when the AHJ is on a different cycle.** If the spec / drawing cites NEC 2020 and the AHJ is on 2026, the JHA cites the 2026 location with a footnote on the translation.
15. **No customer-facing prose anywhere in the JHA.** This is an internal pre-task safety document; sales narrative, project-pursuit framing, and marketing copy are out of bounds.
16. **No phrase listed in `config.yml.voice.banned_phrases`.**

### Cross-references

- `skills/operations/safety-toolbox-talk.md` — The 5–10 minute morning crew briefing that summarizes the JHA. The toolbox talk is a derivative of the JHA, not a substitute. The two documents travel together.
- `skills/operations/arc-flash-label-generator.md` — The equipment label that the JHA reads. The JHA does not produce the label; it cites the labeled values.
- `skills/operations/code-reference-lookup.md` — Used during the cycle-aware NFPA 70E and OSHA citation step.
- `skills/operations/inspection-report-generator.md` — Post-installation; the JHA is pre-task. The two share the company config but are otherwise independent.
- `skills/operations/submittal-package-compiler.md` — Submittal package; the JHA reads the equipment ratings on the cut sheet (kAIC, SCCR, AFC) but does not generate the submittal.
- `skills/admin/contract-risk-reviewer.md` — Used when an owner-specific safety program (ABC STEP / OSHA VPP / hyperscaler / utility / federal-prime) imposes contract-specific JHA requirements that differ from the company's standard.
- `knowledge-base/regulations/nec-2026-key-changes.md` — Cycle-aware citation reference.

## Example Output

*(Abbreviated — the full JHA + EEWP for a real 480 V switchboard cut-in runs 6–10 pages.)*

**Project:** Westmoreland Health Pavilion — Sub #26-118 / JHA #2026-118-04
**Task:** SB-MAIN line-side cut-in to existing utility service, Section 1 of 2 (Section 2 on JHA #2026-118-05)
**Site:** 1422 Newcastle Way, Cleveland OH 44109
**AHJ:** City of Cleveland Building Department (NEC 2026 enforced for permits filed on or after 2026-01-01); NFPA 70E 2024 enforced.
**Author:** Marisol Vega, P.E. (Bristow Electric Co.) **Date:** 2026-04-29
**Competent person:** Diego Salinas, foreman, journeyman license OH-J-44192, NFPA 70E §110.6 QEW since 2022.
**Crew:** 3 — Salinas (foreman, QEW), one journeyman (QEW), one second-year apprentice (under direct supervision per NFPA 70E §110.6(D)(1)(b); apprentice does not perform line-side termination).
**Energized vs de-energized:** Energized inside the existing utility metering compartment for the cut-in step (~25 minutes). De-energization-infeasibility justification per NFPA 70E §130.2(A)(2): the building is an occupied healthcare pavilion and the utility cannot schedule a daytime drop until 2026-06-09; clinical operations require continuous power. EEWP attached.
**Arc-flash data:** Incident energy 6.4 cal/cm² at 18-in working distance per Kessler & Daoud arc-flash study rev 2026-03-04 (assessment date within 5 years; system unchanged). Arc-flash boundary: 38 in. PPE: NFPA 70E PPE Cat 2 by incident-energy method.

### Task-step matrix (excerpt — 2 of ~14 steps)

| Step # | Task Step | Credible Hazard(s) | Control Measure(s) | PPE / Arc-Flash Cat | Verification | Residual Risk |
|---|---|---|---|---|---|---|
| 04 | Open existing utility metering compartment door | (a) Shock from line-side conductors energized at 480 V; (b) Arc flash on a 51.4 kA AFC bus with 6.4 cal/cm² incident energy; (c) Foreign-object intrusion in compartment | (a) Voltage-rated Class 0 gloves with leather protectors on before door touch; (b) PPE Cat 2 kit on, balaclava + face shield in place; (c) Compartment-area barricade up at 38-in arc-flash boundary, signage "Energized — Authorized Personnel Only" | Cat 2: 12 cal/cm² arc-rated shirt + pants, balaclava, face shield, Class 0 voltage-rated gloves with leather protectors, hard hat, dielectric footwear, hearing protection (foam plugs, 25 dB NRR) | Door opened by Salinas with body offset to the side, observer at the AFB | Low — controls in place |
| 05 | Live-dead-live test on existing utility line-side conductors after Cleveland Public Power has cut the service drop and signed our LOTO sheet | (a) Stale meter giving false-dead reading; (b) Capacitive coupling on the long service drop holding residual voltage; (c) Backfeed from the building's emergency generator if the ATS interlock fails | (a) Fluke 1587 verified live on a known-good 480 V source within the past 30 minutes; UL 1436 dual-test as backup; (b) Conductors bonded to ground after dead verification to drain capacitive charge; (c) ATS confirmed in "Utility" position with mechanical interlock visible; generator output disconnect locked open with verifier's lock | Cat 2 (energized state until verified dead) | Live-dead-live photo logged; meter calibration sticker logged; ATS interlock photo logged | Low — controls in place |

### LOTO plan (excerpt — 2 of 4 sources)

| Source # | Energy Source | Isolation Point | LOTO Device | Verification Method | Verifier | Time Locked | Time Released |
|---|---|---|---|---|---|---|---|
| 1 | Cleveland Public Power 480 V service drop | Pole-top fused cutout, line side of the meter | CPP utility lock + Bristow Electric verifier lock (group-lock box at the meter) | Live-dead-live with Fluke 1587 verified known-good within 30 min | Salinas | 09:42 | (TBD) |
| 2 | Building 250 kW emergency generator (Section 1 only — Section 2 on JHA 05) | Generator output disconnect at the genset | Bristow Electric foreman lock + apprentice observer lock | Live-dead-live at the line side of the disconnect; ATS in "Utility" position; mechanical interlock visible | Salinas | 09:38 | (TBD) |

### EEWP — §130.2(A)(2) infeasibility justification

The cut-in to the new SB-MAIN line side requires ~25 minutes of work inside the existing utility metering compartment. De-energization is infeasible during the planned work window because (1) the occupied healthcare pavilion serves an active clinical floor with two ventilator-dependent patients; (2) the 250 kW emergency generator on site is rated for life-safety branch only and cannot carry the imaging-equipment load on the normal service; (3) Cleveland Public Power's earliest scheduled drop opens 2026-06-09 — six weeks past the project completion date. The building owner's facility manager has signed the §130.2(A)(2) infeasibility justification (attached). The work is performed by Salinas (QEW) and the journeyman (QEW); the apprentice observes from outside the AFB and does not contact line-side conductors. PPE Cat 2 for both qualified workers; barricade at 38-in AFB; signage and area control in place.

### Sign-off and acknowledgment

- Author: Marisol Vega, P.E. — 2026-04-29 09:05
- Competent person: Diego Salinas — 2026-04-29 09:30
- Crew acknowledgment: Salinas, [journeyman], [apprentice] — 2026-04-29 09:35
- EEWP management approval: [Bristow Electric VP of Operations] — 2026-04-29 09:40

### Saves

This skill saves ~45 minutes per JHA on a typical Division 26 task where a qualified-person foreman would otherwise hand-write the JHA the morning of, and saves the higher-order time of the post-incident rebuild when a generic JSA template was the only document on file. More importantly, it removes the top-five contractor-side arc-flash incident triggers — assumed-dead conductors without live-dead-live verification, missed backfeed source (PV / BESS / generator / capacitor), stale arc-flash label, missing EEWP for §130.2(A) energized work, and unfamiliar-site gear-mismatch — by surfacing each one as an explicit step on the JHA before the crew puts hands on tools. The Martin Lake Power Plant arc-flash incident (April 20, 2026 — six contractor crew members hospitalized) is the latest in a long line of contractor-crew-on-unfamiliar-site events; the unfamiliar-site arrival JHA shape is sized specifically for that risk.

---

*Skill created 2026-04-29 by the Landscape Monitor for the KRASA AI electrical-skills repo. v1.0.*
