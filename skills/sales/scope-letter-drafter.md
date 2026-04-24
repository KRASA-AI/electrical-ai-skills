---
name: "Scope Letter Drafter"
category: sales
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~25 min/letter"
version: 2.0
last_eval_score: 6.80
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

- Load `config.yml` from the repo root for company legal name, license number (by state), addresses, federal employer ID if sub agreement, MBE/WBE status, bond capacity, insurance certificate reference, standard exclusions boilerplate, owner/estimator name, and voice preferences.
- Reference `knowledge-base/regulations/` for NEC edition citations matching the AHJ adoption cycle.
- Reference `knowledge-base/terminology/` for correct trade terminology (e.g., "EGC" not "ground wire," "luminaire" not "light fixture" in commercial context).
- For commercial scope letters, always organize to CSI MasterFormat Division 26 (and 27/28 if applicable).
- Cross-reference `sales/bid-summary-writer.md` for the shared standard exclusions library — the two skills must use identical language so GC PMs reading both see one voice.

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
