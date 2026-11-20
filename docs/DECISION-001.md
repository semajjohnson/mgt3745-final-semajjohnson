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

## Resolved beneath (2026-11-20)

**Result.** Three sessions, recorded in [FEEDBACK.md](FEEDBACK.md). Add failures: 1 of 3 users — the brother could not find mood tagging, because it is a second step after saving rather than a field in the initial input. Find failures: 0, but the find task only ran with one of the three people, so that side of the comparison rests on a single observation. Both counts are far thinner than the threshold assumed when it was written.

By the letter of the threshold, add failures exceeded find failures and the decision switches to **Option B, capture friction**. I am recording that, and recording that one observation against zero is not a real margin. If the find task had run three times instead of once the comparison could have gone either way.

**Decision: Option B, reduce capture friction — specifically, move mood tagging into the initial input rather than leaving it as a second step.** This is the narrowest version of Option B. It is the one failure actually observed, it is a change to a form rather than to the schema, and it does not foreclose Option A later.

Retrieval is not refuted. Nobody failed to find anything, but with a pool this small and one attempt, that is a statement about the test rather than about F5.

**What the sessions found that this decision did not ask about, and which matters more.** All three users independently described the product as a way of *sharing* music with other people. PROJECT.md scopes a personal pool and ADR-002 records it as single-user by design. The classmate said plainly that using it again depends on other users existing. The brother's objection — "redundant because Spotify exists" — is the same point from the other side: as a private archive it competes with a notes app, and as a shared one it competes with Blend and wins on the thing Blend throws away, which is who the track came from.

So the real fork is not retrieval versus capture. It is whether the pool stays single-user. That question is larger than this decision memo, it belongs in an ADR rather than here, and three sessions is not enough to answer it. What it does is fire a reversal trigger that was already written down.

**Reversal trigger.** ADR-002's first reversal trigger reads: "A second person uses the pool. Every assumption above is single-writer, and two writers on one D1 table is the next ADR." Three people used one shared pool on 2026-11-20 and one of them found another's entry, so the trigger has fired on its own terms. ADR-003 is now owed, and it should decide whether the pool becomes multi-user before December 14 or whether single-user is reaffirmed with this evidence recorded against it.

This decision — mood tagging into the initial input — stands either way, because it is a form change that survives both answers.
