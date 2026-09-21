# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this project.

## What this is

Engagement workspace for the **TradeLink Group** engagement — eight sprints inside
a fictional national home-services franchise network: 450 staff, three brands, six
years of organic growth in the org. Hemayet plays the consultant brought in to
clean it up.

Sprint scope: trust tickets, security model reset, equipment registry, automation
suite, acquisition data migration, contact centre SLAs, board dashboard, packaged
go-live.

## The certification tracks

This engagement covers **three** at once — Advanced Administrator, Sales Cloud
Consultant, Service Cloud Consultant. That is deliberate: the scenario is a messy
inherited org, which is exactly where those three syllabuses overlap. When a design
decision could be justified from more than one of them, say which one it is
serving in the build log.

## The org

Target org alias **`tradelink`** — a dedicated Developer Edition org.

```bash
sf org display --target-org tradelink
sf data query --target-org tradelink --query "SELECT COUNT() FROM Account"
```

Every Developer Edition org in this program provisioned as US locale despite
Australia being selected at signup, and arrives carrying 13 stock Salesforce
sample Accounts. Confirm the locale correction and the purge before seeding —
amounts and dates propagate everywhere and are painful to unpick later.

## The division of labour

**Hemayet builds all Setup configuration by hand** — sharing rules, permission
sets, record types, flows, entitlement processes, dashboards. The certifications
test Setup navigation and so does the job. Do not build config via the Metadata
API on his behalf unless he asks explicitly.

**Claude does:** seed data (Apex anonymous in `seed/`), verification queries,
migration scripts, code review, documentation drafting, build-log entries, and
playing stakeholders in character for discovery exercises.

## What makes this engagement different

The premise is an org that **already has six years of accumulated decisions in
it**, not a greenfield build. Two consequences:

- Nothing gets deleted without a written trace of why. The security model reset
  and the acquisition migration are both destructive; each needs a rollback
  position recorded before it runs.
- "Trust tickets" come first in the sprint order for a reason. The engagement
  assumes the business does not currently believe its own data. Work that
  improves the numbers without demonstrating the improvement is worth nothing.

## Repository conventions

| Folder | Contents |
|---|---|
| `force-app/` | Metadata **retrieved from** the org, not authored here |
| `seed/` | Apex anonymous scripts that build starting data, including its deliberate defects |
| `deliverables/` | Design docs, SOPs, analyses, runbooks — the substance |
| `evidence/` | Before/after screenshots and test results, per sprint |

`deliverables/build-log.md` is the spine: every change with date, component, type,
change and the requirement it traces to. It is currently an empty table.

## Current state

Scaffold only — no metadata, no seed scripts, no build-log entries. Sprint 1 has
not started; do not write documentation that implies it has.

## Rules worth enforcing in review

- Deliberate defects in seed data are the exercise. Do not quietly fix data a
  sprint is supposed to find.
- A destructive change without a documented rollback position is not ready.
- Never commit an sfdx auth URL — it is a full credential. See `.gitignore`.
