<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./agboss-wordmark-dark.svg">
    <img src="./agboss-wordmark-light.svg" alt="AGBoss" width="340">
  </picture>
</p>

<p align="center">
  <b>The Spring-style TypeScript framework for mission-critical LangGraph AI agents.</b><br>
  Compile-time Path Scope DSL, constructor DI, transactional LIFO sagas, FinOps caps, human pauses, and deterministic unit testing.
</p>

<p align="center">
  <a href="https://agboss.dev">Documentation</a> ·
  <a href="https://agboss.dev/getting-started/quick-start/">Quick Start</a> ·
  <a href="https://www.npmjs.com/package/agboss">npm (<code>agboss</code>)</a> ·
  <a href="https://www.npmjs.com/package/agboss-cli">CLI (<code>agboss-cli</code>)</a> ·
  <a href="https://x.com/agbossjs">X</a> ·
  <a href="https://www.linkedin.com/company/agbossjs">LinkedIn</a>
</p>

---

- **Compile-Time Architecture (`@Application` & Path Scope DSL)** — agents, actions, routers, arbiters, tools, MCP servers, and knowledge bases are strictly typed decorator components wired by constructor dependency injection. `agboss check` verifies every DTO slot, review loop (`about`, `historyOf`), and saga rollback before any model runs.
- **Mission-Critical Sagas, Quorums & Human Pauses** — declarative `@Action({ compensateWith, committedBy })` contracts with automatic LIFO unwinding on abort, multimodal/multi-agent quorums (`@Arbiter`), two-phase `@ChannelRole` pauses (`app.resume` + `withdraw()`), and multi-instance work-stealing over SQLite WAL leases.
- **FinOps, Boundary PII & Deterministic Testing** — pre-call `@Budget` reservations across Application/Tenant/Execution scopes with instant sibling abort, reversible `@PiiPolicy` token pseudonymization (`replaceWithToken()`), and offline tests via `testWith(App)` plus 8 isolated component runners.

Zero vendor lock-in. 100% open source under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).

## Repositories

| Repository | Description |
| --- | --- |
| [**agboss**](https://github.com/agbossjs/agboss) | Core TypeScript framework (`agboss`) and CLI (`agboss-cli`): decorators, Path Scope Flow DSL, DI container, FinOps, sagas, and testing |
| [**agboss.dev**](https://github.com/agbossjs/agboss.dev) | Official documentation site, interactive architecture explorer, API reference, and `llms.txt` ([agboss.dev](https://agboss.dev)) |

## Quick start

```bash
npx -p agboss-cli agboss create my-agents
cd my-agents && cp .env.example .env && npm run chat
```

Full documentation: [https://agboss.dev](https://agboss.dev) · Contact: [hello@agboss.dev](mailto:hello@agboss.dev) · Security: [security@agboss.dev](mailto:security@agboss.dev)
