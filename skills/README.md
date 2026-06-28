# ADK Skills

Development skills for building agents with [Google ADK](https://adk.dev). Install into any coding agent via [`npx skills`](https://github.com/vercel-labs/skills).


## Skills

| Skill | Description |
|-------|-------------|
| `google-agents-cli-discovery` | RFC-driven discovery interview — clarifies requirements and tech choices with the user, writes `.agents-cli-spec.md` (run before workflow) |
| `google-agents-cli-workflow` | Development lifecycle, code preservation rules, model selection |
| `google-agents-cli-adk-code` | Python API reference — agents, tools, orchestration, callbacks, state |
| `google-agents-cli-scaffold` | Project scaffolding via Agents CLI |
| `google-agents-cli-eval` | Evaluation lifecycle — datasets, metrics, generate/grade, compare, analyze, optimize, LLM-as-judge |
| `google-agents-cli-deploy` | Deployment — Agent Runtime, Cloud Run, CI/CD, secrets |
| `google-agents-cli-publish` | Gemini Enterprise registration |
| `google-agents-cli-observability` | Observability — Cloud Trace, Cloud Logging, third-party integrations |

## Fork modifications (vs. [google/agents-cli](https://github.com/google/agents-cli))

This fork diverges from upstream in three ways:

- **NEW — `google-agents-cli-discovery`:** an RFC-driven discovery interview that runs *before* specification (Phase −1). It resolves every requirement and technical decision **with** the user — no skips, and for each technical unknown it presents 2–3 production-grade options with pros/cons — then writes a complete `.agents-cli-spec.md`. Author: Shujun Liu (debbyliu0206).
- **MODIFIED — `google-agents-cli-workflow`:** Phase 0 now **deterministically** gates on a complete spec. It always checks for `.agents-cli-spec.md`, validates it against the discovery template, and routes to `/google-agents-cli-discovery` when the spec is missing or incomplete (adds a Phase −1 row to the lifecycle).
- **MODIFIED — `google-agents-cli-deploy`:** added a **"Production Pitfalls & Lessons (Read First)"** section capturing field lessons — validate against the data contract defined in the spec, env-vars/secrets as code, smoke-test with a minimal request first, pagination to avoid N+1 timeouts, regional routing, and structured logging.
