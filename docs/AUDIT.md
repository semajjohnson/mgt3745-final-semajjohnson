# Context Scaffold Audits

## Audit 1: 2026-11-03 (before the November 12 check-in)

Run on the scaffold as carried forward from `mgt3745-hw5`, before any file was changed, as the project brief's setup step requires. Facts gathered by running `.github/prompts/audit.prompt.md`; the verdicts are mine.

**What was carried forward.** Ten HW5 files replaced their template stubs unchanged: PROJECT, USERS, FEATURES, ARCHITECTURE, STANDARDS, STYLE, TOOLS, SKILLS, EVALS, CLAUDE. Two HW5 files were carried that the template does not include: `context/README.md` and `context/curiosity/`. Two template files were deliberately **not** overwritten: `AGENTS.md`, because the HW5 version is a placeholder reading "arrives in Module 6" and Module 6 has passed, and `DESIGN.md`, because HW5 has no source for it.

**Last changed** is the date of the last commit touching that file in the HW5 repository, since this repository's history begins with the carry-forward commit.

| File | Reader | Last changed | Finding | Verdict | PR |
|---|---|---|---|---|---|
| PROJECT.md | Me; the grader; any assistant reading `context/` | 2026-10-01 | Has the deliberate reframe and the scope, but **no success measures**. The brief requires three: one outcome, one leading indicator, one guardrail. Also states the build depends on no streaming platform, which FEATURES.md contradicts. | Rewrite | |
| USERS.md | Me; the grader | 2026-10-01 | Two job statements in the required format, two profiles, each claim marked known or assumed. Current and complete against the brief. | Keep | |
| FEATURES.md | Me; the grader; any builder I delegate to | 2026-10-01 | EARS rows, Kano categories, and exclusions all present. Two defects: it still reads "Spotify is the assumed platform," which contradicts PROJECT.md's constraint that the build depends on no streaming platform; and the acceptance rows name `worker.js`, which is `worker/src/index.js` in this template. | Rewrite | |
| ARCHITECTURE.md | Me; the grader | 2026-10-01 | Carries the Gate and both ADR-001 and ADR-002 inline. The brief puts ADRs in `docs/ADR-001.md` and asks ARCHITECTURE.md for a current diagram and the Trust Boundary; it has **neither**. | Merge | |
| STANDARDS.md | Copilot via `.github/copilot-instructions.md`; Claude via CLAUDE.md | 2026-10-01 | Rules are current and the build obeys them: Rule 2 forbids libraries, CDN scripts, and outside API calls, and the inventory confirms zero of each. Names `wrangler.toml` at the repository root; it is at `worker/wrangler.toml` here. | Rewrite | |
| STYLE.md | Copilot; Claude; any builder | 2026-10-01 | Current and specific. References `docs/JUDGMENT.md`, which does not exist in this repository. | Rewrite | |
| TOOLS.md | Me; the grader; rewritten by HW7's Sourcing Audit | 2026-10-01 | Seven rows with crossing statements and switching costs, more thorough than the template's columns. **No Claude row**, although CLAUDE.md and FEATURES.md both name Claude. Columns lack Ring, Checked on, and Limit or price, which HW7 requires. | Rewrite | |
| SKILLS.md | Me | 2026-10-01 | Current, short, and accurate. No findings. | Keep | |
| EVALS.md | Me; the grader | 2026-10-01 | Holds the RAT, four Prediction Stakes, and the HW5 error log. Two broken references: `docs/JUDGMENT.md` does not exist here, and **`evals/pool.test.mjs` will not run** — `package.json` globs `evals/**/*.test.js`, so a `.mjs` file is skipped silently rather than failing loudly. | Rewrite | |
| CLAUDE.md | Claude | 2026-10-01 | Current and specific to this project. Overlaps `context/AGENTS.md`: both state who may change what and how work is reviewed. Not contradictory, but two files now answer one question. | Merge | |
| AGENTS.md | Agents reading `AGENTS.md`; the root pointer file sends them here | Template, unchanged | Kept from the template rather than overwritten with the HW5 version, which is a placeholder. Real content, correct for this repository. | Keep | |
| DESIGN.md | Nobody yet | Template, unchanged | Empty stub. The brief requires it for a project with a visual interface, and The Pool has one. Nothing in HW5 to carry. | Rewrite | |
| context/README.md | Nobody | 2026-09-30 | An index of the scaffold, stale on arrival: claims ten of eleven files are real, calls AGENTS.md a preview "until Module 6" which has passed, and lists `PROBLEM.md`, which no longer exists. The template does not ask for this file, and the directory listing already tells a reader what is in it. | Remove | |
| context/curiosity/ | Nobody | 2026-10-01 | Five optional, ungraded files. `FAILURES.md` holds 2.4 KB of real recorded failures worth keeping. The other four total under 700 bytes and are little more than headings. The brief names eleven context files and these are not among them. | Merge | |

### What the audit found that I did not expect

**A test that cannot fail.** `evals/pool.test.mjs` is named in EVALS.md as the evidence for three acceptance rows, and under this template's test glob it never runs. A skipped test reports nothing, so the scaffold would have gone on citing passing evidence that was never executed. This is the only finding here that was invisible from reading the files alone; it took comparing EVALS.md against `package.json`.

**Four files with no reader.** `context/README.md` and four of the five `curiosity/` files are read by nobody and referenced by nothing. By the brief's own standard, a file nobody reads is a file that will drift, and these had already drifted: the index describes a scaffold that no longer exists.

**The scaffold is more current than the architecture.** Every rules file — STANDARDS, STYLE, CLAUDE, SKILLS — survives this audit with at most a path correction. The two files that need real work, PROJECT and ARCHITECTURE, are the ones carrying requirements the final brief added after HW5 was written. The drift is against the new brief, not against the build.

### Changes made

- Carry-forward of the HW5 scaffold onto the template stubs, unchanged, as the audit baseline: `c37886d`
- 

## Audit 2: <YYYY-MM-DD> (final-iteration window, December 9 to 14)
