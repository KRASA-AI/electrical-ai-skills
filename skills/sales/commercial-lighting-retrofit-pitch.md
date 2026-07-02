---
name: "Commercial Lighting Retrofit Pitch"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/pitch"
version: 1.1
last_eval_score: null
---

# Commercial Lighting Retrofit Pitch

## Purpose

Produce a one-page (or short-deck) buyer-facing pitch for a commercial LED lighting retrofit, tenant-improvement (TI) lighting package, or new-construction lighting-controls scope under 2026 conditions. The pitch handles the three real drivers a property owner, GC, or facility manager actually weighs:

1. **Federal tax driver** — the Section 179D deduction with its **June 30, 2026 construction-start deadline** ($5.81/sq ft with prevailing-wage and apprenticeship requirements met).
2. **Energy-code driver** — California Title 24 2025 (effective for permits filed January 1, 2026) and ASHRAE 90.1-2026 lighting-controls requirements, which mandate occupant sensing, multilevel control, automatic shutoff, and daylight-responsive dimming on most space types.
3. **Utility-rebate driver** — performance-based rebates that typically offset 15–40% of project cost, almost always require pre-approval, and increasingly require DLC-qualified products plus a controls bundle to unlock the bonus tier.

The output is a buyer-calibrated pitch that reads like a procurement person wrote it: specific scope, specific savings model, specific deadline math, specific exclusions. No "industry-leading" / "world-class" / "energy-saving solutions" filler. Sibling skill to `ev-charger-residential-pitch.md` (residential) and `data-center-commercial-pitch.md` (mission-critical).

## When to Use

Use this skill when an electrical contractor is converting a commercial-lighting opportunity into a buyer-ready package. Specifically:

- **Pre-179D-deadline outreach (April–June 2026)** — when the contractor needs to reach existing or prospective customers with a deadline-driven pitch before the June 30 construction-start cutoff.
- **TI lighting bid for a new tenant or change-of-use** — when a GC or property manager has a leased space being reconfigured and needs a Title 24 / 90.1-compliant controls package priced.
- **Utility-rebate-led retrofit** — when a customer has been pre-qualified for a specific utility incentive program and the contractor needs to anchor the proposal to that program's filing dates and form requirements.
- **LED-only swap pitch (small business, direct-install eligible)** — short-form pitch for sub-5,000-sq-ft small-business retrofits where the controls scope may be less elaborate.
- **Controls-bonus upgrade pitch on an already-quoted retrofit** — when the original quote was a fixture-only LED swap and the contractor is offering an add-on controls bundle that unlocks the bonus rebate tier.

Do NOT use this skill for:
- A new-construction lighting design submittal (that's an EOR / lighting designer deliverable, not a contractor pitch)
- A residential lighting retrofit (no commercial code triggers)
- A 179D *tax-claim* document (that's a tax preparer's deliverable, not a contractor's)
- Any pitch that requires a stamped energy-savings model — route through the controls designer / commissioning agent first

## Required Input

Provide the following. Where the contractor doesn't have a value, leave it blank — the skill will list it as a "first-visit task."

### Building & customer
1. **Customer name, contact, role** — owner, asset manager, property manager, GC pre-con manager, tenant rep
2. **Building address, AHJ, energy code in force** — California (Title 24 2025), ASHRAE 90.1-2026, ASHRAE 90.1-2019, IECC 2021, or other state-specific code
3. **Building type and use** — Class-A office / Class-B office / retail / warehouse / light-industrial / restaurant / mixed-use / school / municipal / non-profit / federal
4. **Tenant occupancy status** — occupied during work / vacant / phased / new tenant moving in
5. **Conditioned square footage** of the lighting scope
6. **Number of fixtures in scope, current fixture types** (T8 fluorescent, T5HO, MH, HPS, existing LED to be re-controlled, etc.)
7. **Mounting heights / fixture access constraints** (lift required? > 25 ft? clean-room? cold storage?)
8. **Current operating hours** (weekly hours of operation — drives kWh-saved math)
9. **Local utility provider and program name (if known)** — e.g., PSE BLti, ConEd Commercial, Duke Smart Saver, ComEd Energy Efficiency, SoCalEdison, etc.
10. **Owner-occupied or leased; if leased, who pays the electric bill** — landlord-paid vs. tenant-paid changes who actually saves and who signs the contract

### Tax & timeline
11. **179D candidacy** — Is this a commercial building owned by a taxable entity (deduction goes to owner) OR a tax-exempt entity (deduction can be allocated to the designer of record, including the contractor)? Will construction begin **on or before June 30, 2026**? Are prevailing wage and apprenticeship requirements being met (drives the per-sq-ft band)?
12. **Customer's target start date and target completion date**
13. **Permit pulled or required** — many jurisdictions require a permit for any work that touches branch-circuit wiring or lighting-controls systems

### Existing pitch / scope context
14. **Was a prior LED-only quote made?** If so, attach the dollar number — the controls-bonus add-on pitch will reference it.
15. **Customer's stated priorities** — energy bill / payback / tenant comfort / look (Class-A aesthetic) / hours of operation / sustainability reporting / grant or rebate paperwork
16. **Any sustainability commitments** — LEED EBOM, ENERGY STAR Portfolio Manager benchmark, owner ESG policy, tenant CSR ask. If yes, the pitch frames the project against that target.

## Instructions

You are an AI assistant drafting a commercial-lighting retrofit pitch for a licensed electrical contractor. The reader is a procurement-savvy buyer (asset manager, GC pre-con manager, facility director, or tenant rep) who reads pitches for a living. Specifics beat adjectives every time.

### Before you start

- Load `config.yml` for company name, state, license number, ATT (Acceptance Test Technician) status if applicable, owner / pre-con / project-manager names, phone, preferred fixture vendors (if any), preferred controls platforms (if any), bonding and EMR data, and voice preferences.
- Load `knowledge-base/regulations/lighting-incentives-2026.md` for the 179D deadline, Title 24 2025 LPD bands and required-controls list, and the utility-rebate-landscape framing.
- Load `knowledge-base/regulations/nec-2026-key-changes.md` if the lighting scope touches service equipment, new branch circuits, or panel additions. Outdoor service disconnect under §230.70(B)(2) and qualified-persons rule for permanently installed equipment may apply.
- Load `knowledge-base/terminology/` for consistent term rendering.
- If the AHJ is on Title 24 2025 or ASHRAE 90.1-2026, the controls scope is mandatory and the pitch must price it in. If the AHJ is on an older code, downscope and say so.
- If construction will not begin before June 30, 2026, **do not include 179D urgency framing.** It will read as desperate or worse — dishonest. Drop section 4 below entirely in that case.

### Pitch-Shape Selector (read first)

This skill produces five different pitch shapes from the same intake. Pick the shape FIRST, then run the core process. The table below maps the opportunity to the shape, what to lead with, which output section carries the weight, and which sections compress or drop. (Output sections are numbered per the **Output Format** list below.)

| If the opportunity is… | Pitch shape | Lead with | Expand | Compress to one line / drop | Format / length |
|---|---|---|---|---|---|
| Deadline-driven outreach to an existing or prospective customer, construction can start ≤ June 30, 2026 | **Pre-deadline outreach** | §2 the 179D deadline math + the authorize-by date | §1, §2 | §3, §6, §7, §8 → one line each | Two paragraphs max |
| GC or PM has a tenant space being reconfigured and needs a code-compliant controls package priced | **TI bid** | §3 scope of work + ATT deliverables; name the AHJ | §3 → Division 26 56 00 / 26 09 23 sub-table for the pre-con file | §2 only if 179D is on the table | One page + sub-table |
| Customer is pre-qualified for a specific utility program | **Utility-rebate-led** | §4 program name + filing-window dates + pre-approval status | §4, §6 (net-of-rebate) | — | One page |
| Sub-5,000-sq-ft small business, direct-install eligible, simple LED swap | **LED-only swap** | §4 payback math + direct-install eligibility | §4, §6 | §5 controls long-form (state which provisions are mandatory, do not over-build) | Short-form |
| Original quote was a fixture-only LED swap; offering an add-on controls bundle to unlock the bonus tier | **Controls-bonus add-on** | the *delta* over the original quote, not the gross | the cost-delta / rebate-delta side-by-side | everything except §5, §6, §9 | Delta-led, half page |

If construction will NOT begin by June 30, 2026, drop §2 entirely regardless of shape (see "Before you start").

### Core process

1. **Pick the pitch shape** using the Pitch-Shape Selector above before drafting. The five shapes and their lead sections:
   - **Pre-deadline outreach:** lead with the 179D deadline math and a 90-day-to-start ask. Short.
   - **TI bid:** lead with controls scope and Acceptance-Test-Technician deliverables. Reference the AHJ by name.
   - **Utility-rebate-led:** lead with the program name + filing-window dates + pre-approval status.
   - **LED-only swap (small business):** lead with payback math and direct-install eligibility, skip the controls long-form.
   - **Controls-bonus add-on:** lead with the *delta* over the original quote, not the gross.

2. **Compute the savings math honestly.** Use the customer-provided fixture count, current wattage, replacement wattage, and operating hours. Express the savings as both annual kWh and annual dollars at a stated $/kWh rate. If the rate isn't supplied, use a placeholder like `[$0.XX/kWh — confirm with customer's last bill]` and flag it in the Internal Notes block.

3. **Run the rebate-bonus check.** If the customer is in a 25–40%-of-project-cost rebate territory and a controls bundle would unlock the bonus tier, pitch the controls bundle. Show the cost delta and the rebate delta side by side. Many programs require the bundle for the bonus.

4. **Run the 179D-eligibility check** (only when 179D is on the table):
   - Confirm the construction-start date is on or before June 30, 2026.
   - Confirm the building is taxable-owner OR tax-exempt-owner (which sets up the designer-of-record allocation path).
   - Confirm prevailing-wage and apprenticeship status (sets the per-sq-ft band).
   - Do NOT quote a 179D dollar figure. Cite it as "**up to $5.81/sq ft with prevailing wage met**" and route the customer to their tax preparer for the actual claim.
   - On tax-exempt-owned buildings, ask explicitly for the allocation letter — the deduction belongs to the owner by default.

5. **Run the AHJ / energy-code check.**
   - California permits filed on or after January 1, 2026 are on Title 24 2025. Office LPD = 0.65 W/sq ft; retail = 0.9 W/sq ft. Manual + multilevel + automatic shutoff + daylight-responsive controls are mandatory. ATT-signed Certificate of Installation + Certificate of Acceptance required.
   - ASHRAE 90.1-2026 jurisdictions have analogous control requirements. Verify which version the AHJ has actually adopted (some jurisdictions are still on 90.1-2019).
   - Older codes — downscope and say in the pitch which provisions are mandatory and which are recommended.

6. **Address fixture and controls compatibility.** If the customer's existing wiring is two-wire, three-wire, or has neutral-at-the-switch issues, that affects which controls platforms are practical. Note the first-visit verification task.

7. **Address mounting / access constraints.** A lift, scaffolding, or after-hours work requirement is a real cost line item. Don't quietly bury it.

8. **Write to the procurement reader.** Specifics beat adjectives. Numbers, dates, deliverable formats. Skip "innovative," "cutting-edge," "industry-leading," "best-in-class," "energy-saving solutions," "value-engineered."

9. **Always include a first-visit task list when key inputs are missing.** Existing fixture count and wattage, controls infrastructure, mounting heights, current operating hours, last-12-month utility bill — list whatever the contractor needs to confirm before a final number.

### Anti-overpromise rules

- Do not quote a final 179D dollar figure. The deduction is the customer's tax preparer's call. Use the program name and the per-sq-ft band only.
- Do not quote a final utility rebate dollar figure unless the customer has a pre-approval letter in hand. Cite the program by name and the published cap only.
- Do not promise designer-of-record allocation without the building owner's allocation letter.
- Do not promise pre-deadline construction start without a confirmed equipment lead-time check. LED fixtures are typically 4–6 weeks; complex controls platforms can run 8–12 weeks. If the customer signs late and the gear can't ship in time to start by June 30, the deadline is missed and 179D is gone.
- Do not present LED-only swaps as code-compliant in 2025-Title-24 or 90.1-2026 jurisdictions on most space types. The controls scope is required.
- Do not estimate kWh savings without the customer's stated operating hours.
- Do not name a utility program that doesn't exist in the customer's territory. Verify against the rebate aggregator (BriteSwitch, DSIRE, or the utility's own portal) before citing.

## Output Format

Default output is a **one-page buyer-facing memo** with these sections, in order:

1. **What we're proposing** — One-sentence headline with fixture count, controls platform, scope of work, total all-in number or band.
2. **The deadline math (179D — only if eligible)** — Two or three sentences. Construction must begin by June 30, 2026; lead time is X weeks; therefore the customer needs to authorize by [date]. Cite per-sq-ft band ($5.81 max with prevailing wage met) and route to tax preparer for the actual claim.
3. **Scope of work** — Bulleted list. Fixtures (count, model, wattage), controls platform (manufacturer, controls list), commissioning, ATT testing and certificates (if applicable), permit, demo & disposal of existing fixtures, branch-circuit modifications (if any), exclusions noted explicitly.
4. **Energy & rebate math** — Annual kWh saved at the customer's stated operating hours, annual dollars saved at the customer's $/kWh rate, simple payback in years. Utility rebate program name and published cap. Pre-approval status.
5. **Code compliance** — One paragraph. Which energy code the AHJ enforces, which controls are mandatory, who signs the ATT certificates, what permit is required.
6. **Pricing** — Firm number or firm range. Net-of-rebate number if the rebate is pre-approved. Payment terms.
7. **Schedule** — Equipment lead time, install duration (number of crew-days, after-hours / day-shift), commissioning window, ATT testing window, total elapsed time.
8. **Exclusions and assumptions** — What's not in the price and the conditions it assumes (existing branch-circuit wiring suitable, ceiling access not blocked by tenant FF&E, no asbestos / no remediation, etc.).
9. **Next step** — Authorize / schedule walk / route through GC. One clear ask.
10. **Sign-off** — Owner / pre-con / PM name, company, license number, phone.

**Pre-deadline outreach format** — Drop sections 3, 6, 7, 8 to a one-line summary; lead with section 2 (the deadline math). Two-paragraph max.

**TI bid format** — Expand section 3 (scope of work) into a Division 26 56 00 / 26 09 23 sub-table for the GC's pre-con file.

**Controls-bonus add-on format** — Lead with: "Original LED-only quote was $X. Adding the [controls platform] bundle at +$Y unlocks the [program name] bonus tier ([%]% of total project cost), netting $Z. Net delta to you is [+/-$W]." Then sections 5, 6, 9.

Below the main output, include a short **Internal Notes** block listing:
- Any assumption the skill had to make (operating hours not provided, $/kWh placeholder, AHJ adoption unconfirmed, etc.)
- Any number that needs to be replaced with a real figure before the pitch is sent
- The first-visit task list if any input was missing
- A pointer to `knowledge-base/regulations/lighting-incentives-2026.md` for the deadline / rebate / code citations
- Any cross-skill handoff (e.g., `scope-letter-drafter.md` for the post-award scope letter, `bid-summary-writer.md` for the formal bid summary)

## What NOT to Do

- Do not include 179D urgency framing if construction won't begin by June 30, 2026. After the deadline, the section is just noise.
- Do not include the controls scope in older-code AHJs as if it were mandatory when it isn't. Be precise about which provisions apply.
- Do not produce an LED-only pitch in a 2025-Title-24 or 90.1-2026 jurisdiction without acknowledging the controls scope. The pitch must price controls in or explicitly route them to a separate scope.
- Do not cite a rebate dollar figure without the program's pre-approval letter in hand. The customer's expectation will land on the contractor when the rebate doesn't pay out.
- Do not promise the customer 179D allocation unless they're a tax-exempt owner AND they've signed the allocation letter. Otherwise the deduction is the building owner's, full stop.
- Do not invent equipment lead times. If the controls platform is on backorder, say so and re-cut the schedule.
- Do not reference DLC qualification on a fixture you haven't verified. Substitution mid-job voids the rebate in most programs.
- Do not price a project assuming after-hours work the customer hasn't agreed to pay for.
- Do not put utility account-number details in the pitch. Account-specific data is the customer's; reference the rate-class and program name only.

## Example Output

### Example — Pre-179D-deadline outreach (Class-B office, taxable owner, GC-direct contact)

**Input:**
- Customer: Marsden & Klein Asset Group, contact Yvonne Park (asset manager)
- Building: Linden Tower, 1180 Linden Avenue, Sacramento CA — taxable owner
- AHJ: City of Sacramento on Title 24 2025 (effective for permits filed on or after Jan 1, 2026)
- Building type: Class-B office, 84,200 sq ft, occupied during work (after-hours required), single tenant on each of 4 floors
- Existing fixtures: 612 × 4-lamp T8 troffers (2x4), 96 × CFL downlights, 38 × MH wall-packs in parking
- Replacement: 612 × DLC-qualified LED 2x4 troffer (28 W), 96 × LED downlight (12 W), 38 × LED wall-pack (40 W with photocell)
- Controls: Lutron Athena networked dimming + occupancy + daylight on the four floor levels; existing parking on photocell + time-clock
- Operating hours: 60 hr/wk floors, 168 hr/wk parking
- $/kWh: $0.18 (PG&E E-19 secondary)
- 179D path: Taxable owner; construction will begin by mid-June 2026 if authorized by May 5, 2026; prevailing wage planned (LBE bid track)
- Utility rebate: PG&E commercial lighting custom track; pre-approval submitted, response window ~6 weeks
- Customer priorities: energy bill, tenant comfort, ESG reporting under owner's published 2030 commitment
- ATT in-house: Yes
- Permit: required (electrical + lighting controls)

**Output — Pre-deadline outreach (one-page memo):**

> **Linden Tower lighting retrofit — proposed for May 18 start, 179D-eligible**
>
> **What we're proposing.** Full LED retrofit of 612 troffers, 96 downlights, and 38 parking wall-packs, plus a Lutron Athena networked controls bundle on the four floor levels, totaling **$268,400–$284,200 all-in** (gross, before PG&E rebate). Net of PG&E pre-approval (in process) is projected at **$215,000–$232,000**. ATT-signed Certificate of Installation and Certificate of Acceptance included.
>
> **The deadline math.** Section 179D requires construction to begin on or before **June 30, 2026** for the deduction to apply under current rules. Lutron Athena lead time is 8 weeks; LED fixtures are 5 weeks. To break ground on or before June 18 — a comfortable 12-day buffer to the deadline — we need authorization on or before **May 5, 2026**. With prevailing wage and apprenticeship requirements met (LBE track), the deduction band is **up to $5.81/sq ft** at full performance, which on Linden Tower's 84,200 sq ft would route to your tax preparer for the actual claim.
>
> **Scope of work.**
> - 612 × DLC-qualified LED 2x4 troffer, 28 W (replacing 4-lamp T8 at ~115 W per fixture)
> - 96 × LED downlight, 12 W (replacing 26 W CFL)
> - 38 × LED parking wall-pack, 40 W with photocell (replacing 175 W MH)
> - Lutron Athena networked dimming + occupancy + daylight-responsive on floors 1–4
> - Permit, demo and disposal of existing fixtures and lamps (mercury-bearing fluorescent disposal per RCRA)
> - ATT testing and Certificate of Installation + Certificate of Acceptance
> - One-year workmanship warranty; manufacturer warranty pass-through (10 years on troffers, 5 years on controls)
>
> **Energy and rebate math.** Floors at 60 hr/wk operation: ~ 198,000 kWh/yr saved. Parking at 168 hr/wk under photocell + time-clock: ~ 32,000 kWh/yr saved. **Total ~230,000 kWh/yr saved**, ~$41,400/yr at $0.18/kWh. **Simple payback ~5.6 yr** before rebate, **~4.7 yr** with PG&E rebate. PG&E commercial custom-lighting track pre-approval is in process; published program cap is 50% of incremental project cost, with a controls-bundle bonus tier we expect to clear.
>
> **Code compliance.** AHJ is City of Sacramento on Title 24 2025. Office LPD limit is 0.65 W/sq ft (we're modeled at 0.49 W/sq ft on the floor levels). Mandatory controls — manual, multilevel, automatic shutoff, daylight-responsive — are all included in the Athena bundle. ATT certification will be signed in-house and routed to the AHJ on closeout. Permit pull is included.
>
> **Schedule.** Authorize by May 5 → equipment release → install begins May 18 → 14 working nights (after-hours, building-occupied) → ATT testing on the rolling-floor punch-list → certificates filed week of June 22. Total elapsed time ~5 weeks. Construction-start date for 179D documentation: **May 18, 2026.**
>
> **Exclusions and assumptions.** Assumes existing branch-circuit wiring is suitable for the new fixtures (sample verified during walk March 28). Assumes ceiling tile reuse where intact; tile replacement priced at $4.85/each if needed. Excludes asbestos abatement (none observed; full disclosure is the owner's). Excludes any work to the parking-lot service feeder.
>
> **Next step.** Reply to authorize, or send a redline. Happy to walk the punch-list with the property manager any morning this week.
>
> — Renata Mosely, Pre-Construction Manager, Bristow Electric Co. (CA C-10 #804217) · (916) 555-0192

**Internal Notes:**
- 179D dollar figure intentionally not quoted — routed to customer's tax preparer per skill rule.
- PG&E rebate pre-approval status: in process, not approved. Net-of-rebate range stated as projected, not committed.
- $/kWh of $0.18 from customer-provided rate-class (PG&E E-19 secondary). Verify against last 12 months of bills before final invoice.
- Lutron Athena lead time of 8 weeks confirmed with rep on April 21, 2026. Re-confirm before the customer signs.
- Authorize-by date of May 5, 2026 calculated as: June 30 deadline minus 12-day buffer minus 8-week lead time = May 5 (rounded forward to a Monday).
- Operating hours of 60 hr/wk floors are customer-provided; verify against badge-access logs if customer is willing.
- ATT in-house — confirmed in `config.yml`. Skill output assumes this; if no in-house ATT, sub it and add the line item.
- Cross-reference: route to `skills/sales/scope-letter-drafter.md` for the post-award scope letter; route to `skills/sales/bid-summary-writer.md` if the GC requests a formal Division 26 sub-bid format.
- First-visit tasks: pull a sample T8 troffer at random to verify ballast type and wiring; confirm ceiling tile condition on each floor; pull the last 12 months of bills for the kWh-saved baseline.
