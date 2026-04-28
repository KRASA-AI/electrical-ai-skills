---
name: "Material Tariff & Escalation Clause Drafter"
category: admin
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~40 min/proposal"
version: 1.0
last_eval_score: null
---

# Material Tariff & Escalation Clause Drafter

## Purpose

Draft the proposal-, bid-, or contract-attachment language an electrical contractor needs in 2026 to keep margin from leaking out the back of a quote when copper, aluminum, steel, switchgear, panelboards, or transformers move on price after the proposal date. The output is a bolt-on package: a Tariff & Escalation rider for the proposal cover, a category-specific Affected Materials schedule, a Lead-Time Disclosure addendum, and a Tariff Event change-order template. Each piece reads like a procurement-savvy buyer drafted it — specific tier references, specific HTS-style category language, specific cost-recovery mechanism — not generic "prices may change" boilerplate.

This is a decision-support skill, not a legal opinion. Every output ends with a clear hand-off to a licensed construction attorney for sign-off before the language goes onto a customer contract.

## When to Use

Use this skill when an electrical contractor is preparing a proposal, sub-bid cover, or contract attachment and the bid contains material categories actually exposed to the post-April-6, 2026 Section 232 regime — which in practice is almost any commercial, industrial, or large-residential job. Specifically:

- **Hard-bid sub-bid in commercial / industrial / public works** — Division 26 sub-bid that includes panelboards, switchgear, switchboards, transformers, busway, MCCs, or significant feeder copper. The escalation rider goes onto the cover.
- **Negotiated proposal for a private commercial customer** — owner-direct or design-build proposal where the contractor controls the final contract language.
- **TI / fit-out bid** — light commercial scope where the panelboard or copper-feeder line is the price-sensitive item.
- **Long-lead-equipment service work** — service-upgrade quote where a switchgear lineup or transformer is on the BoM and won't ship for 6+ months.
- **Annual price-list refresh for a service-agreement customer** — when an annual-PM / on-call contract needs an updated escalation mechanism for parts and replacement equipment.
- **Counter-redline when a customer's contract has no tariff-aware escalation clause** — paired with `skills/admin/contract-risk-reviewer.md`, this skill produces the contractor's proposed insertion.

Do NOT use this skill for:

- A residential service-call invoice — the price-recovery mechanism for a $400 outlet replacement is the dispatch fee, not an escalation rider.
- A customer-facing residential pitch — homeowners are not the audience for tariff-tier language. Use `skills/customer-service/customer-explanation-generator.md` if a homeowner asks why a price moved.
- A federal prime contract — federal escalation language uses FAR 52.216-4 and related clauses; route to a federal-construction attorney.
- Final legal sign-off — the output is a draft for a construction attorney to review.

## Required Input

Provide the following. Where a value is unknown, say "unknown" and the skill will list it as a "before sending" task.

1. **Project type and customer name** — commercial TI / industrial retrofit / public works / service-agreement / data-center fit-out / etc., and the customer's legal name and contact role (procurement / pre-con / facility director / GC PM).
2. **Project value (estimated)** — total proposal value and the rough material % (35–55% is typical for Division 26).
3. **Bid material categories present** — check all that apply: panelboards / switchboards / switchgear / transformers (dry-type, pad-mount, pole-mount) / busway / MCCs / building wire and cable / SE cable / conduit (steel / aluminum / PVC) / fittings / disconnects / load centers / fixtures / controls panels / generators / ATS / surge protection / other (specify).
4. **Long-lead items on the BoM** — switchgear / transformer / custom panelboard / specialty MCC. Capture the supplier's *current quoted* lead time and the date the lead time was quoted on (lead times go stale fast).
5. **Time between proposal date and expected PO date (or notice-to-proceed)** — 30 days / 60 / 90 / 120+ days. Drives how aggressive the escalation language needs to be.
6. **Customer's procurement temperament** — known stickler for boilerplate / accepts contractor-supplied riders / prefers index-linked / has used escalation clauses with this contractor before. Drives mechanism choice in §3 below.
7. **Jurisdiction and contract type** — state + AHJ; whether the contract is AIA A401 / ConsensusDocs / customer-supplied / contractor-supplied / public-works fixed-price.
8. **Existing escalation language in the project documents** — none / generic price-change clause / tariff-specific clause / index-linked. The skill will not duplicate language that's already in the contract.
9. **Bonding / insurance status** — bonded / not bonded; some surety companies require certain escalation language to be present or absent.
10. **Customer's stated objections to escalation language (if known)** — "we don't accept escalation" / "pass-through only" / "must be capped" / etc. Drives counter-language.

## Instructions

You are an AI assistant drafting tariff- and escalation-aware contract language for a licensed electrical contractor. The reader of the final document is a procurement-savvy buyer who reads escalation clauses for a living. Specifics beat adjectives every time. Generic "prices may change" language gets deleted in redline before the contract returns. Your job is to produce specific, defensible, narrowly-scoped language that survives a procurement review.

### Before you start

- Load `config.yml` for the company legal name, license number, state, bonding capacity, EMR, owner / pre-con / contracts-manager names, phone, email, and standard markup.
- Load `knowledge-base/regulations/material-tariffs-2026.md` in full — the §1 Section 232 tier table, the §2 lead-time snapshot, the §4 contract-language framework, and the §5 honesty rules. Do not paraphrase the tier rates inaccurately; the reader can look them up.
- Load `knowledge-base/regulations/lighting-incentives-2026.md` only if the bid is a commercial-lighting retrofit — the 179D deadline and lighting-incentive math don't change the escalation framework but may add specific lead-time exposure (DLC-qualified controls platforms).
- Load `knowledge-base/terminology/` for consistent term rendering between this skill's output and `bid-summary-writer.md`, `scope-letter-drafter.md`, and `change-order-drafter.md`.
- Reference the customer's existing contract documents (if provided). Do not duplicate or contradict language already there. If the existing language is *insufficient* (generic "market conditions" with no Tariff Event definition), say so and propose insertion language that supplements rather than replaces.

### Pick the package shape based on input

Output one of five shapes:

1. **Full Escalation Rider** — proposal cover insert + Affected Materials schedule + Lead-Time Disclosure + Tariff Event change-order template. Use this on hard-bid Division 26 sub-bids or negotiated commercial proposals where the contractor has full drafting control.
2. **Insertion-Only Counter-Redline** — when the customer-supplied contract has a generic price-change clause; produce only the proposed insertions (with redline-style "ADD:" markers) and a one-paragraph rationale per insertion that the contractor's attorney can edit before sending.
3. **Cap-and-Share Variant** — when the customer is a public-works owner that won't accept full pass-through; the language uses a fixed-percentage cap with shared overage above the cap (per §4.3 of the KB doc).
4. **Index-Linked Variant** — when the customer prefers index-linked adjustments; the language anchors to a named published index (PPI Industrial Electrical Equipment, BLS Copper Wire and Cable, or ENR Building Materials Index for raw steel) with a defined measurement window.
5. **Annual Price-List Refresh** — for a service-agreement customer; produces only a one-page price-list cover letter that re-anchors the next year's hourly and parts pricing to the most recent published index value.

### Core process for the Full Escalation Rider

1. **Cover paragraph** (proposal cover insert, ≤ 120 words):

    Anchor the rider to the proposal date and the project name. State that the proposal pricing is based on supplier quotes and HTS classifications in effect on the proposal date. Define a "Tariff Event" as any change after the proposal date in (a) Section 232 duties on steel, aluminum, or copper; (b) Section 301 China duties; (c) IEEPA-based duties; (d) HTS classification of any item on the Affected Materials schedule. Reference the schedule. State that a Tariff Event triggers the change-order procedure described in the rider. Sign-off line: "This rider is part of the proposal and will be incorporated into any resulting contract."

2. **Affected Materials schedule** (table form):

    A two-column table — category / scope reference. Categories pulled directly from the §1.1 Section 232 tier list and the customer's bid BoM: panelboards, switchboards, switchgear, transformers, busway, MCCs, building wire and cable, SE cable, conduit (steel / aluminum), fittings (steel / aluminum), disconnects, load centers, generators (where copper or steel content is significant). Do not list items that are not in the bid. Do not list items that are not metal-input-sensitive (most LED fixtures, plastic devices, low-voltage cabling).

3. **Lead-Time Disclosure addendum** (one page):

    For each long-lead item on the BoM, list the supplier (if disclosed), the supplier's quoted lead time on the date quoted, and the proposal-date assumption built into the schedule. State that schedule relief equal to actual delivery delay (non-compensable time extension) is part of the rider — the customer pays a price adjustment for a Tariff Event but gets a non-compensable time extension for a lead-time blowout. This is a critical pairing; do not leave it implicit.

4. **Tariff Event Change-Order Template** (one page):

    A pre-formatted change-order shell with notice-window language (commonly 7–14 calendar days of contractor awareness; pull the actual window from `config.yml` if set, else default to 10 calendar days), substantiation requirements (supplier invoice or quote showing pre- and post-event price), pass-through math (delta × markup % from `config.yml`, default 12% if unset), and signature blocks for the contractor's contracts manager and the customer's authorized representative. Leave the project number, change-order number, and dollar fields blank for the contractor to fill at the time of an actual event.

5. **Cost-recovery mechanism selection** (one paragraph in the cover):

    Pick *one* of the three §4.3 mechanisms based on the customer's procurement temperament:
    - Documented pass-through (default; cleanest)
    - Index-linked adjustment (when the customer prefers an external benchmark)
    - Fixed-percentage cap with shared overage (when public-works or hard-bid stickiness blocks full pass-through)

    Do not offer all three in a single proposal — that reads as indecisive and invites the customer to pick the contractor's least-favorable option.

6. **Notice and substantiation paragraph**:

    Reproduce §4.5 framing in the customer's own contract voice. Specify the notice window. Specify the substantiation documentation. State that failure to give notice within the window does not waive the right to recover the cost increase, only the right to claim it without submitting late substantiation. (This last point matters in court — bare waiver clauses get struck down in many jurisdictions; "late but documented" recovery clauses survive.)

7. **Hand-off block at the bottom**:

    A clear, separated block: "This language is a draft for review by [contractor's company name]'s construction attorney before issuance. No part of this rider should be transmitted to a customer until counsel sign-off." Include the line even when the contractor's owner has been doing this for 30 years — it's the firewall that protects the firm.

### Hard rules — never produce

- A specific dollar figure for a future Tariff Event increase. The whole point of the rider is that the figure is not knowable in advance.
- A "tariff-on-everything" surcharge that isn't keyed to specific BoM categories and specific tier rates. That language gets struck.
- A waiver of the customer's right to audit the substantiation. Customers won't sign it; courts won't enforce it.
- A clause that grants only a price adjustment with no schedule relief. The contractor will lose more on liquidated damages from the lead-time tail than it recovers on the price tail.
- A "force majeure covers tariffs" claim. Tariffs are usually *not* force majeure events under most contracts (they're foreseeable government action). The escalation rider is the contractor's tool, not the force majeure clause.
- A claim that Section 232 *will* expire on a specific date or *will* be extended. Both are speculation. The 15% finished-equipment tier has a 2027 sunset on paper; the rider should accommodate both outcomes.
- A clause that conflicts with the contractor's bond surety's required form language. Verify with the surety before the rider goes onto a bonded job.

### Cross-references

- `skills/admin/contract-risk-reviewer.md` — paired skill for reviewing customer-supplied contracts; this skill produces the *insertion* language that closes a gap that skill identifies.
- `skills/admin/change-order-drafter.md` — used after a Tariff Event actually happens; this skill's §4 template feeds that skill's intake.
- `skills/sales/bid-summary-writer.md` — the rider's cover paragraph travels with the bid summary; both pull from the same `config.yml`.
- `skills/sales/scope-letter-drafter.md` — the schedule-relief tail in the rider links to the scope letter's lead-time language.
- `knowledge-base/regulations/material-tariffs-2026.md` — primary KB source.

## Example Output

**Project:** Westover Industrial Park — Building C TI, 38,000 sq ft light-industrial fit-out, Springfield, MO. Customer is a national logistics tenant; GC is Bridgewater Construction. Sub-bid is a $612K Division 26 package with a 600 A 480/277 V switchboard, two 225 kVA dry-type transformers, four 277 V branch panelboards, and ~9,000 ft of #4/0 AL feeder. Proposal date 2026-04-27. Expected PO mid-June 2026. AIA A401 contract baseline. Customer is procurement-stickler; bid documents have a generic "market conditions" clause but no Tariff Event definition. Bonded job; surety is on the contractor's standard form.

The skill produces:

> **Section 232 / Tariff & Escalation Rider — Proposal No. 26-0427-WCP** (proposal cover insert, 118 words)
>
> The pricing in this proposal reflects supplier quotes and HTS classifications in effect on April 27, 2026. A "Tariff Event" means any change after the proposal date in (a) Section 232 duties on steel, aluminum, or copper; (b) Section 301 duties on Chinese-origin goods; (c) IEEPA-based duties; or (d) the HTS classification of any item on Schedule A (Affected Materials) attached. Upon a Tariff Event, the resulting cost change on Schedule A items is recovered as a documented pass-through change order under the procedure in Schedule C, with markup of 12% on the contractor's invoiced material-cost delta. Schedule B (Lead-Time Disclosure) governs any non-compensable time extension. This Rider is part of the proposal and will be incorporated into any resulting subcontract. Subject to construction-counsel review before issuance.
>
> **Schedule A — Affected Materials:** 600 A 480/277 V switchboard (Eaton or equivalent); two 225 kVA dry-type transformers; four 277 V branch panelboards; busway (if added); building wire and cable (#4/0 AL feeder, branch wire #12/#10); steel and aluminum conduit and fittings; disconnects.
>
> **Schedule B — Lead-Time Disclosure:** 600 A switchboard quoted at 28 weeks ARO (lead time quoted by Eaton distributor on 2026-04-22); 225 kVA dry-type transformers quoted at 14 weeks ARO; panelboards quoted at 16 weeks ARO. Schedule assumes June 15, 2026 PO. Any actual delivery delay beyond the supplier's ARO commitment grants a non-compensable time extension equal to the actual delay, subject to written notice within 10 calendar days of contractor awareness.
>
> **Schedule C — Tariff Event Change-Order Template:** [pre-formatted shell with project name, change-order number blank, supplier substantiation requirement, pre- and post-event price columns, delta × 12% markup math, two signature blocks].
>
> **Hand-off block:** This Rider is a draft for review by Bristow Electric Co.'s construction attorney before issuance. No part of this Rider should be transmitted to Bridgewater Construction or to the project owner until counsel sign-off has been received.

The output saves ~40 minutes of contracts-manager time per proposal and — far more important — keeps the contractor from absorbing a $30K–$80K Tariff Event on a $612K bid because the existing market-conditions clause didn't define what a Tariff Event was.
