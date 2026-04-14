# Electrical AI Skills

**Free, open-source AI prompts and workflows built for electricians.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for electrical. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| ⚡ Bid Summary Writer | Turn takeoff data into a clean bid summary with scope, exclusions, and payment terms. | ~30 min/bid |
| 📘 Code Reference Lookup | Answer NEC code questions in plain English with article and section references. | ~10 min/lookup |
| 📄 Change Order Drafter | Document scope changes with cost impact and get customer sign-off faster. | ~15 min/CO |
| 🦺 Safety Toolbox Talk | Generate a job-specific safety briefing based on the day's tasks and conditions. | ~10 min/talk |
| 🔌 Panel Schedule Documenter | Create or update panel schedules from as-built notes and photos. | ~20 min/panel |
| 💬 Customer Explanation Generator | Translate technical findings into homeowner-friendly language for the invoice or email. | ~5 min/job |
| 📝 Scope Letter Drafter | Draft formal scope-of-work letters with inclusions, exclusions, and NEC references. | ~20 min/letter |
| 💡 Estimate Explainer | Translate technical estimates into plain-language summaries for clients. | ~10 min/estimate |
| ⏰ Project Delay Communicator | Draft empathetic delay notifications with mitigation plans for any audience. | ~10 min/notice |
| 🔧 Preventive Maintenance Scheduler | Build structured PM schedules with NFPA 70B references and compliance checklists. | ~45 min/schedule |
| 📋 Material List / BOM Generator | Create organized bills of materials from job descriptions for supply house ordering. | ~30 min/list |
| 🔍 Inspection Report Generator | Produce professional inspection reports from field notes with NEC citations. | ~25 min/report |

**Total time saved per use: ~230+ minutes across all skills.**

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
