# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this project.

## What this is

Engagement workspace for the **TradeLink Group engagement** — eight sprints inside a
fictional national home-services franchise network: 450 staff, three brands, and six
years of organic growth in the org. It spans three certification tracks at once —
**Advanced Administrator, Sales Cloud Consultant, Service Cloud Consultant** — which
makes it the broadest engagement in the program.

TradeLink Group is fictional; no real customer data is in here.

**Current state: scaffold.** `force-app/`, `seed/` and `evidence/` hold only
`.gitkeep`. Only `sfdx-project.json` (API version 67.0) exists beyond the README.

## The eight sprints, and the order they are in

Trust tickets → security model reset → equipment registry → automation suite →
acquisition data migration → contact centre SLAs → board dashboard → packaged go-live.

The order is the design. **Trust tickets come first** because six years of organic
growth means nobody believes the data, and every later sprint rests on that credibility;
the security reset comes before the automation because automation built on a broken
sharing model has to be rebuilt. Do not reorder sprints for convenience without saying
in the build log what that costs.

## The premise to hold onto: this is a brownfield org

Every other engagement in this program starts from something close to empty. This one
starts from **six years of accumulated configuration across three brands**, and that
changes the default answer to almost every question:

- The first move on any object is to find out what is already there and what still
  fires. Unused fields, dead workflow rules, overlapping automation and orphaned profiles
  are the expected finding, not a surprise.
- **Deleting is a decision that gets recorded**, with what production would have required
  instead (export first, deprecate before delete). Sprint 5's acquisition migration makes
  this acute — migrated data that silently drops a field is the failure mode.
- Three brands means multi-brand design questions are real: record types vs. separate
  objects, shared vs. per-brand page layouts, one queue set or three. Decide explicitly.

## The org

Target org alias **`tradelink`** — a Developer Edition org.

```bash
sf org display --target-org tradelink
sf data query --target-org tradelink --query "SELECT COUNT() FROM Account"
```

Every other org in this program provisioned as US despite the signup form, and needed
Country, locale, time zone and **Currency Locale** corrected to Australian. Currency
Locale is not settable through the API in a single-currency org — Setup → Company
Information → Edit. Check this org and fix it **before seeding anything with an amount
on it**. Stock Salesforce sample data (typically 13 Accounts) should also be purged
before seeding; the pattern is `seed/00-purge-sample-data.apex` in the
**sunrise-solar-internship** repo.

## The division of labour on this engagement

**Hemayet builds all Setup configuration by hand** — the security model, custom objects,
Flows, sharing rules, entitlement processes, dashboards, and the packaging for go-live.
The certifications test Setup navigation and so does the job. Do not build config via
the Metadata API on his behalf unless he asks explicitly.

**Claude does:** seed data (Apex anonymous in `seed/`), including the deliberate defects
and the brownfield cruft the engagement depends on; verification queries; evidence
extraction; code review; deployment mechanics; ERD and documentation drafting; and
playing stakeholders in character for discovery exercises.

## Documentation standards

`deliverables/` is the substance and the interview evidence. The configuration proves
the clicks happened; the documents prove the thinking did.

- **Every change goes in `deliverables/build-log.md`** with its date, the component, the
  change, and the requirement it traces to. Corrections are appended as new rows, never
  edited over.
- **Claim only what was verified** — a query or a screenshot backs every "verified".
- **Accepted risks are recorded, not hidden.** In a brownfield org this section carries
  most of the weight.
- **Dates are Australian** — `dd/mm/yyyy`.
- `evidence/` holds before/after extracts and screenshots per sprint.

## Never commit

Auth files and sfdx auth URLs — an auth URL is a full credential. `.gitignore` covers
`**/*authFile*.json`, `**/*sfdxAuthUrl*`, `.env*`, `.sf/` and `.sfdx/`. A credential
that reaches git history has to be *rotated*, not deleted.

## Agent workflow

Superpowers is expected to be installed as a **user-level plugin**
(`/plugin install superpowers@claude-plugins-official`), not vendored into this repo.
There is no test runner here and most work is Setup configuration, so the red/green TDD
skills have little to bite on; the planning, verification and code-review skills apply
to the seed scripts and the written deliverables.
