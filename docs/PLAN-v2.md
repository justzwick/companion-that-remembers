# Building a Companion That Remembers — Series Plan v2 (APPROVED)

**Status:** APPROVED — Justin yes 2026-10-27 (America/Denver). Per-piece approval still required for every article/syndicate post.  
**Series name:** Building a Companion That Remembers  
**Product:** Fresh SFW AI companion (NOT Desire / desireai.app — never publish about Desire)  
**Decision locked (Oct 21, 2026):** Fresh SFW build-in-public series over filming DesireAI.  
**Basis:** Improves `PROJECT-PLAN.md` with distribution, SEO/AEO, risks, approval gates, name-vote process, log templates, and anti-jobs. Locked by Justin 2026-10-27; per-piece publish approval still required.

**Credit (open, every Episode 1+):** Inspired by Mohamed Abu Saif's Lumine AI build-in-public series (Hashnode, Jun 2026) and his r/Development write-up. We credit him, link his five parts, and rebuild with his failure points fixed **by design**. Do not copy his prose.

Inspiration sources (study; credit; never paste):
- Reddit: https://www.reddit.com/r/Development/comments/1uhrwlp/i_built_an_ai_companion_from_scratch_memory_rag/
- Hashnode series (5 parts): https://mohamedabusaif.hashnode.dev/

---

## 0. How to use this plan (approval gate)

Every public piece is a **separate approval**:

1. Companion Series bot (or Justin) drafts the piece from the decision/cost logs + episode outline.
2. Draft is shown to Justin with: title, channel(s), canonical URL plan, and full body (or outline + first publishable draft).
3. Justin replies with an **explicit yes for that piece only** (e.g. "yes, publish Ep 2 Hashnode"). Silence, "LGTM later," or approval of a different piece does **not** count.
4. Only after that yes: publish / syndicate that piece. No batch "approve the series" unlocks.

Sprint slips are content: if a week slips, the episode documents the slip, the cause, and the revised date — do not invent progress.

**PLAN-v2 itself is not locked** until Justin says so. Prefer amending this draft over inventing process mid-flight.

---

## 1. Concept — three locked product hooks (do not dilute)

Mohamed built a capable 3D companion that could chat, remember via naive RAG, and speak. Ours differentiates on three hooks he did not ship:

1. **Memory you can see.** Memory Vault UI: every stored memory is readable, editable, pinnable, deletable. Headline feature, trust feature, and the strongest demo moment ("watch — it learned this, and I can delete it").
2. **Companion with a job.** Default: accountability / follow-through (goals, habits, people, things you said you'd do). Personality is configurable; follow-through is the purpose.
3. **Costs published live from day 1.** Public `/costs`: cost per conversation, per voice minute, per active user. Mohamed discovered TTS economics at the end and stopped. We design to a budget at the start and report every episode.

Working series title: **"Building a Companion That Remembers"**  
Character/product name: **TBD via Episode 1 name vote** — not a pre-decision. Do not buy a domain or claim handles until the vote closes and Justin approves the spend (see §8 and anti-jobs).

### SFW hard rules (v1 system layer, not bolted on)

- No romantic / sexual roleplay modes. Friendship / accountability register only.
- Adults-oriented (not marketed to minors).
- Clear **not-a-therapist** boundary + crisis-resource response in the system prompt from v1.
- User owns memory: export + full delete in the UI.

---

## 2. What Mohamed taught us — failure points we fix by design

Credit Mohamed openly in Episode 1 (and link all five Hashnode parts). Steal the *lessons*, not the prose. Episode hooks that his series did not cover — because these are the failure points we design around:

| Failure point (from his journey) | Our design fix | Episode that proves it |
|---|---|---|
| Refresh → total amnesia; "chatbot with a face" | Intentional v0 that forgets, then layered memory + Vault | Ep 1 → Ep 2 |
| Naive RAG: embed every message, top-k cosine, no facts/forgetting/recency | Episodic log + semantic facts + hybrid retrieval + decay + eval harness | Ep 2–3 |
| Memory is opaque (user can't see/edit what the model "knows") | Memory Vault UI (edit/pin/delete) | Ep 2 |
| Weeks lost to 3D (Meshy/Mixamo/VRoid path) | 2D animated portrait first; 3D only if audience votes later | Ep 4 (avatar states), not Ep 1 |
| ElevenLabs economics killed the product (~$300/1M chars; projections broke free tier) | Kokoro self-hosted TTS from day one; no paid TTS in v1; `/costs` live | Ep 4, Ep 6 |
| Costs discovered at the end | `cost_events` table + public `/costs` from Episode 1 empty state | Ep 1 teaser → Ep 6 walkthrough |
| Proactive messaging as free-running agent (charming demo, creepy/expensive at scale) | Scheduler + caps + quiet hours + "why it fired" in Vault | Ep 5 |
| Vendor lock-in (single LLM path) fragile for economics | Router: Groq (cheap/fast) + quality model for evals + Ollama offline | Ep 1–6 cost boxes |
| No public artifact / repo as distribution | Public GitHub from commit #1 | Ep 1 |
| Relationship "feelings" stages toward romantic framing | SFW accountability register; relationship state = trust/threads/follow-ups, not romance meters | Ep 5 + hard rules |

These rows are Episode hooks: each article should open or climax on one "his failure → our fix" beat, with a live demo, not a lecture.

---

## 3. Stack — write ABOUT it; do not ship product code from this bot

Justin builds. This series bot plans, drafts articles, and waits for approval. It does **not** write companion product code or stand up infra (see §12 Anti-jobs).

| Layer | Pick | Why / improvement vs Mohamed |
|---|---|---|
| Backend | Python 3.12 + FastAPI | Justin's language; native ML ecosystem |
| DB | PostgreSQL + pgvector | One DB for facts + vectors (his good call) |
| ORM/migrations | SQLAlchemy 2 + Alembic | Boring, documentable |
| Embeddings | sentence-transformers `all-MiniLM-L6-v2`, local | Same 384-dim model idea; native Python |
| LLM | Router: Groq (dev) + quality model (evals) + Ollama on Mac mini (offline) | Avoid single-vendor fragility |
| TTS | Kokoro-82M self-hosted first; Chatterbox-Turbo as stretch | Kill ElevenLabs bill |
| STT | faster-whisper, local | Free, private; already in Justin's toolkit |
| Avatar v1 | 2D animated portrait (Rive/CSS/Lottie states) | Ship in week 2-ish; 3D is a later vote |
| Frontend | Simple responsive web (SSR + light JS, or small React if needed) | Mobile-first |
| Deploy | Google Cloud Run + managed Postgres/pgvector | Scale-to-zero while audience is small |
| Repo | Public GitHub from commit #1 | Artifact + backlinks |

Explicitly **not** in v1: 3D world, VR, real-time video avatar, native mobile apps, payments. Each is a later episode decision with usage data.

---

## 4. Memory architecture (Episodes 1–3 core)

His design: embed every message → pgvector → inject top-5 by cosine. Breaks under duplicates, no fact extraction, no forgetting, no recency, similar-but-wrong wins.

Ours, layered:

1. **Episodic log** — raw conversations; never injected wholesale.
2. **Semantic facts** — cheap-model extraction into a facts table (subject/predicate/value, confidence, source, superseded_by, expires_at).
3. **Relationship state** — structured (trust/familiarity, inside jokes, ongoing threads, open follow-ups); visible in Vault.
4. **Rolling summaries** — weekly narrative continuity.
5. **Retrieval** — hybrid: vector + keyword + recency + importance, reranked; pinned always available.
6. **Forgetting** — low-importance episodic decay; user delete is instant and total.
7. **Eval harness** — fixed 50-fact "golden conversations"; after each memory change, run and publish the **real** recall score in that episode. Never invent scores.

Draft tables (conceptual — Justin implements): `users`, `characters`, `conversations`, `messages`, `facts`, `memories` (text, vector(384), importance, last_accessed, pinned), `summaries`, `relationship_state`, `followups`, `cost_events` (model, tokens/chars, cost, session_id) — `/costs` reads from `cost_events`.

---

## 5. Voice, avatar, proactive behaviour

**Voice + avatar**

- Text first; voice is a toggle; cache aggressively (same phrase → same audio file).
- Kokoro local for v1; measure and publish real words-per-minute cost (mini vs Cloud Run) — never invent numbers.
- Avatar states from reply emotion tags (neutral / happy / concerned / thinking / laughing). Viseme-lite is an Ep 4 stretch, labelled honestly.

**Proactive — rules before agents**

- Max 1 proactive message/day by default; quiet hours; user sets off / check-ins / active.
- Triggers are explainable (due follow-up, streak, user-requested date).
- Every proactive message logs *why* it fired, shown in the Vault. No engagement-maximising patterns.
- Safety review of proactive guardrails is a hard gate before public demo (Ep 5/6).

---

## 6. Episode table (weekly) — slips are content

| Wk | Build sprint (Justin) | Article (canonical + syndicate) | Demo moment | Publish note |
|---|---|---|---|---|
| 1 | Repo, FastAPI + Postgres/pgvector skeleton, chat that forgets on refresh + name shortlist | Ep 1: Memory is the product | Refresh-forgets-everything, live | Name vote opens; empty `/costs` mock |
| 2 | Layered memory: fact extraction, retrieval, Memory Vault UI v1 | Ep 2: Memory you can see | Remembers after restart; delete → forgets | First real Vault screenshots |
| 3 | Hybrid retrieval + reranking, decay, 50-fact eval harness | Ep 3: Beating naive RAG | Published **measured** recall score, before vs after | No invented metrics |
| 4 | Kokoro + faster-whisper, avatar states, response caching | Ep 4: Killing the voice bill | Blind A/B vs premium TTS clip + **measured** cost/min | No paid TTS |
| 5 | Relationship state + follow-ups + proactive scheduler | Ep 5: Check-ins with guardrails | Check-in that fires for a visible reason | Safety review gate |
| 6 | Cloud Run deploy, public demo, `/costs` live, export/delete | Ep 6: Shipping + what a user costs | Live `/costs` walkthrough | Real numbers only |
| 7 | Audience-voted feature + retro | Ep 7: What broke, what I'd rebuild | Failures reel (written) + roadmap vote | Scope dump + next season |

**If a sprint slips:** publish a short "slip note" (or fold into the episode): what blocked, what changed, new target week. That is on-brand honesty, not failure.

**Episode outlines:** `EPISODE-1-OUTLINE.md` remains the Ep 1 starting point. After Justin approves PLAN-v2, update outlines to match the article-first syndication checklist (see §7). Do not invent Ep 2–7 full outlines until Ep 1 is approved and shipping.

Per episode production habit:

1. Fill **decision log** + **cost log** during the sprint (≤5 min/day) — see `templates/`.
2. Logs → article outline → draft → Justin approval gate → publish.
3. Structure template: hook (demo moment) → problem → diagram → build/failure → demo → cost box → one question for comments.

---

## 7. Distribution plan + syndication checklist

**Article-first.** Canonical long-form on Justin's own domain, then syndicate. Video / Shorts / voiceovers are **out of scope for the Companion Series bot** (anti-job). If Justin personally films later, that is outside this bot's publish checklist and still needs per-piece approval.

### Per-episode syndication checklist

Copy this into each episode's publish ticket; check only after Justin's **yes for that channel**:

- [ ] **Canonical article** on Justin's domain (canonical URL set)
- [ ] **Hashnode** syndicate with canonical back-link
- [ ] **dev.to** syndicate with canonical back-link
- [ ] **X thread** (summary + link; no cold DMs)
- [ ] **LinkedIn** builder post (summary + link)
- [ ] **Reddit** — one sub per episode; rewrite opener for that sub; lead with the technical problem; link repo + article. Ep 1 default candidate: r/Development (where Mohamed posted). Never spam multiple subs the same day.
- [ ] **German article** — 2–3 days after English canonical (see §7.1)
- [ ] Update public GitHub README with episode link
- [ ] Update series index / roadmap graphic with "shipped" mark

Do **not**: message strangers; buy ads; invent engagement; cross-post identical blobs without canonical tags.

### 7.1 German workflow

1. Draft German version from the approved English canonical (translation or rewrite — Justin's call per episode).
2. Flag for **Justin QA** explicitly: mark the draft `needs-justin-qa-de` and wait.
3. Justin returns edits or "yes, publish DE".
4. Publish DE only after that yes. Typical lag: 2–3 days after English.

### 7.2 SEO / AEO (answer-engine visibility)

Goal: each episode answers a real question a developer (or answer engine) would ask — not just rank for a brand phrase.

**Per article:**

- One clear **primary question** in the title/H1 (e.g. "Why do AI companions forget you on refresh?").
- Open with a **40–80 word direct answer** (AEO snippet) before the narrative hook expands.
- Use descriptive H2s that match search/answer queries ("How hybrid retrieval beats naive RAG", "Self-hosted TTS cost vs ElevenLabs").
- Include: architecture diagram, repo link, cost box (real numbers or "not measured yet — budget caps are…"), glossary of one new term.
- Schema-friendly structure: problem → approach → steps → result → limits.
- Internal links across episodes; evergreen series hub page.
- Credit + outbound link to Mohamed's relevant part (generosity + topical authority).
- Avoid thin syndicate copies: each platform gets a native opener; body may reuse with canonical.

**Series hub (AEO):** a single page listing all episodes with one-sentence answers + links — useful for "building an AI companion with memory" style queries.

**Do not:** keyword-stuff; claim metrics you did not measure; publish identical walls of text that look like spam to mods or crawlers.

---

## 8. Name-vote process (companion / product naming)

Name is Episode 1 content, not a pre-decision.

1. **Brainstorm** 10–12 candidates (Justin + bot draft; Justin edits).
2. **Pre-flight checks** (before the vote list goes public): RDAP/WHOIS for domains (not dig alone — local DNS can hijack NXDOMAIN); handle availability for X, GitHub, YouTube, TikTok as relevant.
3. Publish shortlist **only after** checks; mark any "taken / risky" clearly.
4. **Open vote** in Ep 1 comments (and optionally X poll) with a close date.
5. Justin picks winner (vote informs; Justin decides — brand/legal may override plurality).
6. **Only then** consider domain/handle acquisition — and **only Justin** may buy/spend (bot never buys domains/handles; see anti-jobs). Prefer waiting until vote closes.

Job vote runs in parallel (accountability partner / conversational friend / learning partner) — recommended default remains accountability/follow-through unless Justin locks otherwise.

---

## 9. Decision log + cost log (where they live; how they feed articles)

**Location:**

- Working copies during the series: `/workspace/companion-series/logs/` (create when Sprint 1 starts) **and** mirror into the public GitHub repo (e.g. `/docs/decision-log.md`, `/docs/cost-log.md`) once the repo exists.
- Templates: `/workspace/companion-series/templates/decision-log.md`, `/workspace/companion-series/templates/cost-log.md`.

**Habit:** ≤5 minutes/day while building. Do not backfill fiction.

**How they feed outlines:**

- Decision log rows → "why we picked X" sections and diagrams.
- Cost log rows → episode **cost box** (only real rows; if empty, say "$0 measured — caps are…").
- Failures / slips → hooks and Ep 7 material.
- Never invent costs, recall scores, or demo results for a prettier chart.

---

## 10. Budget guardrails (targets — not invented spend)

Set before code; report honestly:

- Hosting/dev target: **≤ $25/mo all-in until Episode 6** (Cloud Run scale-to-zero + small Postgres).
- LLM dev spend cap: **$20/mo** during build; every call logged to `cost_events` when instrumentation exists.
- **No paid TTS in v1.**
- Time: one build sprint/week. Content project with a product output — not a second DesireAI.
- Bot **never spends money** (domains, handles, ads, API top-ups). Justin owns all spend.

---

## 11. Risks and mitigations

| Risk | Likelihood / impact | Mitigation |
|---|---|---|
| Scope creep | High / High | Episode table is the scope. New ideas → Ep 7 vote list only. |
| Safety / crisis / "therapy" misuse | Med / Critical | SFW + not-a-therapist + crisis resources in v1 prompts; proactive guardrails reviewed before public demo (Ep 5/6 gate). |
| Cost surprise (LLM or hosting) | Med / High | Caps above; `cost_events` + `/costs`; kill paid TTS; router for cheap models. |
| Naive memory quality | High / Med | Layered memory + eval harness; publish real recall scores. |
| 3D rabbit hole | Med / Med | 2D first; 3D only on audience vote with data. |
| Name/domain collision or trademark | Med / Med | RDAP + handle checks before vote; Justin legal gut-check before buy. |
| Time competition (DesireAI / other projects) | High / High | Weekly cadence only; slips become content; no daily publish fantasy. |
| Accidental Desire crossover | Med / High | Hard rule: never publish about Desire in this series; separate repos/docs. |
| Invented metrics in drafts | Med / Reputation | Anti-job: no invented costs/scores/demos; cost box says "unmeasured" when true. |
| Bot overreach (code, spend, DMs, video) | Med / Process | Anti-jobs §12; approval gate §0. |
| Reddit / platform bans | Med / Distro | One sub, native rewrite, value-first; no multi-sub spam. |
| German QA bottleneck | Med / Schedule | Explicit `needs-justin-qa-de` flag; 2–3 day lag planned in. |

Honest unknowns (carry from v1 plan): companion safety review depth before public demo; exact domain after vote; how much audience wants voice vs text.

---

## 12. Anti-jobs (Companion Series bot — hard no)

The bot / agents helping this series must **never**:

1. Produce **videos, Shorts, or voiceovers** (article + text syndication only in this plan).
2. Write or deploy **companion product code / infra** (plan and article drafts about the stack only).
3. **Publish about Desire** / desireai.app or mix Desire assets into this series.
4. **Invent** costs, recall scores, demo results, or user counts.
5. **Buy domains or handles** or spend money of any kind.
6. **Message strangers** (no cold DMs, no unsolicited Reddit/LinkedIn outreach).
7. Publish or syndicate **without Justin's explicit yes for that piece**.
8. Treat PLAN-v2 or any prior plan as a blanket publish license.

When unsure: draft, show Justin, wait.

---

## 13. Immediate next steps (order)

1. [x] **Justin approved PLAN-v2** (2026-10-27).
2. [ ] Justin locks companion **job** emphasis: accountability/follow-through (recommended) vs friend vs learning partner.
3. [ ] Name shortlist brainstorm → RDAP/handle checks → Ep 1 vote list (no purchases yet).
4. [ ] Create public GitHub repo + copy decision/cost log templates into `/docs/` (Justin or approved action).
5. [ ] Revise `EPISODE-1-OUTLINE.md` to article-first syndication (drop bot-owned Shorts/video tasks) after PLAN-v2 approval.
6. [ ] Sprint 1 (Justin): skeleton + intentional forget-on-refresh demo.
7. [ ] Draft Episode 1 article from outline + logs → **approval gate** → publish checklist.

---

## 14. Files in this project

| File | Role |
|---|---|
| `PROJECT-PLAN.md` | Original plan (preserved; historical) |
| `EPISODE-1-OUTLINE.md` | Ep 1 outline (preserved; update after PLAN-v2 approval) |
| `PLAN-v2.md` | Improved plan — **locked 2026-10-27** |
| `CHANGELOG-PLAN-v2.md` | What changed vs original and why |
| `templates/decision-log.md` | Daily decision log template → feeds articles |
| `templates/cost-log.md` | Daily cost log template → feeds cost boxes |
| `logs/` | (create at Sprint 1) working decision/cost logs |

Sister context (do not merge into this series): DesireAI build-in-public notes live elsewhere; never publish Desire material under this series name.
