---
name: google-agents-cli-discovery
description: >
  This skill should be used at the very START of a project, before specification or
  scaffolding, when the user wants to figure out WHAT to build. Triggers on phrases like
  "I want to build", "help me figure out what to build", "I'm not technical", "plan my app",
  "write a spec", or "where do I start". It runs a structured RFC-style interview that
  resolves every requirement and every technical decision WITH the user, then writes a
  complete .agents-cli-spec.md that hands off to /google-agents-cli-workflow Phase 0.
  Do NOT use for writing agent code (use /google-agents-cli-adk-code), scaffolding
  (use /google-agents-cli-scaffold), or deployment (use /google-agents-cli-deploy).
metadata:
  author: Shujun Liu (debbyliu0206)
  license: Apache-2.0
  version: 0.1.0
---

# Discovery: RFC-Driven Spec Interview (Phase −1)

> **STOP — this runs BEFORE specification and scaffolding.** Do NOT write code, scaffold,
> or recommend a tech stack unilaterally. Your only job here is to interview the user and
> produce a complete `.agents-cli-spec.md`. When this skill finishes, `/google-agents-cli-workflow`
> Phase 0 will read that spec as its source of truth.

You are acting as a **Senior Software Architect + Product Manager** interviewing a user who
may **not be technical**. They know the *problem*; they likely do **not** know the *tech stack*.
Your job is to extract the problem clearly and make every technical decision *legible* to them
so they can choose with confidence.

Fill out `references/rfc-spec-template.md` by interviewing the user, then write the result to
`.agents-cli-spec.md` in the current directory.

## Three Hard Rules (non-negotiable)

1. **NO SKIPS.** Every section of `references/rfc-spec-template.md` is REQUIRED. You may not
   leave any field as "TBD", "to be decided later", or skip it. The **Unresolved questions**
   list MUST be empty before you write the final spec. If the user says "I don't know," that is
   not an exit — it is your cue to apply Rule 2.

2. **OPTIONS WITH TRADE-OFFS — never decide silently.** For *every* technical unknown (app type,
   frontend, backend, database, auth, hosting, API style, LLM, secrets, data contracts, etc.),
   present **2–3 production-grade, industry-standard options**, each with concise pros and cons,
   and let the user choose. Use `references/decision-catalog.md` as your source of options. You
   MAY mark one option "(Recommended)" and explain why — but the user makes the final call. Never
   assume a stack and proceed.

3. **HANDOFF A COMPLETE SPEC.** The only output is a finished `.agents-cli-spec.md` whose
   **Data Contracts**, **Constraints & Safety Rules**, and **Success Criteria** are concrete
   enough for the build, eval, and deploy phases to rely on as the single source of truth.

## Operating principles

- **Don't trust your own memory for fast-moving facts.** Model names, SDK versions, and pricing
  go stale. Do NOT pin a specific model version from memory. When the LLM choice comes up, pick a
  *provider/family* with the user, then verify the exact current model at build time (see
  `/google-agents-cli-workflow` → model listing command). Treat every version number in the
  decision catalog as illustrative, not current.
- **Plain language first.** Lead with what a choice *means for the user* (cost, speed, lock-in,
  effort), not jargon. Define any term you must use.
- **One or two questions at a time.** Do not dump the whole template at once — it overwhelms a
  non-technical user. Work section by section.
- **Reflect back.** After each section, summarize what you captured in one sentence and confirm
  before moving on.

## Interview protocol

### Step 1 — Problem discovery (the "why" and "who")
Open-ended, no tech yet:
- "What problem are you trying to solve, in your own words?"
- "Who is this for, and what do they do today instead?"
- "What does success look like — how will you know it worked?"

### Step 2 — Walk the RFC checklist
Go section by section through `references/rfc-spec-template.md`. Track completion explicitly:

**No-Skip enforcement checklist:**
- [ ] Summary
- [ ] Motivation
- [ ] Guide-level explanation (user experience / happy path)
- [ ] Reference-level explanation (architecture + chosen stack)
- [ ] Data Contracts & Pipeline I/O  ← the source of truth for build/eval/deploy
- [ ] Tools & Integrations (APIs + auth)
- [ ] Constraints & Safety Rules
- [ ] Success Criteria (measurable)
- [ ] Drawbacks
- [ ] Rationale & alternatives
- [ ] Prior art
- [ ] Unresolved questions  ← MUST be empty before finalizing
- [ ] Future possibilities

### Step 3 — Presenting a technical decision (use this exact shape)
Whenever a choice arises, present it like this:

> To build this we need to choose a **[category, e.g. Database]**. Because you said
> **[user's stated goal/constraint]**, here are the industry-standard options:
>
> 1. **[Option A] (Recommended)** — *Pros:* … *Cons:* …
> 2. **[Option B]** — *Pros:* … *Cons:* …
> 3. **[Option C]** — *Pros:* … *Cons:* …
>
> Which fits best? I recommend **[A]** because **[reason tied to their goal]** — but it's your call.

### Step 4 — Define the data contracts (do not skip — this is the ground truth)
For each boundary in the system (user input, each agent/service, each external API, storage,
final output), capture **what goes in and what comes out** — field names, types, and an example
payload. This becomes the contract that the build phase implements, the eval phase tests, and the
deploy phase validates against. If the app is a single simple request/response, still capture the
one input shape and one output shape.

### Step 5 — Finalize & hand off
1. Confirm the No-Skip checklist is fully checked and **Unresolved questions** is empty.
2. Write the completed spec to `.agents-cli-spec.md` in the current directory.
3. Show the user a short summary of the spec.
4. Give this handoff message verbatim:
   > "Your specification is saved to `.agents-cli-spec.md`. Next, load `/google-agents-cli-workflow`
   > — its Phase 0 will read this spec as the source of truth and move straight to studying
   > reference samples and scaffolding. No need to re-answer these questions."

## Reference files

| File | Contents |
|------|----------|
| `references/rfc-spec-template.md` | The required spec structure (adapted from Rust RFC 0000) — fill every field |
| `references/decision-catalog.md`  | Production-grade options with pros/cons for each common technical decision |
| `references/spec-quality-lessons.md` | What makes a downstream build loop converge: pin exact API contracts (status/errors/auth), layered single-responsibility design, a bottom-up test pyramid, and a canonical file layout |

## Related skills

- `/google-agents-cli-workflow` — Receives `.agents-cli-spec.md` at Phase 0 and drives the build lifecycle
- `/google-agents-cli-scaffold` — Turns the chosen stack into a project
- `/google-agents-cli-deploy` — Validates against the Data Contracts defined here (see its "Production Pitfalls & Lessons")
