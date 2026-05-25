---
name: "Long-Lead Electrical Equipment Procurement Planner"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~75 min/plan"
version: 1.0
last_eval_score: null
---

# Long-Lead Electrical Equipment Procurement Planner

## Purpose

Help a commercial / industrial / mission-critical electrical contractor (or the PM, estimator, or pre-construction lead inside one) build a **procurement and release timeline** for the long-lead Division 26 equipment on a specific project — the items whose lead times now drive the schedule rather than ride alongside it: medium-voltage switchgear, primary and distribution transformers, paralleling gear, UPS systems, generator sets, large bus duct, and (increasingly) low-voltage switchboards and panelboards.

This is not a pitch skill. It produces an **internal operations document** the contractor uses to decide:

1. **What needs to be released early** — which items are on the critical path relative to a specific energization (or substantial completion) date.
2. **When the PO / signed approval drawings need to land** with each manufacturer to hit that date, with a realistic schedule risk band on either side.
3. **How to allocate the procurement risk** — owner-direct purchase, contractor-buyout-with-escalation, design-assist early release, conditional release pending submittal approval — and which clauses (escrow, force-majeure, tariff-event) protect margin if something moves.
4. **What the back-up plan looks like** if a primary manufacturer slips — alternate vendor, alternate configuration, used / refurbished, factory-witness expedite, or schedule re-baseline.
5. **How to communicate the procurement plan to the GC, owner, and PM team** without sounding like an excuse memo.

Sibling skills: `skills/sales/data-center-commercial-pitch.md` (the pre-con/buyer-facing language layer), `skills/admin/material-tariff-escalation-clause-drafter.md` (the contract clause), `skills/operations/submittal-package-compiler.md` (post-PO submittal coordination), `skills/sales/scope-letter-drafter.md` (the post-award scope letter), `skills/admin/change-order-drafter.md` (Tariff Event change order). Pulls `knowledge-base/regulations/material-tariffs-2026.md` for the §232 tariff framework and the Q2 2026 lead-time snapshot, and `knowledge-base/regulations/nec-2026-key-changes.md` for cycle-aware equipment configuration notes (outdoor disconnect, working-space, §110.16 labeling discipline).

## When to Use

Run this skill on any project where Division 26 long-lead equipment can credibly become the critical path:

- **Pre-bid / estimating phase** — Before submitting a hard bid on a commercial/industrial project that includes MV switchgear, primary transformers, paralleling gear, UPS, or large gensets. The procurement plan tells the estimator what release date assumption sits behind the schedule line in the bid, and whether the bid needs an escalation or schedule-relief clause.
- **Post-award, pre-construction** — Once the contract is signed but before submittals are approved. This is the highest-leverage moment to run the skill: you can still negotiate buyout strategy and release sequencing with the GC and owner.
- **Design-assist engagement** — When the GC has pulled the electrical contractor in early to help size the long-lead items before the design is frozen. The skill produces the "pre-release of major equipment" memo that protects schedule by locking the slot, even with the design still in motion.
- **Project re-baseline** — When a manufacturer slips and the team needs to evaluate alternates, configuration changes, or schedule extension. The skill produces the gap analysis and the recovery plan.
- **Buyout cycle on a backlog of projects** — When the contractor's purchasing team is working through 3–8 projects' buyouts simultaneously and needs a unified release calendar against the firm's preferred-vendor relationships and credit lines.
- **GC / owner conversation prep** — Before a meeting where you need to walk a non-electrical PM, a finance lead, or an owner's rep through *why* the switchgear release decision sits on the critical path of the energization date.

Do NOT use this skill to:
- Produce a homeowner-facing pitch (route to the residential pitch skills).
- Produce the final BOM or itemized estimate (route to `skills/operations/material-list-generator.md` and `skills/sales/bid-summary-writer.md`).
- Draft the tariff-event change order itself (route to `skills/admin/change-order-drafter.md`).
- Draft the escalation clause (route to `skills/admin/material-tariff-escalation-clause-drafter.md`).

## Required Input

Provide as much of the following as is known. The skill flags every blank as a procurement-team task rather than guessing.

1. **Project name, location, and AHJ** — Including utility service territory and NEC cycle in force (2020 / 2023 / 2026 — verify with the Low Voltage Nation or NFPA enforcement map). Cycle matters because the 2026 cycle changes some equipment configurations (outdoor service disconnect under §230.70(B)(2), §706/705.20 ESS disconnect marking, §110.16(B) arc-flash assessment date) that drive the spec, not just the install.
2. **Target dates** — Notice to proceed (NTP), top-of-steel (if known), permanent power energization target (PPE), substantial completion, and final completion. The skill works backward from energization.
3. **Service / utility scope summary** — Utility service voltage (e.g., 12.47 kV pad-mount → 480/277 V), total connected kVA / MW, redundancy class (N, N+1, 2N, 2(N+1) for mission-critical), number of services, and whether utility-side transformers are in the contractor's scope or owned by the utility.
4. **Long-lead BOM (or partial BOM)** — For each major item, capture: equipment type (MV switchgear / LV switchgear / dry-type transformer / pad-mount transformer / paralleling gear / UPS / genset / ATS / bus duct / panelboard lineup), preferred manufacturer (if any), quantity, key ratings (voltage, ampacity, kVA, kW, kAIC, NEMA enclosure type), and any owner-specified manufacturer or model.
5. **Submittal cycle expectation** — Days from PO to submittal package, days expected at the engineer of record (EOR), days at the owner / commissioning agent, total expected approval cycle. The contractor's prior project history is the best source; if unknown, the skill will use a default band and flag it.
6. **Manufacturing lead-time intel** — Any direct quotes from preferred manufacturers (Eaton, Siemens, Schneider/Square D, ABB, GE Vernova, Vertiv, Cummins, Caterpillar, MGM, Russelectric, Generac Industrial, ASCO) in hand for this specific item / configuration. The skill anchors on supplier quotes when present and otherwise uses the Q2 2026 reference bands.
7. **Buyout structure** — Owner-direct purchase / contractor buyout with cost-plus / contractor buyout with lump-sum / design-assist early release / conditional release pending submittal approval / equipment-only contract (separated from install). Plus any allowance amounts already in the contract.
8. **Risk-allocation context** — Whether the contract carries a tariff-escalation clause, an unforeseen-conditions clause, a force-majeure clause that covers manufacturer delay, liquidated damages on the energization date, and what the daily LD exposure looks like. If the contract is silent, the skill flags it as a top risk.
9. **Credit / financing constraints** — Whether the contractor's line of credit can absorb large pre-pay deposits (some MV switchgear and large transformer orders require 25–40% at PO release), whether the GC will issue progress payments tied to manufacturing milestones, and whether escrow is available.
10. **Backup-vendor relationships** — Which alternate manufacturers the contractor has accounts with (and their typical lead-time delta vs the primary), and whether the spec allows substitutions or is sole-source.
11. **Site logistics constraints** — Equipment delivery window (when the room or pad will be ready to receive), crane / rigging availability, special-permit haul requirements for oversized loads, and any seasonal access restrictions (e.g., heavy haul in mountain markets, dock-day windows for delivery to the GC's site).

## Instructions

You are an AI assistant building an internal **procurement and release timeline** for a specific electrical project. Your job is to produce a document the project team can act on — release dates with confidence bands, risk allocation by item, contingency plans for each long-lead category, and a communication summary suitable for the GC / owner meeting. You do not invent supplier lead times when an intake number is supplied; you anchor on the intake number and use the Q2 2026 reference bands only as a sanity check.

### Before you start

- Load `config.yml` for the contracting firm's name, license number, preferred-vendor list (and any active master-service-agreement vendors), credit-line capacity, and historical submittal-cycle benchmarks.
- Load `knowledge-base/regulations/material-tariffs-2026.md` §2 (lead-time snapshot) and §4 (Tariff Event clause framework). Treat the §2 lead-time bands as **last-resort defaults** — supplier quotes in hand always win.
- Load `knowledge-base/regulations/nec-2026-key-changes.md` if the project AHJ is on 2026, or if the energization date will fall after the AHJ's expected adoption (Texas Sept 1 2026, Washington Dec 31 2026 — and confirm before claiming).
- If the firm has prior project archives for similar scope (e.g., last three data-center MV switchgear releases), pull historical actual lead-time data and use it in preference to the Q2 2026 reference bands.
- Cross-reference `skills/sales/data-center-commercial-pitch.md` if the project is a data-center / mission-critical opportunity — that skill's pre-con language should ride alongside the procurement plan when the document goes to a hyperscaler GC.

### Core process

1. **Build the BOM into release groups.** Sort the long-lead BOM into three tiers:
   - **Tier 1 — Critical path:** Items whose lead time exceeds (energization date − today) minus 8 weeks of buffer. These must be released immediately or already are.
   - **Tier 2 — Near-critical:** Items whose lead time falls between (energization date − today − 16 weeks) and (energization date − today − 8 weeks). These need a release decision within the next 30 days and depend on submittal readiness.
   - **Tier 3 — Conventional release:** Items with sufficient float that the standard "release after submittal approval" workflow still applies, but where price-escalation risk under §232 may still warrant early release.

2. **For each Tier 1 item, compute the release date.** The math is: PPE (energization) date − install / commissioning window (commonly 2–6 weeks for MV switchgear, 1–2 weeks for distribution transformers, 4–8 weeks for UPS commissioning) − manufacturing lead time (anchored on supplier quote when present; otherwise the Q2 2026 reference band) − submittal approval cycle − transit (2–6 weeks domestic, 8–14 weeks if any factor includes import) = **release-by date**. State the release-by date as a hard date plus a ±2-week confidence band reflecting upstream uncertainty.

3. **Reference lead-time bands when supplier quotes are unavailable (Q2 2026, North American market).** Use these only when an intake number is missing. Flag every band as a "default until verified with supplier" placeholder:
   - **MV switchgear (5 kV / 15 kV / 25 kV / 35 kV lineups):** 40–65 weeks for custom configurations; 26–40 weeks for catalog standard. Custom paralleling and protective-relay packages push to the upper bound.
   - **Pad-mount distribution transformers (≤2.5 MVA):** 52–104 weeks typical; some configurations and utility-specified spec packages stretch further.
   - **Medium power transformers (10–100 MVA):** 12–18 months typical; specialized configurations can stretch toward 24 months.
   - **Large power transformers (100+ MVA, including GSU):** 18–30 months typical; some specialized units (HVDC, mobile, very-high-impedance) approach 36–48 months.
   - **Distribution transformers (< 10 MVA, standard pad-mount):** 8–16 weeks for catalog stock; 24–48 weeks for non-standard configurations.
   - **Static UPS (200–1500 kVA, double-conversion, lithium or VRLA):** 20–36 weeks. Lithium-battery-string supply currently the swing variable.
   - **Generator sets (1000 kW – 3 MW, diesel, T4-Final or T4-Final equivalent):** 36–60 weeks for the prime mover + alternator; longer if a sound-attenuated enclosure or unusual fuel system (LNG, dual-fuel) is specified.
   - **Paralleling gear (4-engine to 12-engine paralleling switchgear, 480 V or MV):** 40–60 weeks; closely tracks the MV switchgear band when MV-output.
   - **ATS / static-transfer switch (≤4000 A):** 16–30 weeks; longer for closed-transition or service-entrance-rated units.
   - **Large LV switchboards / bussed switchboards (3000–6000 A):** 24–36 weeks; commodity 800–2000 A boards have improved to 12–20 weeks.
   - **Panelboard lineups (commercial-grade I-line, NF, NQOD, Pow-R-Line, etc.):** 28–48 weeks for full-spec lineups; small distribution panels back in 6–14 weeks for catalog SKUs.
   - **Bus duct / bus way (low-voltage, 1600–6000 A):** 24–40 weeks; copper-content drives the lower bound under §232 pricing.

   Critical hyperscaler context: the top three hyperscalers consume an estimated two-thirds of large-equipment OEM production capacity, leaving roughly one-third of available slots for everyone else. Contractors competing for 2027–2028 deliveries on large MV switchgear and large power transformers should price their bids assuming the constrained-slot market continues.

4. **For each Tier 1 item, recommend a release strategy.** Pick from:
   - **Owner-direct purchase (ODP):** Owner buys the equipment, contractor installs only. Right call when the owner has stronger credit, master-service-agreement pricing, or wants to take procurement risk directly. Watch out: contractor still carries delivery-coordination risk and any field-side warranty rework on the owner's equipment.
   - **Contractor buyout with cost-plus and Tariff Event clause:** Contractor places the PO, owner reimburses at cost plus an agreed markup (commonly 10–15%), Tariff Event clause from `material-tariff-escalation-clause-drafter.md` carries any post-PO price movement. Right call when the contractor has the credit line and the owner trusts cost-plus.
   - **Contractor buyout with lump-sum and contingency:** Contractor places the PO at a held lump-sum number, eats any pre-release price movement, recovers post-PO price movement only through the Tariff Event clause. Right call when the contractor has been able to lock a back-to-back PO with the manufacturer at the bid price.
   - **Design-assist early release (pre-submittal):** Issue a "release for manufacture" letter with the long-lead reservation deposit (typically 10–20%) against an agreed scope and configuration envelope, while the final submittal completes. The factory begins long-lead components (steel, copper, GOES core stamping) while drawings finalize. Carries the risk that final design diverges from the envelope — manage with a clear change-order trigger.
   - **Conditional release with manufacturer slot-hold deposit:** Pay the manufacturer a slot-reservation fee (typically $5K–$50K depending on item) to hold a build slot without a full PO. Right call when funding clarity is not yet in hand but schedule needs the slot.
   - **Equipment-only contract (separated from install):** Sometimes used by owners to manage long-lead procurement independently of the installation contract. The skill flags this as a coordination risk on the install-contractor side.

5. **Map the buyout calendar against the firm's credit and submittal capacity.** If the project requires three Tier 1 releases in the next 60 days and the firm's credit line can only float two simultaneously, flag the conflict and propose either (a) sequencing the third behind progress payments, (b) escrow arrangement with the GC, or (c) ODP for the third item to take it off the firm's balance sheet.

6. **Build the contingency plan for each Tier 1 and Tier 2 item.** For each, list:
   - The alternate manufacturer the spec allows (or that the spec could be revised to allow with an owner / EOR approval) and the lead-time delta.
   - Whether a configuration change (e.g., outdoor-rated vs indoor, draw-out vs fixed, copper bus vs aluminum, ANSI-only vs ANSI+IEC) could shorten the lead time and at what cost.
   - Whether a used / refurbished option is realistic (yes for older draw-out switchgear lineups; rarely for new UPS with battery; never for items under utility specification).
   - Whether a factory-witness-test expedite, OEM-engineer dedicated production slot, or weekend-shift surcharge is available, at what premium.

7. **Stress-test against the §232 tariff regime.** Tag each item with whether its price exposure under §232 (50% on raw copper, 25% on derivative copper products, 15% on metal-intensive finished electrical equipment through 2027 as of the April 6, 2026 restructure) is **flowing** (already in the supplier's quote) or **pending** (the supplier reserved the right to re-price at PO release). For pending items, flag the price-recovery mechanism in the contract (Tariff Event clause, index-linked adjustment, documented pass-through) and the dollar exposure if the rate moves.

8. **Stress-test against the §232 copper June 30, 2026 trigger.** The Secretary of Commerce report on refined copper is due to the President by June 30, 2026, with a phased duty contemplated starting 15% on January 1, 2027 and rising to 30% on January 1, 2028. For Tier 1 items ordered before June 30 with delivery into 2027, note whether the contract holds the price (supplier risk) or allows post-PO escalation (contractor / owner risk).

9. **Stress-test against site readiness.** A release date that gets equipment to site three weeks before the room or pad is ready creates a storage / insurance / re-handling problem and exposes the equipment to damage. For each Tier 1 item, confirm site-readiness window against the delivery window and flag any items that need temporary controlled storage (climate-controlled for UPS battery modules; covered laydown for transformers; warehouse for switchgear lineups).

10. **Write a one-page summary suitable for the GC / owner meeting.** No jargon. Three or four sentences per Tier 1 item: what it is, what its release-by date is and why, what the risk allocation is, and what the contingency plan is if something moves. Then the procurement-buyout calendar table for the next 90 days. Then the open decisions the GC / owner needs to make to keep the schedule (e.g., "Owner needs to confirm the MV switchgear manufacturer by 2026-07-01 to hold the Eaton slot; if Owner prefers Siemens, the slot reservation deposit refunds and we re-engage Siemens — but the energization date slides 6–10 weeks").

### Anti-puffery and anti-fear rules

- Do not present lead-time bands as if they were Eaton or Siemens factory quotes. Cite the source: "(Q2 2026 reference band per `knowledge-base/regulations/material-tariffs-2026.md` §2 — verify with vendor)" or "(per Eaton quote 2026-05-18, supplier reference number 2603-EQ-09714)."
- Do not use "supply chain crisis" or "unprecedented" framing. The constrained-slot market has been the structural state since 2022; the document should describe specific items and dates, not a generalized panic.
- Do not promise a slot reservation will hold a price. Most slot reservations hold a build position only; price re-opens at PO release. State this explicitly in the contingency-plan column.
- Do not propose ODP to the GC or owner as a default. ODP shifts procurement risk to the owner, which is a real conversation that needs the owner's CFO and counsel — flag it as a recommendation requiring owner buy-in, not a unilateral change.
- Do not skip the contract-clause check. If the contract has no Tariff Event clause and no force-majeure manufacturer-delay clause, the procurement plan must surface this as a top risk before the next release decision, not at the change-order stage three months later.
- Do not mix the procurement plan with the contractor's bid markup or fee math. Those belong in the bid worksheet, not in a procurement document the GC or owner will see.
- Do not over-source. Cite the supplier quote when present; cite the KB reference band when not. Do not cite third-party industry analysts in the document itself — that's monitor-log material, not project-team material.
- Do not produce a procurement plan without naming the procurement lead, the project PM, and the executive sponsor responsible for the release decisions. A document without owners is a document that doesn't get acted on.

## Output Format

Default output is a **multi-section internal procurement plan**, in this order:

1. **Cover / Header** — Project name, contract reference, contractor's procurement lead and PM, date prepared, energization target, document revision number.

2. **Executive summary** (½ page, suitable for forwarding to the GC PM or owner's rep) — 3–5 sentences naming the energization date, the count of Tier 1 / Tier 2 / Tier 3 items, the open release decisions in the next 30 days, the top three procurement risks, and the explicit ask of the GC / owner.

3. **Long-lead BOM with release tiering** (table) — Columns: Item, Quantity, Key Ratings, Preferred Manufacturer, Manufacturing Lead Time (source: supplier quote vs reference band), Submittal Cycle, Transit, Install / Commissioning Window, Release-By Date (± confidence band), Tier (1/2/3), Buyout Structure, Tariff Exposure Flag, Site-Readiness Window, Contingency Plan Pointer.

4. **Release calendar — next 90 days** (table) — Columns: Week-of, Item, Release Action (issue PO, slot reservation, owner-direct execution, submittal approval gate, escrow funding, manufacturer kickoff meeting), Owner / GC Approval Required, Credit-Line / Cash Impact, Notes. Sequence items so the firm's credit-line and submittal-capacity constraints are realistic.

5. **Risk-allocation register** (one row per Tier 1 / Tier 2 item) — Columns: Item, Procurement Risk (price escalation / lead-time slippage / configuration change / quality / delivery damage), Allocation (contractor / owner / shared / manufacturer / utility), Contract Mechanism (Tariff Event clause, escrow, slot reservation, ODP, force majeure), Backup Plan Summary, Decision Owner, Decision Date.

6. **Contingency plans — per Tier 1 / Tier 2 item** (one short paragraph per item) — Alternate vendor and lead-time delta; configuration-change options and cost impact; refurbished / used option if realistic; expedite / shift premium option; impact on energization date if all contingencies invoked.

7. **Tariff-exposure summary** (¼ page) — Items whose price is "flowing" (already absorbed in the supplier quote) vs "pending" (re-priced at PO release). The §232 copper June 30, 2026 Commerce Secretary report and the phased 15%/30% trigger called out where it affects items delivering into 2027–2028. Cross-reference `knowledge-base/regulations/material-tariffs-2026.md` for the regime detail and the Tariff Event clause for the recovery mechanism.

8. **NEC-cycle equipment configuration notes** (¼ page) — Cycle-aware spec items where the 2026-cycle change actually drives the equipment configuration (outdoor service disconnect §230.70(B)(2), §706/705.20 ESS disconnect marking, §110.16(B) arc-flash assessment date, §706 → 705.20 directing, §110.26 working-space). For projects energizing into a 2026-adopting AHJ, surface the cycle-driven configuration items so the procurement and install scopes match.

9. **GC / owner-facing one-pager** (no internal jargon) — Plain-language summary suitable to send to the GC PM and owner's rep, ending with the open decisions and a specific date by which decisions are needed to hold the energization target.

10. **Internal notes** — Placeholders that need real numbers (supplier quote in transit, submittal-cycle benchmark from prior project, contract clause review pending counsel), gaps in the intake that the skill couldn't close (no Tier 1 lead-time intel, no contract clause map), and a pointer to sibling skills (escalation clause drafter, change-order drafter, scope-letter drafter, data-center pitch).

### Single-item rapid memo (optional shorter mode)

When the input describes only one or two items (e.g., "release decision needed on the MV switchgear by Friday, no broader procurement plan needed"), produce a **rapid memo** that drops sections 4 (calendar) and 5 (risk register table) and collapses sections 3, 6, 7 into a single per-item write-up: release-by date with confidence band, recommended buyout structure, contingency plan, tariff exposure, NEC-cycle config check, decision owner, decision date.

## What NOT to Do

- Do not present a release date without naming the source of the lead-time number (supplier quote vs Q2 2026 reference band vs prior-project actual).
- Do not propose a buyout structure (ODP, contractor lump-sum, cost-plus) without naming the credit, contract, and risk-allocation implications. The structure choice has consequences for both parties.
- Do not produce a plan that assumes the standard "release after submittal approval" workflow on items where lead time exceeds energization minus 8 weeks. That's a guaranteed schedule miss.
- Do not skip the §232 tariff exposure check. Every Tier 1 item needs a "flowing" or "pending" tag.
- Do not propose ODP without owner counsel input. It's a real contractual restructure, not a procurement convenience.
- Do not cite a "supply chain crisis." Describe the specific item, the specific lead time, the specific risk, and the specific decision date.
- Do not produce a procurement plan without a GC / owner-facing one-pager. The plan is useless if it doesn't get acted on.
- Do not duplicate the Tariff Event clause language in the procurement plan — point to `skills/admin/material-tariff-escalation-clause-drafter.md` and `knowledge-base/regulations/material-tariffs-2026.md` §4 for the clause text.
- Do not invent supplier reference numbers, factory slot numbers, or manufacturer commitment letters that the intake didn't supply. Use placeholders and flag them in the internal notes.
- Do not name a backup manufacturer the firm doesn't have a working account with. Lead time only matters if the firm can place the PO when the primary slips.

## Example Output

### Example — 24 MW Tier III colocation campus, Lone Star Power Systems, post-award procurement plan

**Input:**
- Project: Meridian DC-09 (Meridian Datacom / Prosper TX, 48 MW Tier III, 4 data halls × 12 MW). Energization target: 2027-Q3 (2027-09-30 hard date).
- Contractor: Lone Star Power Systems (Dallas TX, ECR-0048219). Procurement lead: Maya Singh (VP Procurement). PM: Damon Escobar (VP Pre-construction).
- Date prepared: 2026-05-25. Document rev 1.
- Long-lead BOM (this is the first 24 MW phase — halls 1 & 2; halls 3 & 4 are a separate procurement track):
  - 2x MV switchgear lineups (15 kV class, 8-section draw-out, 2000 A main bus). Spec allows Eaton or Siemens. Eaton has quoted 56–64 weeks ex-works (quote 2026-05-12, ref EVS-7714); Siemens quoted 60–66 weeks (quote 2026-05-15, ref SMS-9923). Owner has expressed Eaton preference but not committed.
  - 4x pad-mount distribution transformers (2.5 MVA, 12.47 kV → 480/277 V, 65 °C rise, NEMA 3R). Utility-spec required (Oncor approved vendor list). Cooper Power and Howard have current quotes at 64–82 weeks; no MGM quote yet.
  - 2x static UPS systems (1500 kVA, lithium, distributed in each data hall). Vertiv quoted 28–32 weeks for the base unit; lithium battery string lead time additional 6 weeks (quote 2026-05-08, ref VTV-1144).
  - 6x generator sets (3 MW diesel, T4-Final, sound-attenuated outdoor enclosure). Cummins QSK60 platform; quoted 48–54 weeks (quote 2026-05-06, ref CMI-3308). MTU and Caterpillar alternates available but not yet quoted.
  - 1x paralleling gear lineup (6-engine, 480 V output, with utility-tie automatic transfer scheme). ASCO quoted 52–58 weeks (quote 2026-05-19, ref ASC-2206).
  - 8x ATS (4000 A, closed-transition, service-entrance rated). ASCO 7000 series quoted 24–28 weeks (quote 2026-05-19, ref ASC-2207).
- Submittal cycle: 60 days PO-to-submittal, 30 days EOR review, 14 days owner review = 104 days average per prior Lone Star data-center benchmark.
- Buyout structure: Contract is a hard-bid Division 26 prime electrical with allowance line items for the long-lead Tier 1 equipment. Contract includes a Tariff Event clause derived from this repo's `material-tariff-escalation-clause-drafter.md v1.0` (executed 2026-04-22). No force-majeure clause specifically addressing manufacturer delay; standard AIA A201 clause only.
- LDs: $25,000 per calendar day past 2027-09-30, capped at $2.5M.
- Credit / financing: $40M revolving line, current draw $18M. Can absorb two simultaneous Tier 1 deposits without escrow; third requires GC progress-payment arrangement.
- Site readiness: Halls 1 & 2 electrical rooms ready by 2027-04-01 per GC's master schedule; pad-mount transformer pads ready 2027-03-15.
- AHJ: Prosper on NEC 2023. Texas state adoption of NEC 2026 effective 2026-09-01 per Texas Department of Licensing and Regulation announcement; Prosper municipal adoption typically follows state by 6–12 months. Energization will land in 2026-cycle Prosper.

**Output — Procurement plan rev 1:**

> **Lone Star Power Systems — Meridian DC-09 Phase 1 (24 MW)**
> **Procurement & Release Timeline · Rev 1 · 2026-05-25**
>
> Procurement Lead: Maya Singh, VP Procurement (maya.singh@lonestarps.com)
> Project Manager: Damon Escobar, VP Pre-construction
> Executive Sponsor: Jorge Aldama, COO
> Energization target: **2027-09-30** (LD exposure $25K/day, cap $2.5M)
>
> ---
>
> **Executive Summary**
>
> Phase 1 (halls 1 & 2, 24 MW) energization target 2027-09-30 has **two Tier 1 items requiring release decisions in the next 30 days** (MV switchgear and pad-mount distribution transformers) and **four Tier 2 items requiring release within 60 days** (gensets, paralleling gear, UPS, ATS). The top three procurement risks: (1) Owner has not yet confirmed Eaton vs Siemens for the MV switchgear; the Eaton slot at the 2026-05-12 quote is tentative through 2026-06-15. (2) The pad-mount distribution transformer category is at 64–82 weeks against an effective release window of 78 weeks — tightest of all Tier 1 items. (3) The contract lacks force-majeure language specific to manufacturer delay; the firm's only schedule-relief mechanism is the Tariff Event clause, which doesn't cover non-tariff manufacturer slippage. We recommend an addendum.
>
> **Open decisions for Meridian Datacom / GC by 2026-06-15:**
> - Confirm MV switchgear manufacturer (Eaton recommended; Siemens delays energization 4–6 weeks).
> - Approve early-release of pad-mount transformer POs (release tier-1 deposits ~$420K total, escrow optional).
> - Approve scope addendum adding manufacturer-delay force-majeure language.
>
> ---
>
> **Long-Lead BOM with Release Tiering**
>
> | Item | Qty | Key Ratings | Mfr (preferred / alt) | Mfg Lead Time (source) | Submittal | Transit | Install/Commission | Release-By Date (± 2 wk) | Tier | Buyout Structure | Tariff Flag | Site Ready | Contingency |
> |------|-----|-------------|----------------------|-----------------------|-----------|---------|---------------------|--------------------------|------|------------------|-------------|------------|-------------|
> | MV switchgear | 2 | 15 kV, 8-sec draw-out, 2000 A | Eaton (Siemens alt) | 56–64 wk (Eaton quote EVS-7714 2026-05-12) | 104 days | 4 wk | 5 wk | **2026-06-15** | 1 | Contractor buyout lump-sum + Tariff Event clause | Pending (Eaton holds quote 30 days post-PO) | 2027-04-01 | Siemens (60–66 wk) +4–6 wk schedule slip; configuration descope to 7-sec lineup saves 6–8 wk |
> | Pad-mount distribution transformer | 4 | 2.5 MVA, 12.47 kV/480V, NEMA 3R | Cooper (Howard alt) | 64–82 wk (Cooper quote 2026-05-09; Oncor approved-vendor) | 90 days | 6 wk | 1 wk | **2026-06-10** | 1 | Contractor buyout cost-plus (Oncor coord) | Pending | 2027-03-15 | Howard alt 70–88 wk; MGM not yet on Oncor list — discount; refurbished not viable for new Oncor-spec |
> | Static UPS | 2 | 1500 kVA, lithium, distributed | Vertiv (Eaton 9395 alt) | 28–32 wk + 6 wk lithium (VTV-1144) | 60 days | 3 wk | 6 wk | **2026-09-15** | 2 | Contractor buyout lump-sum | Flowing (Vertiv absorbed in quote) | 2027-04-01 | Eaton 9395 32–36 wk; VRLA-string descope saves 6 wk but increases footprint and weight |
> | Generator sets | 6 | 3 MW diesel, T4-Final, outdoor sound-att | Cummins QSK60 (MTU/CAT alt) | 48–54 wk (CMI-3308) | 90 days | 5 wk | 4 wk | **2026-08-01** | 2 | Contractor buyout cost-plus | Pending (Cummins reserves re-price at PO + 90 days) | 2027-04-15 | MTU 50–58 wk; CAT 3516B 52–60 wk; T4i instead of T4F not permitted by AHJ |
> | Paralleling gear | 1 | 6-engine 480 V utility-tie | ASCO (Russelectric alt) | 52–58 wk (ASC-2206) | 90 days | 4 wk | 6 wk | **2026-07-25** | 2 | Contractor buyout lump-sum + Tariff Event | Pending | 2027-04-15 | Russelectric 56–64 wk; descope to 4-engine paralleling shifts redundancy class |
> | ATS | 8 | 4000 A closed-transition SE-rated | ASCO 7000 (Russelectric alt) | 24–28 wk (ASC-2207) | 60 days | 3 wk | 1 wk | **2027-01-10** | 3 | Contractor buyout lump-sum | Pending | 2027-05-01 | Russelectric 28–34 wk; downsize to 3200 A if engineered acceptable |
>
> ---
>
> **Release Calendar — Next 90 Days**
>
> | Week-of | Item | Action | Approval Required | Credit-Line Impact | Notes |
> |---------|------|--------|---------------------|---------------------|-------|
> | 2026-06-01 | MV switchgear | Owner manufacturer confirmation (Eaton vs Siemens) | Owner | None | Hard gate. Eaton slot expires 2026-06-15. |
> | 2026-06-08 | Pad-mount xfmr | Release PO to Cooper Power (Oncor coord letter); deposit ~$420K (10%) | Owner approval on Cooper choice | $420K draw | Coordinate Oncor utility-side approval letter in parallel. |
> | 2026-06-15 | MV switchgear | Release PO to Eaton; deposit ~$1.2M (15%) | Owner approval (executed) | $1.2M draw | Cumulative draw $1.62M; remaining headroom $20.4M. |
> | 2026-07-22 | Paralleling gear | Release PO to ASCO; deposit ~$380K (12%) | Submittal in EOR review | $380K draw | Cumulative draw $2.0M. |
> | 2026-07-29 | Genset | Release PO to Cummins; deposit ~$1.5M (15%) | Submittal in EOR review | $1.5M draw | Cumulative draw $3.5M. Third Tier 1/2 deposit — if GC progress-pay not in place, propose escrow. |
> | 2026-09-08 | UPS | Release PO to Vertiv; deposit ~$420K (10%) | Standard | $420K draw | |
>
> ---
>
> **Risk-Allocation Register**
>
> | Item | Procurement Risk | Allocation | Contract Mechanism | Backup Plan | Decision Owner | Decision Date |
> |------|------------------|------------|-----|-----|----------------|---------------|
> | MV switchgear | Lead-time slip beyond 64 wk; price re-open post-PO | Contractor (lead-time) / Shared (price via Tariff Event clause) | Tariff Event clause executed 2026-04-22 | Siemens alt; descope to 7-sec | Maya Singh + Owner | 2026-06-15 |
> | Pad-mount xfmr | Lead-time slip beyond 82 wk; Oncor utility coord | Contractor (lead-time) / Owner (Oncor utility-side coord) | Cost-plus | Howard alt | Maya Singh + Damon Escobar | 2026-06-10 |
> | UPS | Lithium-string supply variability | Contractor (Vertiv absorbed) | Lump-sum | Eaton 9395 alt | Maya Singh | 2026-09-15 |
> | Genset | Price re-open at PO+90 days | Shared (Tariff Event clause covers tariff-only; non-tariff covered by cost-plus markup) | Cost-plus | MTU / CAT alt | Maya Singh | 2026-07-29 |
> | Paralleling gear | Lead-time slip; configuration drift | Contractor / Shared | Tariff Event clause | Russelectric alt | Maya Singh | 2026-07-22 |
> | ATS | Standard procurement risk | Contractor | Lump-sum | Russelectric alt | Maya Singh | 2027-01-10 |
> | Force-majeure manufacturer-delay clause | Contract silent | Currently Contractor (uncapped) | **Needs contract addendum** | None | Jorge Aldama + Damon Escobar | 2026-06-15 |
>
> ---
>
> **Contingency Plans**
>
> *MV switchgear:* If Eaton lead time slips past 64 weeks, Siemens at 60–66 weeks holds an alternate slot through 2026-07-15 (per Siemens quote SMS-9923, ref letter on file). Descope to a 7-section lineup (eliminate one tie breaker) saves 6–8 weeks but requires EOR re-coordination of the redundancy scheme. Refurbished draw-out gear in this voltage class is not realistic for new Tier III spec. Factory-witness-test expedite available from Eaton at 4% surcharge; not yet quoted from Siemens.
>
> *Pad-mount distribution transformer:* Howard Industries alternate at 70–88 weeks; both Cooper and Howard are on the Oncor approved-vendor list. MGM is not on the Oncor list and would require utility approval (typical 90-day review). Configuration changes (NEMA 4X instead of 3R, lower temperature rise) do not meaningfully shorten lead time in this category. No refurbished option for new Oncor spec. Order is highly time-sensitive.
>
> *Static UPS:* Eaton 9395 alternate at 32–36 weeks; comparable lithium-string supply window. Configuration descope to VRLA-string saves 6 weeks but increases room footprint by ~15% and floor loading by ~30% — coordinate with structural and architectural before considering.
>
> *Generator sets:* MTU 16V4000 G84 at 50–58 weeks; Caterpillar 3516B at 52–60 weeks. T4-Final certification required by Prosper AHJ (and by Oncor parallel-interconnection agreement); T4i is not permitted. Weekend-shift expedite available from Cummins at 6% surcharge.
>
> *Paralleling gear:* Russelectric alternate at 56–64 weeks. Descope to 4-engine paralleling (from 6) changes the redundancy class — requires owner approval and may impact Tier III certification.
>
> *ATS:* Russelectric 28–34 weeks alternate. Downsizing to 3200 A would require engineered re-coordination with the upstream OCPD scheme; not recommended without EOR sign-off.
>
> ---
>
> **Tariff-Exposure Summary**
>
> Two items flowing (Eaton MV switchgear has 15% §232 finished-equipment duty absorbed in EVS-7714; Vertiv UPS has duty absorbed in VTV-1144). Four items pending re-price at PO release (pad-mount transformers, paralleling gear, gensets, ATS) — all four covered by the Tariff Event clause executed 2026-04-22, with a documented-pass-through cost-recovery mechanism at agreed 12% markup. The Commerce Secretary's June 30, 2026 report on refined copper may trigger an additional 15% duty starting January 1, 2027 (rising to 30% January 1, 2028) under the proclamation as published; the pad-mount transformer delivery window (2027-Q1) and the genset delivery (2027-Q2) both land in the 15% trigger window. Add a Tariff Event clause exposure line to the contingency reserve at approximately $180K (refined-copper differential on transformer + paralleling gear + bus duct estimated content).
>
> See `knowledge-base/regulations/material-tariffs-2026.md` §1–§4 for the regime detail and the clause framework.
>
> ---
>
> **NEC 2026 Equipment Configuration Notes**
>
> Energization 2027-09-30 will land in Prosper post-Texas-state 2026 NEC adoption (TX state effective 2026-09-01; Prosper municipal adoption confirmation pending — verify with Prosper AHJ in 2026-Q4 before final submittal). Cycle-driven configuration items to confirm in the equipment spec:
>
> - **§230.70(B)(2) outdoor service disconnect:** The pad-mount transformer / utility-side service equipment scope must include the outdoor "EMERGENCY DISCONNECT" labeled disconnect in ½-inch white letters on red background. Confirm Cooper Power configuration includes the disconnect provision or specify a separately mounted enclosure.
> - **§706 / §705.20 ESS disconnect marking:** If BESS is added to halls 3 & 4 in the Phase 2 scope, the ESS disconnect provisions and "ENERGY STORAGE SYSTEM DISCONNECT" marking apply. Not in Phase 1 scope; flag for Phase 2 procurement plan.
> - **§110.16(B) arc-flash assessment date:** All factory-applied arc-flash labels on the switchgear and paralleling gear must include the assessment date per the 2026 cycle. Confirm Eaton and ASCO label specs match — both vendors have 2026-cycle-aware label templates as of Q2 2026.
> - **§110.26 working-space:** No change in the equipment configuration but confirm the electrical-room layout shows 2026-cycle clearances; coordinate with EOR.
>
> ---
>
> **GC / Owner-Facing One-Pager**
>
> Subject: Meridian DC-09 Phase 1 — Long-Lead Equipment Decisions Needed by June 15
>
> Mike (Vanguard Constructors) and the Meridian Datacom team:
>
> We've completed the procurement plan for the 24 MW Phase 1 of DC-09. Energization target 2027-09-30 is achievable, but two long-lead items need a release decision in the next three weeks to hold it:
>
> 1. **MV switchgear (2 lineups, 15 kV).** Eaton has held a build slot for us with a 56–64 week lead time. We need Meridian's confirmation of Eaton vs Siemens by June 15. Siemens is the qualified alternate and would slide energization 4–6 weeks. Estimated PO value $8M, 15% deposit at release ($1.2M).
>
> 2. **Pad-mount distribution transformers (4 units, 2.5 MVA, Oncor-spec).** Cooper Power has quoted 64–82 weeks. This is our tightest item. We need to release the PO by June 10. Estimated PO value $4.2M, 10% deposit at release ($420K). Howard is the qualified alternate with similar lead time.
>
> A third decision: the contract currently has a robust Tariff Event clause (executed April 22) but no force-majeure language specifically covering manufacturer delay. The Tariff Event clause protects against price movement but not against, for example, a transformer-factory fire or a steel-strike production halt. We'd like to add a short addendum covering manufacturer-delay schedule relief; happy to draft and have your counsel review.
>
> Available for a 30-minute call this week or next to walk through the table and confirm the release sequence. Maya (procurement lead) can join.
>
> — Damon Escobar, VP Pre-construction, Lone Star Power Systems
>
> ---
>
> **Internal Notes**
> - All Tier 1 release dates carry ± 2-week confidence bands based on Lone Star's prior 3 DC procurements (AWS Phoenix, QTS Fort Worth, Digital Realty Garland — actuals on file in PMIS).
> - Eaton quote EVS-7714 has a 30-day price hold from PO release; verify with Eaton sales rep on day of PO that the 30-day window is still in force before deposit wire.
> - Oncor approved-vendor list current as of 2026-04-01 (Cooper, Howard, ABB); MGM filed for approval 2025-Q4, status pending. Confirm before assuming MGM as an alternate.
> - LD exposure of $25K/day × cap $2.5M = 100-day cap. The 4–6-week Siemens delay (if chosen) lands inside the cap. The pad-mount transformer slip (if Cooper falls past 82 weeks) is a more direct LD-trigger risk.
> - GC progress-payment arrangement for the third Tier 1/2 deposit (genset) is pending — Maya to confirm with Vanguard's PM before 2026-07-22.
> - Force-majeure manufacturer-delay clause: route the addendum draft through `skills/admin/contract-risk-reviewer.md` and then to Lone Star's general counsel before sending to Vanguard.
> - Phase 2 procurement plan (halls 3 & 4 + BESS) to be initiated 2026-Q3; pull `skills/sales/bess-residential-pitch.md` voice for the BESS-spec section though the install is commercial scale; primary skill remains this one.
> - Sources for non-quote lead-time bands: `knowledge-base/regulations/material-tariffs-2026.md` §2 (Q2 2026 snapshot — verify before relying on any specific row).
