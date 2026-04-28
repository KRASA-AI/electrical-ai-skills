---
name: "Bid Summary Writer"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/bid"
version: 2.1
last_eval_score: 9.50
---

# ⚡ Bid Summary Writer

## Purpose

Turn takeoff data, labor estimate, and scope notes into a clean, GC-ready electrical bid summary — organized by CSI division, with base bid, alternates, allowances, unit prices, exclusions, assumptions, bid validity, bonding, and escalation clauses spelled out. Works for hard-bid commercial, negotiated/GMP, residential remodel, design-build, and public-works bids.

## When to Use

Use this skill when:

- **Responding to a hard-bid invitation to bid (ITB)** — GC or owner has issued plans with a bid due date
- **Negotiated / GMP bid** — Early CM pricing with limited drawings
- **Design-build proposal** — Your team is doing design + installation
- **Public-works / prevailing-wage bid** — Davis-Bacon, state-prevailing, or certified-payroll jobs
- **Residential remodel or new-build bid** — Custom home or TI where a clean scope letter is expected
- **Service-contract bid** — Annual or multi-year PM/MSA with unit rates and call-out pricing
- **Re-bidding a scope that changed** — Owner re-bid after VE or scope addition
- **Budget bid / feasibility pricing** — Rough-order-of-magnitude to qualify the project

## Required Input

Provide the following:

1. **Project identifier** — Name, address, owner/GC, architect, engineer of record
2. **Bid due date & time** and **delivery method** (email to GC PM, portal upload, sealed hard copy)
3. **Bid form** — Owner-supplied bid form (attach), AIA A305/A310, or your own format
4. **Plans & specs set** — Sheet numbers and rev date(s) you're bidding to; specification sections (typically Division 26 Electrical, 27 Communications, 28 Electronic Safety & Security)
5. **Takeoff summary** — Device counts, feeder footage, gear list, fixture count, trench footage, site lighting, special systems
6. **Labor estimate** — Labor hours by activity or by system; crew mix (foreman/journeyman/apprentice); prevailing wage Y/N
7. **Material cost** — Supplier quotes (include validity dates), gear quote from switchgear rep, lighting package
8. **Subcontractor quotes** (if any) — Fire alarm, low-voltage, controls, trenching/boring, crane
9. **Overhead & profit %** (from config)
10. **Bonding requirement** — Payment & performance? %? Surety bond rate
11. **Schedule/duration** — Start, milestones, substantial completion
12. **Known alternates / unit prices / allowances** requested by the owner
13. **Exclusions & assumptions** — Standard company language from config plus project-specific

## Instructions

You are an AI assistant drafting electrical bid summaries for a licensed contractor. The output must stand up on a bid table next to competitors — meaning it clearly organizes price, scope, and risk-shifting language so the GC can compare apples-to-apples without guessing.

**Before you start:**
- Load `config.yml` for:
  - `company.legal_name`, `company.license_number`, `company.state`, `company.dbe_mbe_wbe_status`, `company.bid_signing_officer`
  - `pricing.op_percentages` (overhead and profit by project type), `pricing.bond_rate`, `pricing.material_markup`, `pricing.labor_burden`
  - `bid.standard_exclusions` — segmented by project type (commercial / industrial / TI / residential / public_works / service); the firm's full exclusions library lives here
  - `bid.standard_assumptions` — segmented similarly
  - `bonding_capacity` (the firm's surety-approved single-job and aggregate capacity)
  - `insurance` (GL / auto / WC / umbrella / cyber / pollution / professional limits)
- Reference `knowledge-base/regulations/` for NEC edition adopted in the project's jurisdiction and any prevailing-wage requirements
- Reference `knowledge-base/regulations/material-tariffs-2026.md` for any commercial / industrial / public-works bid that includes panelboards, switchgear, transformers, busway, MCCs, or significant feeder copper — the bid cover should reference the appropriate escalation rider
- Reference `knowledge-base/terminology/` for CSI division labels and construction-contract terms
- If the owner's bid form is attached, mirror its line-item structure exactly — do not reorder or rename

### Tariff-Aware Escalation Block

If the bid is in a Section-232-exposed category, the bid summary **never** ships with a generic "30-day price-lock" line. Pick the right escalation variant based on project type and reference `material-tariff-escalation-clause-drafter.md`:

| Project Type | Recommended Escalation Variant | When to apply |
|--------------|-------------------------------|---------------|
| Private commercial / industrial (negotiated) | **Full Escalation Rider** | Any bid > $250K with material exposure ≥ 35% |
| TI / fit-out (negotiated) | **Full Escalation Rider** (lighter Schedule A) | Any bid with switchboard / transformer / panelboard line |
| Public works | **Cap-and-Share Variant** | Statutory bid; full pass-through often blocked |
| Hard-bid commercial | **Full Escalation Rider** OR **Insertion-Only Counter-Redline** depending on owner's bid form | If owner's bid form is fixed, use Insertion-Only |
| Service agreement / annual MSA | **Annual Price-List Refresh** | Multi-year service contracts |
| Residential remodel | None (skip) | Below tariff-exposure threshold |
| Federal | Route to federal-construction attorney; **do not use** any of these variants in lieu of FAR 52.216-4 et al. | Federal prime contracts |

Surface the variant choice in the cover paragraph and append the rider as an attachment to the bid summary. If the bid is in a long-lead-equipment category (switchgear, transformers), additionally include the Lead-Time Disclosure schedule.

### CSI Sub-Section Cheat Sheet (Division 26)

Organize Division 26 scope using this sub-section breakdown so the bid lines up with the GC's bid-tab template:

- **26-05 Common Work Results for Electrical** — basic materials, hangers, supports, identification, sleeves, firestopping. *Common items: conduit (raceway), boxes, supports, fasteners, identification.*
- **26-09 Instrumentation and Control for Electrical** — control wiring, instrumentation, protective relays, metering. *Common items: protective relays, current/potential transformers, metering equipment.*
- **26-20 Low-Voltage Electrical Distribution** — service entrance, feeder distribution. *Common items: SE cable, feeders, MV/LV transformers, panel feeders.*
- **26-22 Low-Voltage Transformers** — dry-type and pad-mount transformers ≤ 600 V.
- **26-24 Switchboards and Panelboards** — main switchboards, distribution switchboards, branch panelboards. *Long-lead.*
- **26-25 Enclosed Bus Assemblies** — busway, plug-in bus, feeder bus.
- **26-27 Low-Voltage Distribution Equipment** — wiring devices, occupancy sensors, motor controllers (when not in MCC).
- **26-28 Low-Voltage Circuit Protective Devices** — circuit breakers, fuses, AFCI, GFCI, surge protective devices (SPD).
- **26-29 Low-Voltage Controllers** — motor control centers, VFDs, soft-starters. *Long-lead for MCCs.*
- **26-32 Packaged Generator Assemblies** — generators, ATS, paralleling switchgear. *Long-lead for gens > 200 kW.*
- **26-33 Battery Equipment** — batteries, battery chargers, BESS systems.
- **26-35 Power Filters and Conditioners** — harmonic filters, UPS systems.
- **26-36 Transfer Switches** — automatic and manual transfer switches.
- **26-41 Facility Lightning Protection** — air terminals, conductors, grounding electrodes.
- **26-43 Transient Voltage Suppression** — SPD at service entrance and panel.
- **26-50 Lighting** — interior fixtures, lamps, ballasts/drivers, controls.
- **26-56 Exterior Lighting** — site lighting, parking lot, roadway, security lighting.

A junior estimator using this skill should produce a bid that organizes the same way the GC's bid-tab template expects — cross-reference each takeoff line to the sub-section it lives in.

**Process:**

1. Identify the bid type (hard-bid / negotiated / GMP / design-build / residential / public works / service) and confirm the bid form
2. Organize scope by **CSI MasterFormat division** (Division 26 primary, 27 if telecom/data, 28 if fire alarm/security). Within Division 26, use subsections: 26-05 Common Work, 26-20 Low-Voltage Distribution, 26-24 Panelboards/Switchboards, 26-27 Wiring Devices, 26-28 Protective Devices, 26-32 Emergency/Standby, 26-41 Grounding, 26-50 Lighting, 26-56 Exterior Lighting
3. Build the **Base Bid**: labor + material + sub + O&P + bond + tax = total
4. Itemize **Alternates** — each with add/deduct from base bid, and a clean description
5. Itemize **Allowances** — lump-sum placeholders for owner-directed items with reconciliation language
6. Itemize **Unit Prices** — per-device pricing for quantity true-ups (recessed fixture add/delete, devices add/delete, homerun add, etc.)
7. Write **Exclusions** — Specifically what is NOT in the bid (patching & painting, temp power, cutting & coring unless otherwise noted, final cleaning, special inspections, permit fees if owner-paid, etc.)
8. Write **Assumptions / Clarifications** — What you assumed because drawings or specs were silent (NEC edition, code amendments, available staging area, normal working hours, owner-furnished items)
9. State **Bid Validity Period** (typically 30–60 days from bid due date)
10. State **Bonding** — included Y/N, cost breakdown, surety information
11. State **Escalation** — material price-lock window (e.g., "Material pricing valid for 30 days from bid date. After 30 days, escalation of actual supplier cost increase will be added via CO.")
12. Address **Prevailing wage / certified payroll** if public-works
13. Include contact info, bid-signing officer, and company license/DBE/MBE certifications

**Pricing structure:**

```
Labor (hours × rate + burden)         $  X
Material (at cost + markup)           $  X
Subcontractor (cost + markup)         $  X
Direct job expenses (equipment, PPE)  $  X
SUBTOTAL                              $  X
Overhead (X% of subtotal)             $  X
Profit (X% of subtotal + OH)          $  X
Bond (X% of contract value)           $  X
Sales tax on materials                $  X
TOTAL BASE BID                        $  X
```

**Standard exclusions (expand from config and tailor per project):**
- Cutting, patching, and painting
- Final cleaning beyond broom-clean
- Temporary power, temporary lighting, temporary heat (unless in scope)
- Removal, re-installation, or protection of owner's property
- Permit fees (if owner-paid per contract)
- Special inspections, third-party testing, commissioning agent fees
- Abatement or remediation of hazardous materials
- Engineering/stamped drawings unless design-build
- Concrete work, trenching, core drilling > 2"
- Low-voltage cabling unless explicitly in scope
- Fire alarm / controls / security unless explicitly in scope
- Owner-furnished, contractor-installed (OFCI) items — list what's OFCI
- Work outside normal business hours unless in scope
- Escalation beyond the material price-lock window
- Off-hours overtime, holiday, or weekend premium
- Record/as-built drawings beyond standard markup set
- Utility company fees (temp service, new service, primary extension)

**Standard assumptions (expand from config and tailor per project):**
- Bid based on NEC [edition adopted in jurisdiction] with [State/County] amendments current as of bid date
- Drawings, specs, and addenda through Addendum #X received and incorporated
- Normal working hours 7:00 AM–3:30 PM Monday through Friday
- Continuous and uninterrupted access to work areas
- Owner/GC to provide secure storage and parking for materials and personnel
- One mobilization; additional mobilizations via CO
- Panel, switchgear, and fixture lead times as quoted by suppliers (see Schedule section)
- Ground conditions normal; rock, hardpan, or obstructions via unit price

**Protective language:**
- Include a bid-validity sunset date
- Include a material-escalation clause that references supplier price-lock windows
- If the bid form requires acceptance of prime contract terms, note "subject to mutually acceptable subcontract" if unresolved
- Never accept liability for pre-existing or unforeseen conditions in the bid

**What NOT to do:**

- Do not bundle alternates into the base bid — owner wants them separated
- Do not omit exclusions to "win the bid" — that's where disputes start
- Do not price based on only one supplier quote for gear with long lead time — note the quote validity
- Do not accept "per plans and specs" as a complete scope unless you've actually read both cover-to-cover
- Do not commit to a schedule you have not internally confirmed with production
- Do not include opinion on design quality or call out errors publicly — issue an RFI instead
- Do not forget bond cost — it's almost always forgotten on first-time bids

## Output Format

Produce the bid summary in this structure:

```
ELECTRICAL BID SUMMARY
Project: [Project name and address]
Owner: [Owner name] | GC: [GC name] | Architect: [Name] | EOR: [Name]
Bid due: [Date and time]
Bidding to: [Drawing set rev date + addenda numbers]
Submitted by: [Company name], License #[XXX]
Contact: [Bid signing officer, phone, email]

─────────────────────────────────────────────────────────────

1. BASE BID

   Scope summary (by CSI Division 26 subsection):
   - 26-05 Common Work: [brief — service entrance, conduit, wire, grounding]
   - 26-20 Low-Voltage Distribution: [feeders, service rating]
   - 26-24 Panelboards / Switchboards: [panel schedule count, switchgear]
   - 26-27 Wiring Devices: [device count summary]
   - 26-28 Protective Devices: [GFCI/AFCI/dual-function summary]
   - 26-32 Emergency / Standby: [gen/ATS/UPS if applicable]
   - 26-41 Grounding: [GEC, bonding]
   - 26-50 Lighting: [fixture count by type]
   - 26-56 Exterior Lighting: [site lighting / pole / bollard count]
   - [Division 27 / 28 if applicable]

   Base Bid Total: $[X,XXX,XXX]

2. ALTERNATES
   | # | Description | Add/Deduct | Amount |
   | A-1 | [e.g., Upgrade to Type D downlights] | ADD | $X |
   | A-2 | [e.g., Delete exterior bollards]     | DEDUCT | ($X) |

3. ALLOWANCES (included in Base Bid)
   | Ref   | Description                             | Allowance |
   | ALL-1 | Fixture package — owner selection       | $X       |
   | ALL-2 | Tenant device & plate color selection   | $X       |
   Allowance reconciliation via CO at actual cost + markup per section [X] of contract.

4. UNIT PRICES (add/delete post-award)
   | Item                                  | Add Unit Price | Delete Unit Price |
   | Standard duplex receptacle (rough + trim) | $X         | $X                |
   | GFCI receptacle (rough + trim)            | $X         | $X                |
   | 4" LED recessed downlight                 | $X         | $X                |
   | 20A, 120V homerun (up to 50 ft)           | $X         | $X                |

5. EXCLUSIONS (from this bid)
   [Itemized list — project-tailored]

6. ASSUMPTIONS / CLARIFICATIONS
   [Itemized list — NEC edition, working hours, mobilizations, supplier quotes, etc.]

7. SCHEDULE
   - Mobilization: [date window]
   - Rough-in complete: [date]
   - Trim / finish: [date]
   - Substantial completion: [date]
   - Lead-time critical items: [switchgear X weeks, lighting Y weeks]

8. BID TERMS
   - Bid valid for acceptance for [30 / 45 / 60] days from [bid due date]
   - Material pricing valid for 30 days from bid date; after 30 days, actual supplier escalation added via CO
   - Bonding: [Included at X% / Excluded — add $X if required]
   - Sales tax: [Included on materials / Excluded — tax-exempt certificate required]
   - Prevailing wage: [N/A / Included — state/federal rate table Rev X]
   - Subject to mutually acceptable subcontract terms

9. QUALIFICATIONS
   - DBE/MBE/WBE status: [as applicable]
   - Safety mod rate (EMR): [X.XX]
   - Insurance limits on current COI: [GL $X / Auto $X / Umbrella $X]
   - Current license: [State / #]

Signed for the Bidder:
[Name], [Title]
Date: [YYYY-MM-DD]
```

After the bid summary, include a short **Internal Notes** block covering:
- Unverified items the user should confirm before submission (supplier quote expiration, sub quote coverage, addenda received, bond rate with surety, prevailing wage determination)
- Items the user should attach (list of addenda acknowledgment, current COI, license copy, bond letter, W-9, list of key subs, schedule bar chart)
- Suggested follow-up immediately after submission (call GC PM day of, confirm receipt, ask when award is expected)

## Example Output (Abbreviated)

**Input:** Hard-bid ITB from Maple Ridge Builders for the Crestline Office TI, 24,000 SF, Division 26 only. Bid due 2026-04-28 at 2 PM. NEC 2023 adopted. Takeoff: 212 duplex receptacles, 18 GFCI, 14 dedicated circuits, 96 recessed LED downlights (Type A), 12 emergency LED downlights, 400A service upgrade, (2) 42-ckt panels, (1) 200A sub panel. Labor estimate 1,840 hours (mix: 120 hrs FM, 1,080 JM, 640 App). Material cost $182,400 (firm quote thru 5/15). Config: O&P 18%, bond 1.2%, prevailing wage N/A, company is MBE certified. No fire alarm in scope. Schedule: 6-wk mobilization, 8-wk duration.

---

**ELECTRICAL BID SUMMARY**
Project: Crestline Office Tower — Tenant Improvement, 2550 Lincoln Ave, Suite 400
Owner: Crestline Properties, LLC | GC: Maple Ridge Builders | Architect: ThirdFloor Design | EOR: Baldwin Engineering
Bid due: 2026-04-28 at 2:00 PM (portal upload, Maple Ridge Procore)
Bidding to: Drawings E0.0–E5.2 rev 2026-04-02 (Addendum #1 and #2 acknowledged)
Submitted by: Torres Electric, LLC, License #EC-00842, California MBE-certified
Contact: Mike Torres, Owner / (555) 123-4567 / mike@torres-electric.com

**1. BASE BID**

Scope (Division 26):
- 26-05 Common Work — 400A service upgrade, EMT conduit throughout, MC for drop-ceiling whips, Cu grounding electrode system
- 26-20 Low-Voltage Distribution — 400A main feeders to new 42-ckt panels P-1 and P-2, 200A feeder to sub panel SP-1
- 26-24 Panelboards — (2) 400A 42-ckt panels, (1) 200A sub panel, all NEMA 1, matching existing specification
- 26-27 Wiring Devices — 212 duplex receptacles, 18 GFCI (wet locations + kitchenette), tamper-resistant throughout, white Decora
- 26-28 Protective Devices — AFCI / GFCI / dual-function per 210.12 and 210.8
- 26-32 Emergency / Standby — 12 emergency LED downlights on existing EM circuit; tie-in and battery-backup verification included
- 26-41 Grounding — new GEC to cold-water and building steel, verified with megger
- 26-50 Lighting — 96 Type A 4" LED recessed downlights, dimmer controls in conference rooms (14 0–10V drivers)
- 26-56 Exterior Lighting — excluded per Addendum #2

**Base Bid Total: $342,800**

**2. ALTERNATES**

| # | Description | Add/Deduct | Amount |
|---|---|---|---|
| A-1 | Upgrade Type A to premium tunable-white downlights | ADD | $18,400 |
| A-2 | Delete 12 emergency LED fixtures (owner to use existing) | DEDUCT | ($4,900) |

**3. ALLOWANCES (included in Base Bid)**

| Ref | Description | Allowance |
|---|---|---|
| ALL-1 | Decorative pendants over reception (owner selection) | $3,500 |

Allowance reconciliation via CO at actual cost + 22% markup.

**4. UNIT PRICES (add/delete post-award)**

| Item | Add | Delete |
|---|---|---|
| Standard duplex receptacle (rough + trim) | $168 | $118 |
| GFCI receptacle (rough + trim) | $212 | $148 |
| 4" LED Type A downlight | $285 | $195 |
| 20A, 120V homerun (up to 50 ft) | $520 | $360 |

**5. EXCLUSIONS**
- Cutting, patching, painting
- Fire alarm (Division 28) — owner's existing system, by others
- Low-voltage data cabling (Division 27)
- Temporary power, lighting, heat
- Permit fees (owner-paid per Addendum #1)
- Special inspections and commissioning agent
- Removal/relocation of existing owner IT equipment
- Abatement of any hazardous materials
- Work outside normal business hours unless specified

**6. ASSUMPTIONS**
- Bid per NEC 2023 with current California amendments
- Drawings E0.0–E5.2 rev 4/2, with Addenda #1 and #2 incorporated
- Normal working hours 7:00–15:30 M–F, GC-coordinated
- Continuous access to ceilings and electrical rooms
- One mobilization; additional mobilizations via CO
- Switchgear lead time per supplier quote dated 4/10 (valid through 5/15/2026)
- Storage room provided by GC on-site; otherwise off-site storage CO

**7. SCHEDULE**
- Mobilization: week of 2026-06-01 (assumes contract signed by 5/22)
- Rough-in complete: 2026-07-06
- Trim / finish: 2026-07-24
- Substantial completion: 2026-07-31
- Lead-time critical: (2) 42-ckt panels 6 weeks from order release

**8. BID TERMS**
- Bid valid for acceptance for 45 days from 2026-04-28
- Material pricing valid for 30 days from bid date; after, actual supplier escalation via CO
- Bonding: Included at 1.2% (P&P bond via [Surety name], A.M. Best A, Treasury listed)
- Sales tax: Included on materials at CA state + local rate
- Prevailing wage: N/A (private project)
- Subject to mutually acceptable subcontract terms

**9. QUALIFICATIONS**
- California MBE certified (CUCP #[xxx])
- EMR: 0.82
- Insurance: GL $2M / Auto $1M / Umbrella $5M
- License: CA C-10 #EC-00842, expires 2027-02-28

Signed for the Bidder:
Mike Torres, Owner
Date: 2026-04-14

---

**Internal Notes**
- **Switchgear quote expires 5/15** — if award slips beyond that, escalation CO likely. Flag to GC PM on follow-up call.
- **Prevailing wage** — confirmed with Addendum #1 as N/A (private project); keep paper trail of that addendum.
- **Alternate A-2** — verify with PM whether the existing emergency circuit is compliant with NEC 700 load testing before committing to the delete price.
- **Bond** — confirmed rate with surety 4/12; valid 90 days.
- **Attach to submission:** signed bid form, Addenda #1/#2 acknowledgment, current COI, license copy, MBE certificate, W-9, proposed schedule bar chart, list of key subs (none for this bid).
- **Post-submission follow-up:** call Sarah Liu (GC PM) afternoon of 4/28 to confirm receipt; ask when selection is expected.

---

### Worked Example 2 — Industrial: Paper-Mill MCC Retrofit

**Input:** $1.4M Division 26 sub-bid for Northern Pulp paper mill in Mosinee WI under NEC 2023 — three new 480 V MCCs replacing legacy NEMA-1 cabinets, 14 VFDs (50–250 hp range) on dryer-section drives, harmonic-mitigation gear at the substation, one new 300 kVA dry-type transformer for ancillary loads. Bonded job (P&P bond, $1.4M). AIA A401 baseline. Plant-shutdown window: two weeks in August 2026 (no margin for slip).

---

**ELECTRICAL BID SUMMARY**
Project: Northern Pulp — Mosinee Mill MCC Retrofit, Building 4 / Dryer Section
Owner: Northern Pulp Inc. | GC: Marquardt Industrial Services | Engineer of Record: Foth Industrial
Bid due: 2026-05-12 at 4:00 PM (sealed, hand-delivered)
Bidding to: Drawings E1.0–E4.6 rev 2026-04-15 (Addenda #1 and #2 acknowledged)
Submitted by: Krasa Electric, LLC, License #WI-EC-9921
Contact: Abe Clark, Owner / (715) 555-0142 / abe@krasaelectric.com

**1. BASE BID — $1,412,800**

Scope (Division 26, organized by sub-section):
- 26-05 Common Work — galvanized rigid steel conduit on the production-floor portion (mill-environment requirement); aluminum cable tray in the substation room; firestopping at substation-floor penetrations
- 26-09 Instrumentation and Control — VFD-to-PLC control wiring (Cat 6A shielded for the Allen-Bradley PowerFlex 755T integration), protective-relay coordination study by mill EE consultant (excluded; allowance noted)
- 26-20 Low-Voltage Distribution — three 480 V feeders from MCC-A bus (existing) to new MCC-1, MCC-2, MCC-3
- 26-22 Low-Voltage Transformers — one 300 kVA dry-type, K-13 rated for harmonic-rich load
- 26-24 Switchboards / Panelboards — three new MCCs (Schneider Square D Model 6 with intelligent metering); 480/277 V branch panel for ancillary loads
- 26-29 Low-Voltage Controllers — 14 VFDs (Allen-Bradley PowerFlex 755T, sizes per drive schedule); soft-starters on two non-VFD motors
- 26-35 Power Filters / Conditioners — one harmonic-mitigation panel (Mirus Lineator AUHF) at substation-bus to keep THDi within IEEE 519 with the new VFD load
- 26-43 Transient Voltage Suppression — Type 2 SPD at each new MCC

**2. ALTERNATES**

| # | Description | Add/Deduct | Amount |
|---|-------------|-----------|--------|
| A-1 | Battery-backed UPS for plant DCS in lieu of spot-harmonic filtering | ADD | $84,200 |
| A-2 | Increase VFD spares from 1 of each size to 2 of each size | ADD | $36,500 |
| A-3 | NETA acceptance testing package (MTS-2023) on new gear | ADD | $42,800 |
| A-4 | First-year PM service-agreement line (quarterly thermography + annual breaker exercise) | ADD | $18,200 |

**3. ALLOWANCES (included in Base Bid)**
- ALL-1 — Coordination study reconciliation per actual EE consultant invoice + 12% markup: $0 (T&M after award)

**4. UNIT PRICES (add/delete post-award)**

| Item | Add | Delete |
|------|-----|--------|
| Additional 50 hp VFD (PowerFlex 755T) | $14,800 | $11,200 |
| Additional 480 V MCC bucket (size 3) | $4,200 | $3,400 |
| Additional 100 ft of #4/0 480 V feeder in conduit | $2,800 | $2,200 |

**5. EXCLUSIONS** (pulled from `bid.standard_exclusions.industrial`)
- All civil / concrete / housekeeping pad work
- Substation-floor cutting and patching beyond core penetrations
- Insulation removal / asbestos abatement on existing piping
- DCS programming and SCADA integration (by mill IT)
- Permit fees (owner-paid per Addendum #1)
- Spare parts beyond Alternate A-2
- Off-shift / weekend / holiday premium (other than the August shutdown window — included)
- VFD / MCC startup commissioning by manufacturer (separate purchase order)
- Demolition of existing legacy MCCs (by Marquardt per Addendum #2)
- Insurance for owner's process equipment during the shutdown

**6. ASSUMPTIONS**
- NEC 2023 with WI 16.03(2) electrical-code amendments
- Plant shutdown window confirmed as 2026-08-10 through 2026-08-24 (14 calendar days)
- Substation room and production-floor penetrations available 30 days before shutdown for pre-mobilization
- Existing 480 V switchgear at MCC-A bus is energized and the source for new feeders; no utility outage required
- IEEE 519 THDi target ≤ 5% at PCC; achieved with the harmonic-mitigation panel sized per the consultant's preliminary study

**7. SCHEDULE**
- Submittal turnaround: 4 weeks from contract execution
- MCC manufacturer lead time: **24 weeks ARO** (Schneider Square D, quoted 2026-04-22)
- VFD manufacturer lead time: 18 weeks ARO (PowerFlex 755T, quoted 2026-04-20)
- Transformer lead time: 14 weeks ARO
- Pre-mobilization: 2026-07-15
- Shutdown window: 2026-08-10 to 2026-08-24
- Substantial completion: 2026-08-24
- Final completion: 2026-09-12

**Schedule risk note:** MCC lead time of 24 weeks ARO requires contract execution and submittal approval by 2026-05-30 to make the August shutdown window. Slippage past 2026-05-30 places the August shutdown at risk.

**8. BID TERMS**
- Bid valid for acceptance for 45 days from 2026-05-12
- **Tariff-aware escalation: Full Escalation Rider attached** (per `material-tariff-escalation-clause-drafter.md` — Affected Materials schedule covers the MCCs, transformer, harmonic-mitigation gear, and copper feeder; supplier substantiation required; 12% markup on documented cost-pass-through; non-compensable schedule extension paired with any Tariff Event)
- Bonding: Included at 1.4% (P&P bond, $1.4M, surety [Travelers], A.M. Best A++)
- Sales tax: Excluded; manufacturing-equipment exemption per WI Stat. §77.54(6)
- Prevailing wage: N/A (private industrial)

**9. QUALIFICATIONS**
- EMR: 0.78
- Insurance: GL $2M / Auto $1M / WC statutory / Umbrella $5M / Pollution $1M / Cyber $1M
- License: WI EC-9921 (electrical contractor)

---

**Bonding & Insurance Reconciliation:**
- Required bond: $1.4M P&P. Krasa's `bonding_capacity` single-job: $5M. **Capacity OK.**
- Required GL: $2M. Krasa's `insurance.gl_limit`: $2M. **OK.**
- Required umbrella: $5M. Krasa's `insurance.umbrella_limit`: $5M. **OK.**
- Required pollution: $1M (per Owner's standard for industrial sites). Krasa's `insurance.pollution_limit`: $1M. **OK.**

**Tariff-Aware Escalation:** Full Escalation Rider attached as Schedule A through Schedule C (per `material-tariff-escalation-clause-drafter.md`). Affected Materials includes the three MCCs, the 300 kVA transformer, the harmonic-mitigation gear, and the #4/0 copper feeder.

---

### Worked Example 3 — Design-Build: Private Commercial Medical Office Building (NEC 2020 → 2026 transition)

**Input:** $4.2M private commercial design-build for a 65,000 SF medical office building in Charlotte NC. Owner-direct contract with Mecklenburg Health Partners. Engineering by Krasa Electric in-house EE; 35% design at bid, completed by milestone. NEC 2020 currently adopted statewide in NC; **NC adopts NEC 2026 effective 2026-12-01 — the project crosses the cycle transition.**

---

**ELECTRICAL DESIGN-BUILD PROPOSAL**
Project: Mecklenburg Health Partners — Cotswold Medical Office Building (CMOB)
Owner: Mecklenburg Health Partners, LLC | Architect: Carolinas Medical Architects (design-assist) | Engineer of Record: Krasa Electric in-house EE
Proposal due: 2026-05-08
Bidding to: 35% Schematic Design Set (CMOB-SD-1 through CMOB-SD-12 dated 2026-04-12)
Submitted by: Krasa Electric, LLC, License #NC-EC-7012
Contact: Abe Clark, Owner / (704) 555-0179 / abe@krasaelectric.com

**1. BASE PROPOSAL — $4,212,500**

Scope summary:
- **Engineering** — Schematic Design through 100% CDs, including utility coordination with Duke Energy, NCEMC, and Charlotte Water; permitting; commissioning support; record drawings. Engineering line item: **$282,400** (separate line — disclosed for transparency)
- **Service** — 1,200 A 480Y/277 V utility-derived service from Duke Energy, with a 750 kVA pad-mount transformer. Outdoor emergency-disconnect per NEC 2020 §230.85 (and NEC 2026 §230.85(B)(2) once NC adopts in December — the disconnect installed will satisfy both cycles)
- **Distribution** — One main switchboard, three 225 A branch panelboards, two 100 A dental-suite panels with isolation transformers
- **Generator / ATS** — 200 kW diesel genset for Type 1 essential electrical system per NFPA 99 (Healthcare Facilities Code, 2024 ed.) — life-safety branch only (egress lighting, fire alarm, exit signs); critical-care branch not required at this MOB (no ICU / surgery)
- **Power & Lighting** — Full Division 26 fit-out per spec including LED lighting with daylight harvesting in patient-facing spaces, occupancy sensing in clinical areas, and DLC-qualified controls for the §179D commercial energy-efficiency deduction (deadline-aware: §179D construction-start by 2026-06-30 to qualify under current rules)
- **Special Systems** — Nurse-call rough-in only (separate vendor for headend); DAS / public-safety radio rough-in coordination with Charlotte FD

**2. RISK ALLOCATION (design-build specific)**

The design-build delivery method allocates risk between design and construction. Krasa Electric, LLC accepts:
- **Design risk** for engineering decisions made in-house through 100% CDs
- **Constructability risk** at the SD-DD-CD interface
- **Coordination risk** with the architect and other-trade design-build partners
- **Quantity risk** to the extent of the 35% SD set — quantity variation > 15% in any major BOM category surfaces as a CO

Krasa Electric does NOT accept:
- **Owner-directed scope changes** post 100% CDs (separate CO)
- **Code-cycle-transition risk** for the NEC 2020 → 2026 transition (see paragraph below)
- **Utility-side design risk** for Duke Energy primary service design (utility-controlled)

**Code-cycle-transition note (NEC 2020 → 2026):** NC adopts NEC 2026 effective 2026-12-01. The substantial-completion target is 2027-03-15, which means the project crosses the cycle transition. The design will satisfy NEC 2026 from the outset (so that the building does not require retrofits as code progresses). Specifically:
- Outdoor emergency-disconnect installed regardless (NEC 2020 §230.85; NEC 2026 §230.85(B)(2) expansion satisfied)
- DFCI on outdoor branch circuits (NEC 2026 §210.12(D)) — installed (small premium, ~$1,800 in scope)
- §240.87 ARMS function-test on the 1,200 A main breaker (NEC 2017+; NEC 2026 retains)
- Article §220 → §120 renumbering — internal documentation updated; no field impact

**3. ALTERNATES**

| # | Description | Add/Deduct | Amount |
|---|-------------|-----------|--------|
| A-1 | Upgrade lighting to tunable-white LED in all clinical spaces | ADD | $74,500 |
| A-2 | Add a 50 kW behind-the-meter solar-PV array on the canopy | ADD | $128,400 |
| A-3 | Add a level-2 EV charger network on the parking lot (8 stations, 14 kW each, behind a load-management hub) | ADD | $96,200 |

**4. ALLOWANCES**
- ALL-1 — Owner-selected decorative lighting in lobby and waiting areas: $42,000

**5. EXCLUSIONS** (pulled from `bid.standard_exclusions.commercial`)
- Civil / concrete / structural beyond electrical-equipment pads
- Site lighting beyond canopy and entry illumination (parking-lot lighting in Alt A-3 if selected)
- Nurse-call headend and software (rough-in only)
- DAS / public-safety radio active equipment (rough-in only)
- Tenant-improvement fit-out beyond shell-and-core (separate after-CO)

**6. SCHEDULE**
- Contract execution: 2026-05-22
- Schematic-design completion: 2026-06-12
- Design-development completion: 2026-08-15
- 100% CDs: 2026-10-30
- Construction start: 2026-11-15
- Substantial completion: 2027-03-15
- Final completion / commissioning: 2027-04-15
- **§179D construction-start deadline: 2026-06-30** — within the schedule

**7. BID TERMS**
- Proposal valid for 45 days from 2026-05-08
- **Tariff-aware escalation: Full Escalation Rider attached** (Affected Materials includes the 750 kVA transformer, the 1,200 A switchboard, the genset, the panelboards, and the copper feeder)
- Bonding: Excluded by owner request (not a bonded job)
- Sales tax: Included on materials at NC state + Mecklenburg County rate
- Prevailing wage: N/A (private)

**Bonding & Insurance Reconciliation:**
- Bond: not required.
- Required GL: $2M. Krasa's `insurance.gl_limit`: $2M. **OK.**
- Professional liability (E&O): Required $1M (because design-build EE in-house). Krasa's `insurance.professional_limit`: $1M. **OK.**

---

### Worked Example 4 — Public Works: Prevailing Wage School Addition

**Input:** $2.1M state-prevailing-wage school addition in Yakima WA under NEC 2023. Yakima School District #7. WAC 296-46B amendments apply. Davis-Bacon does not apply (state-funded only; no federal $). MWBE goal 12%. Cap-and-Share Variant escalation. Bonded job (P&P + bid bond). Bid due 2026-05-19.

---

**ELECTRICAL BID SUMMARY**
Project: Yakima School District #7 — Roosevelt Elementary 14-Classroom Addition
Owner: Yakima School District #7 | GC: Selden Construction | Architect: Schreiber Starling Whitehead | EOR: Larsen-Howell Engineering
Bid due: 2026-05-19 at 2:00 PM (sealed, hand-delivered to YSD #7 District Office)
Bidding to: Drawings E0.0–E5.4 rev 2026-04-30 (Addenda #1, #2, and #3 acknowledged)
Submitted by: Krasa Electric, LLC, License #WA-ELEC-21008, MWBE-certified WA-OMWBE #M2W-12041
Contact: Abe Clark, Owner / (509) 555-0117 / abe@krasaelectric.com

**1. BASE BID — $2,128,400**

Scope (Division 26):
- 26-05 Common Work — EMT throughout the addition; PVC underground from the existing main building to the addition (110 ft); galvanized RGS at exterior penetrations
- 26-20 Low-Voltage Distribution — 600 A 208Y/120 V feeder from existing main switchboard to addition's distribution panel; sub-feeds to (4) classroom panels and (1) MEP-room panel
- 26-24 Switchboards / Panelboards — one 600 A distribution panel; (5) branch panels (NEMA 1, 42-circuit, 208Y/120 V)
- 26-27 Wiring Devices — 384 duplex receptacles (each classroom: 14 + 4 wet-location GFCI), 22 dedicated tech circuits, 14 ceiling-mounted projection-receptacle drops
- 26-28 Protective Devices — AFCI per NEC 2023 §210.12(B); GFCI per §210.8(D); SPD at distribution panel
- 26-32 Emergency / Standby — generator-inverter battery system for egress lighting and fire alarm (existing system extended); 90-min battery-backup on EM fixtures verified per NEC 2023 §700.12 / WAC 296-46B
- 26-41 Grounding — supplemental ground rods at addition; bonding to existing GES verified
- 26-50 Lighting — 232 fixtures (recessed troffer with daylight-harvest dimming in classrooms, surface mount in corridors and MEP room, exit signs throughout)
- 26-56 Exterior Lighting — 6 wall-pack LED at exterior egress doors

**2. ALTERNATES**

| # | Description | Add/Deduct | Amount |
|---|-------------|-----------|--------|
| A-1 | Add 14 occupancy-sensor / vacancy-sensor switches in classrooms (over base scope) | ADD | $9,800 |
| A-2 | Upgrade exterior LED to wildlife-friendly color temperature (per WA wildlife coord.) | ADD | $3,200 |
| A-3 | Add EV-ready conduit and panel space for future staff EV charger network | ADD | $14,500 |

**3. UNIT PRICES (add/delete post-award)**

| Item | Add | Delete |
|------|-----|--------|
| Additional duplex receptacle | $182 | $128 |
| Additional GFCI receptacle | $232 | $164 |
| Additional 2x4 LED troffer | $342 | $228 |
| Additional 20 A homerun (up to 60 ft) | $584 | $402 |

**4. EXCLUSIONS** (pulled from `bid.standard_exclusions.public_works`)
- All civil / concrete / structural work
- Communication / data cabling (Division 27, by separate sub)
- Fire alarm wiring (Division 28, by separate sub) — coordination only
- Permit fees (owner-paid per project manual)
- Special inspections (paid by owner)
- Asbestos abatement / lead remediation in any existing-building tie-in
- Move-management or owner-furnished AV / smartboard equipment installation
- WA State L&I prevailing-wage adjustments retroactively if Wage and Hour reissues rate determinations during the project (CO if material)

**5. ASSUMPTIONS**
- NEC 2023 with WAC 296-46B amendments (cited per `default_jurisdiction.citation_format`)
- WA State Prevailing Wage rates per WA L&I, current as of bid date; certified payroll on Form A-1-1 weekly via L&I online portal
- MWBE goal 12% — Krasa Electric's MWBE certification satisfies the goal directly; subcontractor MWBE participation tracked separately
- Normal working hours per project manual; no work during the school day except summer / weekend
- Continuous access to existing electrical room for tie-in
- One mobilization for rough-in; one for trim
- Existing GES verified at pre-construction; remediation if found inadequate via CO

**6. SCHEDULE**
- Mobilization: 2026-06-22
- Underground rough-in (PVC feeder run): 2026-06-29
- Above-grade rough-in: 2026-07-06 through 2026-08-15
- Trim and finish: 2026-08-17 through 2026-08-31
- Substantial completion: 2026-08-31 (one week before school year starts)
- Final completion: 2026-09-12

**7. BID TERMS**
- Bid valid for acceptance for 60 days from 2026-05-19
- **Tariff-aware escalation: Cap-and-Share Variant** (per `material-tariff-escalation-clause-drafter.md` — public-works owners typically don't accept full pass-through; the variant uses a 3% cap with shared overage above the cap; Affected Materials covers the 600 A switchboard, panelboards, and copper feeder)
- Bonding: P&P bond at 1.0% included; bid bond submitted with bid
- Sales tax: Included on materials at WA state + Yakima County rate (school-district properties not exempt)
- **Prevailing wage: Included** at WA L&I current rates (Yakima County electrical rates)
- §39.04.250 prompt-payment statute language acknowledged

**8. QUALIFICATIONS**
- WA OMWBE-certified MWBE #M2W-12041
- EMR: 0.78
- Insurance: GL $2M / Auto $1M / WC statutory / Umbrella $5M (per project manual minimums)
- License: WA EL-21008 (electrical contractor)

---

**Bonding & Insurance Reconciliation:**
- Required bid bond: 5% of base bid ($106,420). Krasa's `bonding_capacity` accommodates. **OK.**
- Required P&P: 100% of contract value ($2.1M). Krasa's `bonding_capacity` single-job: $5M. **OK.**
- Required GL: $2M. **OK.**
- Required umbrella: $5M. **OK.**

**Bid-Submission Checklist (the 12 items that disqualify a public-works bid):**

1. ☑ Bid form signed by authorized officer (Abe Clark, Owner) per company resolution
2. ☑ Bid bond enclosed (5% of base, AIA A310, Travelers as surety)
3. ☑ All addenda acknowledged (#1, #2, #3 — initialed on bid form)
4. ☑ Public Works contractor's registration current (WA L&I PWC-1234567)
5. ☑ Statement of Bidder's Qualifications enclosed (per project manual §00 4513)
6. ☑ Subcontractor List enclosed (project manual §00 4519)
7. ☑ MWBE participation form enclosed
8. ☑ Certification of Compliance with Wage Payment Statutes
9. ☑ Non-Collusion Affidavit
10. ☑ Insurance certificates current at bid (current COI w/ YSD #7 as additional insured)
11. ☑ Sales tax included as a separate line item (per state DOR public-works rules)
12. ☑ Bid envelope sealed and labeled per project manual §00 2113

**Tariff-Aware Escalation:** Cap-and-Share Variant attached as Exhibit C (per `material-tariff-escalation-clause-drafter.md`). 3% cap on cumulative material price increases; 50/50 share above the cap. Affected Materials includes the 600 A switchboard, the (5) branch panels, and the copper feeder.
