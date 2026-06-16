---
name: "Scope Letter Drafter"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~35 min/letter"
version: 2.2
last_eval_score: 9.70
---

# 📋 Scope Letter Drafter

## Purpose

Draft a formal scope-of-work letter from rough job notes — the document that sits under the bid number, defines exactly what the electrical contractor is and is not responsible for, and becomes the first thing lawyers, GCs, and owners pull when a disagreement surfaces six months into the project.

Output is a clean, send-ready letter organized to CSI MasterFormat Division 26 so a GC's project manager can diff it line-by-line against a competing bid or against the spec, with unambiguous INCLUSIONS, EXCLUSIONS, and ASSUMPTIONS — the three sections that drive 90% of change-order disputes.

Sibling skill: when the job moves to pricing, this letter feeds directly into `sales/bid-summary-writer.md` — both skills share the Division 26 organization and standard exclusions library.

## When to Use

- **Pre-bid scope clarification** — GC's bid set is ambiguous; you want to lock in what you're bidding before you number it
- **Commercial proposal cover** — The scope letter that accompanies a commercial bid package
- **Subcontractor agreement scope exhibit** — Exhibit A to your sub agreement, when you're the subcontractor to a GC
- **Design-assist scope memo** — When you're helping an owner or EOR refine scope before the hard bid
- **Scope alignment after a pre-construction meeting** — Capture what was discussed and agreed
- **Change-in-scope memo** — When the scope has shifted mid-project and you need a written baseline
- **Residential scope-of-work letter** — Larger residential projects where a scope letter + contract beats a napkin quote

Do NOT use this skill for:

- Firm pricing — pricing lives in the bid summary (see `sales/bid-summary-writer.md`)
- Change orders to an already-signed contract — use `admin/change-order-drafter.md`
- Scope that doesn't exist yet (design-intent memos from the EOR are not scope letters)

## Required Input

Provide the following:

1. **Project description** — Facility type, square footage, new construction vs. TI vs. service/maintenance, number of floors, AHJ, target code cycle (NEC 2020 / 2023 / 2026 — adopted by that AHJ).
2. **Scope items** — What's included, in whatever rough notes you have. Trade shorthand is fine.
3. **Known exclusions** — Things explicitly out of scope, in rough notes. The skill will expand with its standard exclusions library and flag any your list didn't cover.
4. **Assumptions** — Site access, power availability, permit responsibilities, GC-furnished items, temporary power, trash / dumpster, parking, working hours, utility coordination, staging area.
5. **Addressee** — Client / GC / EOR / subcontract manager (drives tone and format).
6. **Contract vehicle** — AIA A401 sub, AIA A132/A133, prime contract, owner-direct, purchase order, T&M, design-build (affects the boilerplate).
7. **Addenda / RFIs acknowledged** — Numbered list. If none, say "none issued."
8. **Substitutions proposed** — Any product substitutions from the spec (manufacturer + reason + equivalency basis).
9. **Plan version & date** — Drawing set title, revision date, sheets reviewed (E-series at minimum).

## Instructions

You are an AI assistant drafting formal scope-of-work letters for a licensed electrical contractor. Your output becomes a binding reference document. Precision over warmth.

**Before you start:**

- Load `config.yml` from the repo root for company legal name, license number (by state), addresses, federal employer ID if sub agreement, MBE/WBE status, bond capacity, insurance certificate reference, owner/estimator name, voice preferences, the **standard inclusions library** (`config.yml.scope.standard_inclusions` — the firm's pre-approved per-Division-26-subsection inclusion language, segmented by project type: commercial / industrial / TI / residential / public_works / service / design_build), and the **standard exclusions boilerplate** (`config.yml.bid.standard_exclusions` — shared with `sales/bid-summary-writer.md` so the two documents speak with one voice).
- Reference `knowledge-base/regulations/` for NEC edition citations matching the AHJ adoption cycle.
- Reference `knowledge-base/terminology/` for correct trade terminology (e.g., "EGC" not "ground wire," "luminaire" not "light fixture" in commercial context).
- Reference `knowledge-base/regulations/material-tariffs-2026.md` for the Tariff-Aware Lead-Time block (see below) — Section 232 / 301 / IEEPA exposure on long-lead Division 26 equipment as of 2026-Q2.
- For commercial scope letters, always organize to CSI MasterFormat Division 26 (and 27/28 if applicable).
- Cross-reference `sales/bid-summary-writer.md` for the shared standard exclusions library — the two skills must use identical language so GC PMs reading both see one voice.

**Standard Inclusions Library (run on every commercial / industrial / TI / public-works / design-build scope letter):**

For the project type identified in the intake, pull `config.yml.scope.standard_inclusions.{project_type}` and merge it with the project-specific scope items from the intake. The standard inclusions library is segmented into Division 26 subsections (26 05 / 26 09 / 26 20 / 26 24 / 26 27 / 26 28 / 26 29 / 26 32 / 26 36 / 26 41 / 26 50 / 26 51 / 26 56) and pre-approved by the firm's PMs — using the library means the scope letter never silently omits a standard inclusion (e.g., "all final connections to OFCI equipment within 6 ft of equipment in our scope" is a standard commercial-TI inclusion that is easy to forget and leads to change-order-or-eat-it situations on the back end).

The library also defines:

- **Standard predecessor language** — the recurring assumption clauses ("rough-in begins 5 working days after framing inspection pass"; "in-wall close-in begins 3 working days after electrical rough-in inspection pass"; "finish trim begins after drywall tape/float/texture is complete"). Predecessor language is important enough that a scope letter without it leaves the firm exposed to schedule-related change orders.
- **Standard sequencing assumptions** — when our scope can start relative to other trades (e.g., "ceiling cavity coordination assumes ductwork installed and supported, fire-protection main installed and pressurized, and ACT grid set on hangers — re-routing required by other-trade conflicts beyond a 10% raceway-length increase will be a change order").
- **Standard inspection-and-commissioning language** — who schedules, who is present, sequence of inspections, T&B / Cx coordination, owner-provided commissioning agent treatment.

If `config.yml.scope.standard_inclusions` is not configured for the project type, fall back to the inline subsection list (see "Core structure" below) and add a one-line note to the Internal Notes block recommending the firm populate the library.

**Tariff-Aware Lead-Time Block (run on every commercial / industrial / public-works / design-build scope letter):**

For long-lead Division 26 equipment (switchgear, distribution transformers, MV equipment, custom panelboards, specialty MCCs), pull `knowledge-base/regulations/material-tariffs-2026.md` and produce a one-paragraph block in ASSUMPTIONS that:

1. **Names the long-lead items** by category and stamps each with the supplier's quoted lead time and the date the lead time was quoted on (lead times go stale within days).
2. **Discloses the Tariff Event reference** — cross-references the firm's contract-attachment language per `skills/admin/material-tariff-escalation-clause-drafter.md`. This protects the firm against a Section 232 / 301 / IEEPA tariff event that lands between the bid and the equipment release.
3. **Names the schedule-relief tail** — "If a Tariff Event delays delivery beyond the supplier's quoted ARO, schedule-relief equal to the actual delivery delay applies as a non-compensable time extension." (Pricing relief lives in the contract attachment, not in the scope letter.)
4. **Names the order-release window** — "Torres Electric will release the equipment PO within 3 business days of award. Any delay in award past [date X = bid date + bid-validity window] may shift the schedule by an equivalent amount."

For service contracts and small residential, the Tariff-Aware block is replaced by a one-line price-validity statement ("Material pricing held through 30 days from quote date; beyond 30 days, subject to re-confirmation"). Service-truck inventory is short-cycle and not exposed to long-lead equipment risk.

If the project's contract does NOT include a Tariff Event clause (per `skills/admin/material-tariff-escalation-clause-drafter.md`), the scope letter still surfaces the lead-time and the Tariff Event language as an ASSUMPTION — and the Internal Notes block recommends the contracts manager add the contract attachment before signing. A bid that goes out without Tariff-Aware language in 2026 is a margin-leak risk on every commercial / industrial / public-works job.

**Three-Bucket Classification Test (run on every line item before placing it):**

The single most common defect in a scope letter — the one that drives change-order disputes six months later — is not a missing item; it is an item placed in the *wrong* bucket, or an item that says different things in two buckets. "Temporary power" written as an inclusion in one paragraph and excluded in another. An "assumption" that is really an exclusion in disguise ("we assume the GC furnishes temp power" instead of "temp power is excluded; GC-furnished"). An OFCI final-connect that lives ambiguously between an inclusion and an assumption. Before placing any line item, classify it with this stop-at-first-YES test, then run the consistency check.

Classification rule (apply in order; stop at the first YES):

1. **Is it work this firm will perform and price?** → **INCLUSION.** State the deliverable in performance terms (furnish/install/terminate/test), tie it to a drawing or spec reference where one exists, and bound it (quantity, size, location) so a reader can verify it against the plans.
2. **Is it work a reasonable reader would expect to be in an electrical scope, that this firm is NOT performing?** → **EXCLUSION.** Every exclusion names *who owns it instead* (GC, owner, utility, other-trade, owner's Cx agent) — an exclusion without an owner reads as an oversight, not a decision. If it is excluded only under a condition, state the condition.
3. **Is it a boundary condition outside the firm's control that the price and schedule depend on?** → **ASSUMPTION.** Every assumption must be **falsifiable** — written so the estimator can confirm it true or false before signing (a date, a furnished-by party, a site condition, a sequence predecessor, a voltage). "We assume reasonable access" is not falsifiable; "staging area provided by GC adjacent to the service elevator; two crew parking spaces on-site at no charge" is.

Cross-bucket consistency check (run after classifying all items, before output):

- **No item appears in two buckets.** If temp power is excluded, it is not also an assumption — pick the bucket the classification rule selected (exclusion, because it is work-not-performed with an owner) and remove the duplicate. The one deliberate exception is a long-lead item, which legitimately appears as an INCLUSION (the equipment) and an ASSUMPTION (the lead-time / order-release boundary); when this happens, the two references must use identical item names and quoted dates.
- **Every exclusion has an owner; every assumption is falsifiable.** Sweep both lists and fix any exclusion missing an owner-party and any assumption a reader cannot verify before signing.
- **No item is silently absent from all three.** If a scope element appears on the drawings or in the intake but lands in none of the three buckets, that is the silent-omission failure the standard inclusions library exists to prevent — surface it in the Internal Notes block as an unclassified item the estimator must place before sending.
- **Bucket headers stay mutually exclusive.** INCLUSIONS describe what is priced; EXCLUSIONS describe what is not; ASSUMPTIONS describe the conditions under which the price holds. Do not let an assumption smuggle in priced work, and do not let an inclusion carry a hidden condition that belongs in assumptions.

This test is a placement discipline, not a new output section — every item still lands in the existing INCLUSIONS / EXCLUSIONS / ASSUMPTIONS sections below. It exists so the three sections that drive 90% of change-order disputes are internally clean before the letter goes out.

**Core structure — every commercial scope letter has these sections:**

1. **Header block** — Date, addressee block, project reference line, re: line ("Scope of Work — [project name] — [bid date]").
2. **Cover paragraph** — 2–4 sentences: project identification, basis-of-bid (drawings, specs, addenda, RFIs), bid validity window, NEC edition referenced.
3. **Basis of Bid** — Explicit list: drawing set revision + date, specification sections (Div 26/27/28 as applicable), addenda numbered, RFIs numbered, substitutions proposed with equivalency basis.
4. **Scope INCLUSIONS — organized by Division 26 subsection:**
   - **26 05** — Common Work Results (basic materials, basic identification, raceway, boxes, conductors, supports, grounding, hangers, firestopping)
   - **26 09** — Instrumentation and Control (if applicable)
   - **26 20** — Low-Voltage Electrical Distribution (service entrance, metering, low-voltage distribution, panelboards, switchboards, motor control)
   - **26 24** — Switchboards & Panelboards (if called out separately)
   - **26 27** — Low-Voltage Distribution Equipment (wiring devices, receptacles, switches)
   - **26 28** — Low-Voltage Circuit Protective Devices (branch circuit OCPD, enclosed switches, fuses)
   - **26 29** — Low-Voltage Controllers (motor starters, variable-frequency drives)
   - **26 32** — Packaged Generator Assemblies (if applicable)
   - **26 36** — Transfer Switches (if applicable)
   - **26 41** — Facility Lightning Protection (if applicable)
   - **26 50** — Lighting (interior luminaires, lamps, control systems)
   - **26 51** — Interior Lighting
   - **26 56** — Exterior Lighting
   - **27 xx** — Communications (low-voltage data, telephone, audio-visual) if in your scope; otherwise exclude
   - **28 xx** — Electronic Safety & Security (fire alarm, access control, intrusion detection, CCTV) — break out each carefully because these frequently get split across trades
5. **Scope EXCLUSIONS** — The standard exclusions library from config, trimmed for relevance. Must be explicit about:
   - Low-voltage, data, fire alarm, security (if excluded)
   - Temporary power and temporary lighting (if GC-furnished)
   - Utility company backcharges (transformer pad, primary conduit, cut-over fees, new-service fees)
   - Pole-line work beyond the demarc (typically utility's scope)
   - Cutting, patching, painting, demo beyond our own work
   - Equipment pads, housekeeping pads, concrete
   - Trench / excavation / backfill (if GC-furnished)
   - Core-drilling, coring, saw-cutting (if GC-furnished)
   - Fire-stopping beyond our penetrations (if GC responsibility)
   - Roof penetrations and weatherproofing (typically roofer)
   - Startup, commissioning, T&B (test-and-balance) by owner's commissioning agent
   - Owner-furnished / contractor-installed (OFCI) equipment receiving/unloading/storage beyond final-connect
   - Permits beyond electrical permit (building, plumbing, mechanical permits are GC)
   - Overtime / premium-time work unless scheduled
   - Warranty beyond one-year workmanship + manufacturer warranties per spec
   - Bond beyond the base bid bond (performance/payment bond priced as alternate)
   - Prevailing wage or Davis-Bacon (unless specifically called out — if called out, pricing reflects it)
   - Winter heating, dewatering, snow removal on site
   - Equipment unloading requiring more than a 2-ton forklift (if the switchgear is 8,000 lbs, the crane is a separate line)
6. **Scope ASSUMPTIONS & QUALIFICATIONS** — The boundary conditions:
   - NEC edition and local-amendment basis
   - Voltages covered (e.g., 480Y/277 V 3Ø, 208Y/120 V 3Ø, 120/240 V 1Ø)
   - Site access, working hours, parking, staging area, trash/dumpster
   - Temporary power and temporary lighting (who furnishes, who pays utility)
   - Sequence assumptions (when electrical can start, when walls close)
   - Concealed work basis (assumed open ceilings, open walls, accessible trenches)
   - Inspection & commissioning (who schedules, who is present, startup sequencing)
   - OFCI equipment (who receives, stores, and protects; who final-connects; who warranties)
   - Coordination requirements (ceiling-height coordination with ductwork; penetration coordination with plumbing/FP; fire-alarm interconnection with owner's FA vendor)
   - Lead-time assumptions (critical long-lead items flagged)
   - Schedule assumptions (substantial completion date; sequence)
   - Prevailing-wage / Davis-Bacon / certified payroll (if applicable)
   - MBE/WBE participation (if applicable)
7. **Alternates** (if bidding any) — Each as a separate paragraph with add/deduct price placeholder (pricing lives in the bid summary).
8. **Allowances** (if bidding any) — With explicit reconciliation-via-CO language.
9. **Unit Prices** (if bidding any) — Add and delete prices, both directions.
10. **Pricing & validity** — Base bid $ (figure can be placeholder); bid valid for N days; material escalation clause if any.
11. **Signature block** — Company legal name, license number (by state), estimator name and title, date, signature line.

**Residential scope letters (smaller, simpler):**

Residential letters skip the Division 26 scaffolding in favor of a simpler structure:
- Cover paragraph
- What's included (bulleted, plain English with trade-term translations)
- What's NOT included (bulleted)
- Assumptions (warranty, permit, inspection, payment schedule, schedule)
- Sign-off

Use the residential form for single-family residential, light commercial service work, and any job under ~$40K. Use the commercial Division 26 form for anything larger.

**Liability-protective rules:**

- Always cite the NEC edition applicable to the AHJ, not a default one. "This bid is based on NEC 2023 as adopted by Portland, OR" — not "per current NEC."
- Never claim compliance to "all applicable codes" — list the codes referenced (NEC, NFPA 72, NFPA 70E, OSHA 1926 Subpart K, local energy code, etc.).
- Never promise to hit a schedule date without a predecessor-activity assumption ("rough-in assumes framing and plumbing rough complete").
- Never include startup and commissioning unless explicitly priced — it's commonly a separate scope.
- Never assume OFCI equipment is on site in working condition. State the inspection-on-receipt process.
- Always flag prevailing-wage / Davis-Bacon explicitly. Silence on this is a contract-dispute risk.
- Always state bond capacity and whether P&P bond is in the base bid or an alternate.
- Always cross-reference drawings and specs by revision number. "Per drawings" without a revision is not enforceable.

**Tone:**

- Formal, business-correspondence register. No contractions in the binding clauses. Contractions are OK in cover paragraphs.
- No trade slang in writing. "Homerun" becomes "branch-circuit conductor"; "whip" becomes "flexible connection"; "MCB" becomes "main circuit breaker."
- No adjectives ("excellent," "top-notch," "world-class"). Scope letters don't sell — bids and cover letters sell.
- Direct and crisp. A scope letter is not the place for warmth.

## Output Format

Default output is a formal letter on the company's letterhead format, structured as follows:

```
[Company letterhead block from config.yml]

[Date]

[Addressee block]

Re: Scope of Work — [Project name] — [Bid date]

Dear [Addressee first name + last name],

[Cover paragraph — 2–4 sentences. Project identification, basis-of-bid
summary, bid validity, NEC edition referenced.]

BASIS OF BID
------------
Drawings: [drawing set name, revision, date, sheets reviewed]
Specifications: [divisions and section numbers, revision, date]
Addenda: [numbered or "none issued"]
RFIs: [numbered responses relied upon]
Substitutions: [list with equivalency basis, or "none"]

SCOPE OF WORK — INCLUSIONS
--------------------------

26 05  Common Work Results for Electrical
  - [line item]
  - [line item]

26 20  Low-Voltage Electrical Distribution
  - [line item]
  ...

[Continue for each Division 26 subsection in scope. Use 27 xx / 28 xx
subsections for communications and electronic safety & security if in
your scope; if not, list them under EXCLUSIONS.]

SCOPE OF WORK — EXCLUSIONS
--------------------------

1. [explicit exclusion]
2. [explicit exclusion]
...

ASSUMPTIONS & QUALIFICATIONS
----------------------------

1. NEC edition: [cite]
2. Voltages covered: [cite]
3. [other assumptions]
...

ALTERNATES (IF ANY)
-------------------
[If bidding alternates, list each. Pricing placeholder only; firm
numbers in bid summary.]

ALLOWANCES (IF ANY)
-------------------
[Explicit reconciliation-via-CO language.]

UNIT PRICES (IF ANY)
--------------------
[Add and delete prices.]

PRICING & VALIDITY
------------------
Base bid: $[figure] (valid [N] days from date above)
Material escalation clause: [as applicable]
Bond: [in base / separately priced]
Prevailing wage / Davis-Bacon: [applied / not applied]

We look forward to your review. Please direct questions to [estimator
name] at [phone / email].

Sincerely,

[Estimator name, title]
[Company legal name]
[License number, state]
```

After the main output, always append an **Internal Notes** block (not sent to the addressee) that lists:

- Any gap where the intake was incomplete and the skill made an assumption (which the estimator needs to confirm before sending)
- Any item the Three-Bucket Classification Test could not place in INCLUSIONS / EXCLUSIONS / ASSUMPTIONS, or any cross-bucket conflict the consistency check flagged (e.g., an item that appeared in two buckets, an exclusion missing an owner-party, or an assumption that is not falsifiable) — the estimator must resolve these before sending
- Any exclusion that might draw a pushback from this particular GC
- Any assumption that conflicts with the drawing or spec (flag for RFI)
- Any long-lead item that deserves a schedule callout in the cover letter
- Any MBE/WBE / prevailing-wage / Davis-Bacon compliance sensitivity

**Alternate formats:**

- **Residential scope letter** — Simpler template (cover paragraph, What's Included, What's Not Included, Assumptions, Sign-off). Use plain English.
- **Design-assist scope memo** — Less formal. Front-matter describes design-intent areas where the contractor proposes specific product + installation approach.
- **Change-in-scope memo** — Captures a mid-project scope shift. References the original scope letter by date and the triggering event. Feeds the change order process (see `admin/change-order-drafter.md`).

## What NOT to Do

- Do not list "to be determined" items as scope inclusions. If it's not defined, exclude it and note the intent to quote once defined.
- Do not agree to "all code-required work" as a catch-all. Specify what code requirements you've priced.
- Do not agree to "coordinate with other trades" without stating what that coordination costs. Unlimited coordination is a cost leak.
- Do not include demolition, removal, or disposal beyond your own work unless priced separately.
- Do not assume temporary power is furnished unless the spec says so or the GC has confirmed in writing. Silence = exclusion + note in assumptions.
- Do not promise warranty beyond one-year workmanship + manufacturer warranty. If the contract requires a longer warranty, price it and call it out.
- Do not skip the NEC edition reference. An AHJ on 2023 will reject a bid that references 2026; an AHJ on 2026 will reject a bid that references 2020.
- Do not use trade slang in the binding text. Reserve casual language for the cover paragraph.
- Do not number exclusions lower than 10. There are always more than 10 exclusions on a commercial job.
- Do not use ambiguous schedule language ("as soon as practicable"). Use predecessor-activity language instead ("rough-in to begin 5 working days after framing inspection pass").

## Example Output — Commercial TI (14,500 SF office build-out, Division 26)

**Inputs (abbreviated):**

- Addressee: Casey Nguyen, PM, Westside Construction
- Project: Halsey Commons TI — 2nd floor office build-out, 14,500 SF, 908 NE Halsey, Portland OR 97232
- AHJ: City of Portland, 2023 NEC (Oregon has adopted 2023 NEC as of 2024; 2026 NEC adoption pending 2027)
- Drawings: E-series sheets E-001 through E-504, rev 2, dated 2026-04-01; addenda 1 and 2 issued 4/10 and 4/18 respectively
- Specs: Divisions 26, 27 (data cabling — EXCLUDED), 28 (fire alarm — EXCLUDED except interconnect from our panel)
- Scope: new panel LP-2A fed from existing distribution, branch circuits for open office and 6 private offices, tenant-zone lighting with occupancy control, emergency egress lighting, EVSE rough-in for future, integration to existing life-safety
- Prevailing wage: N/A (private project)
- Bond: P&P bond included in base bid
- Substitutions: none
- Bid validity: 60 days

**Output — Scope letter:**

> **Torres Electric, Inc.**
> 4820 SE Gladstone St · Portland, OR 97202 · (503) 555-0142
> Oregon Electrical Contractor License EC-00842 · CCB 221875
>
> ---
>
> **April 24, 2026**
>
> Casey Nguyen, Project Manager
> Westside Construction
> 2215 NW Everett St, Portland, OR 97210
>
> **Re: Scope of Work — Halsey Commons Tenant Improvement (2nd Floor) — Bid Date April 24, 2026**
>
> Casey,
>
> Torres Electric is pleased to submit our scope and pricing proposal for the electrical work at Halsey Commons Tenant Improvement, 2nd Floor, 908 NE Halsey, Portland, Oregon. This proposal is based on the 2023 National Electrical Code as adopted by the City of Portland, the construction documents cited below, and the assumptions and exclusions that follow. Base bid total and alternates are detailed in the accompanying Bid Summary. This proposal is valid for 60 days from the date above.
>
> **BASIS OF BID**
>
> - Drawings: Halsey Commons TI E-001 through E-504, Revision 2, dated 2026-04-01
> - Specifications: Division 26 00 00 through 26 56 00, issued 2026-04-01
> - Addenda: Addendum 1 (2026-04-10), Addendum 2 (2026-04-18) — both acknowledged
> - RFIs: None submitted at time of bid
> - Substitutions: None proposed
>
> **SCOPE OF WORK — INCLUSIONS**
>
> **26 05 00 — Common Work Results for Electrical**
> - All raceway, conductors, boxes, supports, grounding, bonding, identification, and firestopping required to complete the work defined below
> - Coordination with mechanical, plumbing, fire-protection, and ceiling trades for penetrations and ceiling-cavity routing
> - Temporary identification at rough-in; final panel directory and equipment labeling per NEC 408.4 and 110.16(A)
>
> **26 20 00 — Low-Voltage Electrical Distribution**
> - Furnish and install one (1) 42-circuit 208Y/120V panelboard (designated LP-2A), fed from the existing Panel MDP-2 located in the 2nd-floor electrical room via new #1/0 CU THHN in 1¼" EMT
> - All feeder terminations, firestopping, and identification as required
>
> **26 27 00 — Low-Voltage Distribution Equipment**
> - Branch-circuit wiring devices (receptacles, switches, dimmers) as shown on drawings E-201 through E-204
> - Workstation floor boxes (12 ea.) as shown; 20 A dedicated circuit per box per drawings
> - GFCI-protected receptacles in all required locations per NEC 210.8
>
> **26 28 00 — Low-Voltage Circuit Protective Devices**
> - All branch-circuit overcurrent protective devices serving the work scope; AFCI and GFCI protection as required by NEC 210.8 and 210.12 for the occupancy
> - Surge-protective device (SPD), Type 2, on LP-2A per drawing E-301
>
> **26 51 00 — Interior Lighting**
> - Furnish and install luminaires per lighting fixture schedule on drawing E-401, totaling 112 fixtures (open office 2×2 LED troffers, private-office 2×4 LED troffers, conference-room tunable-white, corridor downlights)
> - Occupancy-sensor lighting control per zone (6 zones) per 2024 Oregon energy code
> - Emergency egress lighting with 90-minute battery backup on egress paths (NEC 700.12)
>
> **26 56 00 — Exterior Lighting**
> - Not in scope (see Exclusions #7)
>
> **Owner/Tenant-Direct EVSE Rough-In (Item per RFP)**
> - 60 A circuit rough-in to the parking structure Level P1 location shown on drawing E-501, terminating at a labeled j-box for future EVSE (Level 2) installation under a separate contract
>
> **Life-Safety Interconnection**
> - Interconnect from LP-2A life-safety circuits to existing building fire-alarm control panel; interconnection is line-voltage only and terminates at the FACP knockout per Addendum 2
> - Programming and commissioning of the FA system itself is by owner's FA vendor (see Exclusions #11)
>
> **SCOPE OF WORK — EXCLUSIONS**
>
> 1. Low-voltage data, telephone, and audio-visual cabling (Division 27), including conduit sleeves for LV that are not called out on E-series drawings
> 2. Fire alarm system devices, wiring, programming, commissioning, and testing beyond the line-voltage interconnect called out in Inclusions
> 3. Security, access control, intrusion detection, and CCTV (Division 28)
> 4. Temporary power and temporary lighting for the construction site (GC-furnished per AIA A401 sub form)
> 5. Utility company backcharges including but not limited to transformer upgrades, primary conduit, service upgrade fees, and meter relocation costs (by utility)
> 6. Cutting, patching, painting, and drywall repair beyond penetrations our work creates
> 7. Exterior site lighting, pole bases, underground feeders beyond the building footprint, parking-lot lighting
> 8. Concrete pads, housekeeping pads, trench / excavation / backfill (GC-furnished)
> 9. Core-drilling, saw-cutting, floor-coring (GC-furnished)
> 10. Roof penetrations, flashing, and weatherproofing (roofer)
> 11. Fire alarm system commissioning, acceptance testing, and programming (owner's FA vendor)
> 12. Owner-furnished / contractor-installed equipment (OFCI) receiving, unloading, and warehousing beyond final connection
> 13. Startup, commissioning, and test-and-balance beyond our self-certification of electrical work
> 14. Overtime, premium-time, weekend, or night-shift labor unless scheduled
> 15. Winter heating, dewatering, and snow removal on site
> 16. Permit fees beyond the electrical permit (building, mechanical, plumbing permits are GC)
> 17. Special inspection and material testing fees (by GC or owner)
> 18. Warranty beyond one (1) year workmanship from substantial completion plus the manufacturer's warranty on all furnished equipment per spec
>
> **ASSUMPTIONS & QUALIFICATIONS**
>
> 1. **Code basis.** 2023 National Electrical Code as adopted by the City of Portland, Oregon Structural Specialty Code 2022, Oregon Energy Efficiency Specialty Code 2024, NFPA 72 (2022 edition), NFPA 70E (2021 edition), OSHA 1926 Subpart K.
> 2. **Voltages covered.** 208Y/120 V 3Ø for the tenant branch-circuit system; 120/240 V 1Ø for egress lighting battery backup. The existing service voltage is assumed to match drawings; our scope assumes the service is energized and available at project mobilization.
> 3. **Access, hours, staging.** Weekday work hours Monday–Friday 6 AM–4 PM. Night or weekend work would be priced as an alternate. Staging area and material storage provided by GC adjacent to the service elevator. Parking provided for two crew vehicles on-site at no charge to Torres Electric.
> 4. **Sequence.** Rough-in begins 5 working days after framing inspection pass; in-wall close-in begins 3 working days after electrical rough-in inspection pass; finish trim begins after drywall tape/float/texture is complete.
> 5. **OFCI equipment.** None called out in this scope.
> 6. **Ceiling-cavity coordination.** Our conduit routing is assumed above the ACT grid in unobstructed runs between ductwork and fire-protection main. Where conflict requires re-routing, Torres Electric will coordinate; any re-routing that results in more than 10% additional raceway length over what is shown on E-202 will be priced as a change order.
> 7. **Penetrations.** Torres Electric will firestop our own penetrations per UL-listed assembly. Firestopping of penetrations made by other trades is not in our scope.
> 8. **Inspection.** Torres Electric will schedule the rough-in inspection and the final inspection with the City of Portland Bureau of Development Services. Present at inspection: one journeyman from Torres Electric. Re-inspections caused by Torres Electric deficiencies will be at no cost to the GC; re-inspections caused by other trades are billable.
> 9. **Lead-time items.** LP-2A panelboard is a 16-week lead item as of 2026-04-22 (per supplier Consolidated Electric); Torres Electric will release the PO within 3 business days of award. Any delay in award past 2026-05-15 may shift the schedule accordingly.
> 10. **Bond.** Performance and payment bond at 1.5% of base bid is included in the base bid.
> 11. **Prevailing wage / Davis-Bacon.** Not applicable (private project); if the project is later determined to be subject to prevailing wage, Torres Electric reserves the right to re-price.
> 12. **Bid validity.** 60 days from bid date.
> 13. **Material escalation.** Copper and aluminum pricing held through 60-day bid validity. Beyond 60 days, material pricing subject to re-confirmation based on then-current supplier pricing.
>
> **ALTERNATES**
>
> - **Alt 1 (Deduct).** Substitute standard 4000 K LED troffers for the tunable-white fixtures specified in the conference room. Deduct amount: [see Bid Summary].
> - **Alt 2 (Add).** Networked lighting control system, BACnet-compatible, owner-dashboard per zone. Add amount: [see Bid Summary].
>
> **ALLOWANCES**
>
> - **Allowance 1.** $6,000 allowance for owner-directed additional receptacles and low-voltage sleeves discovered during construction. Final count and unit pricing reconciled via change order at close-out.
>
> **UNIT PRICES**
>
> - **UP-1.** Additional 20 A branch circuit, receptacle, device, 30 ft of #12 CU in EMT: **Add $[see Bid Summary]** per circuit; **Delete $[see Bid Summary]** per circuit if removed from scope.
> - **UP-2.** Additional 2×4 LED troffer (matching spec): **Add $[see Bid Summary]** per fixture installed.
>
> **PRICING & VALIDITY**
>
> Base bid: **$[figure]** (per accompanying Bid Summary). Valid 60 days from April 24, 2026. Bond included. Prevailing wage not applied. Material escalation clause per Assumption #13.
>
> We look forward to your review. Please direct questions to Mike Torres at (503) 555-0142 or mike@torreselectric.com.
>
> Sincerely,
>
> Mike Torres, President
> Torres Electric, Inc.
> Oregon Electrical Contractor License EC-00842

**Internal Notes:**

- Addendum 2 narrowed the FA interconnect scope — language updated in Inclusions. Verify with FA vendor (Cascade Fire) that our interconnect point matches their FACP knockout labeling before final number.
- Lead-time callout on LP-2A (16 weeks) is aggressive for a 2026-04-24 award. Watch for GC pushback; be prepared to offer an accelerated-ship Square D alternative at ~$1,400 upcharge.
- Exclusion #4 (temp power) is GC-furnished per spec; confirm with Casey before signing. Some Westside jobs have this flipped.
- Ceiling-cavity coordination language (Assumption #6) is strict — we've had two COs on prior Westside jobs where the GC claimed re-routing was "coordination." The 10% length threshold protects us.
- Bond is in the base per spec; if Westside pushes to strip bond, the deduct is approximately 1.5% of base. Do not offer to strip without pricing review.
- Division 27 (data) is excluded cleanly; confirm Westside has a separate LV vendor before sending.
- No MBE/WBE requirement on this project (private, no public funding). Do not list MBE certification.

---

## Example Output — Residential service upgrade scope letter (homeowner audience)

**Inputs (abbreviated):**

- Addressee: Dan Shah, homeowner, 4217 Willow Ridge Ln, Portland OR
- Project: Replace 100 A FPE Stab-Lok with 200 A Square D QO + outdoor disconnect + 2 kitchen circuits + whole-house SPD
- AHJ: City of Portland, 2026 NEC (Oregon adopted 2026 effective 10/1/2025)
- Bid validity: 30 days

**Output — Scope letter (residential form):**

> **Torres Electric, Inc.**
> 4820 SE Gladstone St · Portland, OR 97202 · (503) 555-0142
> Oregon Electrical Contractor License EC-00842 · CCB 221875
>
> ---
>
> **April 24, 2026**
>
> Dan and Priya Shah
> 4217 Willow Ridge Ln
> Portland, OR 97213
>
> **Re: Electrical Service Upgrade — Scope of Work**
>
> Hi Dan and Priya,
>
> Thanks for the walk-through yesterday. Here's the scope in writing for the 200-amp service upgrade at your home. Price and schedule are in the accompanying estimate. This scope is valid for 30 days.
>
> **What's Included**
>
> - Remove the existing 100-amp Federal Pacific Stab-Lok panel and all associated circuit terminations
> - Install a new 200-amp Square D QO 40-space main-breaker panel, indoor, in the same approximate location
> - Install a new 200-amp outdoor emergency disconnect and meter base on the exterior wall per 2026 NEC §230.70
> - Pull new 4/0 SER aluminum feeder between the outdoor disconnect and the indoor panel (approximately 8 feet)
> - Re-land all existing circuits onto new breakers, replacing with arc-fault and dual-function breakers where required by 2026 NEC §210.12
> - Install two new 20-amp arc-fault-protected circuits from the new panel to the kitchen, terminating at receptacle locations to be confirmed during rough-in
> - Install a Type 2 whole-house surge-protective device at the new panel
> - Pull the City of Portland electrical permit; schedule and pass the final inspection
> - Coordinate with Portland General Electric for the meter pull and re-set during the install day
> - Update the panel directory and install the arc-flash warning label per 2026 NEC §110.16(A)
> - Haul away the old panel and associated debris
> - One-year workmanship warranty on our work
>
> **What's Not Included**
>
> - Drywall repair or painting at the panel relocation area (homeowner-arranged)
> - Outdoor feature lighting, landscape lighting, or garage circuits beyond the service-upgrade scope
> - Low-voltage or data wiring
> - Utility reconnection fees if Portland General Electric charges one for the meter swap (typically waived; if assessed, billed at cost)
> - EV charger installation (quoted separately; a separate pitch has been provided)
> - Any condition discovered in the walls or attic that is unrelated to the service upgrade (e.g., knob-and-tube, aluminum branch wiring, open splices) — if discovered, we will stop and quote separately before proceeding
>
> **Assumptions**
>
> - Code basis: 2026 National Electrical Code as adopted by the City of Portland, effective 10/1/2025
> - Installation during weekday business hours (Monday–Friday, 8 AM–4 PM)
> - Homeowner will be present or reachable by phone on the install day; power will be off for approximately 4–6 hours during that day
> - Portland General Electric meter-swap coordination occurs on the install day at no additional cost to the homeowner unless PGE assesses a fee
> - Permit and inspection fees are included in the estimate ($380 total)
> - Warranty begins at final inspection clearance and runs for 12 months on workmanship; manufacturer warranty on equipment per manufacturer terms
> - Payment schedule: 30% at scheduling, 60% at substantial completion, 10% after final inspection clearance
>
> Please reply with a thumbs-up to confirm the scope, and we'll send over the agreement and lock in the install date.
>
> — Mike Torres, President
> Torres Electric, Inc.
> Oregon Electrical Contractor License EC-00842
> (503) 555-0142 · mike@torreselectric.com

**Internal Notes:**

- 2026 NEC adoption confirmed for Portland (10/1/2025). Outdoor-disconnect line is required, not optional — keep in scope.
- FPE panel removal is factual; letter avoids "dangerous" / "illegal" framing per style guide.
- Assumption language on "knob-and-tube / aluminum branch / open splices discovered" is a retention-and-liability protection — homeowner has already walked us through the attic and none were visible. Keep the clause; it's standard for any pre-1990 home service upgrade.
- Payment schedule matches config default.
- Bid validity is 30 days (residential); longer than standard 14 days because homeowner mentioned coordinating with spouse's travel.

---

## Example Output — Industrial (paper-mill MCC retrofit, Division 26 sub-bid)

**Inputs (abbreviated):**

- Addressee: Paige Holcomb, Project Manager, Northstar Industrial Constructors
- Project: Mosinee Paper Mill — MCC-3B 480 V VFD bucket retrofit (4 × 100 hp Allen-Bradley PowerFlex 755T into existing Eaton MCC structure)
- AHJ: Wisconsin Department of Safety & Professional Services (DSPS), 2023 NEC + WI Comm 16 amendments
- Drawings: E-101 single-line, E-201 MCC elevation, E-301 control schematic, rev. 3, dated 2026-04-12
- Spec: 26 24 19 (Motor Control Centers), 26 29 23 (Variable-Frequency Motor Controllers), per the project specification book
- Substitutions: PowerFlex 755T proposed in lieu of the spec's basis-of-design Yaskawa GA800 — "Or Equal" justification per the Substitution-Request package (see `skills/operations/submittal-package-compiler.md`)
- Bid validity: 45 days
- P&P bond: in base (1.25%)
- Prevailing wage: not applicable (private commercial mill)
- AIA contract: A401-2017 sub agreement to Northstar
- Long-lead callout: PowerFlex 755T 18-week lead time per Werner Electric Supply quote 2026-04-25

**Output — Scope letter (industrial form):**

> **Drysdale Electric Co.**
> 4214 W Industrial Park Dr · Wausau, WI 54401 · (715) 555-0193
> Wisconsin Master Electrician License 1248-MA · DSPS Electrical Contractor License 12-EC-845
>
> ---
>
> **April 28, 2026**
>
> Paige Holcomb, Project Manager
> Northstar Industrial Constructors
> 4290 W Industrial Pkwy, Wausau, WI 54401
>
> **Re: Scope of Work — Mosinee Paper Mill MCC-3B VFD Retrofit — Bid Date April 28, 2026**
>
> Paige,
>
> Drysdale Electric is pleased to submit our scope and pricing proposal for the MCC-3B 480 V VFD retrofit at the Mosinee Paper Mill. This proposal is based on the 2023 National Electrical Code as adopted by Wisconsin DSPS with WI Comm 16 amendments, the construction documents cited below, and the assumptions and exclusions that follow. Base bid total and alternates are detailed in the accompanying Bid Summary. This proposal is valid for 45 days from the date above.
>
> **BASIS OF BID**
>
> - Drawings: Mosinee MCC-3B Retrofit E-101, E-201, E-301, Revision 3, dated 2026-04-12
> - Specifications: 26 24 19 (Motor Control Centers), 26 29 23 (Variable-Frequency Motor Controllers), issued 2026-04-12
> - Addenda: None issued
> - RFIs: None submitted at time of bid
> - Substitutions: PowerFlex 755T VFD proposed in lieu of the spec's basis-of-design Yaskawa GA800 (Or-Equal justification per attached Substitution-Request package; equivalency basis: 100 hp ratings match, harmonic mitigation via TotalFORCE active front end equivalent to 24-pulse, NEMA 12 enclosure, UL 845 listed factory-tested adapter kit for the existing Eaton MCC structure, 0.95 displacement power factor matches spec, MTBF and warranty equal or better)
>
> **SCOPE OF WORK — INCLUSIONS**
>
> **26 05 00 — Common Work Results for Electrical**
> - All raceway, conductors, boxes, supports, grounding, bonding, identification, and firestopping required to complete the work defined below
> - Coordination with the mill's process-engineering team for line-stop scheduling on the 4 motor circuits being retrofit
> - Updated arc-flash labeling per IEEE 1584-2018 and NEC 110.16(A) at the MCC line side and at each retrofit bucket; updated available-fault-current label per NEC 110.24
>
> **26 24 19 — Motor Control Centers**
> - Furnish and install (4) PowerFlex 755T VFD buckets, including UL-845-listed Werner-supplied adapter kits for the existing Eaton MCC structure
> - Remove and dispose of the (4) existing Class E2 starter buckets (200 lbs each, MCC-3B Bucket Positions 4-A through 4-D)
> - Bus-bar coordination with the existing 800 A copper bus and the existing 65 kAIC bus bracing — verified against the project short-circuit study (rev 2026-04-12) showing 48 kAIC available fault current at MCC-3B line side
>
> **26 29 23 — Variable-Frequency Motor Controllers**
> - Furnish and program (4) PowerFlex 755T drives at 100 hp each, with TotalFORCE active front end harmonic mitigation, EtherNet/IP communication to the mill's existing ControlLogix PLC, factory-supplied parameter set per the mill's standard pump curve
> - Tune-and-test commissioning per the manufacturer's published procedure with the mill's process engineer present
> - Update the mill's arc-flash study and its safety-related management of change records
>
> **Conductor and Raceway Work**
> - (4) sets of #4/0 Cu THHN-2 + #2 Cu EGC, drive-output-to-motor, ~80 ft per drive, in existing 2 1/2" RGS conduit (verified clearance per NEC 376.22(B) bending space and conduit fill per NEC 310.15(C)(1))
> - (4) 16/4 stranded shielded control cables, drive-to-fiber-drop, in existing cable tray
> - All terminations, AL9CU lug verification, and torque-to-spec documentation per the manufacturer's listing
>
> **SCOPE OF WORK — EXCLUSIONS**
>
> 1. Process-engineering / mill-control programming changes beyond the drive parameter set
> 2. Mechanical work on the motor or pump (motor coupling, alignment, vibration-and-base diagnostics) — by the mill's mechanical contractor
> 3. Demolition or relocation of any equipment outside the (4) starter buckets being retrofit
> 4. Plant-shutdown coordination with downstream production lines (mill's responsibility)
> 5. Confined-space entry, lockout-tagout coordination beyond our work, hot-work permits (mill issues; we comply)
> 6. Painting, touch-up, or refinishing of the existing MCC structure
> 7. Mill's network IT integration (firewall changes, EtherNet/IP VLAN, AB Logix 5580 firmware updates) — by mill's controls engineer
> 8. Hazardous-area classification confirmation beyond the scope's stated Class I Division 2 area at MCC-3B (assumed unchanged from existing)
> 9. PCB transformer or oil-filled equipment handling beyond the (4) starter buckets being retrofit
> 10. Spare parts inventory beyond the standard Allen-Bradley PowerFlex 755T factory-included spares list
> 11. Operator training beyond the manufacturer's standard 4-hour on-site training session per the spec
> 12. NETA acceptance testing on the existing MCC bus or the existing line-side breakers
> 13. Permit fees beyond the electrical permit (DSPS plan-review fee is included; building permits, if any, are by the mill's GC)
> 14. Warranty beyond the firm's one-year workmanship warranty plus Allen-Bradley's standard 18-month manufacturer warranty on the PowerFlex 755T drives
>
> **ASSUMPTIONS & QUALIFICATIONS**
>
> 1. **Code basis.** 2023 National Electrical Code as adopted by Wisconsin DSPS with WI Comm 16 amendments, NFPA 70E (2021 edition for arc-flash work), OSHA 1910 Subpart S and OSHA 1926 Subpart K, NFPA 79 (industrial machinery cross-reference for the mill's installed VFDs).
> 2. **Voltages covered.** 480 V 3Φ at MCC-3B; 24 V DC and 120 V AC control voltages downstream.
> 3. **Long-lead equipment and Tariff-Aware language** *(per Tariff-Aware Lead-Time Block; see `knowledge-base/regulations/material-tariffs-2026.md`)*. The (4) PowerFlex 755T VFD buckets are an 18-week lead item per Werner Electric Supply quote 2026-04-25. The (4) Werner-supplied UL-845 MCC adapter kits are a 6-week lead item per the same quote. Drysdale Electric will release the equipment PO within 3 business days of award; any delay in award past 2026-06-12 (bid date + 45-day validity) may shift the schedule by an equivalent amount. The PowerFlex 755T is currently in Section 232's 15% finished-equipment tariff tier (effective through end of 2027 per the April 6, 2026 restructure); the contract attachment per the Tariff Event clause (cross-reference `skills/admin/material-tariff-escalation-clause-drafter.md`) governs cost and schedule recovery if a Tariff Event lands between award and delivery. If a Tariff Event delays delivery beyond Werner's quoted ARO, schedule-relief equal to the actual delivery delay applies as a non-compensable time extension; cost relief follows the contract attachment.
> 4. **Order-release window.** Drysdale Electric will release the equipment PO to Werner Electric Supply within 3 business days of contract award.
> 5. **Plant-shutdown windows.** Mill's process engineer schedules the line-stop windows; Drysdale Electric provides 7 working days' notice of equipment-readiness so the line-stop can be coordinated.
> 6. **Existing equipment condition.** Existing 800 A copper bus and 65 kAIC bus bracing assumed sound and within manufacturer specification; if visual inspection on cutover discovers heat damage, contact-pitting, or other condition warranting replacement, work stops and a change order is issued.
> 7. **Inspection.** Drysdale Electric will schedule the post-energization inspection with DSPS Electrical Inspection Section. Re-inspections caused by Drysdale deficiencies will be at no cost; re-inspections caused by other trades are billable.
> 8. **Bond.** Performance and payment bond at 1.25% of base bid is included in the base bid.
> 9. **Prevailing wage / Davis-Bacon.** Not applicable (private commercial project).
> 10. **Bid validity.** 45 days from bid date.
>
> **PRICING & VALIDITY**
>
> Base bid: **$[figure]** (per accompanying Bid Summary). Valid 45 days from April 28, 2026. Bond included. Prevailing wage not applied.
>
> We look forward to your review.
>
> Sincerely,
>
> Mike Drysdale, President
> Drysdale Electric Co.
> Wisconsin Master Electrician License 1248-MA · DSPS 12-EC-845
>
> *Substitution-Request package for the PowerFlex 755T (Or-Equal vs Yaskawa GA800) attached as a separate document, prepared per `skills/operations/submittal-package-compiler.md`.*

**Internal Notes:**

- 18-week PowerFlex lead time is the schedule driver; bid validity at 45 days is the longest the firm typically goes (longer because lead time is 18 weeks and award delay compounds the schedule risk).
- Tariff-Aware block included per the v2.1 standard. Cross-reference to `skills/admin/material-tariff-escalation-clause-drafter.md` is critical here — the 18-week PO release window is a major exposure window if a Tariff Event lands.
- Substitution-Request package (PowerFlex 755T vs Yaskawa GA800) attached separately per `skills/operations/submittal-package-compiler.md`. The substitution is the cost-and-lead-time path; the Yaskawa BoD is 22-week lead time per the spec's referenced supplier.
- WI Comm 16 amendments referenced explicitly — required for any DSPS-jurisdiction Wisconsin job; silence on WI Comm 16 is a rejection trigger on permit review.
- IEEE 1584-2018 arc-flash label update is in scope; coordinate with the mill's process-safety officer that the updated labels go on file in the mill's safety-program records.

---

## Example Output — Public-Works (state-prevailing-wage school addition, Division 26 sub-bid)

**Inputs (abbreviated):**

- Addressee: Stephanie Owens, Project Manager, Cascade Public Works Construction
- Project: Yakima School District — Davis HS Performing Arts Addition — 22,000 SF, 1 story, attached to existing Davis HS main building
- AHJ: Washington L&I (WAC 296-46B), 2023 NEC + WA amendments
- Drawings: E-001 through E-602, rev. 2, dated 2026-04-08
- Specs: Division 26 + 27 (data — INCLUDED in this scope), 28 (FA — INCLUDED in this scope, per spec)
- Bid validity: 60 days
- P&P bond: in base (1.0% per RCW 39.08)
- Prevailing wage: applicable (Washington state-funded school construction; Davis-Bacon does not apply, but state prevailing-wage does)
- MWBE: 10% goal stated in the bid documents
- Long-lead callout: 1200 A 480/277 V Eaton Pow-R-Line C switchboard — 28-week lead per Platt Electric Supply quote 2026-04-25; (4) 75 kVA dry-type transformers — 12-week lead
- AIA contract: A401-2017 sub agreement to Cascade
- Substitutions: None proposed
- Cap-and-Share Variant tariff escalation per `skills/admin/material-tariff-escalation-clause-drafter.md` (public works requires Cap-and-Share rather than Full Rider per WA RCW 39.04)

**Output — Scope letter (public-works form, excerpt):**

> **Drysdale Electric Co.**
> 4214 W Industrial Park Dr · Wausau, WI 54401 · (715) 555-0193
> Wisconsin Master Electrician License 1248-MA · DSPS 12-EC-845 · WA EL01 Electrical Contractor License DRYSEL*884JJ
>
> ---
>
> **April 28, 2026**
>
> Stephanie Owens, Project Manager
> Cascade Public Works Construction
> 1844 Tieton Dr, Yakima, WA 98902
>
> **Re: Scope of Work — Davis HS Performing Arts Addition — Bid Date April 28, 2026**
>
> Stephanie,
>
> Drysdale Electric is pleased to submit our scope and pricing proposal for the electrical work at the Davis HS Performing Arts Addition. This proposal is based on the 2023 National Electrical Code as adopted by Washington L&I with WAC 296-46B amendments, the construction documents cited below, and the assumptions and exclusions that follow. Base bid total, alternates, and unit prices are detailed in the accompanying Bid Summary. This proposal is valid for 60 days.
>
> **BASIS OF BID**
>
> - Drawings: Davis HS Performing Arts Addition E-001 through E-602, Revision 2, dated 2026-04-08
> - Specifications: Division 26 (full), Division 27 (Communications — full), Division 28 (Electronic Safety & Security — fire alarm + low-voltage life-safety only), issued 2026-04-08
> - Addenda: None issued
> - RFIs: None submitted at time of bid
> - Substitutions: None proposed
>
> **SCOPE OF WORK — INCLUSIONS**
>
> *(Inclusions section detailed by Division 26 subsection per the standard inclusions library; abbreviated for this excerpt — the actual letter would run 4–6 pages of inclusions.)*
>
> **SCOPE OF WORK — EXCLUSIONS**
>
> *(Exclusions section drawn from the firm's standard public-works exclusions library — typically 22–28 items for a public-works school project; abbreviated for this excerpt.)*
>
> **ASSUMPTIONS & QUALIFICATIONS**
>
> 1. **Code basis.** 2023 National Electrical Code as adopted by Washington L&I with WAC 296-46B amendments, 2021 Washington State Energy Code, NFPA 72 (2022 edition), NFPA 70E (2021 edition), OSHA 1926 Subpart K.
> 2. **Voltages covered.** 480Y/277 V 3Φ 4W for the new 1200 A switchboard service; 208Y/120 V 3Φ 4W for the addition's branch-circuit panels via two new 75 kVA step-down transformers; 24 V DC and 120 V AC for control and FA.
> 3. **Long-lead equipment and Tariff-Aware language** *(per Tariff-Aware Lead-Time Block)*. The Eaton Pow-R-Line C 1200 A switchboard is a 28-week lead item per Platt Electric Supply quote 2026-04-25. The (4) 75 kVA dry-type transformers are a 12-week lead per the same quote. Drysdale Electric will release equipment POs within 3 business days of award; any delay in award past 2026-06-27 may shift the schedule. Per the public-works contract framework (cross-reference `skills/admin/material-tariff-escalation-clause-drafter.md` Cap-and-Share Variant), the Cap-and-Share escalation language applies — a Tariff Event materializing between award and delivery is shared between Drysdale Electric and the school district at the variant's documented split, with schedule-relief equal to the actual delivery delay as a non-compensable time extension. The Cap-and-Share Variant is the public-works equivalent of the private-commercial Full Escalation Rider; it complies with WA RCW 39.04 prompt-payment and prevailing-wage frameworks while protecting both parties from a Tariff Event neither side can foresee.
> 4. **Order-release window.** Drysdale Electric will release the equipment PO to Platt Electric Supply within 3 business days of contract award.
> 5. **Prevailing wage.** Washington state prevailing wage applies to this project; Drysdale Electric will submit certified payroll weekly per WAC 296-127. Davis-Bacon does not apply (no federal funding).
> 6. **MWBE.** Drysdale Electric is a non-MWBE prime sub on this project. Our MWBE participation plan, attached separately, identifies (1) MWBE-certified IT cabling subcontractor (Catalyst LV, MBE-certified) for the Division 27 data scope, contributing 8.5% of our base bid value to the project's 10% goal. The remaining 1.5% is met via the firm's MWBE-certified material suppliers per `config.yml.mwbe.material_suppliers`.
> 7. **Inspection.** Drysdale Electric will schedule rough-in, in-wall, and final inspections with WA L&I Electrical Inspection Section. Re-inspections caused by Drysdale deficiencies are at no cost; re-inspections caused by other trades are billable per WA L&I's published re-inspection fee schedule.
> 8. **Bond.** Performance and payment bond at 1.0% of base bid (per RCW 39.08) is included in the base bid.
> 9. **Bid validity.** 60 days from bid date.
> 10. **Bid-Submission Checklist** *(per the firm's public-works submission protocol, cross-reference `sales/bid-summary-writer.md`).* Confirmed before submission: bid form signed by authorized officer, bid bond at 5% of base bid attached, all addenda acknowledged in the bid form, MWBE participation plan attached, certified-payroll commitment included in the cover letter, prevailing-wage commitment included, contractor license verified active in WA L&I database as of bid date, public-works retainage acknowledgment included.
>
> *(Alternates, allowances, unit prices, and pricing & validity sections per the standard structure.)*
>
> Sincerely,
>
> Mike Drysdale, President
> Drysdale Electric Co.
> Wisconsin Master Electrician License 1248-MA · WA EL01 DRYSEL*884JJ

**Internal Notes:**

- Cap-and-Share Variant referenced per `skills/admin/material-tariff-escalation-clause-drafter.md` — this is the public-works pattern; do not use the Full Escalation Rider on a state-prevailing-wage job because the Rider's pass-through default conflicts with WA RCW 39.04 prompt-payment language.
- MWBE 10% goal — 8.5% from Catalyst LV (MBE-certified Division 27 sub) + 1.5% from MWBE-certified material suppliers; Internal Note flags that MWBE participation plan attachment is a bid-submission disqualifier per the bid documents.
- WAC 296-127 certified-payroll commitment is on the cover letter — not buried in the assumptions. Public-works bids that don't surface certified-payroll language up front get scrutiny on prequalification.
- Bid bond at 5% per the bid documents (different from the 1.0% P&P bond per RCW 39.08); both are line items on the Bid-Submission Checklist.
- 28-week switchboard lead time is the long-lead schedule driver; coordinate with Cascade's superintendent on cutover sequencing for the existing Davis HS service tie-in.

---

## Example Output — Design-Assist Scope Memo (pre-construction phase, no firm pricing)

**Inputs (abbreviated):**

- Addressee: Lin Park, Senior Architect, Brown & Park Architects (EOR proxy on a design-assist contract)
- Project: Eastside Hospital Outpatient Pavilion — design-assist phase, currently 30% DD
- AHJ: City of Bellevue WA, 2023 NEC + WA amendments
- Drawings: 30% DD set issued 2026-04-15 — E-series sheets E-001 through E-201 only
- Spec: outline spec only at 30% DD; no Division 26 detailed spec yet
- Engagement: design-assist subcontract (not a hard-bid sub agreement); deliverable is a design-assist scope memo with proposed product approach + budgetary pricing
- Long-lead callout: 4000 A switchboard with onboard distribution; recommend long-lead early-release (per `skills/operations/submittal-package-compiler.md` long-lead early-release shape)

**Output — Design-Assist Scope Memo (excerpt; less formal than the hard-bid scope letter):**

> **Drysdale Electric Co. — Design-Assist Memo**
>
> **Date:** April 28, 2026
> **To:** Lin Park, Senior Architect, Brown & Park Architects
> **Cc:** Janet Holland, Mechanical Lead, Brown & Park; Marcus Sayre, EOR-of-Record (design-assist subconsulting only at 30% DD)
> **Re:** Eastside Hospital Outpatient Pavilion — Design-Assist Phase Scope and Budget Memo
>
> Lin,
>
> Thanks for the design-assist invitation. This memo captures Drysdale Electric's proposed approach for the Division 26 scope at the 30% DD stage, our budgetary pricing range, and the design decisions we recommend locking before we move to 60% DD. Pricing in this memo is budgetary and subject to firm pricing at bid; the design-assist engagement is paid on a time-and-materials basis per our signed design-assist agreement dated 2026-03-10.
>
> **DESIGN INTENT — DIVISION 26 PROPOSED APPROACH**
>
> 1. **Service entrance.** 4000 A 480/277 V 3Φ 4W main service switchboard fed from the utility's 13.2 kV primary via a pad-mount 2500 kVA dry-type transformer (utility-owned, contractor-installed pad). Section 1 main breaker, Section 2 distribution to the building's load centers. Recommended basis-of-design: Eaton Pow-R-Line C with onboard distribution. Alternate: Square D Power-Style Q-Frame (longer lead time as of Q2 2026; currently 32 weeks vs Eaton's 28 weeks per Platt Electric Supply quote 2026-04-25). Recommend Eaton.
>
> 2. **Distribution architecture.** 480 V branch panels for HVAC and motor loads (mechanical scope); (4) 75 kVA step-down dry-type transformers for general 208Y/120 V branch-circuit distribution; (3) 75 kVA isolation transformers for hospital-grade isolated power systems on the four (4) wet-procedure rooms (per NEC Article 517 Part III). Recommend Square D EE series for the dry-types and PowerCet IsoBlock for the isolation transformers.
>
> 3. **Branch-circuit and emergency systems.** Hospital essential electrical system (EES) per NEC Article 517 — life-safety branch (10s autotransfer), critical branch (10s autotransfer), equipment branch (10s autotransfer); ATS sizing TBD pending the 60% DD load summary; assume (3) 800 A 4-pole ATSs with delayed-transition for equipment branch. Generator sizing TBD pending the 60% DD load calculation; budgetary based on a 750 kW diesel generator (Cummins or Caterpillar) with 96-hour runtime tank.
>
> 4. **Lighting.** All-LED, occupancy-and-daylight-controlled per WA Energy Code 2021. Recommend Acuity nLight or Lutron Quantum networked lighting controls — DALI-2 or 0-10 V depending on the architectural fixture selection at 60% DD. Title 24 cross-reference does not apply (WA, not CA).
>
> 5. **Communications and life-safety.** Division 27 data per the IT consultant; Division 28 fire alarm per the FA consultant — Drysdale Electric's scope is the line-voltage interconnect from the FA panel to the EES.
>
> **BUDGETARY PRICING RANGE**
>
> | Scope component | Budgetary range (Q2 2026 dollars) |
> |---|---|
> | Service entrance + distribution | $1.95M – $2.35M |
> | EES (life-safety + critical + equipment branches) + (3) ATSs + 750 kW genset | $1.40M – $1.75M |
> | Branch-circuit, devices, lighting | $2.20M – $2.65M |
> | Hospital-grade isolated power (4 wet-procedure rooms) | $185K – $235K |
> | Total Division 26 budgetary range | **$5.74M – $6.99M** |
>
> Pricing range reflects the Q2 2026 input-price baseline (Section 232 finished-equipment tier 15% on switchboards, transformers, panelboards; 50% raw tier on copper basis; 25% derivative tier on aluminum and steel raceway). Budgetary range tightens to a single point at 60% DD and to a firm price at hard bid.
>
> **DESIGN DECISIONS WE RECOMMEND LOCKING BEFORE 60% DD**
>
> 1. **EES architecture.** Confirm the (3)-branch architecture (life-safety / critical / equipment) vs a (4)-branch with separate optional standby; the (3)-branch matches NEC Article 517 minimum and is our recommendation absent a specific owner program requirement.
> 2. **Generator fuel type.** Diesel vs natural gas. Diesel gives better cold-start reliability for a Bellevue WA seismic-zone hospital; natural gas is lower fuel-storage maintenance. Recommend diesel.
> 3. **ATS transition type.** Closed-transition vs delayed-transition. Closed-transition costs ~$8K more per ATS but eliminates the 10-second outage on the critical branch during weekly testing, which the hospital's IT and clinical teams will appreciate.
> 4. **Switchboard lead time and tariff exposure** *(per Tariff-Aware Lead-Time Block)*. The Eaton Pow-R-Line C 4000 A switchboard is a 28-week lead item per Platt Electric Supply quote 2026-04-25. If hard-bid award is targeted for Q4 2026, equipment delivery is Q2 2027 — this aligns with the project's substantial-completion target Q3 2027. If award slips past Q4 2026, the schedule moves accordingly. The Section 232 finished-equipment tier 15% (effective through end of 2027) is in effect at the time of this memo; recommend the construction documents include the Tariff Event clause per `skills/admin/material-tariff-escalation-clause-drafter.md` so a Tariff Event between award and delivery is contracted.
>
> Lin, I'd like to schedule a 30-minute call to walk through these items before the 60% DD deadline. The four design decisions above (EES architecture, generator fuel, ATS transition, and the tariff-language confirmation) drive ~$280K of the budgetary range.
>
> — Mike Drysdale, President
> Drysdale Electric Co.

**Internal Notes:**

- This is a design-assist memo, not a hard-bid scope letter. Tone is consultative; pricing is budgetary, not firm; sign-off is informal.
- Tariff-Aware Lead-Time Block included even at 30% DD — recommend the EOR include the Tariff Event clause language in the contract documents so the hard-bid version inherits it.
- Hospital-grade isolated power on the four wet-procedure rooms is the scope item most easily missed at 30% DD; called out explicitly.
- WA Energy Code 2021 referenced (not Title 24); WA project, not CA.
- Long-lead early-release submittal recommendation cross-references `skills/operations/submittal-package-compiler.md` so the EOR knows the contractor will produce an early-release at the appropriate phase.
