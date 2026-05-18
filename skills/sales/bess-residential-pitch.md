---
name: "Residential Battery Storage (BESS) Pitch"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/pitch"
version: 1.0
last_eval_score: null
---

# Residential Battery Storage (BESS) Pitch

## Purpose

Produce a one-page homeowner-facing pitch for a residential battery energy storage system (BESS) install — Powerwall 3, Enphase IQ Battery 5P/10C, FranklinWH aPower 2, LG ESS, SolarEdge Home Battery, or another major brand — that:

1. **Opens with the homeowner's real driver** (whole-home backup, time-of-use bill arbitrage, recent outage, solar self-consumption, EV pair-up) rather than a generic battery brochure.
2. **Is honest about the post-2025 incentive landscape.** The 30% Residential Clean Energy Credit (IRC §25D) expired for systems placed in service after December 31, 2025 under the One Big Beautiful Bill Act. As of 2026, cash and loan-financed residential battery purchases no longer get the federal 30% credit. The 48E commercial ITC remains available for systems owned by a third-party lessor or PPA provider, and several state programs (CA SGIP, CT EnergizeCT, NY-Sun Energy Storage, CO Storage Rebate) remain active. The pitch frames this directly rather than implying credits the homeowner cannot claim.
3. **Sizes the system to the homeowner's actual outage and bill goals** — not to "the biggest battery that fits." Typical residential installs run 13.5–30 kWh usable; whole-home backup of a normal 200 A service typically requires 20+ kWh and a smart-panel or critical-load-subpanel architecture decision.
4. **Picks the architecture honestly.** Three integration shapes — (a) battery + critical-load subpanel on a conventional service panel, (b) battery + smart panel (SPAN-class) with no critical-load subpanel, (c) battery + whole-home backup transfer switch / system controller (Enphase IQ System Controller 3, Tesla Backup Switch, FranklinWH aGate). Each has different parts and labor; the pitch shows the math.
5. **Covers 2026 NEC compliance** — Article 706 ESS disconnecting means (706.15 referring to 705.20), the "ENERGY STORAGE SYSTEM DISCONNECT" marking, residential emergency-shutdown initiation device outside the building, NFPA 855 setbacks for residential installs, and any service-equipment §230.70 outdoor disconnect that gets pulled in when the BESS scope touches the meter.
6. **Quotes contractor-side installer pricing** in concrete bands grounded in May 2026 market data, not vendor marketing pages.

The output is a one-pager the homeowner reads and says *"I understand what the battery does for me, what it costs, what the federal/state incentive picture really looks like in 2026, and why this architecture is the right one for my house."*

Sibling skills: `skills/sales/smart-panel-residential-pitch.md` (when SPAN is the integration choice), `skills/sales/ev-charger-residential-pitch.md` (when an EV charger is also in scope), `skills/operations/nec-2026-load-calculation-helper.md` (for the firm load calc), `skills/sales/scope-letter-drafter.md` (scope-of-work letter once the homeowner says yes), `skills/sales/service-upgrade-avoidance-decision-tree.md` (when the real question is whether a service upgrade is needed at all).

## When to Use

Use this skill when a homeowner or lead has one or more of these triggers:

- **Recent outage** — Multi-hour or multi-day outage in the last 12 months. Lead with backup runtime math: "Your normal evening load is ~2 kW; a 13.5 kWh Powerwall 3 gives you ~6–7 hours of full-house use, or 24+ hours if we set HVAC and well pump to load-shed."
- **Time-of-use (TOU) bill arbitrage** — Homeowner is on a TOU rate with a high on-peak / off-peak delta (CA NEM 3.0, CT, MA, NY, AZ APS, FL FPL TOU). Lead with monthly savings math; the battery cycles daily to push the on-peak load to off-peak hours.
- **Existing solar, no battery** — Customer has PV that was sized to a net-metering rate that has since shifted to TOU or "buy-low / sell-low" (CA NEM 3.0 since April 2023). Battery converts the lost export value back into self-consumption. Lead with the export-rate-vs-retail-rate delta.
- **Solar + battery as a single bundle** — Customer is buying a new PV system and wants the battery sized to it. Pitch the battery as part of the system design from day one, not as an add-on.
- **EV pair-up** — Customer has or is installing an EV charger and wants to charge from solar / off-peak grid. Reference `ev-charger-residential-pitch.md` for the EV scope; this skill handles the battery side.
- **Whole-home backup ambition** — Customer specifically wants the whole house on backup. Trigger the architecture conversation: how much usable kWh do they really need, and is a smart panel cheaper than a 200 A whole-home transfer switch + critical-load subpanel.
- **CA NEM 3.0 customer asking about export reduction** — Battery is now the only way to retain meaningful value from rooftop solar in CA. Lead with that.
- **Proactive pitch to recent solar installs in the CRM** — Customer signed for PV in the last 18 months without a battery. Pitch the battery as the missing piece, with NEC 2026 disconnect/marking work bundled in as a one-truck-roll add.

Do NOT use this skill when:

- The homeowner's real problem is a service upgrade or panel replacement (route to `service-upgrade-avoidance-decision-tree.md` or `smart-panel-residential-pitch.md`).
- The homeowner is asking about a portable / plug-in battery (EcoFlow, Anker, Bluetti, Goal Zero). Those are appliance-class products, not permanent installs.
- The scope is commercial/industrial storage — different code chapter (Article 706 Part III for commercial, NFPA 855 for >20 kWh aggregate, plus the 48E commercial ITC). This skill is residential.
- The homeowner has an active fire-code or HOA prohibition the contractor hasn't yet cleared — solve that first.

## Required Input

Provide the following. Where the contractor doesn't have a value, leave it blank — the skill will list it as a first-visit task.

1. **Homeowner name and address** — For greeting and jurisdiction (state + AHJ for NEC adoption status, state rebate eligibility, fire-code residential-battery setback).
2. **Jurisdiction and NEC cycle in force** — State, county, city, and whether the AHJ is on 2026 NEC, 2023, or 2020. Determines whether NEC 2026 §706.15/705.20 marking and the residential emergency-shutdown-initiation rule apply now or come at next code cycle.
3. **Existing service amperage and panel details** — 100 A / 125 A / 200 A / 225 A / other, panel manufacturer, approximate age. Flag FPE Stab-Lok, Zinsco, Sylvania, Challenger, Pushmatic (these are panel-replacement candidates regardless of battery scope).
4. **Existing solar (if any)** — Inverter manufacturer (Enphase / SolarEdge / SMA / Tesla / Generac / other), DC- or AC-coupled, system size in kW DC, installation year, and current net-metering tariff (NEM 1.0 / 2.0 / 3.0 in CA; equivalent in other states). Determines AC-coupled vs. DC-coupled battery choice and whether a new inverter is needed.
5. **Primary trigger** — Outage, TOU arbitrage, solar self-consumption, EV pair-up, whole-home backup ambition, recent NEM 3.0 changeover, panel/service upgrade conversation that pivoted to a battery.
6. **Backup ambition** — Critical-loads-only, "most of the house except AC and dryer," or whole-home. Sets the architecture choice (subpanel vs. smart panel vs. whole-home transfer switch).
7. **TOU schedule and rate (if applicable)** — Utility name, rate schedule, peak/off-peak hours, and approximate $ per kWh delta. The pitch will quote daily and monthly arbitrage rough numbers only — never quote a firm payback figure without the homeowner's actual bill.
8. **Battery preference (if any)** — Powerwall 3, Enphase IQ 5P or 10C, FranklinWH aPower 2 + aGate, LG ESS, SolarEdge Home Battery, or "open." If the homeowner has an Enphase microinverter PV system, default to Enphase IQ Battery (no second inverter needed). If they have a SolarEdge system, default to SolarEdge Home Battery for the same reason. If they have a string inverter or Tesla solar, Powerwall 3 (DC-coupled, includes inverter) is usually the cleanest fit.
9. **Outdoor wall or garage real estate for the battery** — Approximate location, distance from main panel, and whether there's a code-compliant location available (NFPA 855 setbacks: 3 ft from openings/egress; not in interior bedrooms or closets; outdoor-rated for the brand chosen).
10. **Prior quote (if any)** — If the homeowner has a competing battery quote, include the dollar figure. Pitch will address it directly.
11. **State / utility incentive status** — CA SGIP (Equity Resiliency / General Market), CT EnergizeCT, NY-Sun Energy Storage, CO Storage Rebate, MA SMART storage adder, HI Battery Bonus, or local utility-specific demand-response payments. Reference programs by name only; confirm current funded status before quoting a dollar figure.
12. **Whether the homeowner is interested in a third-party-owned (TPO) lease or PPA structure** — Relevant because 48E ITC remains available under TPO ownership in 2026 even though §25D expired. Contractor partners with the TPO provider for the install; pitch frames this as an option, not a recommendation.

## Instructions

You are an AI assistant drafting a residential BESS pitch for a licensed electrical contractor in May 2026. Your job is technically accurate and sales-useful at the same time. You do not promise tax credits the homeowner cannot claim, do not invent battery features, and always run the architecture math (subpanel vs. smart panel vs. whole-home) before recommending the integration shape.

### Before you start

- Load `config.yml` for company name, state, license number, owner/tech names, phone, preferred battery brands, certified installer status by brand (Tesla Certified Installer, Enphase Installer Network tier, FranklinWH Certified Installer, SolarEdge Pro), and voice preferences.
- Load `knowledge-base/regulations/nec-2026-key-changes.md` for the Article 706 / 705.20 disconnect language and the residential emergency-shutdown-initiation rule.
- Load `knowledge-base/regulations/material-tariffs-2026.md` only if the scope includes service equipment, switchgear, or transformer-scale components — most residential battery scopes do not trigger the Section 232 transformer/grid-equipment language. If the scope is panel-touching, surface the copper-content note.
- Load `knowledge-base/terminology/` for consistent term rendering.
- Confirm the homeowner's NEM tariff before any export-rate or bill-arbitrage math.

### Core process

1. **Pick the pitch shape** based on the primary trigger. Outage-driven, TOU-arbitrage-driven, solar-export-recovery, and whole-home-backup pitches read differently; do not blend them.
2. **Run the architecture decision first.** Before pricing, decide which of the three integration shapes fits:
   - **(a) Battery + critical-load subpanel on a conventional panel** — Lowest upfront, narrowest backup scope (lights, fridge, internet, well pump, one HVAC zone). Adds $800–$2,500 for the subpanel plus labor. Best for homeowners who want backup but not whole-home and have a recent panel.
   - **(b) Battery + SPAN-class smart panel, no critical-load subpanel** — Mid-upfront, full circuit-level backup priority programmable in the app. Adds $2,500–$3,500 over a conventional panel swap (see `smart-panel-residential-pitch.md` for the SPAN-specific pitch). Best for homeowners who want whole-home flexibility and are open to a panel swap.
   - **(c) Battery + whole-home transfer / system controller** — Tesla Backup Switch, Enphase IQ System Controller 3, FranklinWH aGate, or equivalent. Backs up the whole 200 A service from the battery during outage. Adds $1,500–$3,500 for the controller plus install labor. Best when whole-home backup is the explicit goal and the existing panel doesn't need replacement.
3. **Size the battery to the goal, not the wallet.** Use this rough decision aid:
   - Outage-only, critical loads, 12–24 hour ride-through → 13.5–15 kWh usable (one Powerwall 3, one FranklinWH aPower 2, one IQ 10C, or two IQ 5Ps)
   - Whole-home backup, 8–12 hour ride-through with HVAC load-shedding → 20–30 kWh usable (one Powerwall 3 + expansion pack, two FranklinWH aPower 2s, two IQ 10Cs)
   - Solar self-consumption + TOU arbitrage, no critical backup goal → match to daily PV export and on-peak window — usually 10–15 kWh usable
   - Heavy electrification (EV + heat pump + heat-pump water heater) + backup → 30+ kWh usable; consider two batteries
4. **Quote real installer pricing.** Use these May 2026 contractor-side bands (national reference; adjust for regional labor). State the band, not a single number, and always note that hardware-only vs. installed prices differ.
   - **Tesla Powerwall 3 (13.5 kWh, hybrid inverter included):** hardware ~$9,200; installed $12,000–$16,500 single unit. Backup Switch ~$1,000–$1,500 added when whole-home backup is requested. Expansion pack ~$6,200 hardware.
   - **Enphase IQ Battery 5P (5 kWh, AC-coupled):** installed $6,000–$8,000 per unit. Best when the home is already on Enphase microinverters.
   - **Enphase IQ Battery 10C (10 kWh, AC-coupled):** installed ~$13,000 per unit. IQ System Controller 3 adds $1,500–$2,500 when whole-home backup is requested.
   - **FranklinWH aPower 2 (15 kWh) + aGate controller:** installed $14,000–$17,000 for the one-aPower + aGate package; ~$18,000 typical full-system price.
   - **SolarEdge Home Battery (9.7 kWh):** installed $11,000–$14,000 when paired with a SolarEdge inverter.
   - **LG ESS Home 8 (14.4 kWh):** installed $13,000–$16,000.
   - **Critical-load subpanel (architecture (a)):** $800–$2,500 parts + labor.
   - **Outdoor service §230.70 emergency disconnect (when scope touches service equipment in a 2026-NEC AHJ):** $1,000–$1,650 added; do not omit when applicable.
5. **Be honest about the 2026 incentive landscape.** State plainly:
   - The 30% federal Residential Clean Energy Credit (§25D) was repealed for systems placed in service after Dec 31, 2025 under the One Big Beautiful Bill Act (signed July 4, 2025).
   - Homeowners who buy a battery with cash or a loan in 2026 cannot claim the 30% federal credit.
   - The 48E commercial ITC remains available through 2032 for third-party-owned systems (lease or PPA). The TPO provider claims the credit and prices it into the lease payment.
   - State and utility incentives are unaffected and remain the primary remaining lever. List by program name only; confirm funded status before quoting dollars.
   - Do not imply or print "30%" as a homeowner-claimable figure in any 2026 pitch unless the system is on a TPO structure and the TPO partner has confirmed it in writing.
6. **Cover 2026 NEC marking and shutdown.** In 2026-NEC jurisdictions, every BESS install has:
   - An ESS disconnecting means marked "ENERGY STORAGE SYSTEM DISCONNECT" (Article 706 referring to 705.20 in the 2026 edition).
   - A readily accessible disconnect within sight of (or within 10 ft of) the ESS.
   - For one- and two-family dwellings: an emergency-shutdown initiation device located outside the building that ceases ESS power export to premises wiring.
   - NFPA 855 setbacks: minimum 3 ft from openings, doors, windows, and egress paths; not in interior bedrooms or closets; outdoor-rated enclosure for outdoor installs.
   - If the scope replaces service equipment, the §230.70(B)(2) outdoor "EMERGENCY DISCONNECT" red-background label.
   Disclose these costs as line items, not surprises.
7. **AC- vs. DC-coupled — match to the inverter that's already on the house.** Enphase microinverter PV → IQ Battery (AC-coupled, no second inverter). SolarEdge string inverter PV → SolarEdge Home Battery (DC-coupled, replaces or adds to inverter). Tesla solar or generic string inverter → Powerwall 3 (DC-coupled, includes its own hybrid inverter — can replace or work alongside the existing inverter). FranklinWH is brand-agnostic AC-coupled; will work behind any inverter. Document the decision in the pitch so the homeowner sees why the battery brand chosen is the right one for their existing system.
8. **Address battery permitting and interconnection honestly.** Permitting timeline is 4–8 weeks from signed contract to operational system in most jurisdictions, occasionally longer in CA NEM 3.0 markets where utility interconnection review queues are slow. Set the expectation in writing.
9. **Address NEM 3.0 (CA) or equivalent export-rate-cut markets directly.** For CA customers on NEM 3.0, the battery is now the only way to retain meaningful economic value from PV. Quote rough cycle math: a 13.5 kWh battery cycled once daily at the on-peak/off-peak delta yields $X/month; over 10 years that's $Y. Never present this as guaranteed payback — frame as illustrative.
10. **Write to the homeowner.** Translate every code citation and product spec into a benefit the homeowner can verify or feel. Reserve technical citations for the NEC Notes paragraph.

### Anti-upsell rules

- Do not pitch a 20–30 kWh battery to a homeowner whose stated goal is critical-load-only backup. Show the smaller option and let them upsize.
- Do not invent compatibility. If the homeowner has a 2018 string inverter, do not claim the new battery "AC-couples seamlessly" without confirming the inverter has an unused AC port and adequate excess PV capacity.
- Do not quote the §25D 30% credit for a 2026 install. The credit is gone for cash/loan residential. The pitch must say so.
- Do not quote a TPO lease as "free" or "no upfront cost" without disclosing the lease payment and the typical 10–20 year term.
- Do not promise grid-export revenue or NEM crediting that requires utility approval not yet in hand.
- Do not present a payback figure as a guarantee. Frame all bill-arbitrage math as illustrative.
- Do not skip the NFPA 855 setback check. If the homeowner's only proposed location violates the setback, surface it in the pitch and propose alternatives — do not pretend the location works.
- Do not describe a competitor's quote as wrong. If the competitor's number is lower, explain what's missing (controller, subpanel, outdoor service disconnect, brand-specific install training).

## Output Format

Default output is a **one-page homeowner-facing memo** with these sections, in order:

1. **What we're recommending** — One-sentence headline. Battery model, kWh, architecture choice, total all-in band.
2. **Why this battery for your house** — 3–5 sentences. Lead with the homeowner's trigger (outage, TOU, solar self-consumption, EV pair-up). Explain the brand choice in one sentence (AC- vs. DC-coupled match to existing inverter).
3. **What you get when the grid goes down** — Concrete numbers: kWh usable, expected ride-through hours under typical load, what is and isn't on backup, how the battery refills from solar or grid the next day.
4. **The architecture choice (pick one)** — Critical-load subpanel / smart-panel swap / whole-home transfer controller. State which one we picked and why.
5. **What's included** — Battery hardware, controller/transfer switch, mounting, conduit, permits, inspection, interconnection paperwork, commissioning, app setup, one-year workmanship warranty, manufacturer warranty (state years).
6. **What's not included** — Service upgrade unless quoted, panel replacement unless quoted, solar inverter replacement unless quoted, Wi-Fi router setup, drywall patching, finish paint, snow/ice removal at install location.
7. **2026 NEC and fire-code notes** — Article 706 ESS disconnect and "ENERGY STORAGE SYSTEM DISCONNECT" marking, residential outside-building emergency shutdown initiation device, NFPA 855 setbacks observed at proposed location, §230.70 outdoor service disconnect if scope touches service equipment.
8. **2026 incentive reality** — One short paragraph. Federal §25D 30% credit no longer available for cash/loan in 2026; 48E is available via TPO/lease only; list applicable state/utility programs by name; confirm dollar amounts before contract.
9. **Pricing** — Firm number or firm range, with line items for hardware, install labor, controller/subpanel, permits, and any outdoor disconnect or smart-panel upgrade. Note hardware-only vs. installed clearly.
10. **Timeline** — Permit pull, install day, utility interconnection, expected total elapsed time. Set realistic expectations (4–8 weeks total).
11. **Next step** — One clear ask.
12. **Sign-off** — Owner/tech name, company, license number, phone, brand-specific certified installer status (Tesla Certified, Enphase Installer Network, FranklinWH Certified Installer, SolarEdge Pro).

Below the main output, include a short **Internal Notes** block with:

- Any assumption the skill made (service size, existing inverter brand, NFPA 855 setback at the proposed location, NEM tariff)
- First-visit task list if the pitch is first-touch
- Brand-specific certified installer status (confirmed, pending, not yet started)
- Pointer to `skills/operations/nec-2026-load-calculation-helper.md` if the install pulls in service equipment
- Pointer to `skills/sales/smart-panel-residential-pitch.md` if architecture (b) is selected
- Pointer to `skills/sales/ev-charger-residential-pitch.md` if EV pair-up is part of scope
- Any justified upsell or referral (EV charger, additional battery for whole-home, solar add-on) implied by what the homeowner said — never invented

## What NOT to Do

- Do not present the federal 30% credit as available for a 2026 cash/loan residential battery. It is not.
- Do not skip the NFPA 855 setback check at the proposed install location.
- Do not omit the NEC 2026 §706/705.20 disconnect marking from the line items.
- Do not size the battery to the price the homeowner mentioned. Size to the goal; then show the cost.
- Do not invent inverter compatibility. AC-coupled vs. DC-coupled is the first technical question; document the choice.
- Do not promise utility interconnection in less than 4 weeks unless the AHJ has a confirmed fast-track. NEM 3.0 markets are often 6–10 weeks.
- Do not describe a competing quote as careless or wrong. Explain what's included at the price delta.
- Do not present a payback figure as a guarantee. Frame all bill math as illustrative.

## Example Output

### Example — CA NEM 3.0 customer with existing Enphase solar wants whole-home backup

**Input:**

- Homeowner: Priya and Sameer Banerjee, 2417 Olive Hill Ct, San Jose, CA (PG&E territory; AHJ on 2023 NEC as of May 2026; 2026 adoption expected late 2026 / early 2027)
- Service: 200 A Eaton CH (2017, good condition)
- Existing solar: Enphase IQ8M microinverter system, 8.4 kW DC, installed April 2024, on NEM 3.0 since April 2023 changeover (high on-peak / low export rate)
- Primary trigger: PSPS outage Sept 2025 (3 days), plus NEM 3.0 export economics
- Backup ambition: whole-home except 3-ton AC and electric dryer
- TOU schedule: PG&E EV2-A (peak 4–9 PM, off-peak overnight; ~$0.55 peak / ~$0.30 off-peak delta)
- Battery preference: open; suggesting Enphase IQ Battery 10C (×2) for AC-couple match to existing Enphase microinverters
- Outdoor location: south-facing garage exterior wall, 14 ft from main panel; setback OK
- Prior quote: $34,000 from competitor for one Powerwall 3 + Backup Switch + new subpanel
- Brand-specific certification: Enphase Installer Network — Platinum tier
- CA SGIP: General Market track, currently funded

**Output — Whole-home BESS pitch (CA NEM 3.0):**

> **Two Enphase IQ Battery 10Cs with whole-home backup on your existing Enphase system, all-in $28,500–$30,500.**
>
> You have an Enphase IQ8M system from 2024. Adding two Enphase IQ Battery 10Cs (20 kWh usable) and an Enphase IQ System Controller 3 keeps your house powered through PSPS events like last September's, and it captures the on-peak / off-peak delta that NEM 3.0 took away from your solar export. Because your inverters are already Enphase, we don't need to add a hybrid inverter or replace any equipment — the batteries AC-couple directly to your existing system.
>
> **What you get when the grid goes down.**
>
> - 20 kWh usable — enough for ~12–18 hours of normal evening household use with AC load-shed, or ~24+ hours if we set the heat pump water heater to off-peak only
> - Whole-home backup with two exceptions: the 3-ton AC and the electric dryer stay off automatically during outage (configured in the Enphase app)
> - Refills from your 8.4 kW solar the next morning; refills from grid overnight when grid is back and on off-peak rates
>
> **Architecture choice — whole-home transfer controller.**
>
> The Enphase IQ System Controller 3 replaces your existing combiner-style controller and acts as the automatic transfer switch. We don't need a critical-load subpanel — every circuit in your existing panel rides through outage on the battery (less the two load-shed circuits noted above). We don't need a panel swap either; your 2017 Eaton CH is in good standing.
>
> **What's included.**
>
> - Two Enphase IQ Battery 10C units (20 kWh usable total), wall-mounted on the south garage exterior wall
> - Enphase IQ System Controller 3 (whole-home transfer + load management)
> - Mounting, conduit, AC disconnects per NEC 2026 §705.20 and §706 (ENERGY STORAGE SYSTEM DISCONNECT marking)
> - Outside-building emergency shutdown initiation device per NEC 2026 §706 (residential)
> - Permit, inspection, PG&E interconnection paperwork (NEM 3.0 modification), system commissioning, app configuration
> - One-year workmanship warranty, Enphase 15-year battery warranty (we are Enphase Installer Network Platinum tier)
>
> **What's not included.**
>
> - Service upgrade (not needed — 200 A Eaton CH is fine for this scope)
> - Panel replacement (not needed)
> - Solar inverter replacement (not needed — your IQ8Ms stay)
> - Drywall patching or paint at the garage exterior penetration
> - Wi-Fi configuration if your router doesn't reach the garage (we can quote a separate Wi-Fi extender if needed)
>
> **2026 NEC and fire-code notes.**
>
> San Jose / PG&E AHJ is on 2023 NEC as of May 2026; 2026 NEC adoption is expected late 2026 / early 2027. We are installing to the 2026 disconnect and marking requirements anyway because the Enphase IQ System Controller 3 is already 2026-compliant and there is no reason to install a label that will be reworked at next inspection cycle. NFPA 855 residential setbacks are observed at the proposed south garage exterior wall location: ≥3 ft from any opening, door, window, or egress path; no interior bedroom or closet installation. The §230.70(B)(2) outdoor emergency service disconnect is NOT in scope because we are not replacing service equipment.
>
> **2026 incentive reality.**
>
> The federal 30% Residential Clean Energy Credit (§25D) is no longer available for batteries purchased with cash or a loan in 2026 — it ended December 31, 2025 under the One Big Beautiful Bill Act. The pricing below does NOT assume any federal credit. **CA SGIP (General Market track) is currently funded** and your install qualifies — expected rebate $3,000–$4,500 depending on submission timing and queue position; we'll submit the application and pass the rebate through at closing. **PG&E EV2-A on-peak / off-peak arbitrage** — illustratively, cycling 20 kWh once daily at your ~$0.25/kWh delta is ~$5/day or ~$150/month, before considering NEM 3.0 export retention. We will not put a payback number in writing without your last 12 months of PG&E bills.
>
> **Pricing.** **$28,500–$30,500 all-in** before SGIP:
>
> - 2× IQ Battery 10C installed: $24,000–$25,500
> - IQ System Controller 3 + transfer switch labor: $3,500–$4,000
> - Permit, inspection, PG&E interconnection paperwork: $850–$1,000
> - Commissioning + app config: $150–$500
>
> Estimated post-SGIP net: **$24,500–$27,500**. Compare to the competitor's $34,000 Powerwall 3 + Backup Switch + subpanel quote, which would require adding a DC-coupled battery behind a different inverter and has no SGIP-aware quoting.
>
> **Optional add-ons (not included above):**
>
> - Third IQ Battery 10C (30 kWh total): +$11,500–$12,500 — covers whole-home backup including AC and dryer
> - Whole-home critical-load subpanel for an emergency-loads-only configuration with one IQ Battery 10C (~10 kWh): $11,000–$13,500 all-in — listed for completeness if budget is the priority
>
> **Timeline.** Permit pull within 5 business days. Install in 1 day (batteries) + 1 day (controller + commissioning). PG&E NEM 3.0 modification interconnection 4–8 weeks typical from PTO submittal. Expected total elapsed time: 6–10 weeks from contract to permission-to-operate.
>
> **Next step.** Reply to confirm the 20 kWh whole-home path and I'll start the SGIP reservation and the permit application this week. Or call me if you'd like to talk through the 30 kWh option (third battery) or the budget-priority subpanel option.
>
> — Marisol Park, Bay Harbor Electric (C-10 #1098453) · (408) 555-0192 · Enphase Installer Network Platinum

**Internal Notes:**

- Enphase IQ8M existing inverter confirms AC-couple architecture — Enphase IQ Battery is the no-second-inverter choice; DC-coupled batteries (Powerwall 3, SolarEdge) would require swapping the inverter and aren't justified here.
- Service upgrade and panel swap both excluded — 200 A Eaton CH 2017 is in good standing.
- NFPA 855 setback confirmed at south garage exterior wall — verify exact distances from the door, window, and AC condenser at site visit.
- SGIP General Market track funded as of May 2026 — submit application within 30 days of contract.
- PG&E NEM 3.0 modification: standard 4–8 week interconnection. Submit the PTO application same day as permit pull.
- Federal §25D credit explicitly disclaimed in writing — homeowner has been informed in the pitch.
- Two batteries (20 kWh) sized to whole-home with two exceptions (AC, dryer). Third battery offered as upsize, not pushed.
- Bay Harbor Electric is Enphase Installer Network Platinum — surfaces in the sign-off.
- Run `skills/operations/nec-2026-load-calculation-helper.md` if AC or dryer load shed assumptions change.
- No FPE / Zinsco / Challenger flag — Eaton CH 2017 is in good standing.

---

### Example — outage-driven customer, no existing solar, conventional architecture

**Input:**

- Homeowner: David Chen, 4811 Stoneybrook Dr, Frisco, TX (Oncor territory; Texas does not adopt NEC statewide — Frisco is on 2023 NEC as of May 2026)
- Service: 200 A Square D QO (2014, good condition)
- Existing solar: none
- Primary trigger: 4-day winter storm outage (Feb 2024)
- Backup ambition: critical-load-only — fridge, lights, internet, gas furnace blower, well pump for landscaping
- Battery preference: open; suggesting one Powerwall 3 (DC-coupled, includes inverter, future-proofs for solar later)
- Outdoor location: north side of detached garage, 28 ft from main panel; NFPA 855 setback OK
- Prior quote: none
- Brand-specific certification: Tesla Certified Installer
- TX state incentives: none currently funded for residential storage

**Output — Outage-driven critical-load BESS pitch:**

> **One Tesla Powerwall 3 (13.5 kWh) with a critical-load subpanel for outage backup, all-in $17,500–$19,000.**
>
> Frisco's February 2024 outage was 4 days, and your situation is one Powerwall 3 was built for: critical loads ride through a winter storm event, and the system can grow to whole-home or add solar later without replacing the battery. Powerwall 3 includes its own hybrid inverter, so if you add panels in the next few years we won't need to add a second inverter.
>
> **What you get when the grid goes down.**
>
> - 13.5 kWh usable — at ~1 kW continuous critical-load draw (fridge, lights, internet, well pump on/off, gas furnace blower), that's 12–14 hours of full backup. With load management we can stretch a single charge to 24+ hours.
> - Critical-load subpanel: fridge, kitchen lights, master bedroom lights, master bathroom lights, internet/router, gas furnace blower circuit, well pump. AC, dryer, range, water heater, garage outlets stay on grid power only.
> - Refills from grid in 4–6 hours when grid is back.
>
> **Architecture choice — critical-load subpanel on your existing panel.**
>
> We're installing a 12-circuit critical-load subpanel beside your existing 200 A Square D QO and moving the seven circuits above to it. The Powerwall 3 backs up the subpanel only — your main panel keeps powering everything else when the grid is up. This is the cleanest path when whole-home backup isn't a goal and you don't want to swap your existing panel.
>
> **What's included.**
>
> - Tesla Powerwall 3 (13.5 kWh, hybrid inverter included), wall-mounted on the north garage exterior wall
> - 12-circuit critical-load subpanel, installed adjacent to existing main panel
> - Circuit relocations (7 circuits as listed above)
> - Mounting, conduit (28 ft run from panel to garage), AC disconnects per NEC 2023 §706.15 (ENERGY STORAGE SYSTEM DISCONNECT marking — installing to the 2026 wording now since the language is settled)
> - Permit, inspection, Oncor interconnection (storage-only, no PV)
> - System commissioning, Tesla app configuration
> - One-year workmanship warranty, Tesla 10-year battery warranty (we are a Tesla Certified Installer)
>
> **What's not included.**
>
> - Solar PV (can be added later; the Powerwall 3 already has the inverter)
> - Service upgrade or panel replacement
> - Whole-home transfer switch (Tesla Backup Switch) — would add $1,000–$1,500 if you ever decide to go whole-home, but not needed for critical-load-only
> - Drywall patching or paint
> - Wi-Fi setup at the garage
>
> **2026 NEC and fire-code notes.**
>
> Frisco is on 2023 NEC as of May 2026; Texas adopts cycle-by-cycle by municipality. The disconnect marking ("ENERGY STORAGE SYSTEM DISCONNECT") and outdoor-residential emergency shutdown initiation device are installed to the 2026 language — no rework needed when Frisco transitions. NFPA 855 setbacks observed at the north garage exterior wall location.
>
> **2026 incentive reality.**
>
> The federal 30% Residential Clean Energy Credit (§25D) is no longer available for batteries purchased with cash or a loan in 2026 — it ended December 31, 2025. Texas has no statewide residential storage rebate as of May 2026. Oncor has no current storage-specific incentive. Pricing below assumes no federal or state incentives. If you'd like to explore a Tesla lease structure, the 48E commercial credit can be passed through by a Tesla-affiliated third-party owner — happy to introduce you to a TPO partner if interested.
>
> **Pricing.** **$17,500–$19,000 all-in**:
>
> - Powerwall 3 installed: $13,500–$14,500
> - 12-circuit critical-load subpanel + circuit relocations: $2,200–$3,000
> - Permit, inspection, Oncor interconnection paperwork: $500–$700
> - Commissioning + app config: $200–$500
> - Conduit + mounting + misc materials: $400–$800
>
> **Timeline.** Permit pull within 3 business days. Install in 1 day. Oncor interconnection 2–4 weeks (storage-only is faster than solar-plus-storage). Expected total elapsed time: 3–6 weeks from contract to permission-to-operate.
>
> **Next step.** Reply to confirm the critical-load Powerwall path and we'll schedule the site visit this week. Happy to walk through the whole-home option (Tesla Backup Switch, no subpanel) on the phone if you're on the fence.
>
> — Russell Trent, Lone Star Power Systems (TECL-32104) · (469) 555-0144 · Tesla Certified Installer

**Internal Notes:**

- No existing solar — Powerwall 3 (DC-coupled with built-in inverter) is the future-proof choice; if customer adds solar in 1–3 years, no second inverter needed.
- Critical-load subpanel architecture chosen — whole-home Backup Switch offered as a Phase 2 add (avoids forcing the conversation now).
- 28 ft conduit run from main panel to garage — confirm wall route at site visit (interior wall path vs. exterior).
- TX no state storage rebate — federal §25D explicitly disclaimed.
- TPO referral mentioned but not pushed — if customer asks for the cash-vs-lease comparison, route to the Tesla-affiliated TPO partner.
- Tesla Certified Installer status: confirmed, surfaces in sign-off.
- Run `skills/operations/nec-2026-load-calculation-helper.md` only if the subpanel circuit list changes from the 7 listed.
- No FPE / Zinsco / Challenger flag — Square D QO 2014 is in good standing.
