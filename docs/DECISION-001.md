# DECISION-001: Spend the remaining build time on retrieval, or on reducing capture friction

## Committed before the evidence (2026-11-19)

The pool works and is deployed. There are roughly three weeks of build time left before December 14, and enough for one of these, not both.

**Option A — retrieval.** Build F5, *find it later*: search the pool by place, by the person it came from, or by a word in the note. Specified in FEATURES.md, not built. The RAT pointed here: people look for an old track by a word they half-remember, by asking whoever gave it to them, or not at all.

**Option B — capture friction.** Reduce the work of adding an entry. Today both the track and the source name are typed by hand. Profile A, the Casual Listener, is described in USERS.md as someone who will not do admin work, and manual entry is exactly that.

**Default action, if the evidence says nothing clear: Option A, retrieval.** It is the cheaper and more reversible of the two. F5 already has EARS rows, it needs an index on `place` and `author_id` rather than a schema redesign, and if it turns out to be wrong it can sit unused without breaking anything. Option B changes the one path every user must walk through, so getting it wrong is worse than not doing it.

**Owner and deadline:** Semaj Johnson II. Decided by 2026-11-24, so the build has two clear weeks after it.

### Evidence that would change it

Observed in the real-user sessions recorded in [FEEDBACK.md](FEEDBACK.md), with each user asked to do two things: add two tracks someone recommended to them, and then find a specific entry already in the pool.

**Outcome metric.** For each user, two counts: *add failures* (abandoned the form, needed help to finish, or entered something they then had to correct) and *find failures* (could not locate a known entry without being told where to look). Recorded per user in FEEDBACK.md and summed. A rerunnable version lives in `evals/queries/` as a count of entries whose `track` or `source` was corrected within sixty seconds of being written.

**Threshold.** If add failures exceed find failures across the sessions, switch to Option B. If find failures are equal or greater, stay with the default. With fewer than three users this threshold is weak evidence and the resolution below must say so rather than pretending otherwise.

**Guardrail.** No entry is ever lost — the guardrail from PROJECT.md. Whichever option wins, zero rows may disappear between sessions, and every entry a user remembers adding must be there when they return. If a change to either path risks that, it does not ship, regardless of what this metric says.

### Why this is a decision and not a preference

Both options are defensible from evidence already in the repository, and they point opposite ways. The RAT argues for retrieval. The user profiles argue for capture. Nothing I already know settles it, which is why it waits on watching real people rather than on more thinking.

## Resolved beneath (<pending>)

Result:
Decision:
Reversal trigger:
