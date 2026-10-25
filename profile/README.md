<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./agboss-wordmark-dark.svg">
    <img src="./agboss-wordmark-light.svg" alt="AGBoss — Agent Graph Builder Open Source System" width="340">
  </picture>
</p>

<p align="center">
  <b>Build complex AI workflows. Catch graph errors before runtime.</b><br>
  <b>AGBoss</b> is an open-source TypeScript framework with static verification (<code>agboss check</code>) for complex multi-agent workflows on <a href="https://www.langchain.com/langgraph">LangGraph</a>, combining dependency injection, typed dataflow (<b>HarnessDSL</b>), LIFO saga compensation, and deterministic testing &amp; evaluation.
</p>

<p align="center">
  <code>Apache-2.0</code> · <code>TypeScript</code> · <code>v0.1.0-rc (pre-release in source; 0.0.1 placeholder on npm)</code>
</p>

<p align="center">
  <a href="https://agboss.dev">Documentation</a> ·
  <a href="https://agboss.dev/features/">Feature Explorer</a> ·
  <a href="https://agboss.dev/concepts/architecture/">7 Principles</a> ·
  <a href="https://agboss.dev/concepts/verification-funnel/">5-Loop Verification Funnel</a> ·
  <a href="https://github.com/agbossjs/agboss-examples">Examples (<code>01..07</code>)</a> ·
  <a href="https://agboss.dev/getting-started/quick-start/">Quick Start</a> ·
  <a href="https://agboss.dev/llms.txt"><code>llms.txt</code></a> ·
  <a href="https://x.com/agbossjs">X</a> ·
  <a href="https://www.linkedin.com/company/agbossjs">LinkedIn</a> ·
  <a href="https://www.youtube.com/@agbossjs">YouTube</a>
</p>

---

## Core Repositories

| Repository | Role in the AGBoss Ecosystem | Status |
| :--- | :--- | :--- |
| [**`agbossjs/agboss`**](https://github.com/agbossjs/agboss) | **Framework & CLI:** Core TypeScript runtime (`agboss`), static workflow compiler & CLI (`agboss-cli`: `agboss check`, `agboss eval`, `agboss hub`, `agboss describe`), 10 unit runners (`agboss/testing`), `@EvalSuite` (`agboss/eval`), and 13 bundled AI coding skills | `v0.1.0-rc` in source (`0.0.1` placeholder on npm) |
| [**`agbossjs/agboss.dev`**](https://github.com/agbossjs/agboss.dev) | **Website & Documentation:** Official documentation ([agboss.dev](https://agboss.dev)), Interactive Feature Explorer (`/features/`), 65 compiler-verified guides & decorator references, `llms.txt`, and `llms-full.txt` | Live (`check:docs` verified) |
| [**`agbossjs/agboss-examples`**](https://github.com/agbossjs/agboss-examples) | **Executable Proofs:** 7 standalone, CI-verified applications (`01-hello-agboss` through `07-langgraph-migration`) with offline `npm run check`, `npm test`, and `npm run demo` suites | 7 / 7 passing (`P0` + `P1`) |

---

## What is AGBoss?

**AGBoss** (**Agent Graph Builder Open Source System**) is an open-source TypeScript framework (`agboss`) and static workflow compiler (`agboss-cli` / `agboss check`) built on [LangGraph](https://www.langchain.com/langgraph). Its declarative workflow grammar, **HarnessDSL (HDSL)**, replaces shared mutable state bags (`Annotation.Root`) with narrow `@Dto` handoffs, compile-time Path Scope dominance (`.agboss/types.d.ts`), declarative `compensateWith` / `committedBy` LIFO sagas, KV-cache prefix invariance, and a 5-loop verification funnel.

---

## How does AGBoss enforce 7 distributed-systems invariants at compile time?

| # | Architectural Invariant | What `agboss check` & Runtime Enforce | Executable Proof (`agboss-examples`) |
| :--- | :--- | :--- | :--- |
| **1** | [**Computational Role Segregation**](https://agboss.dev/concepts/decorators/) | Strictly separates stochastic reasoning (`@Agent`), classification (`@Router`), quorum consensus (`@Arbiter`), and deterministic side effects (`@Action`) | [`01-hello-agboss`](https://github.com/agbossjs/agboss-examples/tree/main/01-hello-agboss) · [`07-langgraph-migration`](https://github.com/agbossjs/agboss-examples/tree/main/07-langgraph-migration) |
| **2** | [**Path-Scoped SSA Dataflow & Staleness**](https://agboss.dev/concepts/path-scope/) | Verifies upstream `@Dto` slots across declared branches (`[input.missing-on-path]`), emits `Produced<Dto, Producer>`, and invalidates stale loop drafts via `{ about }` (`[input.outdated-on-path]`) | [`02-path-scope-proof`](https://github.com/agbossjs/agboss-examples/tree/main/02-path-scope-proof) |
| **3** | [**Canonical Topology Isomorphism**](https://agboss.dev/concepts/workflow/) | Enforces one chained method call per line (`depth <= 4`), inline chains for `in-degree = 1`, and top-level `flow` entries for `in-degree >= 2` convergence | [`01-hello-agboss`](https://github.com/agbossjs/agboss-examples/tree/main/01-hello-agboss) · [`07-langgraph-migration`](https://github.com/agbossjs/agboss-examples/tree/main/07-langgraph-migration) |
| **4** | [**Prompt Prefix Cache Invariance**](https://agboss.dev/guides/security-and-pii/) | Rejects dynamic state in system instructions (`[prompt.input-in-instructions]`), canonically sorts tool schemas, and uses deterministic HMAC `replaceWithToken()` PII pseudonymization | [`03-support-refund-desk`](https://github.com/agbossjs/agboss-examples/tree/main/03-support-refund-desk) |
| **5** | [**Transactional LIFO Sagas & Idempotency**](https://agboss.dev/guides/error-handling-and-rollback/) | Requires `compensateWith` or `committedBy` on side-effecting `@Action` nodes (`[flow.uncompensated-action]`), unwinds completed steps in strict LIFO order, and forbids LLM calls inside `.onQuotaExceeded()` | [`04-travel-booking-saga`](https://github.com/agbossjs/agboss-examples/tree/main/04-travel-booking-saga) |
| **6** | [**Zero-Leakage DI & 4-Stage Scaling**](https://agboss.dev/guides/persistence-and-recovery/) | Scales identically from `InMemory*` stores to `Sqlite*` WAL persistence, 30-second pod leases, `ChildPlacement.AnyInstance` work-stealing, Kubernetes `@HealthCheck` probes, and [System-2 Digital & Physical AI missions](https://agboss.dev/concepts/mission-orchestration/) | [`05-cluster-work-stealing`](https://github.com/agbossjs/agboss-examples/tree/main/05-cluster-work-stealing) · [`06-enterprise-agent-mesh`](https://github.com/agbossjs/agboss-examples/tree/main/06-enterprise-agent-mesh) |
| **7** | [**Progressive Crystallization**](https://agboss.dev/concepts/progressive-crystallization/) | Joins `agboss describe --json` (`configHash`, `promptHash`, `schemaHash`) with production traces and `agboss hub` Crystallization Radar to crystallize settled `DecidedBy.Model` routes and tool guards into `$0`, `0ms` `DecidedBy.Code` | [`03-support-refund-desk`](https://github.com/agbossjs/agboss-examples/tree/main/03-support-refund-desk) · [`06-enterprise-agent-mesh`](https://github.com/agbossjs/agboss-examples/tree/main/06-enterprise-agent-mesh) |

---

## How does the 5-Loop Verification Funnel test agent systems?

Instead of paying live LLM tokens to catch wiring bugs, AGBoss filters every change through a [**5-Loop Cost & Determinism Verification Funnel**](https://agboss.dev/concepts/verification-funnel/) closed by **two production feedback loops**:

1. **Loop 0 — Static Compiler Proof (`agboss check --federation`, `< 1s`, `$0`):** Runs on every file save and statically checks declared workflow topology, Path Scope DTOs, saga compensations, and cross-service MCP/A2A mesh contracts before any test runs.
2. **Loop 1 — Deterministic Harness & Chaos (`npm test`, `~3s`, `$0`):** Runs on every commit without calling a live LLM — combining [10 isolated unit runners](https://agboss.dev/guides/testing-agents/) (`testTool`, `testToolInterceptor`, `testAction`, `testRouter`, `testArbiter`, `testRag`, `testJudge`, `testCompaction`, `testPii`, `testGuardrail`), full-graph `testWith(App)` with `mockLlm`, `ModelFailure` chaos (`429`, timeouts, truncated JSON), `TestConcurrency.inOrder`, multi-pod `{ instances: 2 }` leases, and VCR cassettes locked to Loop 2 by **`[vcr.stale-cassette]`** (`promptHash` + `schemaHash`).
3. **Loop 2 — Live-Model PR Smoke Gate (`agboss eval run --runs 5 --baseline`, `~1 min`, `$`):** Triggered when `promptHash` or `schemaHash` changes; executes a Fail-Fast `$0 -> $$$` cascade (`requiredTools`/`forbiddenTools`/`toolInvariant` $\to$ `$0` code `@Judge` $\to$ calibrated LLM `@Judge`), compares [`pass^k`](https://agboss.dev/guides/evaluation-and-profiles/) against the cached `main` baseline (`flaky-on-baseline` quarantine), and attaches a self-contained `eval-report.html`.
4. **Loop 3 — Nightly Polygon (`--runs 20 --metamorphic --chaos`, `~15 min`, `$$`):** Verifies paraphrase trajectory invariance (`--metamorphic`), multi-turn `@Compaction` constraint survival, stale-memory override, live fault recovery (`--chaos`), and silent upstream provider quantization drift across 200+ OpenRouter and JEV decision endpoints.
5. **Loop 4 — Production Telemetry & Two Feedback Loops (`24/7`):**
   - **Feedback Loop A (Failure Taxonomy & Calibration):** PII-scrubbed `ExecutionRecord` traces flow into `agboss eval ui`, where engineers tag failures, verify judge agreement via **Cohen's kappa ($\kappa \ge 0.80$)** (`agboss eval calibrate`), and promote failures to `agboss golden`.
   - **Feedback Loop B (Progressive Crystallization):** High `pass@k` (`100%`) with low `pass^k` (`40%`) triggers `$0` `@ToolInterceptor` self-correction (`ToolCallDecision.replyToModel`), while stable `pass^k = 100%` crystallizes `DecidedBy.Model` nodes into `DecidedBy.Code` — moving verification from paid Loop 2 back into free Loop 1.

---

## Executable Architecture & Reliability Proofs ([`agbossjs/agboss-examples`](https://github.com/agbossjs/agboss-examples))

Every standalone project in [`agbossjs/agboss-examples`](https://github.com/agbossjs/agboss-examples) follows the **`Promise → Failure Injection → Reproducible Proof`** discipline (`$0` offline `npm run check`, `npm test`, `npm run demo`), canonical `1 class = 1 file` component folders, and symmetric Spring-style `tests/` mirroring `src/`:

| Project | Primary Engineering Proof |
| :--- | :--- |
| [**`01-hello-agboss`**](https://github.com/agbossjs/agboss-examples/tree/main/01-hello-agboss) | Clean `agboss create` scaffolding, real async HTTP service DI (`PricingCatalogService` with `ExecutionContext.signal` + `step.idempotencyKey`), production HTTP server (`src/http/server.ts`), and 13 synced AI skills |
| [**`02-path-scope-proof`**](https://github.com/agbossjs/agboss-examples/tree/main/02-path-scope-proof) | Compile-time detection of `[input.missing-on-path]`, `[input.outdated-on-path]`, `[launch.branch-outdates-sibling]`, and `[flow.uncompensated-action]` vs ordinary TypeScript / LangGraph |
| [**`03-support-refund-desk`**](https://github.com/agbossjs/agboss-examples/tree/main/03-support-refund-desk) | Reversible `@PiiPolicy` (`replaceWithToken`, `revealPii`), `@ToolInterceptor`, `DecidedBy.Code` guardrails, in-memory BM25/TF-IDF `@Rag`, `PolicyAwareCompaction`, and 5-layer `@EvalSuite` (`pass@k` vs `pass^k`, compact git baseline, Cohen's $\kappa$) |
| [**`04-travel-booking-saga`**](https://github.com/agbossjs/agboss-examples/tree/main/04-travel-booking-saga) | Reverse-order LIFO saga compensation (`compensateWith`, `committedBy`), `@Budget` cutoff, `app.cancel()`, SQLite WAL crash recovery, `.joinQuorum({ min: 2, max: 2 })`, and HITL `Channel.withdraw()` |
| [**`05-cluster-work-stealing`**](https://github.com/agbossjs/agboss-examples/tree/main/05-cluster-work-stealing) | `ChildPlacement.AnyInstance` distributed queue (`SqliteChildExecutionQueueRepository`), recursive `ThisWorkflow` (`maxDepth: 2`), 30s SQLite leases & fencing (`ExecutionLeaseLostError`), `maxConcurrentSteps` backpressure, and `@HealthCheck` |
| [**`06-enterprise-agent-mesh`**](https://github.com/agbossjs/agboss-examples/tree/main/06-enterprise-agent-mesh) | 3-service mesh (`1-order-gateway`, `2-billing-a2a`, `3-catalog-mcp`) over `@A2AAgent` / `@A2AExpose` & `@McpTool` / `@McpExpose`, Ed25519 `agboss-mesh` JWS envelope, `MeshLoopDetectedError`, MCP `_meta.callId` deduplication, and `agboss check --federation` Blast Radius |
| [**`07-langgraph-migration`**](https://github.com/agbossjs/agboss-examples/tree/main/07-langgraph-migration) | Side-by-side comparison of raw `@langchain/langgraph` (`before-langgraph/` with 15-field `Annotation.Root` God State, string edges, unbounded `Send()`, dynamic prompts) vs AGBoss (`after-agboss/` with 10 narrow `@Dto`s, `@Router`, `@Action`, `@Channel`, and `agboss check --strict`) |

---

## Which repositories make up the AGBoss ecosystem?

| Repository | Description |
| :--- | :--- |
| [**agbossjs/agboss**](https://github.com/agbossjs/agboss) | Core TypeScript framework (`agboss`), static compiler & CLI (`agboss-cli`: `agboss check`, `agboss eval`, `agboss hub`, `agboss describe`), HarnessDSL (HDSL), 10 unit runners (`agboss/testing`), `@EvalSuite` (`agboss/eval`), and auto-synced `AGENTS.md` + 13 coding-agent skill modules |
| [**agbossjs/agboss-examples**](https://github.com/agbossjs/agboss-examples) | 7 standalone, CI-verified reference applications (`01-hello-agboss` through `07-langgraph-migration`) proving static graph analysis, PII safety, 5-loop evals, LIFO sagas, cluster work-stealing, multi-service A2A/MCP mesh, and LangGraph migration |
| [**agbossjs/agboss.dev**](https://github.com/agbossjs/agboss.dev) | Official documentation site ([agboss.dev](https://agboss.dev)), interactive architecture & code explorer, 64 compiler-verified guides & decorator references, [`llms.txt`](https://agboss.dev/llms.txt), and [`llms-full.txt`](https://agboss.dev/llms-full.txt) |

## Quick start

**Create a new project:**

```bash
npx agboss create my-app
cd my-app
npm install
npm test
```

**Add to an existing project:**

```bash
npm install agboss
npm install -D agboss-cli
npx agboss check
```

Zero vendor lock-in. 100% open source under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0). Maintained by [Pavel Ravvich](https://github.com/pavelravvich). Full documentation: [https://agboss.dev](https://agboss.dev) · Contact: [hello@agboss.dev](mailto:hello@agboss.dev) · Security: [security@agboss.dev](mailto:security@agboss.dev)
