<div align="center">

<img src="https://github.com/agbossjs.png" width="100" alt="AGBoss" />

# AGBoss

Typed, high-level DI framework for LangGraph agents in TypeScript

[Docs](https://agboss.dev) · [Framework](https://github.com/agbossjs/agboss) · [Docs site source](https://github.com/agbossjs/agboss.dev)

</div>

---

AGBoss gives agents, tools, MCP servers and knowledge bases a typed, dependency-injected component model — annotated classes wired together by a `@Workflow` module, every link checked by the compiler. Routing and judging run on decision models, with FinOps cost accounting, evaluation and tracing built in.

## Repositories

| Repo | What it is |
| --- | --- |
| [**agboss**](https://github.com/agbossjs/agboss) | The framework: components, DI, the flow DSL, CLI, testing |
| [**agboss.dev**](https://github.com/agbossjs/agboss.dev) | Source for the documentation site |

## Quick start

```bash
npx -p agboss-cli agboss create my-agents
cd my-agents && cp .env.example .env && npm run chat
```

Full guide: [agboss.dev](https://agboss.dev).
