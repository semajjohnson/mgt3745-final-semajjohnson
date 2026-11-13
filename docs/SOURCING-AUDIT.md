# Sourcing Audit

## Audit 1, November 2026

### Prediction Stake (commit before any evidence)

**My recorded exit cost for Cloudflare D1 is wrong.** `context/TOOLS.md` rates switching cost from D1 as "Medium — export rows with `wrangler d1 execute` and import them elsewhere." I have never run an export. I predict that testing it will show the exit is worse than Medium: either the command I named is not the one that works, or the output is not in a form another service could import without me writing a converter.

**Resolution: Tight.** It resolves by running an export against the live database and recording what came back. It is wrong if the export succeeds on the first attempt with the command TOOLS.md already names, and produces a file I could load elsewhere without writing anything.

*Committed 2026-11-05, before any evidence row below existed. The weights and this Stake were the only content in that commit.*

### Weights (commit in their own commit, before scoring)

| Criterion | Weight | Why this weight for this project |
|-----------|--------|----------------------------------|
| Exit cost | 35 | The pool's data is the product. Every entry lives in D1 on a free personal account with no contract and no data-processing agreement, and ADR-002 put it there deliberately. If that account or tier goes, this project is whatever export I did or did not make. |
| Institutional fit | 30 | D1 holds the names of real people who never consented to being recorded, which TOOLS.md states plainly and which no vendor agreement here covers. It is the project's sharpest exposure and the first thing a university would ask about. |
| Stability | 20 | The horizon is one semester. "Works unchanged through December 14" is a lower bar than a real product's, so a vendor changing under me costs less here than it normally would. |
| Pricing predictability | 15 | A hard constraint in ADR-001's Gate forbids new spending, but usage is one user on free tiers. The realistic risk is a tier being withdrawn, which lands on exit cost, not a surprise bill. |

Total: 100

### Inventory

Nine rows. `npm ls --all --parseable | wc -l` reports **39 entries**: the root project, one direct dependency, 35 packages wrangler pulls in, and three that nothing declares (see the evidence table).

| Dependency | Kind | Where it is recorded | Ships to users? |
|------------|------|------|------|
| wrangler | dev package (direct) | `package.json` devDependencies, pinned `4.160.0` | No. Deploy tooling only. |
| 35 transitive packages (esbuild, workerd, miniflare, blake3-wasm, unenv, kv-asset-handler and the rest) | dev packages (transitive) | `package-lock.json` | No, with one exception: `workerd` is the runtime the deployed Worker executes on. |
| 3 extraneous packages (`@emnapi/runtime`, `@img/sharp-wasm32`, `tslib`) | packages nothing declares | Present in `node_modules`, in no manifest | No. Nothing imports them. |
| Cloudflare Workers | platform | `worker/wrangler.toml`, `context/TOOLS.md` | **Yes.** Serves every API request. |
| Cloudflare D1 | platform | `worker/wrangler.toml`, `context/TOOLS.md` | **Yes.** Holds every entry and every source name. |
| GitHub, repository and Codespaces | platform | `.devcontainer/devcontainer.json`, `context/TOOLS.md` | **Yes.** Pages serves `index.html`, `app.js`, and `styles.css` to every user; the repository and Codespace are development-only. |
| GitHub Copilot | builder | `context/TOOLS.md`, `.github/copilot-instructions.md` | No, but code it wrote ships. |
| bolt.new (StackBlitz) | builder | `context/TOOLS.md`, HW5 `docs/DDR-001.md` | No, but code it wrote ships. |
| Google AI Studio (Gemini) | builder | `context/TOOLS.md` | No. Nothing it produced is in the current build. |
| Claude | builder | `docs/DDR-001.md`, `docs/DDR-002.md` | No, but documents it drafted are in the repository. |

**Two kinds of dependency this project does not have, confirmed rather than assumed.**

*Zero CDN scripts.* `grep -rn "<script src=" *.html` returns nothing. `index.html` loads `app.js` and `styles.css` from the same origin. STANDARDS.md rule 2 forbids CDN scripts and the build obeys it.

*Zero models at runtime.* `worker/src/index.js` makes no outbound request. Its only bindings are `env.DB` and the request itself, and there is no AI Gateway, no model name, and no API key anywhere in the project. The course template shipped a Gemini proxy with `GEMINI_MODEL`, `CF_ACCOUNT_ID`, and `AI_GATEWAY_ID`; it was deleted in the port commit because FEATURES.md classifies algorithmic suggestion as **Reverse**, meaning it would make the product worse. The cheapest dependency to audit is the one never added.

### Evidence (each fact with its source and the date you read it)

| Dependency | Latest version and date | Maintainers or vendor | License or terms | Limits or price | Other notes | Checked on |
|------------|------|------|------|------|------|------|
| wrangler | `4.160.0`, published 2026-11-10T11:03:24Z (`npm view wrangler version time.modified`) | **One npm publishing account**: `wrangler-publisher <workers-devprod@cloudflare.com>` | MIT OR Apache-2.0 (`npm view wrangler license`) | Free, open source | `npm view wrangler deprecated` returns nothing. Requires Node `>=22.0.0`, which is what actually sets this project's Node floor. On 2026-11-10 I ran a build published 13 hours earlier, because it was unpinned and `npx` fetches latest. | 2026-11-10, 2026-11-12 |
| 35 transitive packages | `workerd@1.20261110.1`, `esbuild@0.28.2` (`npm ls --all`) | Various, not reviewed individually | Not reviewed individually | Free | `workerd`'s version string is a date: 2026-11-10, two days before this audit. `esbuild` fans out to ~15 platform-specific binaries, unmet on this architecture. I chose none of these and have read none of their code. | 2026-11-12 |
| 3 extraneous packages | `@emnapi/runtime@1.11.3`, `@img/sharp-wasm32@0.35.5`, `tslib@2.9.1` | n/a | n/a | n/a | `npm ls` marks all three **extraneous**: installed, declared by nothing. `sharp` is image processing, which this project has no use for. Almost certainly left in the devcontainer image. Nothing imports them, so they ship nowhere, but they are in the environment every build runs in. | 2026-11-12 |
| Cloudflare Workers | Runtime `workerd` 1.20261110.1 | Cloudflare, Inc. | Free personal account. No contract, no data-processing agreement. | **Free plan: 100,000 requests per day, 10 ms CPU per request.** Paid is $0.30 per million requests and $0.02 per million CPU-ms. Source: cloudflare.com/plans | Receives request metadata I did not choose to send: IP address, user agent, timing, paths. | 2026-11-10 |
| Cloudflare D1 | Database `mgt3745-entries`, id `2227fde8-…` | Cloudflare, Inc. | Same free personal account. No DPA. | **Free plan: 5 million rows read per day, 100,000 rows written per day, 5 GB total storage, 10 databases per account.** Source: developers.cloudflare.com/d1/platform/pricing | Holds the names of real people who never consented and have no way to request removal. Export takes the database offline: "during which your D1 database will be unavailable to serve queries." | 2026-11-10 |
| GitHub, repo and Codespaces | n/a | GitHub, Inc. (Microsoft) | Free plan, public repository | Codespaces free hours capped per month on the free plan | Devcontainer uses `javascript-node:22` with no GitHub CLI feature, which is why `gh` is unavailable and PRs are opened through the web UI. | 2026-11-12 |
| GitHub Copilot | n/a | GitHub, Inc. | Free for verified students | Free at current use | `.github/copilot-instructions.md` exists and points at `context/`, but no DDR in this repository cites Copilot for any final-project work. It wrote HW3-era code that still ships. | 2026-11-12 |
| bolt.new (StackBlitz) | n/a | StackBlitz, Inc. | Free tier, consumer terms | Token-limited free tier | Received a full copy of `context/FEATURES.md` during the HW5 delegation and retains it. Nothing in the current build depends on it continuing to exist. | 2026-11-12 |
| Google AI Studio (Gemini) | n/a | Google LLC | Free tier, consumer terms | Rate-limited free tier | Received the specification during HW6. Nothing it produced is in the current build. | 2026-11-12 |
| Claude | Opus 4.5 | Anthropic PBC | Consumer terms | Session-limited | Drafted `docs/AUDIT.md`, both ADRs, and this file. Recorded in DDR-001 and DDR-002 with verification performed. | 2026-11-12 |

### Scores and Rings

Scored 1 to 5 against the weights above, so the maximum is 500.

| Dependency | Stability | Pricing | Institutional fit | Exit cost | Weighted | Ring | Reason in one line |
|------------|---|---|---|---|---|---|---|
| wrangler | 4 | 5 | 4 | 3 | **380** | Adopt | Pinned, permissively licensed, no data crosses it; the 3 on exit is because it is the only way to deploy to Cloudflare at all. |
| 35 transitive packages | 3 | 5 | 3 | 4 | **365** | Adopt | I chose none of them and have read none of them, but they leave when wrangler leaves, so they carry no exit cost of their own. |
| 3 extraneous packages | 3 | 5 | 3 | 5 | **400** | **Remove** | Scores well because they are free, harmless, and trivially removable; removed anyway because no manifest declares them and nothing imports them. |
| Cloudflare Workers | 4 | 3 | 2 | 2 | **255** | Adopt | Low score, deliberate choice: ADR-001 scored it through the Gate and the consequences are recorded. The 2 on exit is real and not yet mitigated. |
| Cloudflare D1 | 4 | 3 | 1 | 3 | **260** | **Trial** | Works in production and the data exports cleanly, but it holds names of people who never consented and have no route to removal, so it runs with a fallback rather than confidence. |
| GitHub, repo and Codespaces | 4 | 4 | 4 | 4 | **400** | Adopt | Highest exit score in the audit: git is portable by construction and nothing here is locked in. |
| GitHub Copilot | 3 | 4 | 3 | 5 | **385** | **Assess** | Score says Adopt and judgment says otherwise: no DDR in this repository cites it, so it is configured but unused, and an unused tool with repository access is worth deciding about rather than leaving on. |
| bolt.new (StackBlitz) | 2 | 3 | 2 | 5 | **320** | **Hold** | No remaining use in this project, and it still holds a copy of my specification. |
| Google AI Studio (Gemini) | 3 | 4 | 2 | 5 | **355** | **Hold** | Same as bolt.new: nothing it produced is in the build, and it received the spec. |
| Claude | 4 | 4 | 3 | 5 | **405** | Adopt | Highest total in the audit. Everything it produced is committed text I reviewed, and nothing breaks if it disappears. |

**Where the weighted total and the ring disagree.** Three rows, and the disagreements are the useful part of this table. The three extraneous packages score 400, higher than Workers and D1 combined, and get Remove: the criteria measure cost and risk, and these carry almost none, but a package that no manifest declares has no business being installed regardless of how harmless it is. Copilot scores 385, Adopt territory, and gets Assess. The score rewards it for being free, permissive, and trivially removable, and none of that answers whether I am using it. No DDR in this repository names it. A tool with read access to everything and no recorded use is a decision I have not made yet, not a tool I have adopted.

The reverse happens with Workers at 255 and D1 at 260, the two lowest real scores in the audit, both of which stay in production. That is the Gate in ADR-001 and ADR-002 outranking this audit, which is correct: those decisions were made deliberately, with the consequences written down, and a sourcing audit is not the place to overturn an architecture decision.

### Exit Plans (every critical dependency, every Trial)

| Dependency | Trigger | Substitute | Hours to swap | Tested? (date and how) |
|------------|---|---|---|---|
| Cloudflare D1 | Free tier stops covering it; or anyone named in the table asks to be removed and I have no mechanism; or the account is lost | SQLite file behind the same Worker interface, or Turso for a hosted equivalent that accepts the same dump | 4 to 6. The data moves in minutes; the Worker's `prepare().bind()` calls are the work. | **Yes, 2026-11-10.** Ran `npx wrangler d1 export mgt3745-entries --remote --output=backup.sql`. Produced a 44-line SQLite dump with `CREATE TABLE` and plain `INSERT` statements, loadable with `sqlite3 new.db < backup.sql` and no converter. See the Stake resolution for what the process cost. |
| Cloudflare Workers | Free tier withdrawn, or 100,000 requests per day exceeded | Any Node or Deno host serving the same five routes; the handler is one file with no Cloudflare-specific imports beyond `env.DB` | 6 to 10. Routing is portable; the D1 binding and the deploy pipeline are not. | No. Untested, and the 2 on exit cost reflects that rather than an estimate I trust. |
| GitHub (repo, Codespaces, and Pages, live 2026-11-12) | Account loss, or Pages withdrawn from free accounts | Any git host; any static host for the three front-end files | 1 to 2. `git remote set-url` and a different static host. | Partially. The repository is cloned locally and on the Codespace, so the git half is demonstrated daily. Pages went live 2026-11-12 and serves the product, but moving the front end to another static host is untested. |
| GitHub Copilot (Assess) | A decision either way: adopt it with a DDR, or remove the pointer file | None needed. Nothing depends on it. | Under 1. Delete `.github/copilot-instructions.md` and the TOOLS.md row. | No test needed; nothing would break. |
| bolt.new (Hold) | Already triggered. No remaining use. | None needed. | 0 to leave. The specification it holds cannot be retrieved. | n/a. The exit is complete except for the copy of FEATURES.md it retains, which I cannot delete. |
| Google AI Studio (Hold) | Already triggered. Nothing it produced is in the build. | None needed. | 0 to leave, same caveat about the retained specification. | n/a |

### Changes Caused by This Audit

- **wrangler pinned to exactly `4.160.0`**, from an unpinned `npx` invocation and HW5's floating `^4`. Justified directly by this audit's evidence: on 2026-11-10 an unpinned run fetched a build published 13 hours earlier. All 7 code evals pass after the change. Commit: the `port/hw5-code` merge.
- **The template's Gemini proxy deleted**, with `GEMINI_MODEL`, `CF_ACCOUNT_ID`, `AI_GATEWAY_ID` and the `<your account id>` placeholders removed from `worker/wrangler.toml`. The project calls no model, and the inventory now says so with evidence instead of carrying a vendor it does not use. Same commit.
- **`evals/pool.test.mjs` renamed to `pool.test.js`.** `package.json` globs `evals/**/*.test.js`, so the `.mjs` file was being skipped silently while EVALS.md cited it as evidence for three acceptance rows. Found in Audit 1 of the Context Scaffold; fixed here. The suite went from 0 tests discovered to 7 passing.
- **`backup.sql` added to `.gitignore`** so a D1 export of real names can never be committed.
- **Three extraneous packages identified for removal**: `@emnapi/runtime`, `@img/sharp-wasm32`, `tslib`. Not yet removed, because they come from the devcontainer image rather than any manifest; removing them means rebuilding the container.
- ADR amended or superseded: none. ADR-001 and ADR-002 both stand. ADR-002's reversal triggers already name this audit's Prediction Stake, and the Stake resolving as it did does not fire any trigger.
- DDR lines added: DDR-001 (Audit 1 fact-gathering), DDR-002 (the ADR split and Gate reconstruction), DDR-003 (this audit). All three in `docs/DDR-LOG.md`.

### Prediction Stake Resolution

**Correct, on one of its two clauses, and the clause it got wrong is the more interesting one.**

*The command was wrong.* TOOLS.md said to "export rows with `wrangler d1 execute`." There is no export in `d1 execute`; the working command is `wrangler d1 export`. The prediction said "the command I named is not the one that works," and it isn't. The sharper detail is that the correct command was sitting in HW5's `package.json` as `db:export` the whole time. The file whose job is to record how to leave had it wrong while a file nobody reads for that purpose had it right.

*The output was fine.* The dump is `PRAGMA`, `CREATE TABLE`, and plain `INSERT` statements. `sqlite3 new.db < backup.sql` loads it, no converter required. The second clause is refuted outright, and I expected it to be the one that held.

**What the test actually showed, which the Stake did not anticipate.** The data is portable and the *process* is not. Reaching that 44-line file took: a documented command that does not exist, an OAuth browser login that cannot complete in a Codespace because Cloudflare redirects to `localhost`, a scoped API token that had to be created from nothing, a credential I then leaked and had to rotate, and a warning that the database goes offline for the duration. Five obstacles, none of them in TOOLS.md's one-word "Medium."

So the honest revision is that **exit cost is two numbers, not one**: Low for the data, High for the first attempt. I scored D1's exit a 3 above on that basis rather than the 4 the clean dump alone would justify. TOOLS.md has been updated to name the correct command and to say that the first export requires an API token.

**On the Stake itself.** It was a good stake because it was cheap to run and it predicted against something I had already written down. It was a weak stake because "worse than Medium" is a vaguer threshold than Tight resolution deserves, and its two clauses were joined by "or," which let it resolve correct on the easier half. A better version would have named one measurable thing: *the first export attempt will fail.* That is unambiguous, and it would also have resolved correct.