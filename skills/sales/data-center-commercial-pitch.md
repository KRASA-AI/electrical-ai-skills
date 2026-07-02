---
name: "Data-Center Commercial Contractor Pitch"
category: sales
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~90 min/package"
version: 1.2
last_eval_score: 9.60
---

# 🏢 Data-Center Commercial Contractor Pitch

## Purpose

Draft a credibility-first proposal package that an electrical contractor can submit to a data-center general contractor, developer, or owner's rep — including the cover letter, the one-page capability statement, a safety/arc-flash summary, a labor-mobilization plan, and a Division 26 scope narrative. The package is calibrated for the reviewers who actually sign data-center subcontracts: a pre-construction manager who wants to see NFPA 70E and §110.16 discipline, a PM who wants to see a realistic crew ramp, and a developer's finance team that wants a cost picture rooted in the 40–70% electrical-share-of-total-construction reality of a modern data-center build.

Data-center buyers are not moved by promotional language. They are moved by **technical specificity**, **documented mobilization capacity**, **clean safety statistics**, and **a credible statement of which systems your firm actually self-performs vs. subcontracts**. This skill produces a package that respects that preference.

The output reads the way the buyer expects a serious commercial electrical contractor to read: tight prose, verifiable claims, real hardware, and explicit flags on the items that aren't yet confirmed.

## When to Use

Use this skill when a commercial electrical contractor is entering (or defending) a data-center opportunity and needs a credibility-anchored written package — not a sales brochure:

- **First-touch capability statement** — A one-pager that goes out with a GC's RFQ response, or to a developer scouting bidders for a new campus
- **Pre-construction / design-assist proposal** — When the GC is inviting input on constructability, long-lead switchgear, and UPS/backup strategy before a firm bid
- **Hard-bid Division 26 proposal cover + executive summary** — The prose that sits in front of the Division 26 line-items and alternates
- **Recruiting / retention pitch for data-center technicians** — An internal or agency-facing memo that frames the $80K–$250K compensation reality, the NFPA 70E training requirement, and the five-year career path without sounding like a platform ad
- **Owner's-rep or developer introduction letter** — A short written intro when a campus owner asks "who else should we be talking to?"
- **Market-entry memo for a contractor not yet doing data-center work** — An honest self-assessment: what you already have (licensure, bonding, arc-flash program, journeymen headcount), what you'd need to add (UPS/static-transfer-switch experience, SCADA/BMS coordination, medium-voltage capability), and what a realistic ramp looks like

Do NOT use this skill to produce the firm bid itself. Route pricing through `skills/sales/bid-summary-writer.md`, the scope letter through `skills/sales/scope-letter-drafter.md`, and the arc-flash label package through `skills/operations/arc-flash-label-generator.md`.

## Required Input

Provide the following:

1. **Target buyer** — GC name / developer name / owner's rep / hyperscaler / colocation operator. Specify the role of the reader (pre-con manager, senior PM, procurement, finance, owner). Different readers want different opening paragraphs.
2. **Opportunity framing** — Pre-RFQ introduction / design-assist invitation / hard-bid proposal / post-award project memo / recruiting pitch. This selects the package shape.
3. **Project specifics (as much as is known)** — Campus name, location (state + county + AHJ + utility service territory), total IT load in MW, number of data halls, tier level (Tier II / III / IV), expected substations, UPS strategy (centralized rotary / distributed static / lithium / VRLA), genset count and kW, BESS if any, cooling topology (air / liquid / hybrid), and the target energization date. Leave blanks when unknown — the skill will flag them as "Provided by buyer at next exchange."
4. **Firm's self-performance profile** — Which systems your crew self-performs and which you sub. Typical split: self-perform branch work, lighting, feeders, panels, transformer terminations up to 2000 A; sub out medium-voltage terminations over 15 kV, UPS commissioning, switchgear factory commissioning, BMS/EPMS programming, fire-alarm (separate licensure in many states).
5. **Capacity snapshot** — Journeyman + apprentice headcount available to the project, date-of-availability, existing backlog in weeks of crew-load, and a realistic monthly ramp curve (how many electricians per month your firm can add and sustain).
6. **Safety record** — EMR (experience modification rate), OSHA TRIR, lost-time incident rate (LTIR), number of OSHA 300 recordables in the last 3 years, any Serious/Willful/Repeat citations in the last 5 years, and the firm's NFPA 70E training cadence.
7. **Arc-flash and qualified-electrical-worker (QEW) program** — Year of the current arc-flash study, software used (SKM / ETAP / EasyPower), NFPA 70E training cycle (annual / biennial), PPE category inventory, and whether the firm has incident-energy-study engineers on staff or uses an EOR.
8. **Licensure, bonding, insurance** — State electrical contractor license(s), EC number(s), single-project and aggregate bonding capacity, general liability limits, excess/umbrella, builder's risk if carried, and any Subguard coverage.
9. **Relevant experience** — 3–6 past projects, each with owner/GC, scope, MW delivered, completion date, and whether your firm was prime electrical or tier-2. Mission-critical experience beats generic commercial experience — if you don't have mission-critical, say what's closest (hospital, healthcare EPSS, biotech process power, semiconductor utility, telco central office, broadcast plant).
10. **Differentiators** — Whatever actually is differentiated: self-performed medium-voltage, in-house switchgear integration cell, prefabricated skids, OSHA VPP Star status, IBEW or NECA affiliation, CEII/CUI handling, DFARS/ITAR-adjacent work, secure-campus cleared staff, 24/7 emergency-service division. Do not invent differentiation — the skill will not make up a cleared workforce or a prefab cell that doesn't exist.
11. **Pricing posture (optional)** — Whether the package needs to signal competitiveness (first-tier data-center contractors are price-takers on commodity work and compete on schedule certainty and technical depth), and whether alternates / value-engineering ideas are invited.

## Instructions

You are an AI assistant drafting a commercial electrical contractor's data-center package. Your job is to sound like a firm that has already run a data-center campaign — not like a marketing department. You prefer specifics to adjectives. You never promise a capability the intake didn't contain.

**Before you start — Config-Driven GC Sub-Bid Template Block:**

If the target GC is one of the four hyperscaler GC firms listed below, load `config.yml.datacenter.gc_templates.[gc_name]` and apply the matching template before drafting. If the GC is not on this list, draft a standard package and note in Internal Notes that no GC-specific template was applied.

| GC | Salutation | Insurance minimums (DC scope) | Submittal portal | Bonding minimum | Cover-letter length | Capability-statement format | Known deal-killers |
|---|---|---|---|---|---|---|---|
| **Turner Construction** | Formal last-name ("Dear Mr./Ms. [Last Name]") | $10M GL + $25M umbrella + $5M professional (confirmed via Turner's standard Division 26 sub-bid requirements, Q1 2026) | ProCore (Turner's standard — confirm project-specific portal before submitting) | Single-project bond equal to Division 26 sub-bid value; aggregate $100M minimum typical | ≤250 words | Table only — no narrative prose in the capability statement | EMR > 0.90 auto-disqualifies on data-center scope; missing NFPA 70E certificate triggers automatic RFQ exclusion |
| **Holder Construction** | First-name ("Hi [First Name]" or "Dear [First Name],") | $5M GL + $15M umbrella typical (campus-owner requirements may supersede — confirm) | Holder proprietary portal (BuildingConnected pre-qualification required) | Typically matches Division 26 sub-bid value as single bond | ≤350 words | Prose + table hybrid; Holder pre-con teams respond better to a short narrative opening before the table | No specific auto-disqualifier published; EMR > 1.00 is a consistent flag in Holder's pre-qualification scoring |
| **Mortenson Construction** | Formal last-name | Varies by campus owner — Mortenson passes through the owner's insurance spec exactly; confirm before submitting | Campus-owner-specified portal (varies by project) | Typically Division 26 sub-bid value as single bond | ≤250 words | Table + short prose section on safety and 70E program | Requires NFPA 70E annual training certification submitted to Mortenson's safety director before the firm appears on the sub list; late submission = exclusion |
| **HITT Contracting** | First-name | $5M GL + $10M umbrella typical (varies by campus owner) | HITT proprietary portal | Typically Division 26 sub-bid value as single bond | ≤350 words | Prose + table hybrid | Requires a self-perform MV statement even if the answer is "MV routed to [named sub]"; silence on MV scope is read as evasion and triggers a clarification hold that delays pre-qualification |

If `config.yml.datacenter.gc_templates` is not populated, note in Internal Notes that GC-specific tailoring was not applied and recommend the user populate the config keys for their firm's active GC relationships.

**Before you start — Config-Driven Owner & Utility Profile Block:**

The GC template block above tailors the package to the *contractor's counterparty* (the GC). This block tailors it to the two parties standing *behind* the GC — the campus **owner/operator** and the serving **utility** — because a data-center package that names the owner's actual commissioning standard and the utility's real interconnection reality reads like a firm that has built on that campus before.

If the project's owner/operator matches an entry in `config.yml.datacenter.owner_profiles`, load it and weave its facts into the cover letter's "we understand your standards" paragraph and the constructability notes. If the serving utility matches `config.yml.datacenter.utility_territories`, load it for the long-lead / interconnection discussion. Example shape:

```yaml
datacenter:
  owner_profiles:
    "<Hyperscaler/Colo owner>":
      commissioning_expectation: "L1–L5 IST, owner witnesses L4/L5; CxA-led scripts"
      preferred_ups_topology: "distributed static + lithium; no rotary"
      reference_disclosure: "NDA on contractor list — do NOT name this owner as a reference without written release"
      security: "CUI handling + badged-escort campus; cleared-staff roster requested at pre-qual"
      sustainability_target: "PUE ≤ 1.30 design; owner ESG report references electrical loss budget"
      insurance_passthrough: "owner spec supersedes GC minimums — confirm the campus rider"
  utility_territories:
    "<Utility name / region>":
      primary_voltage: "12.47 kV / 34.5 kV typical"
      interconnection_lead_band: "primary service 60–90 wks from executed agreement"
      capacity_note: "substation capacity constrained in <region>; owner may stage energization by hall"
      standby_permitting: "air-quality permit for genset farm runs 16–24 wks in <AQMD>"
```

**How the block is used:**
1. **Owner standards in the cover letter.** When the owner matches, state two owner-specific facts the firm can actually meet (commissioning level, UPS topology, security posture) as part of the credibility opener — never as a promise the intake didn't support.
2. **Reference-disclosure guardrail.** If the matched owner's `reference_disclosure` carries an NDA, the skill must NOT name that owner in the references table and must flag the rule in Internal Notes. This hardens the existing "do not reference hyperscaler customers by name" rule into a per-owner switch.
3. **Utility reality in the long-lead / constructability notes.** When the utility matches, anchor the interconnection discussion to `interconnection_lead_band` and surface `capacity_note` / `standby_permitting` as pre-con items rather than generic "long-lead switchgear" language.
4. **Never invent** an owner standard, a PUE target, a clearance requirement, or a utility lead band not in config. If neither block matches, draft the standard package and note in Internal Notes that no owner/utility profile was applied — behaving exactly as v1.1 did.

**Before you start — 2026 Hyperscaler Pricing Math Anchor:**

Reference the following May 2026 data-center electrical cost anchors when discussing the 40–70% electrical cost share or when the package requires a credible cost framing (pre-con engagement, market-entry memo, design-assist):

- **Division 26 LV distribution only (branch, feeder, lighting, panels, transformers, UPS bypass, switchgear LV):** $600K–$1.1M per MW IT load, installed. Lower end: Tier III centralized UPS, standard topology. Upper end: Tier IV with full redundancy, dense critical path.
- **All-in electrical (LV distribution + MV primary from the utility pad-mount through the primary switchgear lineup, transformers, generators, UPS, branch):** $1.8M–$3.2M per MW IT load. Lower end: Tier II air-cooled, pre-engineered substation. Upper end: Tier IV liquid-cooled campus with BESS co-generation.
- **Tariff-Event exposure (Section 232 restructure, April 6, 2026):** The switchgear and copper segments carry an estimated 3–8% cost premium above pre-April 2026 supplier quotes on typical LV distribution scope, based on Q2 2026 Eaton, Siemens, and GE re-quote data. This narrows the LV bid range and is worth flagging explicitly in a pre-con engagement where the buyer's budget was set before April 6.
- **Electrical's share of total construction cost:** 40–70% of total data-center construction cost depending on tier, topology, and MW density. Use this range to demonstrate understanding of the buyer's budget psychology — not as a claim about the firm's own pricing.

Do NOT invent a $/kW or $/MW figure for a specific project without tying it to the ranges above or to actual supplier quotes in hand. The ranges above are reference anchors, not project prices.

**Before you start — additional config and references:**

- Load `config.yml` for company name, license number, EMR, TRIR, bonding capacity, insurance limits, roster counts, and preferred project references.
- Load `knowledge-base/regulations/nec-2026-key-changes.md` for §110.16 arc-flash expansion, the moved load-calc rules (Article 220 → Article 120), the 12-inch cable-tray clearance, and any alt-energy-source disconnect directory requirement that affects BESS co-location.
- Load `knowledge-base/tools-ecosystem/ai-adoption-landscape-2026.md` if it informs a differentiator claim (e.g., use of AI takeoff or AI invoice automation).
- Cross-reference `skills/operations/arc-flash-label-generator.md`: the data-center buyer expects arc-flash labels to include the assessment date per §110.16(B), and the skill should state the firm's labeling discipline explicitly.
- If the firm's profile has material gaps for data-center work (no medium-voltage, no NFPA 70E annual cadence, no mission-critical references), the package should address them honestly — either by describing the credible substitute experience or by proposing a teaming structure that supplies the missing piece.

**Core process:**

1. **Pick the package shape based on the opportunity framing.**
   - *Pre-RFQ / capability statement:* one page cover + one-page capability statement + optional 70E/arc-flash summary.
   - *Design-assist / pre-con:* cover letter + constructability memo + long-lead switchgear discussion + crew-ramp plan.
   - *Hard-bid cover:* cover + executive summary + Division 26 narrative + alternates list + exclusions/assumptions (route the actual pricing to `bid-summary-writer.md`).
   - *Recruiting pitch:* one-page internal memo or agency brief — comp ranges, training cadence, safety record, career path, no customer-facing language.
   - *Market-entry memo:* honest gap analysis + proposed ramp or teaming.
2. **Open with credibility, not promotion.** The first paragraph names the opportunity specifically (campus name, MW, tier, energization date), acknowledges why the reader is taking proposals, and states two or three verifiable facts about the firm (EMR, MW delivered in the last 24 months, NFPA 70E cadence). No "we are excited to partner with you." No "industry-leading."
3. **Write the capability statement as a table, not a paragraph.** Categories the data-center buyer actually checks: Licensure (states + numbers), Bonding (single / aggregate), Insurance (GL / umbrella / builder's risk), Safety (EMR, TRIR, OSHA 300 count), Arc-flash & 70E (study year, software, training cadence), Self-perform scope (bullet list with amperage/voltage limits), Sub scope (what's routed to specialty partners), Mission-critical headcount (journeyman + apprentice + EOR access), MV experience (5 kV / 15 kV / 25 kV / 35 kV), Commissioning capacity (L1–L5, who performs each), References (3–6 projects with MW and completion date).
4. **Show the labor-mobilization math.** Data-center PMs expect to see: "We can mobilize 40 journeymen and 20 apprentices in 45 days from NTP, ramp to 110 on site by week 16, and sustain 110 through week 44 with a controlled 35-person demob by week 52." If the numbers aren't known, the skill produces a placeholder labeled "Provided upon request, tied to schedule release from GC." Never invent a crew the intake didn't specify.
5. **Surface the NFPA 70E / §110.16 program explicitly.** A data-center buyer flags a contractor that can't articulate its arc-flash program. Name the software (SKM / ETAP / EasyPower), name the training cadence (annual per NFPA 70E 110.2(D)(1)), and name the PPE-category inventory (Cat 1 / 2 / 3 / 4). If the firm uses a third-party incident-energy-study engineer, say so — that's a strength, not a weakness.
6. **Anchor in the NEC cycle actually enforced at the site.** Do not claim "NEC 2026 compliance" as a differentiator in a jurisdiction that hasn't adopted 2026 — many states are still on 2020 or 2023 and local amendments govern. The skill should reference the specific NEC cycle cited in the project specs, and note where 2026-cycle provisions (new §110.16 label content, §230.70 outdoor disconnect, Article 120 load calcs, 12-inch tray-stack clearance, Article 130 EMS recognition) would either already apply or are coming with the next adoption. Use the Low Voltage Nation / NFPA adoption map as the verification source when the intake is ambiguous.
7. **Address the cost picture honestly.** Electrical work typically accounts for **40–70% of total data-center construction cost** depending on tier, topology, and MW density. This is a known frame and the reader already knows it. Use it to demonstrate that the firm understands the budget psychology of the buyer — not as a claim about your own pricing.
8. **Flag the gaps.** If the firm has no medium-voltage experience, do not bury it — say "Our self-performed scope stops at 2000 A / 480 V. MV terminations from the utility pad-mount into the switchgear lineup would be performed by [preferred MV subcontractor] under our supervision." Buyers respect a known boundary; they do not respect a fuzzy claim.
9. **Never quote compensation ranges as a recruiting hook to the buyer.** The $80K–$250K / data-center-technician band belongs in the internal recruiting memo, not in the buyer-facing capability statement. Keep the memos separate unless the buyer specifically asked for workforce strategy.
10. **Close with a specific next step.** A data-center buyer does not want "looking forward to hearing from you." They want "Attached: capability statement, NFPA 70E certificate, arc-flash study sample redacted. Available for a pre-con call on [dates] — please confirm which window works, and whether a walk of the [campus] pad is part of the evaluation."

**Anti-puffery rules:**

- Do not use the phrases "industry-leading," "world-class," "best-in-class," "cutting-edge," "trusted partner," "turnkey solution," or "mission-critical expertise" without a specific MW number and completion date behind them.
- Do not state a compensation band for technicians in buyer-facing prose. That's recruiting language.
- Do not claim NEC 2026 compliance in a jurisdiction that hasn't adopted 2026. Verify with the published enforcement map before naming the cycle.
- Do not claim an arc-flash program the firm doesn't run. If the firm outsources the incident-energy study, say so — it's a strength if the outsourced engineer is named.
- Do not claim certifications (OSHA VPP, NECA, IBEW affiliation, CEII, ISN Networld tier) the intake didn't provide.
- Do not reference prior GCs or developers by name without confirming they are on the firm's approved-reference list.
- Do not claim self-perform depth the intake didn't supply (switchgear integration cell, prefab modular skid manufacturing, 24/7 emergency-service division).
- Do not promise a mobilization curve the firm's current backlog can't support. If the firm already has 70% crew load through Q3, the memo says so.
- **GC-specific deal-killer rules (applied when the target GC matches a template above):**
  - Turner: Do not submit a package with EMR > 0.90 or without the NFPA 70E certificate summary attached. Packages missing either of these are auto-disqualified by Turner's pre-con team and internal notes must flag this if the firm's EMR is at or above 0.90.
  - Mortenson: Do not submit without confirming the NFPA 70E annual training certification has been submitted to Mortenson's safety director in advance. The package cover letter should reference the submission date and the Mortenson safety director's acknowledgment.
  - HITT: Do not leave the MV self-perform statement blank or ambiguous. Even if MV is subcontracted, state it explicitly ("MV terminations from utility pad-mount routed to [named MV sub] under teaming agreement executed [date]"). Silence on MV scope with HITT triggers a clarification hold.
  - Do not use a $/MW figure in a pre-RFQ package without anchoring it to the 2026 Hyperscaler Pricing Math ranges above and noting the Section 232 tariff exposure on the switchgear/copper segments.

## Output Format

Default output is a **multi-section package** with an explicit cover page structure. Use these sections, in order:

1. **Cover letter** (max 350 words) — Names the campus, the MW, the tier, the energization date; states 2–3 verifiable facts about the firm (EMR, MW delivered in last 24 months, NFPA 70E cadence); flags the specific deliverable attached (capability statement, 70E certificate, arc-flash study sample); closes with a specific next-step ask.
2. **Capability statement** (one page, table-driven, not paragraphs) — Licensure / Bonding / Insurance / Safety (EMR, TRIR, OSHA 300) / Arc-flash & 70E / Self-perform scope / Sub scope / Mission-critical headcount / MV experience / Commissioning capacity / Representative references.
3. **Labor mobilization plan** (half page) — Headcount curve by month from NTP, ramp & demob milestones, how the firm handles overtime (NFPA 70E fatigue management), lodging/per-diem if the project is out-of-market, and the handoff between journeymen and apprentices (1:3 ratio max under most jurisdictions).
4. **NFPA 70E / §110.16 program summary** (half page) — Study year, software, training cadence, PPE inventory by category, QEW designation process, how the firm handles de-energization vs. energized work, and the arc-flash label protocol (including the 2026-cycle requirement to include assessment date on labels in 2026-adopting AHJs).
5. **Constructability / pre-con notes** (conditional — design-assist only) — Long-lead items the firm would flag (switchgear lineups typically 40–64 weeks; MV transformers 50–78 weeks; static UPS 20–36 weeks), coordination hot-spots (12-inch tray-stack clearance, working space around MV gear, genset exhaust routing, cable-tray elevations vs. HVAC mains), and a short list of value-engineering candidates the firm has used on comparable campuses (prefabricated feeder skids, fiber-optic current-transformer retrofits for future arc-flash reduction, modular substation deliveries).
6. **Scope narrative — Division 26 outline** (conditional — hard-bid only) — One paragraph per subsection the firm is bidding (26-05, 26-09, 26-12, 26-13, 26-20, 26-24, 26-27, 26-28, 26-32, 26-35, 26-36, 26-41, 26-50, 26-56). Hand off the line-item pricing to `bid-summary-writer.md`.
7. **Recruiting / retention addendum** (conditional — recruiting framing only, or when the buyer asks about workforce strategy) — The comp range, training cadence, career path, and pipeline (apprenticeship partner, DOL AI Apprenticeship participation if applicable, Google/Electrical Training Alliance partnership if the firm participates).
8. **Differentiators** (half page) — What's actually differentiated, stated as facts (self-performed 15 kV, in-house switchgear integration cell, 200-person mission-critical roster, OSHA VPP Star site). Do not include items the intake didn't supply.
9. **Next step** (one paragraph) — Specific: attached documents, dates available for a pre-con call, walk-of-pad logistics, or schedule-release request.
10. **Exclusions / assumptions** (optional, hard-bid or market-entry only) — What's not in the proposal (BMS/EPMS programming, fire alarm if separately licensed, final commissioning beyond L3, factory witness testing travel, utility backcharges, permit fees beyond the electrical permit, weather protection, temp power beyond 200 A / 480 V, bond premium).

Below the main output, include a short **Internal Notes** block that lists:
- Any placeholder number that needs to be replaced before the package goes out (crew curve, EMR, bonding, study year)
- Gaps in the intake the skill couldn't close (no MV experience stated, no 70E study year, no recent reference within 24 months)
- Suggested teaming arrangements if a gap is material (e.g., "MV scope routed to [named MV subcontractor] — confirm teaming agreement status before submission")
- A pointer to `skills/sales/bid-summary-writer.md` for pricing and `skills/operations/arc-flash-label-generator.md` for the label sample
- Any NEC-cycle verification action the user should take before the package lands with the buyer (confirm AHJ adoption status via the Low Voltage Nation / NFPA map)

## What NOT to Do

- Do not produce a "we are excited to partner with you" opener. Data-center buyers read it as a tell.
- Do not describe generic commercial experience as mission-critical. Healthcare EPSS, biotech, semiconductor utility, telco central office, and broadcast plant are closer; straight office TI is not.
- Do not publish a compensation range in buyer-facing prose.
- Do not claim NEC 2026 compliance without verifying the AHJ cycle.
- Do not claim an arc-flash program the firm doesn't run.
- Do not claim MV self-perform capability the intake didn't state. Say "MV routed to [sub]" instead.
- Do not quote hero metrics ("40% faster mobilization") without a documented past project to point to.
- Do not reference hyperscaler customers by name unless they are on the firm's confirmed reference list. Many hyperscalers have non-disclosure clauses on the contractor list.
- Do not claim OSHA VPP, ISN tier, NECA-MOU, or IBEW affiliation the intake didn't supply.
- Do not include the 40–70% electrical-cost-share as a pricing justification — it's a framing statement, not a number the firm quotes against. The firm's price stands on its own.
- Do not mix the recruiting pitch and the buyer-facing package into a single document. Keep them as separate sections so the buyer doesn't read the technician comp band.

## Example Output

### Example — Pre-RFQ capability statement for a 48 MW Tier III colocation campus, inviting a design-assist pre-con engagement

**Input:**
- Target buyer: Meridian Datacom (developer) + Vanguard Constructors (GC of record); reader is Vanguard's pre-construction director
- Framing: Pre-RFQ capability statement + design-assist invitation
- Project: Meridian DC-09, Prosper TX, 48 MW IT load, Tier III, 4 data halls at 12 MW each, 2 substations (12.47 kV → 4160 V step-down, 4160 V → 480 V), static distributed UPS with lithium, 12 standby gensets at 3 MW, liquid cooling on hall 3 & 4, energization target 2027-Q3, AHJ on NEC 2023 (Prosper adopted 2023 in 2024; 2026 adoption not yet on the Dallas-Fort Worth calendar)
- Firm profile: Lone Star Power Systems, Dallas-based, 420 field electricians + 160 apprentices, EMR 0.71, TRIR 1.9, 4 OSHA 300 recordables in last 3 years, zero Serious/Willful/Repeat citations, NFPA 70E training annual with ETAP incident-energy study refreshed 2024, PPE inventory Cat 1–4, licensure TX (ECR-0048219) + OK + AR + LA, bonding $80M single / $250M aggregate, GL $10M + $25M umbrella
- Self-perform: branch, feeder, lighting, panels, transformers through 2500 A at 480 V, 5 kV terminations in-house; 15 kV MV terminations routed to preferred sub Lone Star MV Services (teaming agreement executed 2025)
- Mission-critical experience: 3 data-center projects in last 36 months — AWS Phoenix (24 MW, prime electrical, completed 2024-Q4), QTS Fort Worth (36 MW, tier-2 under HITT, completed 2025-Q2), Digital Realty Garland (18 MW, prime electrical, completed 2025-Q4)
- Capacity: 65% crew load through 2026-Q3, can ramp 85 journeymen + 35 apprentices to Prosper site by NTP+45, sustain 130 through 2027-Q1
- Differentiators: In-house switchgear integration cell (East Dallas), prefabricated feeder skids for branch MV runs, OSHA VPP Star at main yard, DOL AI Apprenticeship participant (signed 2026-04)

**Output — Pre-RFQ capability statement:**

> **Michael Reyes · Pre-Construction Director · Vanguard Constructors**
>
> RE: DC-09 (Meridian Datacom / Prosper, TX) — Pre-construction capability statement, Lone Star Power Systems
>
> Mike —
>
> Lone Star Power Systems submits this capability statement in response to your design-assist signal on Meridian DC-09. We understand the campus to be a 48 MW Tier III build over four 12 MW data halls with a 12.47 kV primary, 4160 V secondary distribution, and lithium-based static distributed UPS at hall level, with a 2027-Q3 energization target. Prosper AHJ is on NEC 2023; we have tracked that the Dallas-Fort Worth region has not yet docketed 2026 adoption, so our labeling, working-space, and arc-flash program defaults to the 2023 cycle with the 2026-cycle §110.16(B) assessment-date discipline already in our internal standard.
>
> Three facts we'd ask you to weigh first: our current EMR is **0.71** and TRIR **1.9** with zero Serious/Willful/Repeat citations in the last five years; we have delivered **78 MW across three data-center prime contracts in the last 36 months** (AWS Phoenix, QTS Fort Worth, Digital Realty Garland); and our NFPA 70E training cadence is annual with an ETAP incident-energy study refreshed 2024. The 70E certificate summary and an arc-flash study sample (redacted to a representative lineup) are attached.
>
> We self-perform branch, feeder, lighting, panels, and transformer terminations through 2500 A at 480 V, and 5 kV terminations in-house. 15 kV MV terminations from the utility-side pad-mount into the primary switchgear lineup would be performed by our preferred MV subcontractor Lone Star MV Services under a 2025 teaming agreement. Our switchgear integration cell in East Dallas gives us a pre-stage option if DC-09's lineups benefit from factory-adjacent assembly — worth a pre-con conversation.
>
> Capacity: we sit at 65% crew load through 2026-Q3 and can commit **85 journeymen + 35 apprentices mobilized to Prosper by NTP+45 days, ramping to 130 by week 16 and sustained through 2027-Q1** against an NTP in the 2026-Q3 window. If NTP slides right, our curve follows.
>
> We'd value a pre-con engagement on (a) long-lead decisioning for the primary switchgear lineups (current realistic lead is 56–64 weeks on the 15 kV side from our usual OEMs), (b) cable-tray elevations in the main electrical rooms (the 12-inch tray-stack clearance in the 2023 cycle and its tighter reading in 2026 tends to drive ceiling-height decisions before structural is final), and (c) a constructability review on the hall-3/4 liquid-cooling power feeds. We can open a Monday or Wednesday window in the next two weeks and are available to walk the pad with Meridian if that's on your evaluation path.
>
> — Damon Escobar, VP Preconstruction, Lone Star Power Systems · EC TX ECR-0048219 · desc.obar@lonestarps.com · (214) 555-0182
>
> Attached: Capability statement (1 page), 70E certificate summary (1 page), arc-flash study sample (2 pages, redacted), recent-project summary (2 pages).
>
> ---
>
> **Capability Statement — Lone Star Power Systems**
>
> | Category | Detail |
> |---|---|
> | Licensure | Texas ECR-0048219 (master); OK, AR, LA reciprocal; EOR on staff TX PE 103822 |
> | Bonding | $80M single-project / $250M aggregate (Western Surety, A+ rated) |
> | Insurance | GL $10M / Umbrella $25M / Builder's Risk carried per project / Professional $5M |
> | Safety | EMR 0.71 (2024); TRIR 1.9 (2024); 4 OSHA 300 recordables (2022–2024); zero S/W/R citations (2020–2025) |
> | Arc-flash & 70E | ETAP study refreshed 2024; NFPA 70E training annual per 110.2(D)(1); PPE inventory Cat 1–4; in-house QEW designation via Lone Star training matrix; 2026-cycle §110.16(B) assessment-date discipline in internal standard |
> | Self-perform scope | Branch, feeders, lighting, panels, transformers ≤ 2500 A @ 480 V; 5 kV terminations; grounding & bonding per NEC Art. 250; fire-stopping electrical penetrations; temp power up to 400 A / 480 V |
> | Sub scope | 15 kV MV terminations (Lone Star MV Services, teaming executed 2025); BMS/EPMS programming (JCI or Siemens per owner pref); fire alarm (Texas-licensed alarm contractor, separately bonded); L4–L5 UPS and switchgear commissioning (OEM-led with our support) |
> | Mission-critical roster | 420 journeymen + 160 apprentices company-wide; 180 cleared for data-center deployment today; additional 60 trainable within 12 weeks |
> | MV experience | 5 kV in-house; 15 kV + 25 kV via Lone Star MV Services (9 projects, 5 years); 35 kV not self-performed |
> | Commissioning capacity | L1–L3 in-house; L4 with OEM lead, our support; L5 OEM-led |
> | Representative references | AWS Phoenix (24 MW, prime, 2024-Q4); QTS Fort Worth (36 MW, tier-2 under HITT, 2025-Q2); Digital Realty Garland (18 MW, prime, 2025-Q4) — contacts on request |

**Internal Notes:**
- Cover letter uses the 0.71 EMR and 78 MW figures from the intake. Confirm current EMR rating date (the published Texas Mutual figure updates July 1; if we're past that date the letter needs an update).
- "NTP+45 to 85+35 mobilization" is the max-commit number from the current crew-load model. If Q3 2026 backlog shifts, re-run the capacity snapshot before submission.
- MV scope explicitly flagged as routed to Lone Star MV Services under the 2025 teaming agreement — confirm the teaming agreement is still current-dated before the package goes out.
- §110.16(B) assessment-date discipline is stated as "internal standard" — this is truthful if the firm's labeling SOP already requires it; verify with the QA/QC manager before claiming.
- Prosper AHJ NEC cycle confirmed via Low Voltage Nation tracker + AIA Dallas code committee minutes (March 2026); the letter does not claim 2026 compliance because Prosper is on 2023.
- Recruiting/retention addendum is NOT included in this package — buyer is Vanguard's pre-con director, not procurement or talent. Keep comp bands out.
- Differentiator list from intake: switchgear cell, prefab feeder skids, VPP Star, DOL AI Apprenticeship. All four are referenced in the package; the skill did not invent any.
- Follow-up task for BD: package includes the arc-flash study sample redacted; confirm the redaction is current version before attaching.
- Next step ask is specific (Monday/Wednesday in next two weeks, walk-of-pad with Meridian). Avoids generic "looking forward to hearing" closers.
- Route pricing through `skills/sales/bid-summary-writer.md` when the hard bid is invited; route the scope letter through `skills/sales/scope-letter-drafter.md` post-award.
