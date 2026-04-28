# Electrical AI Skills

**Free, open-source AI prompts and workflows built for electricians.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for electrical. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| Arc Flash Label Generator | Generate NEC 2026 §110.16-compliant arc flash hazard labels for service equipment, feeders, and other covered gear. | ~20 min/label set |
| Code Reference Lookup | Answer NEC code questions in plain English with article and section references, practical application guidance, and **explicit code-cycle awareness** — so a journeyman, master electrician, EOR, AHJ, or estimator can quickly verify a requirement without flipping through three different code books for three different jurisdictions. | ~10 min/lookup |
| Inspection Report Generator | Generate a professional electrical inspection report from field notes, photos, or verbal observations — formatted for the property owner, general contractor, insurance company, or AHJ (Authority Having Jurisdiction). | ~25 min/report |
| Material List / BOM Generator | Generate an organized bill of materials (BOM) or material pick list from job scope notes, work orders, or verbal descriptions of electrical work — formatted for supply house ordering, truck stocking, or estimate backing. | ~30 min/list |
| NEC 2026 Load Calculation Helper | Walk an electrical contractor through a residential or small-commercial service and feeder load calculation under the 2026 NEC — which reorganized load-calc rules out of Article 220 and into the new **Article 120**, lowered the first demand tier from 10,000 VA to 8,000 VA, dropped the general-lighting density from 3 VA/sq ft to 2 VA/sq ft, moved EVSE to 100% of nameplate under §120.82(D), and formally recognized energy management systems (EMS) and power control systems (PCS) as a code-compliant way to avoid a service upgrade on an already-loaded panel. | ~45 min/calc |
| Panel Schedule Documenter | Create or update a formatted panel schedule from as-built notes, photos, or verbal descriptions — producing a clean, NEC **408.4**-compliant circuit directory ready for the panel door or close-out package, plus the required cross-reference to the **arc-flash warning label** (NEC 2026 §110.16(B)/(C)) on every commercial / industrial panel that's likely to be examined or serviced while energized. | ~20 min/panel |
| Preventive Maintenance Schedule Generator | Generate a structured preventive maintenance (PM) schedule for commercial, healthcare, multifamily, or light-industrial electrical systems — covering panels, switchgear, transformers, generators, lighting, fire alarm circuits, emergency power systems, MCCs, and VFDs. | ~45 min/schedule |
| Safety Toolbox Talk | Generate a job-specific safety toolbox talk for an electrical crew based on the day's tasks, site conditions, and hazards — formatted for a 5-10 minute field briefing that meets OSHA documentation requirements. | ~10 min/talk |
| Bid Summary Writer | Turn takeoff data, labor estimate, and scope notes into a clean, GC-ready electrical bid summary — organized by CSI division, with base bid, alternates, allowances, unit prices, exclusions, assumptions, bid validity, bonding, and escalation clauses spelled out. | ~30 min/bid |
| Commercial Lighting Retrofit Pitch | Produce a one-page (or short-deck) buyer-facing pitch for a commercial LED lighting retrofit, tenant-improvement (TI) lighting package, or new-construction lighting-controls scope under 2026 conditions. | ~45 min/pitch |
| Data-Center Commercial Contractor Pitch | Draft a credibility-first proposal package that an electrical contractor can submit to a data-center general contractor, developer, or owner's rep — including the cover letter, the one-page capability statement, a safety/arc-flash summary, a labor-mobilization plan, and a Division 26 scope narrative. | ~90 min/package |
| EV Charger Residential Pitch | Produce a one-page homeowner-facing pitch for a residential Level-2 EV charger installation that (1) opens with the right conversation for the homeowner's situation rather than a generic "we install EV chargers" brochure, (2) accurately sizes the job under the **2026 NEC** rules — Article 120 load calc, §120.82(D) 100%-of-nameplate for EVSE, §625.42 EVEMS recognition, the new §230.70 outdoor service-disconnect requirement, and the §120.82-"qualified persons only" mandate — and (3) always runs the **service-upgrade-avoidance check** before pitching an upgrade, because an EV Energy Management System (EVEMS) can often add a Level-2 charger to an existing 100 A or 200 A service without touching the panel. | ~25 min/pitch |
| Scope Letter Drafter | Draft a formal scope-of-work letter from rough job notes — the document that sits under the bid number, defines exactly what the electrical contractor is and is not responsible for, and becomes the first thing lawyers, GCs, and owners pull when a disagreement surfaces six months into the project. | ~25 min/letter |
| Service-Upgrade-Avoidance Decision Tree | Walk a residential or small-commercial electrification job — heat-pump retrofit, kitchen remodel with a new electric range or induction cooktop, heat-pump water heater, BESS retrofit, FPE/Zinsco panel-replacement-only, ADU sub-feed, or pool/spa equipment — through a **structured service-upgrade-avoidance check** before anyone quotes a $4,000–$24,000 service upgrade. | ~30 min/job |
| Customer Explanation Generator | Translate technical electrical findings, repairs, and recommendations into plain-English explanations that homeowners, tenants, property managers, and non-technical GCs can actually understand — without dumbing it down, scaring them, or hiding the why. | ~5 min/job |
| Emergency Call Triage Script | Produce a structured after-hours / on-call triage script that a human dispatcher, an AI answering agent, or a voicemail-override system can follow the same way every time — so that a **life-safety event** (sparking panel, burning smell near service equipment, energized water, shock-on-contact, smoke from an outlet, oxygen-dependent occupant in a total outage) produces an immediate dispatch, and a **nuisance event** (one breaker tripping that resets cleanly, one outlet not working, a switch that stopped working, a flickering light) gets scheduled for the next business day with safe-interim guidance for the caller. | ~15 min/after-hours call + avoided nuisance truck-rolls |
| Estimate Plain-Language Explainer | Translate a technical electrical estimate — the line items an estimator writes for themselves — into a clear, jargon-free explanation the customer can sit with, compare against a competing bid, and approve without a follow-up phone call. | ~15 min/estimate |
| Project Delay Communicator | Draft the message an electrical contractor has to send when a job slips — switchgear lead time extended, rough-in inspection failed, utility meter-set missed, GC pushed a related trade, storm day, apprentice out sick, permit stuck in AHJ backlog, supplier swapped a substitute that arrived wrong. | ~15 min/message |
| Voice Notes to Service Report | Convert a messy, truck-recorded voice transcript from a field electrician into a clean, customer-facing service report (invoice narrative, follow-up email, or ticket closeout) without losing technical accuracy, inventing work that didn't happen, or leaking trade shorthand into homeowner-facing text. | ~20 min/ticket |
| Warranty Callback Analyzer | Take a customer callback on previously completed work — a homeowner reporting a tripping breaker on the panel the contractor changed last fall, a property manager saying "the receptacle you replaced is loose," a tenant calling about a non-working light circuit five months after the TI buildout — and walk it through a structured analysis that produces four artifacts the contractor's office actually needs: | ~25 min/callback |
| Change Order Drafter | Draft a complete, signature-ready electrical change order that captures scope change, labor/material cost impact, schedule impact, and code/NEC triggers — in the format GCs, owners, and inspectors expect. | ~15 min/CO |
| Contract Risk Reviewer | Review a prime contract, subcontract, purchase order, master service agreement, or service contract for risk exposure to your electrical business before you sign. | ~60 min/contract |
| Material Tariff & Escalation Clause Drafter | Draft the proposal-, bid-, or contract-attachment language an electrical contractor needs in 2026 to keep margin from leaking out the back of a quote when copper, aluminum, steel, switchgear, panelboards, or transformers move on price after the proposal date. | ~40 min/proposal |
| Email Drafter | Turn rough notes into a professional, trade-appropriate email for customers, GCs, property managers, inspectors, suppliers, tenants, or utility coordinators — matching your company's voice and the conventions electrical contractors actually use. | ~10 min/use |
| Meeting Summarizer | Turn raw meeting notes (typed, bullet-point, or transcript) into a clean, action-oriented summary for the kinds of meetings electrical contractors actually hold — pre-job walks, GC coordination, submittal reviews, inspection debriefs, safety committee meetings, and weekly ops huddles. | ~15 min/meeting |
| Review Responder | Craft a professional, platform-appropriate response to an online review — positive, negative, or somewhere in between — that reinforces trust, protects your license, and reads like a real person wrote it. | ~10 min/review |

**Total time saved per use: ~695+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/electrical-ai-skills.git
cd electrical-ai-skills
```

### 2. Open a skill with your AI assistant

Open any file in `skills/` with Claude, ChatGPT, or any major AI assistant. Each skill is a self-contained prompt with clear instructions — no coding required.

The first time you use a skill, your AI assistant will ask for your business details (company name, service area, rates, tools you use, etc.) so it can personalize the output. Save those details to a `config.yml` at the repo root and every future skill will use them automatically.

## Repo Structure

```
electrical-ai-skills/
├── knowledge-base/          # Industry context and references
│   ├── industry-overview.md # Market trends and pain points
│   ├── terminology/         # Industry jargon and acronyms
│   ├── regulations/         # Compliance requirements
│   ├── best-practices/      # Industry standards
│   └── tools-ecosystem/     # Common software and tools
├── skills/                  # The prompt library
│   ├── operations/          # Day-to-day operational skills
│   ├── sales/               # Sales and lead management
│   ├── admin/               # Administrative and compliance
│   └── customer-service/    # Client-facing communication
└── outputs/                 # Your generated content (gitignored)
```

## How Skills Work

Each skill file is a Markdown document with YAML frontmatter:

```markdown
---
name: Skill Name
category: operations
tools: [claude, chatgpt]
time_saved: "~20 min/use"
version: 1.0
---

# Skill Name

## Purpose
What this skill does and when to use it.

## Instructions
Step-by-step prompt for the AI assistant.
```

You open the file in your AI assistant, provide any required input (measurements, notes, client info), and get polished output. Skills reference your `config.yml` automatically for company name, rates, preferred formats, and other business details.

## For AI Assistants

If you are an AI assistant reading this repo, see `.claude/CLAUDE.md` for full instructions. The short version:

1. **Check for `config.yml`** at the repo root. If it exists, load it — it holds the user's business context (company name, rates, service area, tools, team size, etc.) and every skill should use it for personalization.
2. **If `config.yml` is missing**, before running a skill that benefits from personalization, ask the user for the relevant business details and offer to save them to `config.yml` so future runs are automatic.
3. **Load the relevant `knowledge-base/` files** for industry terminology, regulations, and best practices before generating output.
4. **Run the requested skill** from `skills/` using the user's input.
5. **Save any deliverables** to `outputs/` (gitignored) if the user wants to keep them.

## Learn More

- **Electrical AI guide**: [krasa.ai/industries/electrical](https://krasa.ai/industries/electrical)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
