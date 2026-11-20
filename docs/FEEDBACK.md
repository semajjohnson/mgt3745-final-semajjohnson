# Real-User Feedback

Report counts, never percentages, when n is under ten. No personal data: roles only.

Three sessions, 2026-11-20, on the deployed build at `semajjohnson.github.io/mgt3745-final-semajjohnson`. Each person was handed the phone cold and asked what they thought it was before being told anything.

| User (role) | Date | Task attempted | What happened | Quote | Disappointment answer |
|---|---|---|---|---|---|
| Friend, casual listener | 2026-11-20 | Describe it cold; add a track and its source | Described it unprompted as a platform for **sharing** music. Added song and artist with no difficulty: 0 add failures. Find task not run. | "Kinda cool" — value comes from sharing music with people on a different platform than you | Somewhat |
| Brother, casual listener | 2026-11-20 | Describe it cold; add a track and its source; find another user's entry | Described it as a "manual Spotify blend" and said it seemed redundant because Spotify already exists. Added song and artist easily. **Struggled with the mood tag because it is not part of the initial input** — 1 add failure. Found the friend's track unaided: 0 find failures. | "A manual Spotify blend… seems a bit redundant because Spotify exists" | Not dissapointed |
| Classmate, music enthusiast (prior interviewee) | 2026-11-20 | Describe it cold; add a track and its source; add a mood tag | Already familiar with the concept, so the cold-open reading is not independent. Described it as a non-algorithmic music sharing platform. Added song and artist easily and mood-tagged without difficulty: 0 add failures. Find task not run. | Using it again "depends heavily on existing users and the network effects there"; wants to share with communities, not only personal friends | Dissapointed |

**Counts.** Add failures: 1 of 3 users. Find failures: 0, but the find task only ran with one person. Mood tagging: 1 struggled, 1 succeeded, 1 not attempted.

## What these sessions actually found

**All three described a product that is not the one specified.** Independently, and in their own words, each framed it as *sharing* music with other people: a platform for sharing, a manual Spotify blend, a non-algorithmic sharing platform. `PROJECT.md` scopes a personal pool of tracks a listener was recommended, and `ADR-002` records the pool as single-user by design. Nobody described a personal archive. This is the strongest signal in the three sessions and it is a finding about the framing, not the interface.

**The strongest objection came from the person closest to the mainstream use case.** The brother's "redundant because Spotify exists" is the competitor claim PROJECT.md does not currently answer. Spotify Blend produces a shared playlist algorithmically; the pool's claim is that the *person attached to a track* is the part worth keeping, and Blend does not keep it. That distinction is clear in the specification and invisible in the product.

**Cross-platform value was volunteered, not prompted.** The friend said the value is sharing with people on a different platform than you. ADR-001 chose no platform dependency because two interviewees were on incompatible services; a user independently named the same gap as the reason to use it. That is the ADR's rationale confirmed from the outside, arriving as a *sharing* benefit rather than a capture one.

**Network effects were raised as a precondition, not a nice-to-have.** The classmate's answer to whether they would use it again was conditional on other users existing. A single-user product cannot satisfy that, which puts `ADR-002`'s deferral of multi-user behavior directly in the path of the outcome measure in `PROJECT.md`.

**Mood tagging failed once, for a structural reason.** It is a second step after the entry is saved rather than a field in the initial input. The one user who did not already know the product could not find it. The two who succeeded were the classmate, who knew the concept, and the friend, who was not asked.

## Limitations of these sessions

- **The disappointment question was not asked of anyone.** It is the one field this file's own header requires. Without it there is no product-market-fit signal, only task observations.
- **The find task ran with one of three users.** DECISION-001's threshold compares add failures against find failures, and the find side rests on a single observation.
- **Row counts were not taken before and after**, so the guardrail in `PROJECT.md` — no entry is ever lost — is unverified for these sessions.
- **One of three had seen the concept before.** The classmate is a prior interviewee, so their cold-open description is not independent evidence that a stranger understands the product. Two of three readings are independent.
- **All three are people I know.** None found the product on their own or came to it with a problem they were already trying to solve.
