# TOOLS.md

The ledger of Trust Boundary crossings. One row per external service this
repository depends on. Read by the agent on every task, so keep it short: a
service not in use does not belong here.

Never put a credential in this file. A key, token, or password anywhere in
the repository is graded as a security failure regardless of the rest.

Rings, limits, and exits come from `docs/SOURCING-AUDIT.md`, Audit 1, dated 2026-11-12. The crossing statements below the table are the longer-form record: each answers in one first-person sentence what crosses, to whom, and who is accountable.

## The ledger

| Tool | Purpose | Ring | Checked on | Limit or price | Exit |
|---|---|---|---|---|---|
| Cloudflare Workers | Runs the API that answers GET and POST for the pool | Adopt | 2026-11-10 | Free plan: 100,000 requests/day, 10 ms CPU per request. Paid: $0.30 per million requests | Rewrite the five routes for another runtime and repoint `apiBase` in `app.js`. 6 to 10 hours. **Untested.** |
| Cloudflare D1 | The SQL database holding pool entries, source names, and mood labels | **Trial** | 2026-11-10 | Free plan: 5M rows read/day, 100,000 rows written/day, 5 GB storage, 10 databases per account | `npx wrangler d1 export mgt3745-entries --remote --output=backup.sql`, then load the dump into SQLite or Turso. 4 to 6 hours. **Tested 2026-11-10.** |
| GitHub, repository and Codespaces | Hosts the repository and the development environment; will serve the static files via Pages | Adopt | 2026-11-12 | Free plan, public repository. Codespaces hours capped monthly | `git remote set-url` to any host; any static host for the three front-end files. 1 to 2 hours. |
| wrangler (npm) | Creates the D1 database, deploys the Worker, runs exports | Adopt | 2026-11-12 | Free, MIT OR Apache-2.0. Pinned to `4.160.0` | Deploy through the Cloudflare dashboard instead, or leave when Cloudflare leaves. |
| 35 transitive npm packages | Pulled in by wrangler; `workerd` is the runtime the deployed Worker executes on | Adopt | 2026-11-12 | Free | They leave when wrangler leaves. No exit of their own. |
| GitHub Copilot | Code suggestions inside the Codespace | **Assess** | 2026-11-12 | Free for verified students | Delete `.github/copilot-instructions.md` and this row. Under 1 hour. Nothing depends on it. |
| bolt.new (StackBlitz) | Built the mood-label feature delegated in HW5 | **Hold** | 2026-11-12 | Token-limited free tier | Already exited. Nothing in the build depends on it. It retains a copy of FEATURES.md that I cannot delete. |
| Google AI Studio (Gemini) | Built the same feature for the HW5 comparison | **Hold** | 2026-11-12 | Rate-limited free tier | Already exited. Nothing it produced is in the build. Same retained-specification caveat. |
| Claude | Drafted the scaffold audit, both ADRs, and the sourcing audit | Adopt | 2026-11-12 | Session-limited | Everything it produced is committed text I reviewed. Nothing breaks if it disappears. |

**Not in this ledger, confirmed rather than assumed.** No CDN scripts: `grep -rn "<script src=" *.html` returns nothing. No model at runtime: `worker/src/index.js` makes no outbound request and the project holds no API key. The course template shipped a Gemini proxy; it was deleted in the port commit because FEATURES.md classifies algorithmic suggestion as Reverse.

## Crossing statements

**Cloudflare Workers.** Every track and person's name a user types crosses from their browser to Cloudflare, a vendor I have a free personal account with and no contract or data-processing agreement; I am accountable for what gets sent, and Cloudflare is accountable only for running the code I deployed. It also receives request metadata I did not choose to send: IP address, user agent, timing, and the paths called. Credentials: a Cloudflare OAuth token from `wrangler login` stored in the Codespace at `~/.config/.wrangler`, never in the repository. The `database_id` in `worker/wrangler.toml` is an identifier, not a secret.

**Cloudflare D1.** The names of real people my users were recommended music by are stored in a table on Cloudflare's machines that I can read at any time, alongside labels describing how those recommendations felt; the people named never consented, and I am the only one accountable for that. Reached through the `DB` binding; there is no connection string or password anywhere. This is why the ring is Trial rather than Adopt: it works and the data exports cleanly, but there is still no route for a named person to be removed.

**GitHub and Codespaces.** My coursework and the tokens I create while working cross to GitHub, a vendor Georgia Tech has a relationship with but I do not; I am accountable for what I commit, and a credential pasted into a file would be my failure, not theirs. On 2026-11-10 I pasted a live Cloudflare API token into a chat window while testing the D1 exit and had to roll it. The token never entered the repository, but the near miss is recorded here because this row is where it would have landed.

**wrangler.** My whole Worker and schema cross to Cloudflare through an npm package maintained by Cloudflare and its dependency tree, and I am accountable for a supply chain I have not read. One npm publishing account, `wrangler-publisher <workers-devprod@cloudflare.com>`, controls every release. Pinned to `4.160.0` on 2026-11-12 after an unpinned run on 2026-11-10 fetched a build published thirteen hours earlier.

**GitHub Copilot.** The code and instruction files I have open cross to Copilot as prompt context whenever I type, and I am accountable for anything it suggests that I accept. No DDR in this repository cites it for final-project work, which is why the ring is Assess: it has read access to everything and no recorded use.

**bolt.new.** My specification and page source crossed to StackBlitz, a vendor I have a free account with and nothing beyond its terms of service, and I am accountable both for what I sent and for what it sent back — nine unused dependencies and Google Fonts loaded from two external origins, which I caught in review and removed before integrating.

**Google AI Studio.** The same specification crossed a second time, to Google, for one comparison run; none of its output was integrated, and I am accountable for having sent the same material to two vendors to learn one thing.

**Claude.** The contents of this public repository cross to Anthropic when I delegate drafting, along with the course briefs. No credential has crossed and nothing about a real person beyond what is already public here. Recorded in DDR-001, DDR-002, and DDR-003 with the verification performed.

## Revisit triggers

- A second person uses the pool. Every row assumes one user; two writers on one D1 table is ADR-003 territory.
- The free tier stops covering it — D1 row limits or Worker request limits — since every row above assumes a free personal account with no contract behind it.
- Someone named in the pool asks to be removed. The D1 row records a person who never agreed to be recorded, and I have no process for that. This is the trigger I am least prepared for.
- Cloudflare changes terms, or wrangler's telemetry or dependencies change what gets sent.
- Any credential moves out of the Codespace, for example into a GitHub Action or onto a laptop.
- A delegated build returns code that calls an origin not listed here. bolt.new's HW5 output added Google Fonts from two Google domains; it was caught by reading the file rather than by any process, which is the gap this trigger exists to close.
- **Audit 2 is due in the final-iteration window, December 9 to 14.** Every "Checked on" date above is from November and will be stale by then.