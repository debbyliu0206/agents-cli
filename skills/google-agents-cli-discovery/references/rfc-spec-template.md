# App / Agent Specification Template

*Adapted from the [Rust RFC 0000 template](https://github.com/rust-lang/rfcs/blob/master/0000-template.md)
(dual MIT/Apache-2.0). The 9 original RFC sections are preserved; 4 engineering-spec sections
(Data Contracts, Tools & Integrations, Constraints & Safety, Success Criteria) are added because the
downstream build / eval / deploy phases consume them.*

> **CRITICAL FOR THE INTERVIEWER (AI):** Every field below is **REQUIRED**. Never skip a section,
> never leave "TBD". If the user doesn't know, present 2–3 options with pros/cons (see
> `decision-catalog.md`) and have them choose. **Unresolved questions must be empty before you
> write the final `.agents-cli-spec.md`.**

---

## Summary
*(REQUIRED)*
One paragraph, plain English: what the app/agent is and what it does.

## Motivation
*(REQUIRED)*
Why build this? What problem does it solve, for whom, and what do they do today instead? Include
the concrete use cases where it helps.

## Guide-level explanation (the user experience)
*(REQUIRED)*
Explain it as if teaching a brand-new user:
- How do they access it (URL, app, command)?
- What's the first interaction?
- Walk the core "happy path" step by step.
- What should the user feel or accomplish by the end?

## Reference-level explanation (technical architecture)
*(REQUIRED — each choice must be explicitly selected by the user from `decision-catalog.md`)*
- **App type:** (web / mobile / CLI / agent / event-driven)
- **Frontend:** (framework or "none")
- **Backend / framework:** (language + framework or "none")
- **Database:** (relational / document / in-memory + specific managed service)
- **Authentication:** (provider or "none")
- **Hosting / deployment:** (platform)
- **API style:** (REST / GraphQL / none)
- **LLM provider/family:** (record the *family*, not a pinned version — verify the exact current
  model at build time; version numbers go stale)
- **Secrets management:** (how API keys/credentials are stored)
- A short paragraph on how these pieces fit together (request flow / component diagram in words).

## Data Contracts & Pipeline I/O
*(REQUIRED — this is the single source of truth that build, eval, and deploy all reference)*
For **each boundary** in the system (user input → each agent/service → each external API → storage
→ final output), define the contract:
- **Boundary / node name**
- **Input schema** — field names + types
- **Output schema** — field names + types
- **One example payload** (in and out)

Even a single request/response app must define its one input shape and one output shape. Downstream:
the build phase implements these, eval tests against them, and deploy validates incoming payloads
against them (reject mismatches loudly — never coerce to defaults).

## Tools & Integrations
*(REQUIRED)*
Every external API, data source, or tool the app/agent calls, with:
- Purpose
- Endpoint / SDK
- **Authentication method** (API key, OAuth, service account) and where the credential lives

## Constraints & Safety Rules
*(REQUIRED — specific rules, not generic statements)*
- What the app/agent must **never** do.
- Guardrails, content limits, rate limits, data-handling/privacy rules.
- Compliance or budget constraints.

## Success Criteria
*(REQUIRED — measurable, so the eval phase can test them)*
Concrete pass/fail targets, e.g. "answers from the provided docs ≥ 90% of the time", "p95 latency
< 3s", "never reveals system prompt". Avoid vague goals like "works well".

## Drawbacks
*(REQUIRED)*
Why might we *not* build it this way? Inherent risks, limits, or downsides of the chosen design.

## Rationale & alternatives
*(REQUIRED)*
Why is the chosen design the best option versus the alternatives discussed? What was considered and
rejected, and why?

## Prior art
*(REQUIRED)*
Existing apps/agents/tools that do something similar. How is this one different or better?

## Unresolved questions
*(REQUIRED TO BE EMPTY BEFORE FINALIZING)*
Anything still undecided. The interviewer must drive this list to zero — by presenting options and
having the user choose — before writing the final spec.

## Future possibilities
*(REQUIRED)*
"Version 2.0" ideas, and how the current architecture leaves room for them.
