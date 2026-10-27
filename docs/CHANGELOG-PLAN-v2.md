# Changelog — PLAN-v2 vs PROJECT-PLAN.md

Short diff of what the improved draft adds or changes, and why. PLAN-v2 is still a **draft** until Justin approves.

## Added

- **Approval gate (§0)** — Every piece shown to Justin; explicit yes for that piece only. Prevents "plan approved = publish anything."
- **Mohamed failure → fix table (§2)** — Maps his series pain (naive RAG, ElevenLabs economics, 3D sink, opaque memory, free-running proactive, late cost discovery, etc.) to our episode hooks. Credits him; doesn't copy prose.
- **Distribution plan + syndication checklist (§7)** — Canonical → Hashnode → dev.to → X → LinkedIn → Reddit (one sub) → German → README/hub. Concrete checkboxes per episode.
- **SEO / AEO (§7.2)** — Answer-first snippets, question-shaped H1/H2s, series hub, canonical discipline — visibility beyond Google blue links.
- **German workflow (§7.1)** — Draft → `needs-justin-qa-de` → Justin yes → publish; 2–3 day lag.
- **Name-vote process (§8)** — Brainstorm → RDAP/handle checks → public shortlist → vote → Justin decides → only then consider buy (bot never buys).
- **Decision-log + cost-log locations (§9)** — Templates under `templates/`; working logs in `logs/` + public repo `/docs/`; they feed outlines/cost boxes.
- **Risks table expanded (§11)** — Scope, safety, costs, memory quality, 3D rabbit hole, naming, time competition, Desire crossover, invented metrics, bot overreach, platform bans, DE QA bottleneck — each with a mitigation.
- **Anti-jobs (§12)** — Hard no: videos/Shorts/voiceovers; product code/infra from this bot; Desire content; invented metrics; buying domains/handles/spend; messaging strangers; publish without per-piece yes.
- **Stub templates** — `templates/decision-log.md`, `templates/cost-log.md` (referenced by original plan but missing on disk).
- **Draft banner** — Status clearly DRAFT / not locked; preserves `PROJECT-PLAN.md` and `EPISODE-1-OUTLINE.md` intact.

## Changed

- **Distribution: article-first** — Original leaned article + YouTube long + Shorts. v2 makes written syndication the checklist; video/Shorts/voiceovers are out of scope for the Companion Series bot (anti-job). Justin can still film personally later outside this bot checklist.
- **Episode table** — Added "Publish note" column; explicit "slips are content"; measured-metrics reminders; safety gate called out on Ep 5/6.
- **Proactive section title** — Reframed as "Check-ins with guardrails" (SFW accountability; not romantic "she texts first" vibe).
- **Next steps** — First step is now Justin approve/amend PLAN-v2; Ep 1 outline revision listed after approval; no implied "start coding via bot."
- **Stack section** — Clarified bot writes *about* the stack; does not ship product code/infra.
- **Tone** — Technical-writer build-in-public plan, not a Desire product pitch; Desire called out as hard separation.

## Preserved (on purpose)

- Three locked hooks: Memory Vault, companion-with-a-job, live `/costs`.
- SFW hard rules (no romantic/sexual RP; adults-oriented; not-a-therapist + crisis resources; user-owned memory).
- Python/FastAPI/pgvector/local embeddings/Kokoro/Cloud Run stack choices.
- Layered memory architecture and draft table list.
- Budget caps (≤$25/mo hosting, $20/mo LLM, no paid TTS).
- 7-episode arc and demo moments.
- Series name: "Building a Companion That Remembers."
- Open credit to Mohamed Abu Saif / Lumine.

## Intentionally not done in this pass

- Did **not** overwrite `PROJECT-PLAN.md` or `EPISODE-1-OUTLINE.md`.
- Did **not** invent costs, recall scores, or demo results.
- Did **not** publish, buy domains/handles, or write product code.
- Ep 2–7 full outlines still TBD after Ep 1 ships (called out in PLAN-v2).
