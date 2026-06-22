# Material Tariffs & Lead-Time Volatility — 2026

**Scope:** Practical reference for an electrical contractor pricing or negotiating any project — residential service work, commercial TI, public works, data-center build, or industrial retrofit — under the post-April-2026 Section 232 tariff regime. Pulls together the federal trade driver (Section 232 on steel, aluminum, copper, and metal-intensive electrical equipment), the cascading lead-time effect on switchgear / panelboards / transformers, and the contract-language framework an electrical contractor can use to keep margin from leaking out the back of a quote.

**Last updated in this repo:** 2026-06-22 (landscape monitor — added §1.4 for the June 1, 2026 follow-on proclamation effective June 8, 2026: expanded 15% category, broadened derivative coverage, U.S.-content threshold lowered 95% → 85%). Prior update 2026-04-27 (Section 232 restructure effective April 6, 2026; Q1 2026 input-price run-up; switchgear / panelboard / transformer lead-time snapshot).

This document is a working summary, not legal or tax advice. Verify the duty rate for any specific HTS code with a customs broker, and have any contract clause drafted from this framework reviewed by a construction attorney before sending it to a customer.

---

## 1. Section 232 — The April 6, 2026 Restructure

### 1.1 What changed

A presidential proclamation signed April 2, 2026 restructured the Section 232 duty regime on steel, aluminum, and copper. The new rates took effect April 6, 2026 and apply to entries on or after that date. The structure is tiered by how much of the imported article is metal:

- **50%** on items made almost entirely of steel, aluminum, or copper (raw rod, wire, sheet, ingot, billet).
- **25%** on derivative products that are *primarily* steel / aluminum / copper but contain other materials (formed conduit, certain wireway, pre-drawn raceway components).
- **15%** on metal-intensive electrical equipment that incorporates these metals as a major input — explicitly including transformers, panelboards, switchboards, switchgear, conduit-and-wireway systems, busway, and motor control centers. The 15% rate is in force through the end of 2027 under the proclamation as published.
- **10%** on products made abroad using American-sourced metal (rebated tier — verify with broker).

### 1.2 What this means for an electrical contractor

A 50% duty on copper-rod input cascades into the wholesale price of every wire and cable item the contractor buys. A 15% duty on a finished panelboard or switchgear lineup adds directly onto the manufacturer's quoted price at the supply house. The published Q1 2026 input-price run-ups — copper +80% YoY, switchgear +67%, iron and steel +58% — preceded the April 6 restructure; the April 6 rates *added* on top of the already-elevated baseline rather than resetting it.

### 1.3 Sunset / extension status

The 15% finished-equipment tier is published with a 2027 sunset date. Do not assume it expires on time. Do not assume it will be extended. Contract language should accommodate either outcome.

### 1.4 June 8, 2026 refinement — what an estimator needs to know

A follow-on proclamation issued June 1, 2026 (entries on or after June 8, 2026) tuned the April framework without tearing it down. The 50% / 25% / 15% tier structure and the end-of-2027 sunset on the 15% relief tier are unchanged. Three adjustments matter when pricing imported electrical gear:

- **The 15% category got bigger.** More categories of finished industrial equipment were pulled into the 15% relief tier, and derivative-product coverage was broadened. The practical read for a contractor: more of the imported equipment on a typical Division 26 BoM is now explicitly inside the §232 net rather than in a gray zone — so a "no tariff applies" assumption on a borderline item is now riskier than it was in April.
- **The U.S.-content threshold dropped from 95% to 85%.** The carve-out for product made abroad from American-sourced metal now triggers at 85% domestic content instead of 95%. That is a modest *loosening* — a few more imported items may qualify for the reduced (rebated) treatment — but it is the supplier's classification call, not the contractor's. Treat any rebated-tier price as something the supply house must substantiate, not something to assume into a bid.
- **Classification, not product use, sets the rate.** Duty follows the item's HTS subheading and its assigned annex (Annex I-A = 50%, I-B = 25%, I-C = modified, II = excluded, III = 15% through 2027, IV = the content/scope rules), and CBP applies the duty to the full entered value. Two near-identical fittings can land in different tiers on classification alone. This is why the escalation framework in §4 keys to *categories the contractor buys* and to a documented supplier pass-through, not to a single assumed rate.

Net effect on the margin math in §3: the exposure direction is unchanged and the tier rates are unchanged; what shifted is that the dutiable scope is now broader and harder to predict line-by-line. That strengthens, not weakens, the case for a Tariff Event escalation clause on any quote with imported switchgear, panelboards, transformers, or copper-heavy feeder.

---

## 2. Lead-Time Snapshot — Q2 2026

These ranges are widely reported across distributor and manufacturer sources as of April 2026. They are not committed delivery dates from any single vendor; verify with the actual supplier on every quote.

| Equipment | Pre-COVID baseline | Q2 2026 typical | Notes |
|-----------|--------------------|-----------------|-------|
| Medium-voltage switchgear | 12–16 weeks | **26–32 weeks** | Some configurations exceed 40 weeks |
| Panelboards | 4–6 weeks | **28–48 weeks** | Wide range driven by configuration and manufacturer backlog |
| Distribution transformers (pad / pole) | 6–12 weeks | **52–104 weeks** | Utility-side coordination required on many projects |
| Bussed switchboards (commercial) | 8–12 weeks | **24–36 weeks** | |
| Standard NEMA 3R load centers | 1–2 weeks | **6–14 weeks** | |
| #4/0 AL service-entrance cable | In stock | **In stock or 1–4 weeks** | Price has moved more than lead time |
| #12 / #10 / #8 building wire | In stock | **In stock** | Price-driven, not availability-driven |

The cascading effect: a project waiting on switchgear cannot be energized regardless of how much wire and conduit is on site. Lead-time risk has shifted from a procurement nuisance to a project-completion risk.

---

## 3. Margin-Erosion Math

A typical Division 26 commercial bid is 35–55% material by cost. On a $1.0M sub-bid that's $350K–$550K of material exposure. A 15% post-quote price move on the panelboards / switchgear / transformers tier alone is $30K–$50K of margin loss before any wire / conduit / fittings escalation. On a copper-heavy job (long feeder runs, parallel sets, large MV cable) the wire-side exposure on top of that can be another 5–10% of total project cost.

A 90-day quote-to-PO gap with no escalation language transfers that risk entirely onto the contractor.

---

## 4. Contract-Language Framework (Tariff-Specific Escalation)

An electrical contractor's escalation clause in 2026 should do four things that a generic "material price changes" clause does not:

### 4.1 Define "Tariff Event" specifically

A Tariff Event is any change after the proposal date in Section 232 duties (or any successor or replacement program); any change in Section 301 China tariffs; any imposition of a new IEEPA-based duty; or any change in HTS classification that materially affects a product on the bill of materials. Generic "tariff" without a section reference is too easy for a customer's procurement department to read narrowly.

### 4.2 Define affected materials by category, not by SKU

The list should reference *categories* the contractor actually buys (panelboards / switchgear / switchboards / transformers / busway / MCCs / building wire / cable / conduit / fittings) rather than specific manufacturer part numbers. Manufacturer part numbers change between proposal and PO; categories don't.

### 4.3 Specify the cost-recovery mechanism

Three options, in order of buyer-acceptance ease:

- **Documented pass-through** — contractor submits a change order with the supplier's invoice or quote showing the pre- and post-event price; owner pays the delta plus an agreed mark-up (often 10–15%). Cleanest. Buyers accept this most often.
- **Index-linked adjustment** — escalation is tied to a published index (PPI Industrial Electrical Equipment, BLS Copper Wire and Cable, ENR Building Materials Index for raw steel). Less paperwork at the change-order stage. Buyers may push back on which index governs.
- **Fixed-percentage cap with shared overage** — first 5–10% of price movement absorbed by the contractor; movement above that split 50/50 with the owner. Useful on hard-bid public works where a pure pass-through isn't acceptable.

### 4.4 Specify the schedule-relief tail

A tariff-driven price escalation often arrives at the same time as a tariff-driven lead-time blowout. The clause should explicitly grant a non-compensable time extension equal to the *actual* delivery delay caused by the event, not just a price adjustment. Otherwise the contractor pays liquidated damages on a delay it didn't cause.

### 4.5 Notice and substantiation

Most owners will require the contractor to give written notice within a defined window (commonly 7–14 calendar days of the contractor's awareness) and substantiate with supplier documentation. Build that habit into the proposal *before* the contract gets signed; it makes the change-order conversation faster later.

---

## 5. Pitch-Side Honesty Rules

When a contractor talks about tariffs in a customer-facing pitch or proposal, three rules keep the contractor on the right side of the conversation:

1. **No fabricated tariff figure.** The published rates are 50% / 25% / 15% / 10% by tier as of April 6, 2026. Do not use a "20% surcharge" that doesn't map to a tier the customer can verify.
2. **No tariff-on-everything claim.** Building wire is a copper-input pass-through; receptacles are not tariff-affected at the same rate. Be specific about which BoM line items are sensitive.
3. **No "we'll absorb the increase" promise without a mechanism.** Either the contractor builds enough contingency into the bid, or the contract has an escalation clause, or the contractor will eat margin. There is no fourth option.

---

## 6. Cross-Reference for Skill Outputs

Skills in this repo that should pull this KB doc into output:

- `skills/admin/material-tariff-escalation-clause-drafter.md` — primary user; pulls §1, §2, §4 in full.
- `skills/sales/bid-summary-writer.md` — pull §4.3 (cost-recovery mechanism) and the lead-time snapshot in §2 when the bid contains a panelboard, switchgear, or transformer line item.
- `skills/sales/scope-letter-drafter.md` — pull §4.4 (schedule-relief tail) when the post-award scope letter is being drafted with a long-lead item still pending.
- `skills/sales/data-center-commercial-pitch.md` — pull the §2 lead-time snapshot (transformers especially) for the labor-mobilization plan; pull §1 for the cover letter risk-discussion paragraph.
- `skills/sales/commercial-lighting-retrofit-pitch.md` — pull §2 lead-time snapshot for fixture and controls availability when the controls platform uses long-lead components (e.g., custom OEM panels).
- `skills/admin/contract-risk-reviewer.md` — pull §4 in full when reviewing a customer-supplied subcontract or PO that does not contain a tariff-aware escalation clause.
- `skills/admin/change-order-drafter.md` — pull §4.5 (notice and substantiation) when drafting a Tariff Event change order.

---

## 7. Source Trail

Federal sources: U.S. Customs and Border Protection (Section 232 entry instructions); KPMG TaxNewsFlash (Section 232 calculation rules update, April 2026); Perkins Coie (Restructured and Additional Section 232 Tariffs); Plante Moran (Section 232 changes and IEEPA refund status). Industry-impact sources: Construction Owners Association (Construction Tariffs 2026); Utility Dive (Tariffs drove construction input prices up to start 2026); ABC Carolinas (construction material tariff costs guide); STRIPMEISTER (How 2026 Copper Tariffs Are Changing the Math for Electrical Contractors); Buildforce (How Do New Tariffs Affect Electrical Contractors); EC&M (electrical supply chain articles). Lead-time sources: Wood Mackenzie (T&D supply-chain analysis); Power Magazine (transformer shortage in 2026); BASE4 (developers' equipment-order timing); Building Congress & Exchange of Baltimore (project lead-time impact). Contract-language sources: Cohen Seglias (material cost escalation guidance); Maynard Nexsen (handling cost escalations due to tariffs); ConsensusDocs 200.1 (Time and Price Impacted Materials); Woods Aitken (six ways to manage material price escalation risk); Lexology (price escalation impacts).

Verify any specific figure or program rule with the actual source before quoting a number to a customer.
