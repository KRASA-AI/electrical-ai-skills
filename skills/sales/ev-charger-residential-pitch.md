---
name: "EV Charger Residential Pitch"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/pitch"
version: 1.0
last_eval_score: null
---

# ⚡ EV Charger Residential Pitch

## Purpose

Produce a one-page homeowner-facing pitch for a residential Level-2 EV charger installation that (1) opens with the right conversation for the homeowner's situation rather than a generic "we install EV chargers" brochure, (2) accurately sizes the job under the **2026 NEC** rules — Article 120 load calc, §120.82(D) 100%-of-nameplate for EVSE, §625.42 EVEMS recognition, the new §230.70 outdoor service-disconnect requirement, and the §120.82-"qualified persons only" mandate — and (3) always runs the **service-upgrade-avoidance check** before pitching an upgrade, because an EV Energy Management System (EVEMS) can often add a Level-2 charger to an existing 100 A or 200 A service without touching the panel.

The output is a clean one-pager (or short email) that a homeowner reads and says *"I understand what I'm paying for, I understand why the number is what it is, and I don't feel like I'm being upsold."*

## When to Use

Use this skill when an electrical contractor needs to convert a residential EV-charger lead — website form fill, phone call, referral, callback after a rough load calc — into a written pitch the homeowner can sit with:

- **First-touch pitch after a call or form fill** — Before a site visit, based on the few facts the homeowner gave (panel size, dryer/range type, EV model, where the charger needs to go)
- **Written quote follow-up after a site visit** — When the tech has the actual panel amps, the distance from panel to parking, the appliance inventory, and an initial load-calc range
- **Service-upgrade-avoidance memo** — When a homeowner has been told by a previous contractor they need a $6,000–$12,000 service upgrade, and an EVEMS-based solution may eliminate or reduce that scope
- **Three-option comparison pitch** — "Direct 40 A hardwire" vs. "EVEMS on the existing panel" vs. "panel/service upgrade" — with rough budget bands and the tradeoffs of each
- **Referral-ready explainer** — Something a homeowner can forward to a spouse, HOA, or property manager that doesn't require an electrician on the call

Do NOT use this skill to produce a firm contract or a stamped load calculation. Route the final numbers through `skills/operations/nec-2026-load-calculation-helper.md`, and the scope-of-work letter through `skills/sales/scope-letter-drafter.md` once the homeowner says yes.

## Required Input

Provide the following:

1. **Homeowner name and address** — For the greeting and to let the skill know the jurisdiction (state/county/city) so it can check NEC adoption status.
2. **Jurisdiction and NEC cycle in force** — State + AHJ, and whether the jurisdiction has adopted the 2026 NEC, is still on 2023, or still on 2020. This matters because the pitch cannot reference a 2026-only provision (EVEMS §625.42, §120.82(D) 100% rule, §230.70 outdoor disconnect) in a jurisdiction still on 2023 without flagging it as "coming soon" rather than "required now."
3. **Existing service amperage** — 100 A / 125 A / 150 A / 200 A / 225 A / other. If unknown, say so — the skill will list the first-visit tasks needed to confirm.
4. **Panel age, manufacturer, and known issues (if any)** — Federal Pacific Stab-Lok, Zinsco, Challenger, Pushmatic, Sylvania — these are automatic panel-replacement candidates regardless of EV. Note them in the pitch.
5. **Dwelling square footage (conditioned)** — For the 2 VA/sq ft general-lighting load under §120.42.
6. **Major loads already installed** — Electric range/cooktop/oven (kW), dryer (gas or electric + kW), water heater (gas/tank electric/tankless electric + kW), heat (gas forced air / heat pump / electric strip / oil / hydronic), central AC tons, pool/spa heater, well pump. The skill uses these to figure out whether the remaining service headroom fits an EV charger on its own, or whether EVEMS / upgrade is needed.
7. **EV(s) and target charger** — EV make/model/year, onboard charger rating (kW or max AC amps), and the Level-2 charger model if already selected. If not selected, the skill will recommend a sizing range based on the onboard charger. (Typical Level-2 installs land at 40 A / 48 A / 50 A circuits; hardwired 50-amp EV chargers require a dedicated 60 A breaker under NEC Article 625 continuous-load rules.)
8. **Charger location** — Garage interior / garage exterior / driveway / carport / detached garage / unit-front parking (condo/townhouse). Capture approximate distance from the panel to the target location — every foot over ~10 ft adds material cost ($10–$20 per foot is a common estimating band).
9. **Homeowner context** — Single-EV household / two-EV household / planned second EV / solar-PV owner / battery-storage owner / utility with a managed-charging rebate / none of the above. These change the pitch.
10. **Any prior quote** — If the homeowner has been quoted before (especially a "you need a service upgrade" quote), include the number. The pitch will address it directly rather than pretending it doesn't exist.
11. **Incentives / rebates to reference (optional)** — Federal 30C alternative-fuel credit (up to 30% of installation cost through 2032, certain income-and-census-tract caps apply), state EV-charger rebates, utility make-ready or managed-charging rebates. Skill will cite the program by name, not dollar figures, unless you supply the verified current amount.

## Instructions

You are an AI assistant drafting a residential EV-charger pitch for a licensed electrical contractor. Your job is to be technically honest and sales-useful at the same time. You do not upsell, you do not undersell, and you always run the service-upgrade-avoidance check before suggesting a service upgrade.

**Before you start:**

- Load `config.yml` for the company name, state, license number, owner/tech names, phone, preferred Level-2 charger brands (if any), preferred installers/finance partners, and voice preferences.
- Load `knowledge-base/regulations/nec-2026-key-changes.md` for the Article 120 migration summary, §120.82(D) EVSE 100%-of-nameplate rule, §625.42 EVEMS recognition, §230.70 outdoor disconnect, and §120.82 "qualified persons" mandate.
- Load `knowledge-base/terminology/` for consistent term rendering.
- If the jurisdiction is on 2023 or 2020, frame 2026-only provisions as "the next code cycle will…" — never as "required today."
- If a `skills/operations/nec-2026-load-calculation-helper.md` run exists for this customer, use that result. Otherwise, compute a rough calc range inline and clearly say so.

**Core process:**

1. **Pick the pitch shape based on the input.** First-touch pitches are shorter and list the first-visit tasks. Post-site-visit pitches include firm-or-firm-range numbers. Three-option pitches always compare Direct / EVEMS / Upgrade. Service-upgrade-avoidance memos lead with the avoided cost.
2. **Run the service-upgrade-avoidance check.** Before the pitch touches a panel-upgrade line item:
   - Compute the 2026 Article 120 load with the EV charger at 100% of nameplate (§120.82(D)).
   - Compare to the existing service amperage.
   - If the result is under service, go direct (no upgrade, no EVEMS needed).
   - If the result is over service but under service + the EV's continuous draw, check whether an **EVEMS** (§625.42, §750) can throttle the EV in real time to keep total load under service capacity. Most UL-listed EVEMS units (sold by Wallbox, ChargePoint, DCC-type branch-circuit EVEMS, Emporia, SpanPanel, and similar) can. An EVEMS is typically $300–$1,200 in parts, far below a $3,000–$24,000 service upgrade.
   - If even an EVEMS can't fit it, the upgrade is the honest answer — pitch it without hesitation, but show the math.
3. **Address the panel itself honestly.** If it's FPE Stab-Lok, Zinsco, Challenger, Pushmatic, or Sylvania, say so in plain terms: these are known-problem panels and most insurers and inspectors flag them on sight. The EV install is an opportunity to replace the panel as part of the scope. Do not hide this.
4. **Address the 2026 outdoor disconnect.** In any jurisdiction on the 2026 NEC, a service-equipment replacement triggers the outdoor emergency-disconnect requirement under §230.70(B)(2) (labeled "EMERGENCY DISCONNECT" in ½-inch-tall white letters on a red background). That's a real $1,000–$1,650 "2026 NEC premium" over a pre-2023 service upgrade and must be disclosed up front, not as a surprise change order.
5. **Reference the "qualified persons" mandate in NEC 2026.** Permanently installed EVSE now requires installation by a qualified person under the 2026 NEC. Translate this for the homeowner (the DIY-big-box-store install is no longer code-compliant in 2026-adopting jurisdictions) and use it as a trust anchor, not a scare tactic.
6. **Cover real money, not just price.** Rough cost bands:
   - Direct 40 A / 50 A hardwire install with existing panel capacity: **$500–$1,200** labor + materials, $800–$2,000 all-in on a typical suburban home
   - EVEMS device + install, existing panel capacity not quite adequate: **$1,200–$2,500 all-in**, delta over direct
   - 200 A service + panel upgrade with outdoor disconnect (2026-compliant): **$3,500–$6,500** depending on meter relocation, permit scope, utility work, and sidewalk/feeder run
   - Distance premium beyond ~10 ft panel-to-charger: $10–$20 per foot
   - FPE/Zinsco/Challenger panel replacement, same service amperage: typically **$1,800–$3,500** delta over the charger install alone
   - Federal 30C credit on qualifying installations (2032 sunset) — cite by program name; verify percentage and caps before numbering
7. **Write to the homeowner, not to the master electrician.** Keep the tone honest, specific, and non-pushy. Lead with what they want — their EV charging working, on time, without a service-panel surprise. Put the technical reasoning in a short "Why this configuration" section for the curious.
8. **Always include a first-visit task list when the pitch is first-touch.** What the tech needs to confirm (existing service amperage, panel manufacturer, major-appliance nameplates, charger location, distance, conduit path, ground type). Do not pretend to have certainty the skill couldn't have.

**Anti-fear-sell rules:**

- Do not frame the NEC 2026 "qualified persons" mandate as "if you don't hire us, your insurance is void" — it isn't, not under every carrier, and that's not your claim to make. Frame it factually.
- Do not describe an FPE/Zinsco/Challenger panel as "illegal" or "a fire waiting to happen." Say the factual record: "known history of breakers that don't trip reliably; most insurers and inspectors flag it on sight."
- Do not claim the 30C credit applies to a customer without verifying the income and census-tract rules. Say "you may qualify, and the installer who files the paperwork for you is the one who'll confirm."
- Do not claim an EVEMS will "always" avoid a service upgrade. It usually does on a 200 A service; on a 100 A service with electric range, dryer, and heat pump, it sometimes can't and the upgrade is real.
- Do not pitch a service upgrade when an EVEMS solution exists. If a previous contractor told the homeowner they needed a $9,500 upgrade and an EVEMS can handle it for $2,000, say so — and say it as a benefit to the homeowner, not a shot at the prior contractor.

## Output Format

Default output is a **one-page homeowner-facing memo** with these sections, in order:

1. **What we're recommending** — One-sentence headline ("A 48-amp hardwired Wallbox Pulsar Plus on an EVEMS, keeping your existing 200-amp panel, total $2,150–$2,400.")
2. **Why this configuration** — 3–5 sentences explaining the service-upgrade-avoidance logic in plain English.
3. **What's included** — Parts, labor, permit, inspection, any utility notification, one-year workmanship warranty (or whatever `config.yml` specifies).
4. **What's not included** — Panel replacement (unless quoted), service upgrade (unless quoted), trenching to detached structures (unless quoted), drywall patching, finish paint, Wi-Fi network connection of a "smart" charger. Exclusions prevent change-orders later.
5. **2026 NEC notes** — One short paragraph. Outdoor emergency-disconnect requirement if the scope touches service equipment. Qualified-persons mandate for EVSE install. EVEMS legality under §625.42 / §120.82(D).
6. **Pricing** — Firm number or firm range. Federal 30C reference only if supplied. State/utility rebate reference only if supplied.
7. **Timeline** — First-available date for the site visit or install, typical permit turnaround in the AHJ, expected total elapsed time.
8. **Next step** — Schedule the site visit / accept the quote / ask questions. One clear ask.
9. **Sign-off** — Owner/tech name, company, license number, phone.

**Three-option pitch** — If the homeowner is comparing options, replace section 1–4 with a 3-column table: Direct Install / EVEMS / Service Upgrade. Each column lists: what it is, cost range, tradeoffs, best-if. Sections 5–9 stay the same.

**Service-upgrade-avoidance memo** — If the homeowner came in with a prior $X upgrade quote, lead with the headline: "You may not need the $9,500 upgrade. Here's why." Show the load-calc comparison in 3 lines (pre-EV load / post-EV load / post-EV load with EVEMS). Recommend the EVEMS path. Price the delta over the direct install. Provide the upgrade path as the fallback with its own number.

Below the main output, include a short **Internal Notes** block that lists:
- Any assumption the skill had to make (service size unknown, appliance list incomplete, jurisdiction NEC cycle unconfirmed)
- Any number that is a placeholder and needs to be replaced with a real figure before the pitch is sent
- The first-visit task list if the pitch is first-touch
- A pointer to `skills/operations/nec-2026-load-calculation-helper.md` for the firm calc
- Any suggested upsell or referral (generator, solar, battery, sub-panel for a detached structure) that's justified by what the homeowner said — never invented

## What NOT to Do

- Do not produce a service upgrade without running the EVEMS check first and showing the math.
- Do not claim NEC-2026 provisions are enforceable in a jurisdiction that hasn't adopted 2026. Say "coming with the 2026 cycle" instead.
- Do not quote a firm 30C tax credit amount without the homeowner's income bracket or the property's census tract. Use the program name and the range.
- Do not use the "qualified persons" rule as a scare tactic against DIY homeowners. Use it as a trust anchor: this is why the install is worth doing correctly.
- Do not describe the FPE/Zinsco/Challenger panel story with adjectives ("dangerous," "fire-prone") — use the factual record instead ("known history of breakers that don't trip reliably; most insurers flag on sight").
- Do not invent panel amperage, appliance kW, or charger specs the intake didn't provide. Leave placeholders and flag them in the internal notes.
- Do not attach NEC articles to the homeowner-facing prose. Section numbers go in the "2026 NEC notes" paragraph only, and even there, translate: "§625.42 — the code section that officially recognizes real-time EV charger throttling."
- Do not recommend a charger brand that isn't on the config's preferred list — unless the homeowner asked by name.
- Do not disparage the prior contractor's upgrade quote. Reframe it: "Their number is reasonable for a clean 200A upgrade. The EVEMS path is newer and wasn't formally in the code until the 2026 cycle."

## Example Output

### Example — Service-upgrade-avoidance memo (post-site-visit, homeowner had a $9,500 upgrade quote from a competitor)

**Input:**
- Homeowner: Dan and Priya Shah, 4217 Willow Ridge Ln, Portland, OR (AHJ on 2026 NEC as of Oct 2025)
- Existing service: 200 A Square D QO, 2004, good condition
- Dwelling: 2,450 sq ft conditioned
- Major loads: gas range, gas dryer, 50-gal electric water heater (4.5 kW), central AC 3 tons, gas forced-air furnace, no pool, no spa
- EVs: 2025 Tesla Model Y LR (single 11.5 kW onboard charger), planning a second EV in ~2 years
- Charger: Wallbox Pulsar Plus Level-2, 48 A, hardwired in attached garage
- Distance: 18 ft from panel to charger location (conduit run through finished ceiling — existing chase)
- Prior quote from competitor: $9,500 for 400 A service upgrade with outdoor disconnect
- Incentives: Federal 30C (pending eligibility confirmation)

**Output — Service-upgrade-avoidance memo:**

> **You may not need the $9,500 upgrade. Here's why.**
>
> Your 200 A Square D panel (2004, good condition) has enough headroom for a 48-amp hardwired Level-2 charger once we pair it with an EV Energy Management System (EVEMS). An EVEMS is a small piece of equipment that watches the rest of the house's draw in real time and throttles the EV charger down briefly when total load gets close to your service limit — you'd never notice, and you'd charge your Model Y fully overnight the way you expect to.
>
> **The load-calc math (rounded, under 2026 NEC Article 120):**
>
> - **Pre-EV load:** ~ 18 kVA calculated — well under your 200 A (48 kVA) service.
> - **Post-EV at 48 A continuous, no EVEMS:** ~ 29.5 kVA — still under 200 A, but tight once a second EV arrives.
> - **Post-EV with EVEMS:** Maximum total load is capped below your service limit by design, so even a second EV in two years wouldn't require an upgrade.
>
> That math is why the competitor's 400 A upgrade quote isn't wrong — it's just sized for a future you don't have to buy today.
>
> **What we'd install.**
>
> - Wallbox Pulsar Plus 48 A Level-2 charger (hardwired, NEMA 3R garage-interior install)
> - EVEMS branch-circuit module (UL-listed per §625.42 / §120.82(D), recognized under NEC 2026)
> - 60 A dedicated circuit from the existing panel, #6 THHN in 3/4" EMT through the existing ceiling chase (~18 ft)
> - Permit, inspection, one-year workmanship warranty
>
> **What's not included.**
>
> - Panel replacement (not needed — your Square D is in good shape)
> - Service upgrade (not needed — EVEMS keeps you inside the 200 A envelope)
> - Drywall patching or paint at the charger mount
> - Wi-Fi network setup on the Wallbox app
>
> **2026 NEC notes.** Your AHJ (Portland) is on the 2026 NEC. Under §625.42 and §120.82(D), an EVEMS-managed EV circuit can legally stay on your existing service without an upgrade — this is a 2026-cycle change that the competitor's quote didn't use. Permanently installed EVSE now requires installation by a qualified person, which is what we do.
>
> **Pricing.** **$2,150–$2,400 all-in** (parts, labor, permit, inspection). You may qualify for the federal 30C alternative-fuel credit (up to 30% of installation cost, certain income and census-tract caps apply); if eligible, we'll file the paperwork for you.
>
> **If you want the upgrade instead,** we can do a clean 200 A → 400 A service upgrade with the 2026-required outdoor emergency-disconnect at **$6,800–$7,400 all-in** (lower than the competitor's $9,500 because we're on the 2026-cycle outdoor-disconnect spec and we'd reuse your existing meter base if the utility clears it). That's a reasonable path if you want to future-proof past your second EV.
>
> **Timeline.** Site prep + permit pull within 3 business days, install in one day, inspection within a week after install — typical total elapsed time ~10 days.
>
> **Next step.** Reply to this email to approve the EVEMS path, or let me know if you want to walk the upgrade option too. Happy to come back out this week either way.
>
> — Mike Torres, Torres Electric (EC-00842) · (503) 555-0142

**Internal Notes:**
- EVEMS recommendation depends on §625.42 / §120.82(D) — Portland AHJ confirmed on 2026 NEC; skill would not produce this pitch in a 2023-only jurisdiction.
- Load-calc numbers are rounded estimates; pass to `skills/operations/nec-2026-load-calculation-helper.md` for the firm calc before the contract goes out.
- Second-EV future-proofing addressed once, non-pushy. Upgrade path offered as fallback, not as up-sell.
- 30C credit mentioned by program name and range only; no dollar figure yet. Confirm homeowner income bracket and census tract before committing a number.
- FPE/Zinsco/Challenger check: N/A (Square D QO, 2004, good condition).
- No safety-stop instruction needed; no existing issue on the panel.
- Follow-up task for dispatcher: pull photo of panel, measure conduit chase distance, confirm meter base compatibility with utility if the homeowner elects the upgrade path.
