---
name: "Preventive Maintenance Schedule Generator"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/schedule"
version: 1.2
last_eval_score: 9.70
---

# Preventive Maintenance Schedule Generator

## Purpose

Generate a structured preventive maintenance (PM) schedule for commercial, healthcare, multifamily, or light-industrial electrical systems — covering panels, switchgear, transformers, generators, lighting, fire alarm circuits, emergency power systems, MCCs, and VFDs. Output is a calendar-ready maintenance plan with task descriptions, frequencies, NEC / NFPA / NETA references, and the path-specific compliance gates the AHJ and the firm's insurance carrier expect.

## When to Use

Use this skill when you need to:

- Create or update a PM program for a commercial building or facility client
- Propose a recurring maintenance contract to a property manager or building owner
- Document maintenance schedules for insurance, warranty, or code compliance
- Transition a reactive-maintenance client to a proactive PM program
- Prepare for an annual electrical system review or infrared thermography scan

## Required Input

Provide the following:

1. **Facility type** — Commercial office, retail, industrial, multifamily, healthcare (acute care / outpatient / surgery center / assisted living), data center, warehouse, cold storage, machine shop, etc.
2. **Service amperage and voltage** — Main service rating and voltage class (e.g., 1,200 A 208Y/120 V; 2,500 A 480Y/277 V; 3,000 A 480 V delta with separately-derived 208Y/120; 13.8 kV primary with customer-owned transformer).
3. **Key electrical systems** — List major equipment: switchgear or main switchboard, number and rating of distribution panels, transformer(s) (utility-owned vs. customer-owned, dry-type vs. liquid-filled), generator(s) (kW + fuel + paralleling), UPS (kVA), ATS, fire alarm system, emergency lighting, EV chargers (count and amperage), MCCs, VFDs, harmonic-mitigation gear.
4. **Building age / last major upgrade** — Approximate year built or last electrical renovation; whether any equipment in service exceeds 20 years.
5. **Occupancy and criticality** — Hours of operation, whether 24/7, any critical loads (medical, server rooms, refrigeration, life-safety).
6. **Existing maintenance history** (optional) — Any known recurring issues, recent failures, IR scan findings, last arc-flash study date.
7. **Contract scope** (optional) — Whether this is for an internal maintenance team or a service contract proposal.
8. **Jurisdiction and NEC cycle in force** — State + AHJ, and whether the AHJ is on 2026 / 2023 / 2020 / older. PM cycle recommendations on AFCI, GFCI, surge-protective devices, and arc-flash label currency are cycle-dependent.
9. **NFPA 70B adoption status** (optional) — NFPA 70B was elevated to a Standard (from a Recommended Practice) in 2023; the 2026 edition tightens condition-based-monitoring and program-management expectations. Some AHJs and insurance carriers now reference 70B 2026 directly.
10. **Insurance carrier expectations** (optional) — Some carriers require NETA MTS-aligned testing on a published cadence; capture if known.

## Instructions

You are an AI assistant helping electrical contractors build professional preventive maintenance schedules. Your job is to generate a thorough, actionable PM plan tailored to the specific facility, its electrical systems, and the AHJ's adopted code cycle.

### Before you start

- Load `config.yml` from the repo root for company name, license number, owner / lead-tech names, default jurisdiction, default NEC cycle, NETA membership status (if any), `voice` preferences, and the **Equipment-Class PM Template Library** (`config.yml.preventive_maintenance_scheduler.equipment_class_library` — see the dedicated section below).
- Reference `knowledge-base/regulations/nec-2026-key-changes.md` for NEC 2026 §110.16(B)/(C) arc-flash label assessment-date mandate, AFCI / GFCI cycle deltas, and §230.70(B)(2) outdoor-disconnect implications for service-equipment maintenance.
- Reference `knowledge-base/regulations/lighting-incentives-2026.md` only if the facility has a controls-bonus rebate that affects PM scope (rare — most lighting rebates do not require ongoing PM).
- Reference `knowledge-base/best-practices/` for industry PM standards and NETA MTS-2023 categories.

### Path Selection (run FIRST, before the task matrix)

The intake's facility type and service amperage map to one of four paths. Each path has a different baseline cadence, compliance posture, and sign-off expectation. Pick exactly one. If the facility crosses two paths (e.g., a healthcare campus with an attached light-industrial central plant), generate the PM plan for the more rigorous path and flag the override in the Facility Summary.

| Path | Trigger | Defining characteristic | PE / NETA sign-off |
|---|---|---|---|
| **Light Commercial** | ≤ 800 A service, no MV, single genset (or none), 208Y/120 V or single-phase | Office, retail, small multifamily, light warehouse | PE only on arc-flash study; NETA optional |
| **Heavy Commercial** | ≥ 1,200 A service, single or paralleled genset, ATS, UPS, 480Y/277 V dist. | Class A office, mid-rise multifamily, cold storage, mid-size warehouse | PE on arc-flash; NETA MTS-aligned testing recommended |
| **Healthcare** | NFPA 99 EES (Type 1 or Type 2) | Acute care, surgery center, dialysis, assisted living with EES | PE required on EES; NETA MTS-2023 testing required (most carriers) |
| **Light Industrial** | MCC-served motor loads, VFDs, harmonic-mitigation gear, MV or 480 V delta | CNC machine shop, food processing, water/wastewater, light manufacturing | PE on arc-flash; NETA MTS-aligned testing required (most carriers); OSHA 1910.269 trigger if scope crosses utility-owned distribution |

### NEC Cycle Check

Before recommending an arc-flash label refresh, an AFCI / GFCI add, or a surge-protective-device replacement on a 2026-only basis, confirm the AHJ is on NEC 2026:

- §110.16(B)/(C) — 2026 removed the 1,000 A threshold and added the assessment-date mandate; arc-flash label currency requirements changed materially. Do not flag a 2020/2023 facility as "non-compliant" using the 2026 rule. Cite the in-force cycle's equivalent and reserve the 2026 reference for an Internal Notes block.
- §230.70(B)(2) — outdoor emergency disconnect on dwelling services is a 2023+ rule with materially different language in 2026. PM scope on an existing service does not generally trigger this; a service replacement does.
- §240.87 / §240.67 — energy-reducing maintenance switching and arc-energy-reduction methods on circuit breakers ≥ 1,200 A; PM cycle includes function-test of any installed ARMS feature.

### NFPA 70B 2026 Cross-Reference

NFPA 70B was elevated from Recommended Practice to Standard in the 2023 edition. The 2026 edition introduces a maintenance-program scoring matrix (§§9.1–9.4) and tightens condition-based-monitoring (CBM) expectations. Cite by section in the output:

- §§9.1–9.4 — Program management, scoring matrix, document retention, audit cadence
- §§11.x — Equipment maintenance (panelboards, switchboards, switchgear, transformers, busways)
- §§17.x — Testing (insulation resistance, contact resistance, breaker time-current, ground-fault, transformer turns ratio)
- §§19.x — Battery systems (UPS, generator starting, sealed lead-acid vs. NiCd vs. Li-ion cadence deltas)

If the AHJ has not yet adopted NFPA 70B 2026, frame 2026-only sections as best practice, not as required. Most AHJs have not formally adopted 70B 2026 as of Q1 2026.

### NETA MTS-2023 Alignment

For Heavy Commercial / Healthcare / Light Industrial paths, cross-reference NETA MTS-2023 testing categories so the PM proposal aligns with insurance-carrier expectations:

- **Routine Maintenance Tests** — annual cadence on protective devices, TCC verification on adjustable trip units, primary injection on circuit breakers ≥ 600 A every 3 years (or per manufacturer)
- **Acceptance Tests** — only on new / replaced equipment, not in PM plan
- **Maintenance Tests** — recurring; the PM matrix should map to the NETA frequency intervals for each device class

### Equipment-Class PM Template Library (config-driven, optional)

A firm that services the same equipment classes repeatedly (medical EES, industrial MCC/VFD lines, commercial switchgear, residential/multifamily service gear) re-derives the same per-class task lists and intervals on every schedule. Pre-loading the firm's standing PM templates by **equipment class** removes that re-derivation, keeps intervals consistent across every proposal the firm sends, and lets the firm bake its own carrier-driven or fleet-experience cadence overrides into one place. The library is config-driven; **if it is absent, the skill behaves exactly as v1.1 did** — it derives the task matrix from the Path Selection table, the frequency-by-path matrix, and the NFPA 70B / NETA / NFPA 110 / 99 references inline.

`config.yml.preventive_maintenance_scheduler.equipment_class_library` is keyed by an equipment-class identifier. Each entry is a structured record:

```yaml
preventive_maintenance_scheduler:
  equipment_class_library:
    medical_ees:                 # NFPA 99 essential electrical system
      applies_to_paths: [healthcare]
      tasks:                     # each task: description / frequency / reference / priority / sign_off
        - { task: "LIM functional test (push-to-test + impedance)", frequency: monthly, reference: "NFPA 99 §6.3.2.6", priority: high, sign_off: neta_or_in_house }
        - { task: "EES generator 4-hour load bank", frequency: 3-year, reference: "NFPA 110 §8.4.2.3", priority: high, sign_off: neta }
        - { task: "Closed-transition ATS 4-second transfer verification", frequency: annually, reference: "NFPA 99 §6.4.4", priority: high, sign_off: in_house }
      interval_basis: "NFPA 110 / 99 statutory + carrier audit cadence"
      carrier_overrides: "Joint Commission EC.02.05.07 alignment"
      verified_on: "2026-05-30"
    industrial_mcc_vfd:
      applies_to_paths: [light_industrial]
      tasks:
        - { task: "MCC IR + bucket-pull on critical", frequency: quarterly, reference: "NFPA 70B §11.x", priority: high, sign_off: in_house }
        - { task: "VFD parameter-log + fault-history pull", frequency: quarterly, reference: "mfr specs", priority: medium, sign_off: in_house }
        - { task: "Harmonic THD at PCC (V & I)", frequency: semi-annually, reference: "IEEE 519", priority: medium, sign_off: in_house }
      interval_basis: "NETA MTS-2023 + IEEE Std 902 (Buff Book) industrial intervals"
      carrier_overrides: "≥600 A protective-device primary injection annually (Liberty Mutual pattern)"
      verified_on: "2026-05-30"
    commercial_switchgear:       # heavy-commercial distribution
      applies_to_paths: [heavy_commercial, light_commercial]
      tasks:
        - { task: "Switchboard IR thermography under 60%+ load", frequency: quarterly, reference: "NFPA 70B §11.21; NETA MTS §9", priority: high, sign_off: in_house }
        - { task: "Main breaker primary injection", frequency: 3-year, reference: "NETA MTS §7.6", priority: high, sign_off: neta }
      interval_basis: "NETA MTS-2023 routine-maintenance intervals"
      verified_on: "2026-05-30"
    residential_service:         # multifamily / light-commercial house gear
      applies_to_paths: [light_commercial]
      tasks:
        - { task: "House-service IR thermography", frequency: annually, reference: "NFPA 70B §11.21", priority: high, sign_off: in_house }
        - { task: "EVSE-to-panel terminal torque + thermal", frequency: quarterly, reference: "NEC §625; NFPA 70B §11.x", priority: medium, sign_off: in_house }
      interval_basis: "NFPA 70B §11.x + IEEE Std 902 light-commercial intervals"
      verified_on: "2026-05-30"
```

How to use the library:

1. **Match after Path Selection.** Once the Path is selected, match the facility's installed equipment to the `equipment_class` entries whose `applies_to_paths` includes the selected Path. A facility usually matches one to three classes (e.g., a hospital matches `medical_ees` + `commercial_switchgear`).
2. **Merge, never replace.** Merge each matched class's `tasks` into the Maintenance Task Matrix, then layer the path/facility-specific items derived inline on top. The library seeds the recurring spine; the inline derivation still covers everything the library doesn't enumerate. The library never removes a statutory item (NFPA 110 / 99 / 70E currency) even if a class entry omits it.
3. **Equipment-Class Echo.** When one or more class templates are matched, surface a one-line **Equipment-Class Echo** at the top of the Facility Summary listing the matched classes, their `interval_basis`, any `carrier_overrides` applied, and the oldest `verified_on` among them — so a wrong-class match or a stale template is caught at a glance.
4. **Freshness warning.** If a matched entry's `verified_on` is older than the firm's review horizon (default 12 months), append a one-line "confirm intervals against current NFPA 70B / NETA MTS / carrier policy" note next to the Echo.
5. **Never invent.** The library never invents an equipment class, a task, an interval, or a reference. On no match (or absent config), say nothing about the library and derive the matrix inline exactly as v1.1 did — no behavior change, no placeholder.

The library formats and seeds the task matrix only. Path Selection, the NEC Cycle Check, the NFPA 70B 2026 cross-reference, the frequency-by-path matrix, the compliance checklist, and every statutory cadence (NFPA 110 / 99 / 70E) are unchanged and authoritative over any library entry.

### Generate a PM schedule with these sections

1. **Facility Summary** — Brief description of the facility, its electrical infrastructure, and any critical systems. Include the selected Path and any path-override flag. Note special requirements (healthcare, hazardous locations, mission-critical power). Cite the AHJ's adopted NEC cycle and NFPA 70B adoption status. If the Equipment-Class PM Template Library matched one or more classes, lead with the **Equipment-Class Echo** (matched classes, interval_basis, carrier overrides applied, oldest verified_on).
2. **Maintenance Task Matrix** — For each major system component, provide:

   | Component | Task Description | Frequency | Estimated Duration | NEC / NFPA / NETA Reference | Priority |
   |---|---|---|---|---|---|

3. **Seasonal Considerations** — Storm prep, lightning-protection-system inspection, HVAC load transition, battery-room ventilation in summer, generator-cooling-water freeze risk in winter.
4. **Compliance Checklist** — Path-specific. NFPA 70B 2026 program-management items, NFPA 70E arc-flash study currency (≤ 5 years per 130.5(G)), NFPA 110 / 99 generator and EES testing, OSHA 1910.269 cross if applicable, AHJ-specific items.
5. **Recommended Contract Structure** — Tiered service levels (basic visual + thermal; standard with testing; comprehensive with predictive analytics / CBM). Estimate annual visit count and labor hours by path.
6. **ROI Justification** — 2–3 data points on PM cost savings vs. reactive maintenance, insurance premium reduction potential, equipment lifespan extension.
7. **Next Steps** — Site walk schedule, IR baseline, arc-flash study currency check, contract execution timeline.

### Standard task categories to evaluate

- **Visual inspection** — Look for signs of overheating, corrosion, water intrusion, physical damage, code violations
- **Thermal scanning** — Infrared thermography of panels, switchgear, connections, breakers under load (NFPA 70B §11.21; NETA MTS §9.x)
- **Torque verification** — Re-torque connections per manufacturer specs (main lugs, bus connections, breaker terminals); NEC §110.14, NFPA 70B §11.x
- **Testing** — Breaker trip testing, ground-fault testing, arc-fault testing, generator load bank test (NFPA 110 §8.4.2 — 30% nameplate minimum every 3 years if monthly exercise doesn't reach load), transfer switch operation, battery load test, insulation resistance, contact resistance
- **Cleaning** — Vacuum panels and enclosures, clean contacts, remove debris from electrical rooms
- **Labeling and documentation** — Verify panel schedules are current (§408.4), update circuit directories, confirm arc-flash labels current (2026 §110.16(B)/(C) assessment date or pre-2026 equivalent)
- **Predictive analytics / CBM** — On Heavy Commercial / Healthcare / Industrial paths: trend IR readings over time, vibration monitoring on motors, partial-discharge sensing on MV gear, oil DGA on liquid-filled transformers

### Frequency guidelines (path-adjusted)

| Frequency | Light Commercial | Heavy Commercial | Healthcare | Light Industrial |
|---|---|---|---|---|
| Monthly | Visual on critical, genset exercise, EM lighting spot check | Same + UPS visual + IR on critical feeders | Same + EES generator (NFPA 110) + ATS visual + iso-panel monitor | Same + MCC visual + VFD log review |
| Quarterly | Thermal scan main dist. | Thermal scan all dist., torque check critical | Thermal scan EES, EES generator load run | Thermal scan MCCs, harmonic readings |
| Semi-annually | Full panel inspection | Full panel inspection + ATS test | Full EES inspection + line-iso monitor test | Full MCC inspection + VFD parameter check |
| Annually | Comprehensive survey, breaker trip on critical, panel-schedule refresh | Above + arc-flash currency check, NETA-aligned breaker testing per MTS | Above + NFPA 110 §8.4.2 load bank if monthly didn't hit 30%, NFPA 99 EES testing | Above + transformer DGA on liquid-filled, motor megger on critical motors |
| 3-year | Arc-flash study currency review (per 70E §130.5(G)) | Arc-flash study refresh on system change; primary injection on ≥ 600 A breakers | Same + EES study refresh | Same + transformer winding-resistance |
| 5-year | Arc-flash study refresh required | Arc-flash study refresh required | Arc-flash study refresh required + EES retro | Arc-flash study refresh required |
| As-needed | After storms, power events, equipment additions | Same | Same + after any EES branch modification | Same + after any motor / VFD replacement |

### Formatting rules

- Use clear tables for the maintenance matrix
- Include NEC / NFPA / NETA references where applicable; cite NFPA 70B 2026 by section if AHJ has adopted, otherwise frame as best practice
- Write task descriptions that a journeyman electrician can execute without ambiguity
- Flag any tasks requiring a licensed PE sign-off (arc-flash study, EES system mods, MV gear)
- Flag any tasks requiring NETA-certified technician sign-off (per insurance carrier expectations on Heavy Commercial / Healthcare / Industrial)

## Worked Example A — Light Commercial: 50,000 SF Class A Office, Portland OR

**Inputs given:**
- Facility: 50,000 sq ft Class A office, built 2015, 5 floors
- Service: 1,600 A 480Y/277 V (note: at the upper edge of Light Commercial; reads as Heavy Commercial with this service rating — see override)
- Equipment: 1,600 A main switchboard, (6) 225 A 208Y/120 V distribution panels, 2,500 kVA pad-mount transformer (utility-owned), 250 kW diesel standby generator with ATS, 100 kVA UPS for server room, fire alarm system, EV charger bank (8x40 A in the garage)
- Occupancy: M–F 7 AM–7 PM, weekend stragglers, server room 24/7
- Jurisdiction: City of Portland OR, on NEC 2023 (NEC 2026 not adopted as of Q1 2026)
- NFPA 70B: not formally adopted by Portland AHJ; insurance carrier (Travelers) references 70B 2023 in policy

**Path selected:** Heavy Commercial (override from Light Commercial because the 1,600 A service and the EV-charger bank push the facility into the Heavy Commercial cadence under NETA MTS guidance, even though there's no genset paralleling).

**Sample matrix excerpts:**

| Component | Task | Frequency | Est. Duration | Reference | Priority |
|---|---|---|---|---|---|
| 1,600 A switchboard | Infrared thermography under 60%+ load | Quarterly | 2 hrs | NFPA 70B 2023 §11.21; NETA MTS §9 | High |
| 1,600 A switchboard | Torque verification — bus and lug | Annually | 4 hrs | NEC 2023 §110.14; mfr specs | High |
| 1,600 A switchboard | Arc-flash label currency check | Annually | 0.5 hr | NFPA 70E §130.5(G); 2023 §110.16 | High |
| 1,600 A main breaker | Primary injection trip test | 3-year | 4 hrs | NETA MTS §7.6 (≥ 600 A) | High |
| Distribution panels (6) | Visual inspection — overheating, corrosion, loose wiring | Quarterly | 30 min/panel | NFPA 70B §11.7 | Medium |
| 250 kW genset | Exercise run, fuel, coolant, oil | Monthly | 1 hr | NFPA 110 §8.4 | High |
| 250 kW genset | Load bank to 30% nameplate (if monthly exercise doesn't reach) | Annually | 4 hrs | NFPA 110 §8.4.2 | High |
| ATS | Simulate power failure, transfer/retransfer | Semi-annually | 2 hrs | NFPA 110 §8.4 | High |
| 100 kVA UPS | Battery load test, IR on cells | Quarterly | 2 hrs | IEEE 1188; NFPA 70B §19 | High |
| EV charger bank | Visual + thermal on the 8 stations + branch breakers | Quarterly | 1.5 hrs | NEC 2023 §625; NFPA 70B §11.x | Medium |
| Emergency lighting | 30-second functional test all units | Monthly | 1 hr | NEC 2023 §700.12; local | Medium |
| Emergency lighting | 90-min full discharge test | Annually | 3 hrs | NEC 2023 §700.12 | High |
| Fire alarm circuits | Verify integrity, check for grounds | Annually | 2 hrs | NFPA 72 | High |
| Arc-flash study | Refresh / re-validate | 5-year | PE-managed | NFPA 70E §130.5(G) | High |
| All panels | Update panel schedules and circuit directories | Annually | 4 hrs | NEC 2023 §408.4 | Medium |

**Annual hours estimate:** ~84 visit hours/year + ~8 hours PE/NETA technician time on the 3-year and 5-year cadences amortized.

**Compliance flags:**
- NEC 2023 in force; do NOT cite 2026-only §110.16(B)/(C) language as required. Internal Notes recommends rolling the 2026 assessment-date practice in voluntarily as a best-practice positioning before Portland adopts.
- NFPA 70B 2026 not adopted; cite 2023 sections in the contract.
- Travelers carrier reference to 70B 2023 satisfied by the contract structure.

## Worked Example B — Healthcare: 312-bed Acute-Care Hospital Wing, Indianapolis IN

**Inputs given:**
- Facility: 312-bed acute-care wing, 540,000 sq ft, built 2008, last major electrical upgrade 2019
- Service: 4,000 A 480Y/277 V double-ended switchgear with primary-selective scheme; 12.47 kV utility primary; (2) 2,500 kVA dry-type transformers
- EES: NFPA 99 Type 1 essential electrical system; (2) 1,500 kW paralleled diesel generators with closed-transition ATS; isolation transformer panels for OR / ICU / NICU; 4-second NFPA 99 transfer requirement
- Other: 750 kVA static UPS for biomed; surgical-suite isolation panels with line-isolation monitors (LIM)
- Occupancy: 24/7
- Jurisdiction: Marion County / City of Indianapolis IN, on NEC 2017 (Indiana on 2017; NEC 2020 / 2023 / 2026 not yet adopted statewide as of Q1 2026)
- NFPA 70B: not adopted by Indiana DLI as of Q1 2026; The Joint Commission and CMS reference NFPA 99 / 70 directly

**Path selected:** Healthcare. Most rigorous compliance posture in the matrix.

**Sample matrix excerpts:**

| Component | Task | Frequency | Reference | Priority |
|---|---|---|---|---|
| 4,000 A double-ended switchgear | IR thermography under 60% load | Quarterly | NETA MTS §9; NFPA 70B §11.21 | High |
| 4,000 A switchgear | Primary injection on tie + main breakers | 3-year | NETA MTS §7.6 | High |
| 12.47 kV primary | Visual + partial-discharge sensing | Annually | NETA MTS §7.3 | High |
| (2) 2,500 kVA dry-type | IR + winding insulation resistance | Annually | NETA MTS §7.2; NFPA 70B §11.13 | High |
| EES generators (2 × 1,500 kW) | NFPA 110 monthly exercise (no-load) | Monthly | NFPA 110 §8.4.1 | High |
| EES generators | Annual load bank to 30% nameplate if monthly doesn't reach | Annually | NFPA 110 §8.4.2 | High |
| EES generators | 4-hour load bank | 3-year | NFPA 110 §8.4.2.3 | High |
| Closed-transition ATS | 4-second NFPA 99 transfer verification | Annually | NFPA 99 §6.4.4; NFPA 110 §8.4 | High |
| Isolation transformer panels (OR / ICU / NICU) | LIM functional test (push-to-test + impedance verification) | Monthly | NFPA 99 §6.3.2.6 | High |
| Isolation panels | Hazard-current measurement (line-iso impedance ≤ 200 µA) | Annually | NFPA 99 §6.3.2.6.3.2 | High |
| 750 kVA UPS | Battery cell IR scan, load test | Quarterly | IEEE 1188; NFPA 70B §19 | High |
| EES branches (life-safety / critical / equipment) | Branch separation verification | Annually | NFPA 99 §6.4.2; NFPA 70 §517.30 | High |
| Arc-flash study | Refresh on EES + main, study currency check | 3-year | NFPA 70E §130.5(G) | High |
| All EES panels | Update schedules + arc-flash labels | Annually | NEC 2017 §408.4; NFPA 70E §130.5 | High |

**Annual hours estimate:** ~280 visit hours/year on the EES + main + isolation panels alone; ~40 PE/NETA hours amortized. NETA-certified technician sign-off on all primary injection, transformer winding-resistance, and protective-device testing.

**Compliance flags:**
- Indiana on NEC 2017; do NOT cite 2020/2023/2026-only provisions as required. Several modern healthcare practices (e.g., NEC 2020 §517.31 specific-circuit requirements) are best-practice positions in this jurisdiction.
- NFPA 99 / 110 are CMS Conditions of Participation references — non-negotiable cadence.
- The Joint Commission EC.02.05.07 audit alignment included in the contract structure.
- LIM monthly testing: critical compliance item — failure rate on monthly LIM testing is the most-cited deficiency in the carrier audit data.

## Worked Example C — Light Industrial: 95,000 SF CNC Machine Shop, Cleveland OH

**Inputs given:**
- Facility: 95,000 sq ft light-industrial machine shop, 4 production cells with 22 CNC machine tools (5–75 hp each), built 1998, last electrical upgrade 2018
- Service: 3,000 A 480 V delta with separately-derived 208Y/120 V for offices; (1) 750 kVA dry-type transformer for the office derivation
- Equipment: 4 MCCs (one per cell), 28 VFDs (sized 5–100 hp), passive harmonic-mitigation filters on the two largest CNCs, 480 V buck-boost transformer for legacy lathes
- Occupancy: 2 shifts, M–F, 24h on rush orders
- Jurisdiction: Cuyahoga County / City of Cleveland OH, on NEC 2017 (Ohio adopted NEC 2017 in 2018; on schedule to adopt NEC 2023 in 2026 H2)
- NFPA 70B: not adopted by Ohio Building Code as of Q1 2026
- Insurance: Liberty Mutual; carrier policy references NETA MTS testing on protective devices ≥ 600 A annually

**Path selected:** Light Industrial.

**Sample matrix excerpts:**

| Component | Task | Frequency | Reference | Priority |
|---|---|---|---|---|
| 3,000 A main switchboard | IR thermography under 60% load | Quarterly | NETA MTS §9 | High |
| 3,000 A main breaker | Primary injection trip test | Annually | NETA MTS §7.6 (≥ 600 A; carrier req.) | High |
| 750 kVA dry-type transformer | IR + winding insulation resistance | Annually | NETA MTS §7.2 | High |
| 4 MCCs | IR + visual + bucket-pull check on critical | Quarterly | NFPA 70B §11.x | High |
| 28 VFDs | Parameter log review, drive fault history pull | Quarterly | mfr specs | Medium |
| Harmonic-mitigation filters | THD measurement (V & I) at PCC | Semi-annually | IEEE 519 | Medium |
| Buck-boost transformer | IR + visual | Annually | NFPA 70B §11.13 | Low |
| 22 CNC machine motors | Insulation resistance (megger) on critical motors | Annually | IEEE 43 | Medium |
| Disconnect switches at machines | Visual + arc-flash label currency | Annually | NFPA 70E §130.5; NEC §404 | Medium |
| Service-entrance grounding | Continuity verify (ground-rod, GEC) | Annually | NEC 2017 §250.50 | High |
| Lockout/tagout program | Audit + retraining cycle | Annually | OSHA 1910.147 | High |
| Arc-flash study | Refresh / re-validate | 5-year | NFPA 70E §130.5(G) | High |

**Annual hours estimate:** ~110 visit hours/year + ~20 PE/NETA hours amortized.

**Compliance flags:**
- Ohio on NEC 2017 today; expected to adopt 2023 in H2 2026. Internal Notes recommends using the proposal to position a 2023-readiness assessment as a value-add at adoption.
- OSHA 1910.269 NOT triggered (no utility-owned distribution within scope).
- IEEE 519 referenced for harmonic compliance at the point of common coupling — Liberty Mutual policy references this for facilities ≥ 1,000 A with VFD load.
- NETA MTS-aligned testing on the 3,000 A main is the carrier's hard requirement; the contract structure surfaces it as a separate line item to make insurance-renewal documentation clean.

## Worked Example D — Multifamily (Light Commercial): 24-Unit Apartment, Seattle WA

**Inputs given:**
- Facility: 24-unit walk-up multifamily, 28,000 sq ft, built 2011
- Service: 600 A 208Y/120 V house service + 24 unit meters (apartment-side); 200 A common-area panel with 4 EV chargers (40 A each) added in 2024
- Equipment: 600 A house panel, 4 EV chargers, common-area lighting, emergency lighting (battery-pack only — no generator), fire alarm
- Occupancy: residential 24/7
- Jurisdiction: City of Seattle WA, on NEC 2026 (adopted state-wide effective March 2026)
- NFPA 70B: not adopted

**Path selected:** Light Commercial.

**Sample matrix excerpts:**

| Component | Task | Frequency | Reference | Priority |
|---|---|---|---|---|
| 600 A house service | IR thermography | Annually | NFPA 70B §11.21 | High |
| 600 A house service | Torque verification | Annually | NEC 2026 §110.14; mfr | High |
| 600 A house panel | Visual inspection | Semi-annually | NFPA 70B §11.7 | Medium |
| 600 A house panel | Arc-flash label currency check (NEC 2026 §110.16(B)/(C)) | Annually | NEC 2026 §110.16; NFPA 70E | High |
| 200 A common-area panel | Visual + thermal | Annually | NFPA 70B §11.7 | Medium |
| 4 EV chargers (40 A each) | Visual + thermal + EVSE-to-panel terminal torque | Quarterly | NEC 2026 §625; NFPA 70B §11.x | Medium |
| Emergency lighting (battery-pack only) | 30-sec functional test | Monthly | NEC 2026 §700.12; local | Medium |
| Emergency lighting | 90-min discharge test | Annually | NEC 2026 §700.12 | High |
| Fire alarm circuits | Verify integrity, check for grounds | Annually | NFPA 72 | High |
| Tenant unit panels | Visual sample (10% / year) | Annually | NEC 2026 §408.4 | Low |

**Annual hours estimate:** ~24 visit hours/year. Light Commercial cadence with the EV-charger sub-loop driving the only quarterly cadence item.

**Compliance flags:**
- Seattle on NEC 2026; arc-flash label assessment-date practice is required (§110.16(B)/(C)).
- No genset; emergency lighting battery-pack monthly test is the only NFPA 110 / 700.12 cadence item.
- Owner-side note: the EV-charger sub-loop is the highest IR-flag risk; the proposal positions a quarterly thermal as a low-cost early-warning on EVSE terminal heating.

## Worked Example E — Heavy Commercial: 180,000 SF Cold-Storage Warehouse, Boise ID

**Inputs given:**
- Facility: 180,000 sq ft cold-storage warehouse with 4 freezer rooms (-20°F) and 2 cooler rooms (+38°F), built 2017
- Service: 2,500 A 480Y/277 V; (1) 1,500 kVA dry-type transformer for office derivation
- Equipment: (6) MCCs serving refrigeration compressors and evaporator fans, 18 VFDs on the larger compressors, defrost-cycle controls, 600 A house panel, 2 EV chargers in the office lot, no genset
- Occupancy: 24/7 (cold storage)
- Jurisdiction: Ada County / City of Boise ID, on NEC 2020 (Idaho adopted NEC 2020 in 2021)
- NFPA 70B: not adopted

**Path selected:** Heavy Commercial (lifted from the borderline because of the 6-MCC + 18-VFD load profile and the cold-chain criticality).

**Sample matrix excerpts:**

| Component | Task | Frequency | Reference | Priority |
|---|---|---|---|---|
| 2,500 A switchboard | IR thermography under 60% load | Quarterly | NETA MTS §9; NFPA 70B §11.21 | High |
| 2,500 A main breaker | Primary injection trip test | 3-year | NETA MTS §7.6 | High |
| 1,500 kVA dry-type transformer | IR + insulation resistance | Annually | NETA MTS §7.2 | High |
| (6) MCCs | IR + bucket-pull on critical compressors | Quarterly | NFPA 70B §11.x | High |
| 18 VFDs | Parameter log review, fault history pull | Quarterly | mfr specs | High |
| Refrigeration compressor motors | Insulation resistance on critical | Annually | IEEE 43 | High |
| Defrost-cycle controls | Functional test | Annually | mfr specs | Medium |
| Cold-chain alarm | End-to-end test (compressor → BMS → on-call) | Quarterly | facility SOP | High |
| 600 A house panel | Visual + thermal | Semi-annually | NFPA 70B §11.7 | Medium |
| 2 EV chargers | Visual + thermal | Quarterly | NEC 2020 §625; NFPA 70B §11.x | Low |
| Service-entrance grounding | Continuity verify | Annually | NEC 2020 §250.50 | High |
| Arc-flash study | Refresh / re-validate | 5-year | NFPA 70E §130.5(G) | High |

**Annual hours estimate:** ~120 visit hours/year + ~16 PE/NETA hours amortized. Cold-chain alarm test is the highest-leverage item — a single defrost-control failure on a freezer room is a six-figure inventory event.

**Compliance flags:**
- Idaho on NEC 2020 today; do NOT cite 2023/2026-only provisions as required.
- No genset, but cold-chain criticality argues for a backup-power feasibility study as a separate proposal (out of PM scope).
- Most-cited insurance-carrier item on cold-storage facilities is the cold-chain alarm end-to-end test cadence — the proposal positions it as a quarterly carrier-friendly line.

## Output ROI Block (template — populate with actual data points)

- Reactive maintenance avg cost-per-event for facility class: typically 4–8× the equivalent PM line-item cost (industry-aggregated; cite carrier data if known)
- Insurance-carrier premium impact: NETA-aligned PM programs typically reduce E&O / property premium by 5–12% (carrier-specific; verify with broker)
- Equipment lifespan: documented PM extends switchgear, transformer, and motor life by 15–30% (NFPA 70B §1.3 rationale; NEMA application data)
- Unplanned-outage avoidance: 60%+ of low-voltage equipment failures are preceded by a thermographic anomaly visible 30–90 days earlier — the quarterly IR is the highest-leverage line item

## Anti-Plagiarism Notes

The Path Selection table, frequency-by-path matrix, Equipment-Class PM Template Library structure, and the five worked-example scopes are original to this skill but follow standard NFPA 70B / NETA MTS structural conventions; cadence intervals are aggregated industry-wide ranges (NETA MTS-2023 published intervals; NFPA 110 / 99 statutory intervals; IEEE Std 902 industrial intervals). The library's equipment-class template values are illustrative defaults the firm replaces with its own config; they are not lifted from any single vendor's PM schedule. NEC and NFPA section references (§110.14, §110.16, §250.50, §408.4, §625, §700.12, NFPA 70B §§9.1-9.4 / §§11.x / §§17.x / §§19.x, NFPA 70E §130.5(G), NFPA 99 §6.3 / §6.4, NFPA 110 §8.4, NFPA 72) are uncopyrightable code citations. NETA MTS section references (§7.2, §7.3, §7.6, §9) are uncopyrightable industry-standard citations. IEEE 43, IEEE 519, IEEE 1188 references are uncopyrightable industry-standard citations. Worked-example facility identifiers ("Class A office, Portland OR"; "312-bed acute-care wing, Indianapolis"; "95,000 SF CNC shop, Cleveland"; "24-unit multifamily, Seattle"; "180,000 SF cold-storage, Boise") are fictional. Real carrier names (Travelers, Liberty Mutual) are uncopyrightable trade names referenced in their actual business sense. Real utility / building-code authority names (CMS, The Joint Commission, Indiana DLI, Ohio Building Code) are uncopyrightable references to the actual public bodies.
