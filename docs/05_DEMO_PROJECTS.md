# Demo projects (for CV + LinkedIn + interviews)
Goal: give abroad screeners **public proof** that matches target JDs (fintech/payments, distributed backend, Java 21/Spring Boot 3). Two solid projects are better than five toy ones. You must be able to explain every line, so Claude (builder) explains its design choices as it builds; Codex plans each milestone and reviews it.

Rules: built from scratch, generic data, no employer code/schema/names/metrics. Public GitHub. Each repo has a README covering problem, architecture diagram, how to run (`docker compose up`), design decisions, tests and what you'd do next.

---
## #1 `ledger-service` — Payments Ledger API (flagship, Phase 1)
**Why:** PayPay, N26, Zalando Payments, Picnic and most fintechs ask about money correctness, idempotency, concurrency and event-driven design. This project lets you answer those from your own code.

Stack: Java 21, Spring Boot 3, PostgreSQL, Flyway, Spring Data JPA, Testcontainers, JUnit 5, Docker Compose, GitHub Actions; Kafka (or Redpanda) in M3; Micrometer/Prometheus in M4.

| Milestone | Target | Scope | Interview talking points |
|---|---|---|---|
| M1 | Wed 14 Oct | Accounts + transfers REST API; Postgres + Flyway; Docker Compose; `Idempotency-Key` on `POST /transfers`; Testcontainers integration test | Idempotency, REST design, migrations |
| Public | Fri 16 Oct | README v1, push to GitHub | — |
| M2 | Sun 25 Oct | Double-entry journal (debit/credit lines, sum = 0); balance invariant; optimistic locking `@Version` vs `SELECT … FOR UPDATE`; test with 100 concurrent transfers (no lost updates); CI | Isolation levels, deadlocks, race conditions |
| M3 | Sun 1 Nov | Transactional outbox → Kafka `transfer.completed`; consumer with retries + dead-letter; OpenAPI docs | At-least-once vs exactly-once, outbox, eventual consistency |
| M4 | Sun 8 Nov | Actuator + Micrometer metrics, rate limiting, k6 load test results in README, architecture diagram, 2-min demo GIF | Observability, capacity, trade-offs |

After M2: add to CV Projects section + LinkedIn Featured. After M4: write a short LinkedIn post (public-safe) about one design decision.

| M5 (bonus) | Sun 15 Nov | **AI add-on:** read-only MCP server (Java MCP SDK / Spring AI MCP) exposing `getBalance`, `listTransfers` so an AI assistant can query the ledger safely | MCP, tool security, read-only design |

---
## #2 `jobfit-copilot` — AI job-matching assistant (Phase 2, 16 Nov – 6 Dec) ⭐ AI flagship
**Real-life use:** you use it for your own job search every week. Paste a JD → it extracts requirements, scores your CV variants against them **with evidence lines from your CV**, lists gaps, suggests tailored bullets that only reuse facts already in your CV (no invented claims), and logs the application.
Stack: Spring Boot 3, **Spring AI**, PostgreSQL + **pgvector**, **Ollama** local model (free, no paid key; optional OpenAI/Anthropic key via config), structured output (JSON schema), simple React or Thymeleaf UI.
| Milestone | Target | Scope | Talking points |
|---|---|---|---|
| M1 | Sun 22 Nov | JD + CV ingestion, chunking, embeddings in pgvector, requirement extraction to JSON | RAG basics, embeddings, structured output |
| M2 | Sun 29 Nov | Match score with cited evidence; "no-fabrication" guard (every suggested bullet must map to a CV chunk) | Hallucination control, grounding |
| M3 | Sun 6 Dec | Small eval set (10 JDs, expected skills) + accuracy report in README; latency/cost notes; application log | LLM evals, trade-offs |
Public repo: use a **sample CV** (fictional) in the repo; your real CV stays local in `private/`.

---
## #3 `db-migrate-verify` — resumable cross-database migration + verification CLI (Phase 3, December)
**Why:** this is your strongest real-world story (migration lead). It shows the same skills in a public, generic form: chunked copy, checkpoints, parallelism and data verification.

Scope: Spring Boot CLI (or Spring Batch) that copies a table MySQL → PostgreSQL in key-range chunks with parallel workers; checkpoint table for resume after crash; per-chunk row count + checksum compare; mismatch report (CSV/JSON); Testcontainers tests with seeded data; Docker Compose with both DBs.

| Milestone | Target | Scope |
|---|---|---|
| M1 | Sun 13 Dec | Chunked single-thread copy + checkpoint/resume |
| M2 | Sun 20 Dec | Parallel workers + checksum verification + report |
| M3 | Sun 27 Dec | README, benchmark on 1M generated rows, failure-injection test |

Keep it generic. Do not reuse the employer's design documents, naming or numbers.

---
## Idea bank (real-life, pick later — Controller proposes at monthly checkpoint)
| Idea | Real-life use | AI? | Why hirers care |
|---|---|---|---|
| **Exam Tutor RAG** (Bangla/Japanese) | Your Govt + JLPT study: answers with source citations, marks "unverified", builds spaced-repetition quizzes from your mistakes | Yes (RAG + evals) | Grounded AI, multilingual; can grow into the Govt practice app (needs Govt repo authorization) |
| **Receipt → Ledger** | Photo of a receipt/invoice → vision model extracts fields → posts entries to ledger-service | Yes (vision + structured output) | Document AI + payments, joins #1 |
| **Incident log triage** | Paste app logs/stack traces → clusters errors, summarises likely cause, links runbook | Yes | Backend ops + AI; SRE-friendly |
| **Bangla public-notice tracker** | Scrape-free: you paste Govt/bank job circulars → extracts eligibility, deadline, documents into a tracker with reminders | Yes (extraction) | Real local problem; structured extraction |
| **Rate limiter / API gateway** | Token-bucket service with Redis, used in front of your other projects | No | Classic system-design interview topic |
| **Feature-flag service** | Toggle features in your own apps without redeploy | No | Distributed config, caching, SDK design |
Rules for any new project: one at a time; must map to target JDs; user approves before starting (Controller asks).

---
## Build log (Codex appends here)
| Date | Repo | Milestone | Commit | Notes |
|---|---|---|---|---|
| 2026-10-11 | ledger-service | M1 | 6d4f16f | Built by Claude Code. Accounts + recorded transfers, Flyway V1, durable/concurrent Idempotency-Key (UNIQUE + ON CONFLICT DO NOTHING), Compose, README. 39 tests pass on real PostgreSQL 17 via the external-DB override; Testcontainers + `docker compose up` not yet run (no Docker on this PC). Codex review pending. |
