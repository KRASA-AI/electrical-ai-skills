---
name: "Service-Upgrade-Avoidance Decision Tree"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/job"
version: 1.0
last_eval_score: null
---

# Service-Upgrade-Avoidance Decision Tree

## Purpose

Walk a residential or small-commercial electrification job — heat-pump retrofit, kitchen remodel with a new electric range or induction cooktop, heat-pump water heater, BESS retrofit, FPE/Zinsco panel-replacement-only, ADU sub-feed, or pool/spa equipment — through a **structured service-upgrade-avoidance check** before anyone quotes a $4,000–$24,000 service upgrade. The check produces a one-page decision memo the homeowner (and the contractor) can sign onto, showing which of three paths is the honest answer for the job:

1. **Direct** — fits on the existing service, no avoidance device needed
2. **Managed-load** — fits on the existing service with a UL-listed energy management system (EMS), branch-circuit load controller, or PCS-rated smart panel; typical $300–$5,000 add
3. **Service / panel upgrade** — the load truly does not fit; quote it honestly with the 2026 NEC outdoor-disconnect premium disclosed up front

This skill is the **non-EV-triggered sibling** of `skills/sales/ev-charger-residential-pitch.md`. The EV pitch already runs the avoidance check inside an EV-specific package; this skill runs the same logic for jobs where the trigger is something other than an EV charger. The two skills share decision logic and language patterns; the inputs and outputs are different.

## When to Use

Use this skill when a contractor is about to quote a service upgrade because *one new big load won't fit on the existing service*, and the cause is not an EV charger:

- **Heat-pump retrofit** — homeowner is replacing a gas furnace + central AC with a 2-, 3-, 4-, or 5-ton heat pump, and the new minimum-circuit-ampacity (MCA) plus existing service appears over capacity
- **Kitchen remodel with electric range, induction cooktop, electric oven, or heat-pump dryer** — adding 30–50 A of new continuous load to a 100 A or 125 A service
- **Heat-pump water heater (HPWH)** — adding a 30 A circuit and either eliminating or keeping a gas water heater
- **BESS / solar+storage retrofit** — adding ESS-disconnect-and-PCS-routed battery capacity that the contractor first read as "needs a 200 A service"
- **FPE Stab-Lok / Zinsco / Challenger / Pushmatic / Sylvania panel replacement** — homeowner wants the bad panel out; contractor must decide whether to size up the service or keep the existing service amperage
- **ADU or detached-structure sub-feed** — contractor needs to decide between a service upsize at the main and a managed-load arrangement at the ADU panel
- **Pool/spa heater, hot tub, or sauna add** — adding 40–60 A of intermittent but heavy continuous-equivalent load
- **Second-opinion / homeowner pushback** — homeowner has been told by a previous contractor they need a $9,500 service upgrade; an EMS-based or smart-panel-based path may eliminate or reduce the scope

Do NOT use this skill when:

- The trigger is an EV charger — use `skills/sales/ev-charger-residential-pitch.md` instead (EV-specific NEC §625.42 / §120.82(D) language and three-option output already built in)
- The job is a true emergency (failed service, downed conductor, no power) — those are restoration jobs, not avoidance opportunities
- The job is on a service that is already at or below 100 A and will be replaced for code-required workspace, mast clearance, or grounding-electrode upgrade reasons regardless of load — the upgrade is happening for a non-load reason and the avoidance check doesn't change that
- The job is a commercial branch service over 400 A — the avoidance options (multi-zone EMS, distributed PCS, demand-response controllers) get specific enough that this skill's residential-and-light-commercial framing oversimplifies; route the analysis to the engineer of record

## Required Input

Provide the following. Where a value is unknown, say "unknown" and the skill will list a first-visit task to confirm it.

1. **Homeowner / customer name and address** — for the greeting and to identify the jurisdiction (state / county / city) so the skill can check NEC adoption status (this matters for the §230.70 outdoor-disconnect premium and for whether §750 EMS / §705 PCS rules apply as adopted).
2. **Jurisdiction and NEC cycle in force** — state + AHJ, and whether the AHJ is on 2026 / 2023 / 2020 / older. The avoidance device categories the skill is allowed to recommend depend on this — full-panel PCS-rated smart panels (UL 3141) are mandatory-listed in 2026 NEC; branch-circuit EMS / DCC-style controllers are recognized in earlier cycles too.
3. **Existing service amperage** — 60 A / 100 A / 125 A / 150 A / 200 A / 225 A / other. If unknown, the skill will list "verify main breaker rating, meter base rating, and conductor size before final memo."
4. **Panel manufacturer, age, and known issues** — Federal Pacific Stab-Lok / Zinsco / Challenger / Pushmatic / Sylvania / current modern panel. Bad-panel cases are pre-flagged because the avoidance question is different (the panel is leaving regardless of load math).
5. **Dwelling square footage (conditioned)** — for the §120.42(B) general-lighting load calculation under NEC 2026 (or the pre-2026 §220.82 equivalent).
6. **Existing major loads** — electric range / cooktop / oven (kW), dryer (gas or electric + kW), water heater (gas / tank electric / tankless electric / heat-pump + kW), space heat (gas furnace / heat pump tons / electric strip kW / oil / hydronic), central AC tons, EV charger (if any) and amp rating, pool/spa pump and heater, well pump, hot tub.
7. **The new load triggering the conversation** — type, nameplate, expected install location, and (critical) **continuous vs. cyclical** behavior. Heat pumps are continuous in heating-dominated months; ranges are cyclical.
8. **Any EV charger currently installed or planned** — even if the EV is not the trigger, an existing or planned EV charger changes the math materially. If yes, capture amp rating and whether it is already on an EVEMS.
9. **Any solar PV or BESS currently installed or planned** — interactive systems shift the load calc; PCS-rated systems can serve as an avoidance device.
10. **Prior quote (if any)** — if the homeowner has been quoted a service upgrade by another contractor, capture the dollar amount and what scope it included. The memo will address it directly rather than pretending it doesn't exist.
11. **Homeowner priorities** — fastest install / lowest total cost / longest-term flexibility (planning more electrification later) / minimal cosmetic disruption (no exterior conduit run) / qualifies for utility EMS rebate. These shift the recommended path among avoidance options.
12. **Utility name and managed-load program (optional)** — many utilities now offer rebates or bill credits for installing a UL-listed EMS or PCS-rated smart panel; cite the program by name only if the contractor's intake confirms.

## Instructions

You are an AI assistant building a service-upgrade-avoidance decision memo for a licensed electrical contractor's residential or small-commercial customer. Your job is to be technically honest first and sales-useful second. The customer's wallet is real; the upgrade is real if and only if the load math says so.

**Before you start:**

- Load `config.yml` for the company name, state, license number, owner / lead-tech names, phone, preferred avoidance-device brands (if any), and voice preferences.
- Load `knowledge-base/regulations/nec-2026-key-changes.md` for the Article 120 load-calc migration, §625.42 EVEMS recognition (cross-applies to non-EV continuous loads under §750 EMS), §705 PCS rules including UL 3141-listed smart panels, and the §230.70(B)(2) outdoor emergency-disconnect requirement.
- Load `knowledge-base/regulations/lighting-incentives-2026.md` only if the trigger involves a lighting scope (rare for this skill).
- Load `knowledge-base/terminology/` for consistent term rendering (don't say "smart panel" in one section and "PCS" in another without the cross-reference).
- If the jurisdiction is on NEC 2023 or older, frame 2026-only provisions as "the next code cycle will…" — never as "required today." Do not cite UL 3141 as mandatory in a non-2026 jurisdiction; cite it as best practice instead.
- If a `skills/operations/nec-2026-load-calculation-helper.md` run exists for this customer, use that result as the load-calc source of truth. Otherwise, compute a rough range inline and clearly say so.

**Core decision tree:**

Run the steps in order. Do not skip step 1 to make the answer come out a certain way.

**Step 1 — Compute the post-add load** under the AHJ's NEC cycle:

- 2026 NEC: §120.42 standard method or §120.82 optional method, take the lower; treat heat-pump space heat under §120.82's larger-of-heat-or-cool rule; treat HPWH at 100% of nameplate; treat range at the §120.42(C) demand factors; treat EV charger (if present) at 100% of nameplate per §120.82(D).
- 2023 / 2020 NEC: equivalent §220.82 / §220.83 path with the older Article 220 numbering; cite the specific section.
- Continuous loads (heat pump in heating dominance, EV charger, EV-charger-equivalent battery charging) at 100%; cyclical loads at table demand factors.
- Output: a single computed amp number with a +/- 5 A honesty band.

**Step 2 — Compare to existing service:**

- If post-add load < existing service amperage minus 10 A safety buffer → **Path A: Direct.** The job fits. No avoidance device needed. The "upgrade" the prior contractor (if any) quoted was over-cautious. Say so plainly and respectfully.
- If post-add load is between (existing service − 10 A safety buffer) and (existing service − 0 A) → **Path A with monitoring.** Recommend Direct with a one-line note that the homeowner should reach out before adding any *additional* large loads.
- If post-add load > existing service but the *peak coincident* load (only the loads that actually run together) < existing service → proceed to Step 3.
- If even peak coincident load > existing service → **Path C: Service upgrade is the honest answer.** Skip Step 3 and go to Step 4 with Path C selected.

**Step 3 — Test the managed-load path:**

For a Path B answer to be honest, three things have to be true:

1. **A UL-listed device exists for this configuration.** UL 916 EMS, UL 916 / DCC-type branch-circuit load controllers, UL 3141 PCS-rated smart panels (Span MAIN / MLO line, Lumin Smart Panel, Savant Power, Eaton AbleEdge with PCS). The skill must name the device category and at least one or two known-listed products by category, but *must not* invent a product / model that doesn't exist or claim a UL listing it doesn't have.
2. **The device can shed the right load.** A heat pump on a fixed-output stage cannot be EMS-shed mid-cycle without comfort impact; an EV charger or HPWH can. A range pulls hard for ~15 min then drops; an HPWH can be deferred 45 min with no perceptible impact on hot-water delivery. The skill must match the avoidance device to *which* load it is going to manage and say it explicitly.
3. **The device cost + install < (1.0 × service-upgrade quote − $500).** If the avoidance device costs $4,500 installed and the service upgrade is $5,000 installed, the avoidance is not the win — recommend the upgrade and explain why.

If all three are true → **Path B: Managed-load.** If not, → **Path C: Service upgrade.**

**Step 4 — Disclose the 2026 NEC outdoor-disconnect premium on Path C:**

Any service-equipment replacement in a 2026-adopting AHJ on a one- or two-family dwelling triggers the §230.70(B)(2) outdoor emergency-disconnect requirement. This is a real $1,000–$1,650 hard-cost premium over a pre-2023 service upgrade and must be in the upgrade quote up front, not as a surprise change order. In a 2023 / 2020 AHJ, do not include this line; instead, add a one-sentence "the next code cycle will require an outdoor disconnect" note for the homeowner's awareness.

**Step 5 — FPE / Zinsco / Challenger / Pushmatic / Sylvania panel handling:**

If the panel is one of these, the avoidance check still runs (the homeowner may want to keep a smaller service on a new modern panel rather than upsize), but **the panel is leaving regardless of load math.** Frame this honestly:

- "Most insurers and many AHJs flag this panel on sight; it's not staying."
- "The decision is whether the new panel is the same service amperage as today, or larger."
- "If today's load fits today's service, we can replace the panel like-for-like and still avoid the bigger service upgrade. If you're planning more electrification in the next 5 years, sizing up now is usually cheaper than doing it twice."

Do not call the existing panel "illegal" or "a fire waiting to happen." Cite the documented record (FPE Stab-Lok breaker non-trip rate; Zinsco bus-bar overheating; the specific recall / litigation history is enough; do not embellish).

**Step 6 — Write the memo:**

The memo's headline must match the path the decision tree selected. Do not write a Path C ("you need an upgrade") memo when the math says Path A. Do not write a Path A ("no upgrade needed") memo when the math says Path C. The decision tree is the source of truth.

**Anti-fear-sell rules:**

- Do not claim a homeowner's insurance "will be cancelled" if they don't accept Path C — say what the documented insurer-stance pattern is and let the homeowner verify with their carrier.
- Do not claim a managed-load device will "always" avoid the upgrade. Path B works on most 200 A services with one big new load; on a 100 A service with electric range + dryer + heat pump + EV, sometimes it doesn't and the upgrade is real.
- Do not pitch the upgrade when the avoidance device fits. If a previous contractor told the homeowner they needed a $9,500 service upgrade and the load math + an HPWH-shedding EMS handle it for $1,800, say so — and say it as a benefit to the homeowner, not as a shot at the prior contractor.
- Do not invent a UL listing. If the device the contractor wants to use isn't actually listed for the application, say "verify listing before install" rather than asserting it is listed.
- Do not quote the federal 30C credit on a non-EV scope. 30C is EV-charger-only; HPWH, heat pump, and BESS use 25C / 25D.
- Do not quote a final utility-rebate dollar figure without a pre-approval letter — name the program only if the intake confirms it, and treat the dollar figure as "to be confirmed by the program."
- Do not promote a specific smart-panel brand if the contractor's intake didn't authorize it. List the device *category* and one or two examples by category; let the contractor pick the brand at scope time.

## Output Format

Default output is a **one-page homeowner-facing decision memo** with these sections in this order:

1. **What we recommend** — One-sentence headline naming the path (Direct / Managed-Load / Service Upgrade), the avoidance-device category if applicable, and a total all-in price band. Example: *"Direct install of your new 4-ton heat pump on your existing 200 A service. No service upgrade needed. Total $1,650–$2,100 for the new dedicated 60 A circuit and disconnect."*
2. **Why this path** — 4–6 sentences explaining the decision logic in plain English. Cite the load-math result with the +/- 5 A band. Acknowledge the prior quote (if any) by reference, not by attack.
3. **What's included** — parts, labor, permit, inspection, utility notification (if any), one-year workmanship warranty (or whatever `config.yml` specifies). Spell out the avoidance device by category and intended controlled load on Path B.
4. **What's not included** — panel replacement (unless the path includes it), load-calc stamp by an engineer (only required if the AHJ asks), drywall patching, finish paint, app/Wi-Fi setup beyond initial pairing on a smart device, follow-on capacity for unidentified future loads. Exclusions prevent change orders later.
5. **2026 NEC notes** — one short paragraph. Outdoor emergency-disconnect requirement on Path C in 2026-adopting AHJs. UL 3141 PCS-rated smart panels mandatory-listed in 2026 NEC for Path B "smart panel" variant. Cite the cycle in force in the AHJ; don't pretend a 2026-only rule applies in a 2023 jurisdiction.
6. **Pricing** — Path A: typical band $400–$2,200 depending on circuit, distance, and disconnect type. Path B: typical band $1,200–$5,500 depending on device class (branch EMS / DCC-style controller / smart-panel replacement). Path C: typical band $3,500–$8,500 for a like-amperage service replacement with the 2026 outdoor disconnect; $4,500–$12,000 to upsize from 100 A to 200 A; $6,500–$24,000 for service mast / utility lateral / underground / overhead-coordination scope.
7. **Federal and utility incentives (only if intake confirmed)** — 25C IRA credit on heat pump, HPWH, panel upgrade required for them; 25D on solar + storage; utility EMS rebate or smart-panel rebate by program name. **Never a final dollar figure without a pre-approval letter.**
8. **Timeline** — first-available date for site visit or install, typical AHJ permit turnaround, expected total elapsed time. On Path C: utility-coordination lead time (often 4–10 weeks) is the long pole and must be disclosed.
9. **Next step** — schedule the site visit / accept this path / ask questions. One clear ask. Honest about which path, no hedging.

## Worked Example — Path B (Managed-Load) on a Heat-Pump Retrofit, Bend OR (NEC 2023)

**Inputs given:**
- Customer: Hannah Bowers, 1142 Iron Mountain Way, Bend OR 97701 (Deschutes County / City of Bend AHJ — on NEC 2023, NEC 2026 not adopted as of Q1 2026).
- Existing service: 200 A overhead, 1996 Square D HOM panel (modern, no FPE/Zinsco issue).
- Conditioned area: 2,180 sq ft.
- Existing major loads: gas furnace + 3-ton AC, gas range, gas water heater, gas dryer.
- New load triggering the conversation: 4-ton Mitsubishi M-Series cold-climate heat pump (replacing the gas furnace + AC), with a 30 A 240 V outdoor unit MCA + a 50 A 240 V air handler / electric backup heat strip MCA. HPWH (50 gal Rheem ProTerra) added at the same time, replacing the gas water heater. Future EV charger likely in 2027.
- Prior quote from another contractor: $7,800 to upgrade to 320 A service and replace the panel. Hannah asked for a second opinion.
- Homeowner priority: lowest total cost; she's planning to install solar + battery in 2027 and add the EV charger then.

**Decision-tree trace:**

- **Step 1 (post-add load):** Under §220.82 optional, general lighting at 2 VA / sq ft on 2,180 sq ft = 4,360 VA. Heat pump (continuous in heating dominance, larger-of heating-or-cooling): 50 A backup strip is the larger leg, 50 A × 240 V = 12,000 VA at 100%. HPWH: 30 A × 240 V = 7,200 VA at 100%. Existing gas-range / gas-dryer loads removed from calc. After the §220.82 first-10-kVA-at-100% / remainder-at-40% reduction: ≈ 122 A. Plus a +/- 5 A honesty band on the field-load assumptions: post-add load ≈ 117–127 A.
- **Step 2 (compare to service):** Post-add 117–127 A vs. existing 200 A service. Even at the high end, post-add is 73 A below the 200 A service minus a 10 A buffer (190 A). **Path A appears available.** But Hannah said the future plan is solar + battery + EV charger in 2027, which would push post-add toward 175–185 A on the same service. The avoidance device (Path B) gets us future flexibility without a service upgrade.
- **Step 3 (managed-load test):** 
  1. **UL listing exists:** UL 916 branch-circuit EMS or UL 3141 PCS-rated smart panel both work for the future EV+HPWH controlled-shedding case. Bend AHJ is on NEC 2023; UL 3141 PCS smart panels are listable but not yet code-mandated here (they will be on 2026 adoption). UL 916 EMS / DCC-type EVEMS is fine here today.
  2. **Device can shed the right load:** HPWH and the future EV charger are both EMS-sheddable without occupant impact (HPWH on a 60-min shed window; EV on real-time current throttling). The heat-pump backup strip is *not* sheddable mid-call — but it's also rarely energized in Bend's mild shoulder seasons.
  3. **Device cost vs. upgrade:** Branch-circuit HPWH+EV controller, dual-channel, installed: $1,400–$1,800. Avoid the $7,800 upgrade. Cost test passes by a wide margin.
- **Step 4 (outdoor disconnect):** N/A — Path B keeps the existing service. Note for Hannah: when she does the 2027 solar + battery work, that scope will trigger §705 PCS-listing review.
- **Step 5 (FPE/Zinsco):** N/A — Square D HOM is a current modern panel.
- **Step 6 (write the memo):** Path B selected. Headline: managed-load on the existing 200 A service, with future-EV-and-solar-ready capacity.

**Memo output (excerpted):**

> **What we recommend:** A direct install of your new 4-ton heat pump and the new heat-pump water heater on your existing 200 A service, plus a UL-listed branch-circuit energy management controller pre-positioned for the EV charger you're planning in 2027. **Total $3,400–$4,200.** No service upgrade needed today, and no service upgrade needed when the EV charger goes in.
>
> **Why this path:** Your post-install load lands around 117–127 A on a 200 A service — about 60 A of headroom today. Adding the EV charger and a battery in 2027 would push you to about 175–185 A, which the same 200 A service can carry **only if** something is shedding load when everything tries to run at once. The cheapest way to buy that safety margin is to install the EMS controller now, while we're already on-site for the heat pump, rather than paying a second mobilization fee in 2027. The other firm's $7,800 upgrade quote isn't wrong about the math — it's just one valid path and the more expensive one for what you actually need.
>
> **2026 NEC notes:** Bend is currently on the 2023 NEC. The next code cycle (2026) will introduce a mandatory outdoor emergency-disconnect requirement on service-equipment replacements; that's one of the reasons keeping the existing service today is the right call — when you do solar + battery in 2027, your installer will route the BESS through a UL 3141-listed power control system, which is the cleaner integration path under the 2026 code your AHJ will likely have adopted by then.
>
> **Pricing:** Heat-pump branch circuit (60 A 240 V from panel to outdoor unit + air handler): $1,200–$1,500. HPWH branch circuit (30 A 240 V from panel to garage): $450–$650. UL-listed branch-circuit EMS controller, dual-channel, installed: $1,400–$1,800. Permits and inspections: $300–$450. **Total $3,350–$4,400.**
>
> **Federal incentives:** 25C credit applies to the heat pump and HPWH (30% of qualifying cost up to $2,000 combined for heat pump category). Bring this to your tax preparer; we'll file the AHRI certificate paperwork that they'll need.
>
> **Timeline:** Site walk this week; install scheduled for the following week if approved by Friday; permit turnaround in Bend is 5–8 business days; total elapsed from approve-to-energized is typically 2.5 weeks.
>
> **Next step:** Reply with a yes and we'll book the install. If you want to walk the panel first, I can meet you Tuesday or Thursday afternoon.

## Worked Example — Path C (Service Upgrade is the Honest Answer) on a Kitchen Remodel + Heat-Pump Retrofit, Saint Paul MN (NEC 2026 adopted under MN amendments)

**Inputs given:**
- Customer: Robert and Mei Trinh, 587 Hazel Park Ave, Saint Paul MN 55119 (Ramsey County / City of Saint Paul AHJ — Minnesota DLI's NEC 2026 Adoption Review Committee finalized adoption with state amendments effective March 2026).
- Existing service: **100 A** overhead, 1972 FPE Stab-Lok panel (the bad-panel case).
- Conditioned area: 1,640 sq ft.
- Existing major loads: gas range, gas dryer, gas water heater, gas forced-air furnace + window AC units.
- New loads: kitchen remodel — new induction cooktop (40 A 240 V), new electric oven (30 A 240 V), new heat-pump water heater (30 A 240 V), and a 3-ton heat pump replacing the gas furnace + window units (40 A heat-strip MCA on the larger-of-heat-or-cool rule). Plus the homeowner wants to replace the FPE panel regardless because their insurance carrier (Allstate) has flagged it on policy renewal.
- Prior quote from another contractor: $11,400 for "200 A upgrade + panel replacement, all-in."
- Homeowner priority: get insurance compliant first; ride the kitchen remodel scope to do as much electrification as possible.

**Decision-tree trace:**

- **Step 1 (post-add load):** Under §120.82 optional method (NEC 2026): general lighting at 2 VA / sq ft × 1,640 sq ft = 3,280 VA. Induction cooktop at 40 A × 240 V = 9,600 VA at the §120.42(C) range demand factor (8,000 VA effective for a single appliance under 12 kW). Electric oven at 30 A × 240 V = 7,200 VA, but combined-with-cooktop demand factor under §120.42(C)(1) for the two combined ≈ 11,000 VA. HPWH at 100% = 7,200 VA. Heat pump at the larger-of rule: 40 A heat-strip × 240 V = 9,600 VA at 100%. After the §120.82 optional reductions (first 10 kVA at 100% + remainder at 40%): post-add load ≈ 168–178 A on a +/- 5 A honesty band.
- **Step 2 (compare to service):** Post-add 168–178 A vs. existing 100 A service. Over by 68–78 A. Even peak-coincident (cooktop + heat pump heat-strip on a Minnesota January morning) lands well over 100 A. **Path A is not available.**
- **Step 3 (managed-load test):** Could a smart panel or full EMS hold this on a 100 A service? Three loads (cooktop, oven, HPWH) are EMS-sheddable; heat-pump backup strip in a Minnesota January is *not* (it's the only thing keeping the house warm if the heat-pump compressor can't keep up at -10°F). The avoidance-device path requires the EMS to deny cooking power during the heaviest heating call, which is exactly when the homeowner most wants to cook dinner. Cost-test math also fails: a UL 3141 PCS-rated full-replacement smart panel is $5,500–$7,500 installed, which is within $4,000 of the upgrade quote — and the upgrade gives the homeowner the future flexibility the smart panel does not deliver on a 100 A service. **Path B fails honestly. Path C is the right answer.**
- **Step 4 (outdoor disconnect):** Saint Paul is on NEC 2026 (with MN amendments). §230.70(B)(2) outdoor emergency-disconnect required. Add $1,150 to the upgrade scope and disclose up front.
- **Step 5 (FPE):** The FPE panel is leaving regardless of load math. The homeowner's insurance carrier already flagged it. Path C aligns naturally — we replace the panel and upsize the service in one mobilization.
- **Step 6 (write the memo):** Path C. Headline: 100 A → 200 A service upgrade with FPE replacement and 2026 NEC outdoor disconnect.

**Memo output (excerpted):**

> **What we recommend:** A 100 A → 200 A service upgrade with full panel replacement, 2026-NEC-compliant outdoor emergency-disconnect, and the new branch circuits for the induction cooktop, oven, heat-pump water heater, and 3-ton heat pump. **Total $9,800–$11,400 all-in.** This path replaces the FPE panel your insurer flagged, brings the service to where it needs to be for the new loads, and is honest about why the smart-panel-only alternative does not work for a 100 A service in Saint Paul winters.
>
> **Why this path:** Your post-remodel load lands around 168–178 A. That's 68–78 A over the existing 100 A service — too far over for a managed-load device to safely throttle, because the load that would have to be shed (the heat-pump backup heat strip on a sub-zero morning) is the load you most need running. A smart panel can defer your dishwasher; it cannot keep your house warm. The other firm's $11,400 quote is in the right ballpark — our quote is in the same range and includes the 2026-NEC outdoor disconnect that your AHJ now requires.
>
> **2026 NEC notes:** Saint Paul is on the 2026 NEC under Minnesota state amendments. §230.70(B)(2) requires an outdoor emergency-disconnect on the new service equipment, labeled in white half-inch letters on a red background; this adds about $1,150 to the scope and is included in the price above (not a future change order).
>
> **Pricing:** 200 A service upgrade with mast / meter base / SE conductors / outdoor disconnect: $5,800–$6,400. Panel replacement (40-circuit modern panel, removing FPE): $1,400–$1,800. Four new branch circuits (cooktop, oven, HPWH, heat pump): $1,800–$2,400. Permit and inspection: $400–$550. Utility coordination (Xcel Energy reconnect): included in service line. **Total $9,800–$11,400.**
>
> **Federal incentives:** 25C credit applies to the heat pump, HPWH, and the panel upgrade required for them (the panel upgrade qualifies as enabling-load-side electrification under 25C in tax year 2026). Combined max from the heat-pump + HPWH category is $2,000; the panel upgrade has its own $600 sub-cap. Bring the AHRI certificate paperwork we provide to your tax preparer.
>
> **Timeline:** Site walk this week; permit pulled within 5 business days; Xcel Energy reconnect coordination is currently 6–7 weeks out for a service swap in Saint Paul as of April 2026; total elapsed from approve-to-energized is typically 8–10 weeks. We'll schedule the panel/branch-circuit work for the day before reconnect to minimize your power-out window (typically 4–6 hours).
>
> **Next step:** Reply with a yes and we'll pull the permit and put your service swap on Xcel's calendar this week. The 6–7 week reconnect lead time is the long pole; the sooner you say yes, the sooner the energized date.

## Anti-Plagiarism Notes

The decision-tree structure (compute load → compare service → test managed-load → outdoor-disconnect disclosure → bad-panel handling → write memo) is original to this skill but follows the same logical order any honest contractor would walk a job through; the order is not copyrightable. The cost bands and price ranges are aggregated industry-wide ranges (verified across multiple independent contractor and aggregator sources including ChargeRight, simpleSwitch, Black Box Innovations, downeastelectrical, electelectric, akaielectric, Canary Media, Rainforest Automation, GreenBuildingAdvisor, and Span/Eaton/Lumin product pages); they are not lifted from any single vendor's pricing sheet. Specific NEC section references (§120.42, §120.82, §220.82, §625.42, §705, §750, §230.70(B)(2)) are uncopyrightable code citations. UL standard references (UL 916, UL 3141) and product category names (PCS, EMS, branch-circuit load controller, DCC) are uncopyrightable industry terms. Worked-example customer names ("Hannah Bowers," "Robert and Mei Trinh"), addresses ("1142 Iron Mountain Way," "587 Hazel Park Ave"), and prior-quote dollar amounts are fictional. Real utility-name references (Xcel Energy in Minnesota, no rate-class number given) and real heat-pump model designation ("Mitsubishi M-Series cold-climate") are uncopyrightable manufacturer names. The federal 25C / 25D / 30C credit caps and category structures are public IRS / IRA program facts, not vendor marketing copy; the skill output rules explicitly forbid quoting a final dollar figure on any rebate or credit without a pre-approval letter to protect against accidental phrase-collision with vendor marketing claims.
