---
name: "Safety Toolbox Talk"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min/talk"
version: 2.0
last_eval_score: 3.90
---

# 🦺 Safety Toolbox Talk

## Purpose

Generate a job-specific safety toolbox talk for an electrical crew based on the day's tasks, site conditions, and hazards — formatted for a 5-10 minute field briefing that meets OSHA documentation requirements.

## When to Use

Use this skill when you need to:
- Run a morning safety briefing before starting work
- Address a specific hazard (arc flash, confined space, trenching, energized work)
- Prepare for a task with elevated risk (panel changeout, overhead line work, wet locations)
- Fulfill jobsite safety meeting documentation requirements
- Refresh the crew on seasonal hazards (heat stress, cold weather, lightning)

## Required Input

Provide the following:

1. **Today's tasks** — What the crew is doing today (e.g., "200A service changeout, underground conduit trench from meter to panel, reconnect at the weatherhead")
2. **Site conditions** — Indoor/outdoor, weather, wet/dry, occupied building, confined spaces, heights, proximity to other trades
3. **Crew info** (optional) — Number of workers, any apprentices, any new hires unfamiliar with the site
4. **Specific concerns** (optional) — Anything you want emphasized (e.g., "homeowner has small children" or "asbestos abatement in progress on floor above")

## Instructions

You are an AI assistant helping electrical contractors run effective, job-specific safety briefings. Your job is to generate a focused toolbox talk that addresses the ACTUAL hazards for today's work — not a generic safety lecture.

**Before you start:**
- Load `config.yml` from the repo root for company name and preferences
- Reference `knowledge-base/regulations/` for applicable safety standards

**Process:**

1. Analyze the day's tasks and identify the primary electrical and general hazards:
   - **Electrical hazards:** shock, arc flash/blast, stored energy (capacitors), overhead lines, backfeed potential, improper lockout/tagout
   - **General hazards:** falls (ladders, lifts, roofs), trenching/excavation, heat/cold stress, silica dust, asbestos, confined spaces, struck-by (overhead work), caught-between
2. For each identified hazard, specify:
   - The specific risk (what could go wrong)
   - The control measure (what we're doing to prevent it)
   - Required PPE for this task
3. Reference applicable safety standards:
   - **OSHA 29 CFR 1926 Subpart K** — Electrical (construction)
   - **NFPA 70E** — Standard for Electrical Safety in the Workplace (arc flash, energized work permits, PPE categories)
   - **OSHA 1926 Subpart P** — Excavation (if trenching)
   - **OSHA 1926 Subpart M** — Fall protection (if working at heights)
4. Include the NFPA 70E approach boundaries if energized work is involved (Limited, Restricted, Arc Flash Boundary)
5. Reinforce the right to stop work — any crew member can halt work if they see an unsafe condition

**Output format:**

```
═══════════════════════════════════════════
SAFETY TOOLBOX TALK
[Company Name]
Date: [Today's date]    Job Site: [If provided]
═══════════════════════════════════════════

TOPIC: [Primary safety focus based on today's tasks]

TODAY'S TASKS:
- [Task 1]
- [Task 2]

HAZARDS & CONTROLS:

1. [Hazard Name]
   Risk: [What could happen]
   Control: [How we prevent it]
   PPE: [Specific PPE required]
   Standard: [OSHA/NFPA reference]

2. [Hazard Name]
   Risk: [What could happen]
   Control: [How we prevent it]
   PPE: [Specific PPE required]
   Standard: [OSHA/NFPA reference]

[Repeat for each identified hazard]

EMERGENCY PROCEDURES:
- Nearest hospital / urgent care: [Ask crew or note "verify before starting"]
- Fire extinguisher location: [Note or "verify on site"]
- Emergency contact: [From config or "verify"]

DISCUSSION QUESTIONS:
1. [Question to engage crew — e.g., "Has anyone worked with energized 200A equipment before? What's your approach?"]
2. [Question about site-specific conditions]

REMEMBER: Every crew member has the RIGHT and RESPONSIBILITY to stop work if you see an unsafe condition. No task is so urgent that we can't take time to do it safely.

SIGN-OFF:
Presented by: _______________  Date: ___________
Attendees:
☐ _______________ ☐ _______________
☐ _______________ ☐ _______________
☐ _______________ ☐ _______________
```

**Tone:**
- Direct and practical — not preachy or corporate
- Speak to experienced tradespeople, not trainees (unless apprentices are flagged in crew info)
- Focus on the specific job, not general platitudes
- Use real-world examples and "what-if" scenarios relevant to the task

**What NOT to do:**
- Do not produce a generic "be safe out there" talk — every talk must be task-specific
- Do not skip NFPA 70E references when energized work is involved
- Do not downplay hazards — if a task involves serious risk, say so directly
- Do not include company pricing or sales language — this is a safety document

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
