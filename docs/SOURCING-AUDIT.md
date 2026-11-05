# Sourcing Audit

## Audit 1, November 2026

### Prediction Stake (commit before any evidence)

**My recorded exit cost for Cloudflare D1 is wrong.** `context/TOOLS.md` rates switching cost from D1 as "Medium — export rows with `wrangler d1 execute` and import them elsewhere." I have never run an export. I predict that testing it will show the exit is worse than Medium: either the command I named is not the one that works, or the output is not in a form another service could import without me writing a converter.

**Resolution: Tight.** It resolves by running an export against the live database and recording what came back. It is wrong if the export succeeds on the first attempt with the command TOOLS.md already names, and produces a file I could load elsewhere without writing anything.

### Weights (commit in their own commit, before scoring)

| Criterion | Weight | Why this weight for this project |
|-----------|--------|----------------------------------|
| Exit cost | 35 | The pool's data is the product. Every entry lives in D1 on a free personal account with no contract and no data-processing agreement, and ADR-002 put it there deliberately. If that account or tier goes, this project is whatever export I did or did not make. |
| Institutional fit | 30 | D1 holds the names of real people who never consented to being recorded, which TOOLS.md states plainly and which no vendor agreement here covers. It is the project's sharpest exposure and the first thing a university would ask about. |
| Stability | 20 | The horizon is one semester. "Works unchanged through December 14" is a lower bar than a real product's, so a vendor changing under me costs less here than it normally would. |
| Pricing predictability | 15 | A hard constraint in ADR-001's Gate forbids new spending, but usage is one user on free tiers. The realistic risk is a tier being withdrawn, which lands on exit cost, not a surprise bill. |

Total: 100

### Inventory

| Dependency | Kind (package, dev package, platform, model, builder, CDN script) | Where it is recorded | Ships to users? |
|------------|------|------|------|

### Evidence (each fact with its source and the date you read it)

| Dependency | Latest version and date | Maintainers or vendor | License or terms | Limits or price | Other notes | Checked on |
|------------|------|------|------|------|------|------|

### Scores and Rings

| Dependency | Stability | Pricing | Institutional fit | Exit cost | Weighted | Ring | Reason in one line |
|------------|---|---|---|---|---|---|---|

### Exit Plans (every critical dependency, every Trial)

| Dependency | Trigger | Substitute | Hours to swap | Tested? (date and how) |
|------------|---|---|---|---|

### Changes Caused by This Audit

- 

### Prediction Stake Resolution

<Unresolved. Resolve after the evidence and the exit test.>
