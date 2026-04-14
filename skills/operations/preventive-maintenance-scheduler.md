---
name: "Preventive Maintenance Schedule Generator"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/schedule"
version: 1.0
last_eval_score: null
---

# ⚡ Preventive Maintenance Schedule Generator

## Purpose

Generate a structured preventive maintenance (PM) schedule for commercial or residential electrical systems — covering panels, switchgear, transformers, generators, lighting, fire alarm circuits, and emergency power systems. Output is a calendar-ready maintenance plan with task descriptions, frequencies, and compliance references.

## When to Use

Use this skill when you need to:
- Create or update a PM program for a commercial building or facility client
- Propose a recurring maintenance contract to a property manager or building owner
- Document maintenance schedules for insurance, warranty, or code compliance
- Transition a reactive-maintenance client to a proactive PM program
- Prepare for an annual electrical system review or infrared thermography scan

## Required Input

Provide the following:

1. **Facility type** — Commercial office, retail, industrial, multifamily, healthcare, data center, warehouse, etc.
2. **Key electrical systems** — List major equipment: main switchgear rating, number of panels, transformer(s), generator(s), UPS, fire alarm, emergency lighting, EV chargers, etc.
3. **Building age / last major upgrade** — Approximate year built or last electrical renovation
4. **Occupancy and criticality** — Hours of operation, whether 24/7, any critical loads (medical, server rooms, refrigeration)
5. **Existing maintenance history** (optional) — Any known recurring issues, recent failures, or inspection findings
6. **Contract scope** (optional) — Whether this is for an internal maintenance team or a service contract proposal

## Instructions

You are an AI assistant helping electrical contractors build professional preventive maintenance schedules. Your job is to generate a thorough, actionable PM plan tailored to the specific facility and its electrical systems.

**Before you start:**
- Load `config.yml` from the repo root for company name and preferences
- Reference `knowledge-base/regulations/` for applicable NEC articles and NFPA 70B / NFPA 70E requirements
- Reference `knowledge-base/best-practices/` for industry PM standards

**Generate a PM schedule with these sections:**

### 1. Facility Summary
- Brief description of the facility, its electrical infrastructure, and any critical systems
- Note any special requirements (healthcare, hazardous locations, mission-critical power)

### 2. Maintenance Task Matrix
For each major system component, provide:

| Component | Task Description | Frequency | Estimated Duration | NEC / NFPA Reference | Priority |
|-----------|-----------------|-----------|-------------------|---------------------|----------|

**Standard task categories to evaluate:**
- **Visual inspection** — Look for signs of overheating, corrosion, water intrusion, physical damage, code violations
- **Thermal scanning** — Infrared thermography of panels, switchgear, connections, breakers under load
- **Torque verification** — Re-torque connections per manufacturer specs (main lugs, bus connections, breaker terminals)
- **Testing** — Breaker trip testing, ground-fault testing, arc-fault testing, emergency generator load bank test, transfer switch operation, battery load test
- **Cleaning** — Vacuum panels and enclosures, clean contacts, remove debris from electrical rooms
- **Labeling and documentation** — Verify panel schedules are current, update circuit directories, confirm arc flash labels

**Frequency guidelines:**
- Monthly: Visual inspections of critical systems, generator exercise runs, emergency lighting spot checks
- Quarterly: Thermal scans of main distribution, battery inspections, ground-fault testing
- Semi-annually: Full panel inspections, torque checks on high-amperage connections, transfer switch testing
- Annually: Comprehensive system survey, breaker trip testing, full arc flash study review, fire alarm circuit verification
- As-needed: After storms, power events, or equipment additions

### 3. Seasonal Considerations
- Note any weather-related tasks (storm prep, lightning protection checks, HVAC load transition inspections)
- Flag seasonal load changes that affect maintenance timing

### 4. Compliance Checklist
- List applicable NFPA 70B (Recommended Practice for Electrical Equipment Maintenance) tasks
- Note OSHA requirements for electrical safety and lockout/tagout documentation
- Flag any AHJ-specific inspection requirements mentioned by the user
- Reference arc flash study currency (NFPA 70E requires review when modifications occur)

### 5. Recommended Contract Structure (if applicable)
- Suggest tiered service levels (basic visual + thermal, standard with testing, comprehensive with predictive analytics)
- Estimate annual visit count and labor hours
- Note upsell opportunities (arc flash study, power quality monitoring, thermal imaging report)

### 6. ROI Justification
- Provide 2-3 data points on PM cost savings vs. reactive maintenance
- Note insurance premium reduction potential
- Reference equipment lifespan extension from regular maintenance

**Formatting rules:**
- Use clear tables for the maintenance matrix
- Include NEC article numbers and NFPA references where applicable
- Write task descriptions that a journeyman electrician can execute without ambiguity
- Flag any tasks requiring a licensed professional engineer (PE) sign-off

## Example Output

*(Abbreviated — full output would include complete task matrix)*

**Facility:** 50,000 sq ft Class A office building, built 2015. 1600A main switchgear, (6) 225A distribution panels, 250 kW standby generator with ATS, 100 kW UPS for server room, fire alarm system.

**Sample Maintenance Tasks:**

| Component | Task | Frequency | Est. Duration | Reference | Priority |
|-----------|------|-----------|--------------|-----------|----------|
| Main switchgear | Infrared thermography scan under 60%+ load | Quarterly | 1.5 hrs | NFPA 70B 11.17 | High |
| Main switchgear | Torque verification — bus and lug connections | Annually | 3 hrs | NEC 110.14, mfr specs | High |
| Distribution panels | Visual inspection — signs of overheating, corrosion, loose wiring | Quarterly | 30 min/panel | NFPA 70B 11.7 | Medium |
| Standby generator | Exercise run under load, check fuel and coolant | Monthly | 1 hr | NFPA 110 8.4 | High |
| ATS | Simulate power failure, verify transfer and retransfer | Semi-annually | 2 hrs | NFPA 110 8.4 | High |
| UPS system | Battery load test, check connections and ventilation | Quarterly | 1.5 hrs | IEEE 1188 | High |
| Emergency lighting | 30-second functional test of all units | Monthly | 1 hr | NEC 700.12, local code | Medium |
| Emergency lighting | 90-minute full discharge test | Annually | 3 hrs | NEC 700.12 | High |
| Fire alarm circuits | Verify circuit integrity, check for grounds | Annually | 2 hrs | NFPA 72 | High |
| All panels | Update panel schedules and circuit directories | Annually | 4 hrs | NEC 408.4 | Medium |
