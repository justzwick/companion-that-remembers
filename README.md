# Building a Companion That Remembers

Public build-in-public series: a fresh **SFW** AI companion with memory you can see, a job (accountability / follow-through by default), and costs published live from day one.

**Canonical articles:** [justinzwick.com](https://justinzwick.com)  
**Series plan:** [`docs/PLAN-v2.md`](docs/PLAN-v2.md) (approved)

## What this repo is

- Architecture notes, decision/cost logs, and episode drafts for the series
- **Not** Desire / desireai.app — separate product, never mixed here

## Stack (written about in the series; built by Justin)

Python · FastAPI · PostgreSQL + pgvector · local embeddings · Kokoro TTS · Cloud Run

## Credit

Inspired by [Mohamed Abu Saif's Lumine series](https://mohamedabusaif.hashnode.dev/) and his [r/Development write-up](https://www.reddit.com/r/Development/comments/1uhrwlp/i_built_an_ai_companion_from_scratch_memory_rag/). We credit him openly and fix his failure points by design. Do not copy his prose.

## Status

Episode 1 draft approved; publishing to justinzwick.com. Product code ships in later commits as Justin builds — this repo starts as the series artifact + logs.

## Logs

- [`docs/decision-log.md`](docs/decision-log.md)
- [`docs/cost-log.md`](docs/cost-log.md)

Never invent costs or recall scores — only measured numbers.
