<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./agboss-wordmark-dark.svg">
    <img src="./agboss-wordmark-light.svg" alt="AGBoss — Agent Graph Builder Open Source System" width="340">
  </picture>
</p>

<p align="center">
  <b>Your coding agent writes the graph. AGBoss proves it right.</b><br>
  Static graph compiler (<code>agboss check</code>) &amp; production runtime that scales to hundreds of nodes on <a href="https://www.langchain.com/langgraph">LangGraph</a>, with its built-in declarative <b>HarnessDSL (HDSL)</b> grammar, <b>7 distributed-systems invariants</b>, <b>5-loop verification funnel</b>, and offline-synced <code>AGENTS.md</code> coding skills.
</p>

<p align="center">
  <a href="https://agboss.dev">Documentation</a> ·
  <a href="https://agboss.dev/concepts/architecture/">7 Principles</a> ·
  <a href="https://agboss.dev/concepts/verification-funnel/">5-Loop Verification Funnel</a> ·
  <a href="https://agboss.dev/concepts/ai-coding-skills/">AI Coding Skills</a> ·
  <a href="https://agboss.dev/getting-started/quick-start/">Quick Start</a> ·
  <a href="https://agboss.dev/llms.txt"><code>llms.txt</code></a> ·
  <a href="https://www.npmjs.com/package/agboss">npm (<code>agboss</code>)</a> ·
  <a href="https://x.com/agbossjs">X</a> ·
  <a href="https://www.linkedin.com/company/agbossjs">LinkedIn</a> ·
  <a href="https://www.youtube.com/@agbossjs">YouTube</a>
</p>

---

## What is AGBoss and HarnessDSL (HDSL)?

**AGBoss** (**Agent Graph Builder Open Source System**) is an open-source TypeScript framework (`npm i agboss`), static graph compiler (`agboss check`), and production runtime on [LangGraph](https://www.langchain.com/langgraph), and **HarnessDSL (HDSL)** is its built-in declarative graph grammar and verification specification (analogous to JSX in React or PSL in Prisma). Engineered for teams building 100+ node multi-agent graphs with AI coding agents (**Codex**, **Cursor**, **Windsurf**, **GitHub Copilot**, **Gemini CLI**, **Claude Code**), AGBoss replaces mutable God State Objects with compile-time Path Scope dominance (`.agboss/types.d.ts`), transactional LIFO sagas, KV-cache prefix invariance, and a 5-loop cost and determinism verification funnel.

---

## How does AGBoss enforce 7 distributed-systems invariants at compile time?

| # | Architectural Invariant | What `agboss check` & Runtime Enforce |
| :--- | :--- | :--- |
| **1** | [**Computational Role Segregation**](https://agboss.dev/concepts/decorators/) | Strictly separates stochastic reasoning (`@Agent`), classification (`@Router`), quorum consensus (`@Arbiter`), and deterministic side effects (`@Action`) |
| **2** | [**Path-Scoped SSA Dataflow & Staleness**](https://agboss.dev/concepts/path-scope/) | Proves `100%` of upstream `@Dto` slots on every branch (`[input.missing-on-path]`), emits `Produced<Dto, Producer>`, and invalidates stale loop drafts via `{ about }` (`[input.outdated-on-path]`) |
| **3** | [**Canonical Topology Isomorphism**](https://agboss.dev/concepts/workflow/) | Enforces one chained method call per line (`depth <= 4`), inline chains for `in-degree = 1`, and top-level `flow` entries for `in-degree >= 2` convergence |
| **4** | [**Prompt Prefix Cache Invariance**](https://agboss.dev/guides/security-and-pii/) | Rejects dynamic state in system instructions (`[prompt.input-in-instructions]`), canonically sorts tool schemas, and uses deterministic HMAC `replaceWithToken()` PII pseudonymization |
| **5** | [**Transactional LIFO Sagas & Idempotency**](https://agboss.dev/guides/error-handling-and-rollback/) | Requires `compensateWith` or `committedBy` on side-effecting `@Action` nodes (`[flow.uncompensated-action]`), unwinds completed steps in strict LIFO order, and forbids LLM calls inside `.onQuotaExceeded()` |
| **6** | [**Zero-Leakage DI & 4-Stage Scaling**](https://agboss.dev/guides/persistence-and-recovery/) | Scales identically from `InMemory*` stores to `Sqlite*` WAL persistence, 30-second pod leases, Kubernetes `@HealthCheck` probes, and [System-2 Digital & Physical AI missions](https://agboss.dev/concepts/mission-orchestration/) without changing workflow code |
| **7** | [**Progressive Crystallization**](https://agboss.dev/concepts/progressive-crystallization/) | Joins `agboss describe --json` (`configHash`, `promptHash`, `schemaHash`) with production traces to crystallize settled `DecidedBy.Model` routes and tool guards into `$0`, `0ms` `DecidedBy.Code` |

---

## How does the 5-Loop Verification Funnel test agent systems?

Instead of paying live LLM tokens to catch wiring bugs, AGBoss filters every change through a [**5-Loop Cost & Determinism Verification Funnel**](https://agboss.dev/concepts/verification-funnel/) closed by **two production feedback loops**:

1. **Loop 0 — Static Compiler Proof (`agboss check --federation`, `< 1s`, `$0`):** Runs on every file save and proves 100% of graph topology, Path Scope DTOs, saga compensations, and cross-service MCP/A2A mesh contracts before any test runs.
2. **Loop 1 — Deterministic Harness & Chaos (`npm test`, `~3s`, `$0`):** Runs on every commit without calling a live LLM — combining [10 isolated unit runners](https://agboss.dev/guides/testing-agents/) (`testTool`, `testToolInterceptor`, `testAction`, `testRouter`, `testArbiter`, `testRag`, `testJudge`, `testCompaction`, `testPii`, `testGuardrail`), full-graph `testWith(App)` with `mockLlm`, `ModelFailure` chaos (`429`, timeouts, truncated JSON), `TestConcurrency.inOrder`, multi-pod `{ instances: 2 }` leases, and VCR cassettes locked to Loop 2 by **`[vcr.stale-cassette]`** (`promptHash` + `schemaHash`).
3. **Loop 2 — Live-Model PR Smoke Gate (`agboss eval run --runs 5 --baseline`, `~1 min`, `$`):** Triggered when `promptHash` or `schemaHash` changes; executes a Fail-Fast `$0 -> $$$` cascade (`requiredTools`/`forbiddenTools`/`toolInvariant` $\to$ `$0` code `@Judge` $\to$ calibrated LLM `@Judge`), compares [`pass^k`](https://agboss.dev/guides/evaluation-and-profiles/) against the cached `main` baseline (`flaky-on-baseline` quarantine), and attaches a self-contained `eval-report.html`.
4. **Loop 3 — Nightly Polygon (`--runs 20 --metamorphic --chaos`, `~15 min`, `$$`):** Verifies paraphrase trajectory invariance (`--metamorphic`), multi-turn `@Compaction` constraint survival, stale-memory override, live fault recovery (`--chaos`), and silent upstream provider quantization drift across 200+ OpenRouter and JEV decision endpoints.
5. **Loop 4 — Production Telemetry & Two Feedback Loops (`24/7`):**
   - **Feedback Loop A (Failure Taxonomy & Calibration):** PII-scrubbed `ExecutionRecord` traces flow into `agboss eval ui`, where engineers tag failures, verify judge agreement via **Cohen's kappa ($\kappa \ge 0.80$)** (`agboss eval calibrate`), and promote failures to `agboss golden`.
   - **Feedback Loop B (Progressive Crystallization):** High `pass@k` (`100%`) with low `pass^k` (`40%`) triggers `$0` `@ToolInterceptor` self-correction (`ToolCallDecision.replyToModel`), while stable `pass^k = 100%` crystallizes `DecidedBy.Model` nodes into `DecidedBy.Code` — moving verification from paid Loop 2 back into free Loop 1.

---

## Which repositories make up the AGBoss ecosystem?

| Repository | Description |
| :--- | :--- |
| [**agbossjs/agboss**](https://github.com/agbossjs/agboss) | Core TypeScript framework (`agboss`), static compiler & CLI (`agboss check`, `agboss eval`, `agboss describe`), HarnessDSL (HDSL), 10 unit runners (`agboss/testing`), `@EvalSuite` (`agboss/eval`), and auto-synced `AGENTS.md` + 12 coding-agent skill modules |
| [**agbossjs/agboss.dev**](https://github.com/agbossjs/agboss.dev) | Official documentation site ([agboss.dev](https://agboss.dev)), interactive architecture & code explorer, 64 compiler-verified guides & decorator references, [`llms.txt`](https://agboss.dev/llms.txt), and [`llms-full.txt`](https://agboss.dev/llms-full.txt) |

## Quick start

```bash
npx agboss create my-app --yes
cd my-app
npx agboss check && npm test
```

Zero vendor lock-in. 100% open source under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0). Maintained by [Pavel Ravvich](https://github.com/pavelravvich). Full documentation: [https://agboss.dev](https://agboss.dev) · Contact: [hello@agboss.dev](mailto:hello@agboss.dev) · Security: [security@agboss.dev](mailto:security@agboss.dev)
