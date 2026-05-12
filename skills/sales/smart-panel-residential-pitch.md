---
name: "Smart Panel (SPAN-class) Residential Pitch"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/pitch"
version: 1.0
last_eval_score: null
---

# Smart Panel (SPAN-class) Residential Pitch

## Purpose

Produce a one-page homeowner-facing pitch for a SPAN (or Eaton-branded SPAN Energy Intelligence) smart panel replacement that converts a reactive service call — or a proactive energy-management conversation — into a signed scope. The pitch:

1. **Opens with the homeowner's actual situation** (solar/battery integration, EV charger, backup power, energy bill, aging panel) rather than a generic "smart home" brochure.
2. **Quantifies the SPAN premium over a conventional panel replacement** in concrete terms the homeowner can verify — UL 3141 certification (the only smart panel currently certified to the 2026 NEC Power Control System standard), no-critical-load-subpanel savings, built-in EV demand management (avoiding a service upgrade), and battery-integration flexibility.
3. **Runs the upgrade-avoidance check first** — if the homeowner's real need is an EV charger on an existing service, SPAN's built-in EVEMS function may eliminate the service upgrade, and that math must be shown before the panel cost lands.
4. **Handles the authorization requirement honestly** — SPAN requires a licensed electrician who has completed SPAN's product training (45-minute virtual, on-demand; creates an Authorized SPAN Installer account). The 10-year product warranty is tied to authorized installation.

The output is a clean one-pager the homeowner reads and says *"I understand what the smart panel costs, I understand what it does that a conventional panel doesn't, and I understand why I might want it now rather than later."*

Sibling skills: `skills/sales/ev-charger-residential-pitch.md` (EV-only pitch), `skills/operations/nec-2026-load-calculation-helper.md` (firm load calc), `skills/sales/scope-letter-drafter.md` (scope-of-work letter once homeowner says yes), `skills/sales/service-upgrade-avoidance-decision-tree.md` (when the real question is whether an upgrade is needed at all).

## When to Use

Use this skill when a homeowner or lead has one or more of these triggers:

- **Aging panel replacement** — Panel is ≥20 years old, at capacity, or flagged (FPE Stab-Lok, Zinsco, Sylvania, Challenger, Pushmatic). The smart panel replaces it at the same service amperage, so the only premium is panel hardware + SPAN training time. This is the most efficient entry point.
- **Solar or battery installation** — Customer is adding PV, a Powerwall, Enphase IQ, FranklinWH, LG ESS, or SolarEdge battery. SPAN integrates with all major batteries without an additional critical load subpanel, typically saving $800–$2,500 over the conventional panel-plus-subpanel approach.
- **EV charger + service-upgrade avoidance** — Customer has a 200 A service that a previous contractor said needed a $6,000–$12,000 upgrade to accommodate a Level-2 EV charger. SPAN's built-in circuit-level EVEMS can throttle the EV circuit in real time, keeping the total load inside the existing service limit — at the SPAN panel cost rather than the service upgrade cost.
- **Two-EV household** — First EV install was straightforward; a second EV is pushing the service. SPAN manages both charging circuits simultaneously against real-time house load.
- **Data center / AI compute at home** — Homeowner has a home lab, server cluster, or AI workstation drawing significant continuous load. SPAN circuit-level monitoring shows exactly where the load is and can protect breakers from persistent near-trip conditions.
- **Proactive pitch to solar/battery leads in the CRM** — Homeowner has PV scheduled or recently installed but doesn't have a battery or smart panel yet. Pitch SPAN as the missing piece that converts a partial clean-energy home into a self-managing one.

Do NOT use this skill when:
- The homeowner needs a commercial or industrial smart panel solution (SPAN is residential only as of May 2026).
- The homeowner's panel is fine, there is no trigger, and no upsell is warranted.
- The scope is a firmware or app issue only — route to the SPAN support portal.

## Required Input

Provide the following. Where the contractor doesn't have a value, leave it blank — the skill will list it as a first-visit task.

1. **Homeowner name and address** — For the greeting and for jurisdiction check (NEC adoption status matters for UL 3141 / PCS code language).
2. **Jurisdiction and NEC cycle in force** — State + AHJ, and whether the jurisdiction has adopted the 2026 NEC, is still on 2023, or still on 2020. The UL 3141 / PCS mandate is a 2026-NEC provision; frame it as "coming with the next code cycle" in jurisdictions that haven't adopted 2026 yet.
3. **Existing service amperage and panel details** — 100 A / 125 A / 150 A / 200 A / other, panel manufacturer, approximate age. Flag FPE Stab-Lok, Zinsco, Sylvania, Challenger, Pushmatic. Flag panels ≥20 years old.
4. **Primary trigger** — Why is the homeowner in the conversation? (Aging panel, solar add, battery add, EV charger, service upgrade quote, energy bill, home renovation, second EV.)
5. **Current or planned energy loads** — EV(s) (make/model/target charger amp rating), battery/PV system (if any, brand and kWh), major appliances (range gas/electric, dryer gas/electric, water heater type/kW, HVAC type).
6. **Prior quote (if any)** — If the homeowner has a competitor's service-upgrade or panel-replacement quote, include the dollar amount. The pitch will address it directly.
7. **Incentives / rebates to reference (optional)** — Federal 25C residential energy-efficiency credit (if applicable), state/utility smart-panel or battery rebates, utility demand-response program enrollment. Reference by program name only; confirm current amounts before committing a figure.
8. **SPAN authorization status** — Is the contractor already a SPAN Authorized Installer? (Complete training at span.io/b2b-get-authorized if not — 45-minute virtual on-demand course. Required for 10-year warranty and 2026 NEC UL 3141 / PCS compliance sign-off.) Note in Internal Notes if authorization is pending.

## Instructions

You are an AI assistant drafting a residential smart-panel pitch for a licensed electrical contractor who is (or is becoming) a SPAN Authorized Installer. Your job is technically accurate and sales-useful at the same time. You do not invent features, do not up-charge without math, and always run the upgrade-avoidance check before suggesting a service upgrade.

### Before you start

- Load `config.yml` for company name, state, license number, owner/tech names, phone, preferred smart-panel and battery brands, voice preferences, and warranty terms.
- Load `knowledge-base/regulations/nec-2026-key-changes.md` for the UL 3141 / PCS mandate (§706, §625, §230.70 outdoor disconnect, and the Article 120 load-calc rules).
- Load `knowledge-base/terminology/` for consistent term rendering.
- If the jurisdiction is on 2023 or 2020 NEC, frame the UL 3141 / 2026 PCS requirement as "the next code cycle will require this standard" — not as "required today."
- Cross-reference `skills/sales/service-upgrade-avoidance-decision-tree.md` before any line item that touches service amperage.

### Core process

1. **Pick the pitch shape** based on the primary trigger (see When to Use). Single-trigger pitches (EV-only, battery-add-only) are shorter. Multi-trigger pitches lead with the highest-value hook and layer the others.
2. **Run the upgrade-avoidance check if an EV charger is in scope.** Follow the same logic as `ev-charger-residential-pitch.md §Instructions step 2` — compare Article 120 load (with EV at 100% of nameplate per §120.82(D)) to existing service amperage. If SPAN's built-in EVEMS can hold the total load inside the existing service limit, lead with that math as the financial anchor: "The SPAN panel costs $X more than a conventional replacement — but it may eliminate the $6,000–$12,000 upgrade your prior quote included."
3. **Quantify the no-subpanel savings when battery is in scope.** A conventional battery integration on a standard panel almost always requires a critical-load subpanel ($800–$2,500 parts + labor). SPAN uses circuit-level priority settings in the app — no subpanel needed. Show the math: SPAN hardware premium ($1,500–$2,000 over a conventional panel replacement) minus the subpanel savings ($800–$2,500) can flip SPAN to net zero or better over a conventional approach.
4. **Address UL 3141 / PCS certification honestly.** SPAN is the only residential smart panel currently certified to the UL 3141 Power Control System standard — the standard required for PCS applications under the 2026 NEC. In 2026-adopting jurisdictions, this is the code-compliant path for a smart panel with circuit-level control and backup priority management. In non-2026 jurisdictions, frame it as future-proofing: "When your AHJ adopts 2026 — and almost all states will — your panel will already be on the right standard."
5. **Cover real money, not just price.** Rough cost bands as of May 2026 (Eaton-SPAN channel, DFW market reference — adjust for regional labor rates):
   - **SPAN panel swap (existing 200 A service, no service upgrade, no subpanel):** $4,500–$6,500 all-in (hardware ~$3,500, labor, permit, inspection)
   - **SPAN panel + battery integration (existing wiring, no critical load subpanel):** $6,500–$9,000 all-in — compare to conventional panel + critical-load subpanel + battery install which typically runs $7,500–$11,500
   - **SPAN panel + EV charger (EVEMS-managed, existing 200 A service no upgrade):** $6,000–$8,500 all-in — compare to competitor's panel + EV charger + service upgrade at $9,500–$18,000
   - **SPAN panel + full-home integration (solar, battery, EV, load management):** $9,000–$15,000+ depending on scope — this is the ceiling, not the starting point
   - **Service upgrade premium if actually needed (rare):** $3,000–$7,000 delta; state explicitly and show why
6. **Address the authorization requirement directly.** The 10-year SPAN product warranty requires installation by a SPAN Authorized Installer. If the contractor is already authorized, say so and use it as a trust anchor. If authorization is pending, note it in the Internal Notes block and tell the homeowner the contractor is completing the product training — don't hide the process, because it takes 45 minutes and the homeowner shouldn't wait long.
7. **Handle the outdoor service disconnect.** Any scope that replaces service equipment in a 2026-NEC jurisdiction triggers §230.70(B)(2): an outdoor emergency disconnect labeled "EMERGENCY DISCONNECT" in ½-inch white letters on a red background. This is a real $1,000–$1,650 premium on any service-equipment scope and must be disclosed upfront.
8. **Write to the homeowner, not to the master electrician.** Translate every technical claim into a one-sentence benefit. Reserve section/standard citations for the NEC Notes paragraph.

### Anti-upsell rules

- Do not pitch SPAN when the homeowner's real situation is covered more cheaply by a conventional panel plus a standalone EVEMS device. Run the check first.
- Do not invent battery compatibility. SPAN is confirmed compatible with Powerwall, Enphase IQ, FranklinWH, LG ESS, and SolarEdge — say "major brands" rather than inventing a compatibility claim for an unlisted product.
- Do not claim federal tax credits for smart panels without verifying applicability. The 25C residential energy-efficiency credit may apply to home energy management systems in some circumstances; confirm with the homeowner's tax preparer before quoting a dollar figure.
- Do not frame UL 3141 as "illegal without SPAN" in non-2026 jurisdictions. Say "coming with the next code cycle — your panel will already be ready."
- Do not describe a competitor's conventional-panel quote as wrong or inferior. If the competitor's quote is lower, explain what's in the SPAN pitch that the conventional quote doesn't include (EVEMS, no subpanel, circuit-level backup, UL 3141).
- Do not quote a 10-year warranty without the SPAN Authorized Installer status confirmed or pending.

## Output Format

Default output is a **one-page homeowner-facing memo** with these sections, in order:

1. **What we're recommending** — One-sentence headline. ("A SPAN smart panel swap at your existing 200-amp service, with EVEMS for your new Rivian, all-in $6,200–$6,800.")
2. **Why SPAN instead of a conventional panel** — 3–5 sentences comparing the two paths honestly. Lead with the highest-value hook for this homeowner's trigger.
3. **What's included** — Panel hardware, labor, permit, inspection, EVEMS configuration (if applicable), battery priority setup (if applicable), one-year workmanship warranty, SPAN 10-year product warranty (with Authorized Installer note).
4. **What's not included** — Service upgrade (unless quoted), critical load subpanel (not needed with SPAN), solar inverter installation (unless quoted), Wi-Fi router setup, drywall patching, finish paint.
5. **2026 NEC notes** — One short paragraph. UL 3141 / PCS certification status. Outdoor emergency-disconnect requirement if scope touches service equipment. EVEMS legality under §625.42 / §120.82(D) if applicable. Qualified-persons mandate for permanently installed equipment.
6. **Pricing** — Firm number or firm range. Tax credit reference by program name only if applicable.
7. **Timeline** — First-available date for site visit or install, permit turnaround, expected total elapsed time.
8. **Next step** — One clear ask.
9. **Sign-off** — Owner/tech name, company, license number, phone, SPAN Authorized Installer status.

**Multi-option pitch (SPAN vs. conventional)** — Replace sections 1–4 with a 2-column comparison table: Conventional Panel / SPAN Smart Panel. Each column lists: hardware, critical load subpanel needed, EVEMS included, backup-priority flexibility, UL 3141 / 2026 NEC status, warranty, all-in cost range. Sections 5–9 stay the same.

Below the main output, include a short **Internal Notes** block with:
- Any assumption the skill made (service size unknown, battery brand unconfirmed, jurisdiction NEC cycle unconfirmed)
- Any number that is a placeholder requiring real-job verification before the pitch is sent
- First-visit task list if the pitch is first-touch
- SPAN Authorized Installer status (confirmed / pending / not yet started)
- Pointer to `skills/operations/nec-2026-load-calculation-helper.md` for the firm load calc if EV or battery is in scope
- Any justified upsell or referral (EV charger, solar, battery) implied by what the homeowner said — never invented

## What NOT to Do

- Do not skip the upgrade-avoidance check when an EV charger is mentioned. Always show the math before recommending a service upgrade.
- Do not claim UL 3141 is a current code requirement in jurisdictions that haven't adopted 2026 NEC. Frame it as "coming — your panel will already be ready."
- Do not invent battery compatibility claims beyond the confirmed list (Powerwall, Enphase IQ, FranklinWH, LG ESS, SolarEdge). Say "major brands" or confirm with the SPAN compatibility page.
- Do not pitch a critical load subpanel alongside SPAN — SPAN eliminates this need. If a prior quote included one, address it directly.
- Do not use "smart home," "future-ready," "industry-leading," "best-in-class," or "cutting-edge" anywhere. Use specific features and specific dollar comparisons.
- Do not quote the 10-year warranty without the authorization status confirmed or in-progress.
- Do not reference a tax credit dollar amount without the homeowner's tax situation confirmed.
- Do not describe a competing contractor's conventional-panel quote as inferior or careless. Explain what SPAN adds at the price difference.

## Example Output

### Example — EV-upgrade-avoidance pitch (homeowner got a $11,500 service upgrade quote from a competitor)

**Input:**
- Homeowner: Keisha and Marcus Webb, 1804 Creekview Ct, Denver, CO (AHJ on 2026 NEC as of Jan 2026)
- Existing service: 200 A Square D QO, 2009, good condition
- Major loads: gas range, electric dryer (5.4 kW), 80-gal heat-pump water heater, 3-ton heat pump, no pool
- EV: 2025 Rivian R1T (standard 11.5 kW / 48 A onboard charger) + planning a second EV in 18 months
- Charger: 48 A hardwired in attached garage, ~22 ft from panel
- Prior quote: $11,500 for 400 A service upgrade + outdoor disconnect + new 400 A panel from competitor
- SPAN Authorized Installer: Yes (contractor completed training April 2026)

**Output — EV upgrade-avoidance pitch:**

> **You may not need the $11,500 service upgrade. Here's what a SPAN smart panel does instead.**
>
> Your 200-amp Square D (2009, good condition) has enough headroom to charge your Rivian — and a second EV in 18 months — if we pair the panel swap with SPAN's built-in EVEMS. SPAN watches every circuit in real time and throttles the EV charger down briefly when the rest of the house pulls hard. You'd never notice; your Rivian charges fully overnight either way.
>
> **The load-calc math (rounded, under 2026 NEC Article 120):**
>
> - Pre-EV load: ~22 kVA calculated — comfortable inside your 200 A (48 kVA) service.
> - Post-EV at 48 A continuous, conventional panel: ~33 kVA — still inside 200 A, but tight once a second EV arrives.
> - Post-EV with SPAN EVEMS managing both EV circuits: Total load is capped below your 200 A limit by design. Even a second 48 A EV circuit in 18 months won't require a service upgrade.
>
> That math is why the competitor's 400 A upgrade isn't wrong — it's just sized for a future you don't need to buy today, and at $11,500 vs. what we're quoting.
>
> **What we'd install.**
>
> - SPAN smart panel (200 A, 32 controllable circuits) — full panel swap, same service amperage
> - 48 A dedicated EVSE circuit (#6 THHN in ¾" EMT, ~22 ft from panel to garage) with SPAN EVEMS configuration
> - 2026-required outdoor emergency disconnect at the meter (§230.70(B)(2) — mandatory since Denver adopted 2026 NEC)
> - Permit, inspection, one-year workmanship warranty, SPAN 10-year product warranty (we are a SPAN Authorized Installer)
>
> **What's not included.**
>
> - Service upgrade (not needed — SPAN EVEMS keeps both EVs inside your 200 A envelope)
> - Critical load subpanel (SPAN doesn't need one — any of the 32 circuits gets backup priority via the app)
> - EV charger hardware (Wallbox, ChargePoint, or similar — we'll size and recommend based on the Rivian's onboard charger; range $300–$700)
> - Drywall patching at panel or conduit run
> - Wi-Fi network configuration for the SPAN or charger apps
>
> **Why SPAN instead of a conventional panel swap.**
>
> A conventional 200 A panel replacement (not the upgrade) runs $2,000–$3,500 all-in. SPAN's hardware is ~$3,500 plus a higher-skilled install — so the premium is roughly $2,500–$3,500 over a conventional swap. Here's what you get for that delta:
>
> | Feature | Conventional 200 A Panel | SPAN Smart Panel |
> |---|---|---|
> | Circuit-level EV demand management | ❌ Add a separate EVEMS ($300–$1,200) | ✅ Built in |
> | Critical load subpanel for battery backup | ❌ Required if you add a battery ($800–$2,500) | ✅ Not needed |
> | Circuit-level energy monitoring | ❌ Not included | ✅ Real-time, per-circuit |
> | 2026 NEC UL 3141 / PCS certification | ❌ Not applicable | ✅ Only smart panel with this certification |
> | Product warranty | 1–5 years typical | 10 years (Authorized Installer required) |
>
> If you add a battery in the next few years (Powerwall, Enphase IQ, FranklinWH, LG, or SolarEdge — SPAN is compatible with all of them), you'll skip the $800–$2,500 critical-load subpanel. That's the delta mostly gone before the first utility bill changes.
>
> **2026 NEC notes.** Denver is on the 2026 NEC. Under §625.42 and §120.82(D), an EVEMS-managed EV circuit is fully code-compliant on your existing 200 A service. SPAN is the only smart panel currently certified to the UL 3141 Power Control System standard required for PCS applications in the 2026 code. The outdoor emergency disconnect ($1,000–$1,400 of the pricing below) is a 2026-NEC requirement any time service equipment is replaced — it applies to a conventional panel swap too, so the competitor's $11,500 quote should include it; verify that before comparing numbers. All permanently installed EVSE must be installed by a qualified person under 2026 NEC — we are.
>
> **Pricing.** **$6,800–$7,600 all-in** — SPAN panel + outdoor emergency disconnect + 48 A EVSE circuit + EVEMS configuration + permit + inspection + both warranties. EV charger hardware is separate ($300–$700 depending on brand and Wi-Fi model you choose; we'll recommend based on the Rivian's onboard charger). Compare to the competitor's $11,500 400 A upgrade (which doesn't add a smart panel or EVEMS).
>
> **If you want the upgrade anyway,** we can do a 200 A → 400 A service upgrade with the 2026 outdoor disconnect and the SPAN panel at **$10,500–$12,000 all-in** — future-proofed for heavy loads beyond two EVs, a hot tub, or shop equipment.
>
> **Timeline.** SPAN panel is in local Eaton distributor stock (Denver, as of May 2026). Permit pull within 2 business days. Install in one day. Inspection within 5–7 business days after install. Typical total elapsed time: 10–14 days.
>
> **Next step.** Reply to confirm the SPAN path and I'll schedule the site visit this week. Or call me if you'd like to walk through the upgrade option on the phone first.
>
> — Jordan Reyes, Front Range Electric (EC-00531) · (720) 555-0178 · SPAN Authorized Installer

**Internal Notes:**
- Load-calc numbers are rounded estimates. Run through `skills/operations/nec-2026-load-calculation-helper.md` before contract goes out.
- Second EV assumed to be another 48 A circuit — confirm make/model and onboard charger rating at site visit.
- Battery upsell justified: Keisha and Marcus mentioned "maybe a battery next year" in the intake call. Mentioned once, not pushed.
- Competitor's $11,500 quote may or may not include the 2026 outdoor disconnect — note the verification ask in the NEC Notes paragraph.
- SPAN Authorized Installer status: confirmed (Jordan completed training April 2026).
- EV charger hardware excluded from the all-in price — flag for the site-visit follow-up quote.
- Conduit path for the 22 ft EVSE run: confirm ceiling route vs. wall surface at site visit.
- No FPE/Zinsco/Challenger flag — Square D QO 2009 is in good standing.
