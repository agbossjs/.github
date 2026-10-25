# AGBoss v0.1.0 — Cross-Repository Pre-Release Readiness & Verification Report

> **Release Embargo Status:** Active (`0` packages published, `0` git tags created, `0` GitHub Releases created, `0` merges to `main`, `0` production deployments triggered).
>
> **Release Readiness Verdict:** **`READY FOR OWNER MANUAL RELEASE`** (`v0.1.0`)

---

## 1. Repository State & Pull Request Matrix

| Repository | Branch | Verified Commit SHA | Open PR (Unmerged) | GitHub Actions Verification Run | CI Status |
| :--- | :--- | :--- | :--- | :--- | :---: |
| [`agbossjs/agboss`](https://github.com/agbossjs/agboss) | `chore/pre-release-remediation` | `fe351f66692c5c09ebdb0b7d1fddc578953c96d9` (`fe351f66`) | [PR #465](https://github.com/agbossjs/agboss/pull/465) | [Run #40231329807](https://github.com/agbossjs/agboss/actions/runs/40231329807) (`CI`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/agboss-examples`](https://github.com/agbossjs/agboss-examples) | `chore/pre-release-remediation` | `0cbb71ad00d99dc23ec1243f1862103a02a2ab7f` (`0cbb71a`) | [PR #21](https://github.com/agbossjs/agboss-examples/pull/21) | [Run #40231354733](https://github.com/agbossjs/agboss-examples/actions/runs/40231354733) (`push`) · [Run #40231358848](https://github.com/agbossjs/agboss-examples/actions/runs/40231358848) (`pull_request`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/agboss.dev`](https://github.com/agbossjs/agboss.dev) | `chore/pre-release-remediation` | `743443ab73bf8296628ad737739fee300e720a23` (`743443a`) | [PR #2](https://github.com/agbossjs/agboss.dev/pull/2) | [Run #40231356604](https://github.com/agbossjs/agboss.dev/actions/runs/40231356604) (`push`) · [Run #40231361685](https://github.com/agbossjs/agboss.dev/actions/runs/40231361685) (`pull_request`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/.github`](https://github.com/agbossjs/.github) | `chore/pre-release-remediation` | *(this commit)* | [PR #2](https://github.com/agbossjs/.github/pull/2) | N/A (static org profile & policy markdown) | **PASSED** (`Locally verified` + `Manually inspected`) |

---

## 2. Release Version Decision (`v0.1.0` vs. Historical `0.0.1` / `v0.0.2`)

- **Selected Release Version:** **`v0.1.0`** (`agboss@0.1.0` and `agboss-cli@0.1.0`).
- **Actual npm Registry State (`registry.npmjs.org`):** Only `0.0.0-stage` and `0.0.1` have ever been published (`agboss@0.0.1`, `agboss-cli@0.0.1`, `@agboss/core@0.0.1`, `@agboss/cli@0.0.1`). Those `0.0.1` packages were initial package-name reservation placeholders containing no framework code.
- **Why `v0.0.2` Appeared Earlier:** Early documentation drafts on `agboss.dev` (`origin/main`) used `0.0.2` as a working label before the complete v2 architecture (`@Application` DI, Path-Scoped SSA Flow DSL, 13 bundled AI coding skills, 10 unit runners, `@EvalSuite`, and Mesh Hub) was consolidated into the first substantive minor release **`0.1.0`**. `0.0.2` was never published to npm or tagged on GitHub.
- **Current Consistency:** Across all four remediation branches (`agboss`, `agboss-examples`, `agboss.dev`, `.github`), there are **`0` remaining references to `0.0.2` or `v0.0.2`** outside third-party transitive dependencies in `package-lock.json`.

---

## 3. Final Release Preparation & Release Engineering Summary

| ID | Repository | Finding / Preparation Item | Fix Applied | Verification Level |
| :--- | :--- | :--- | :--- | :--- |
| **P0-1 (A)** | `agboss` + `agboss-examples` | `RELEASE_CHECKLIST.md` Phase C.2 referenced a nonexistent `npm run install:all` script in `agboss-examples`. | Implemented [`scripts/install-published.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/install-published.mjs) (`npm run install:published`) and `npm run collect:published` in `agboss-examples/package.json`. | `Locally verified` + `GitHub CI verified` |
| **P0-1 (B)** | `agboss` + `agboss.dev` | Website pre-release vs. published mode switching across `RELEASE_META` and documentation pages. | Updated [`src/config/release.ts`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/src/config/release.ts), [`src/components/nav.ts`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/src/components/nav.ts), [`src/components/Head.astro`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/src/components/Head.astro), [`scripts/set-release-mode.mjs`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/scripts/set-release-mode.mjs) (`npm run release:mode -- --published` / `--pre-release`), and [`scripts/verify-release-meta.mjs`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/scripts/verify-release-meta.mjs) (`npm run verify:release-meta`). | `Locally verified` + `GitHub CI verified` |
| **P0-1 (C)** | `agboss` | Single authoritative manual release runbook (`OWNER MANUAL RELEASE — START HERE`) and GitHub Release draft. | Rewrote [`RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md) with the 8 chronological phases (Merge, Final Source Verification, Tag, Publish, Registry Smoke, Website Activation, GitHub Release, Final Public Verification) and created [`.github/RELEASE_NOTES_v0.1.0.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/.github/RELEASE_NOTES_v0.1.0.md). | `Locally verified` + `GitHub CI verified` |
| **P0-2** | `agboss-examples` | Hardened `scripts/verify-published.mjs` with explicit `--from-packed` vs. `--live-registry` modes. | Rewrote [`scripts/verify-published.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/verify-published.mjs) to require `--from-packed` or `--live-registry` and `--version <semver>`, print `SOURCE: LOCAL_PACKED_RELEASE_CANDIDATE` vs `SOURCE: PUBLIC_NPM_REGISTRY`, query `npm view` & `dist-tags` in `--live-registry` mode, verify all 11 subpath exports, CLI binary resolution, `agboss create`, 13 AI skill reference files, `check`, `tsc --noEmit`, `npm test`, and `describe --json`. | `Locally verified` + `GitHub CI verified` |
| **P0-3** | `agboss-examples` | Separate published-package evidence from RC evidence. | Updated [`scripts/run-all.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/run-all.mjs) and [`scripts/collect-results.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/collect-results.mjs) with explicit `--provenance rc` vs `--provenance published` modes and 24 unit tests in [`scripts/run-all.test.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/run-all.test.mjs). | `Locally verified` (`24/24` unit tests) + `GitHub CI verified` |
| **P1-1** | `agboss` + `agboss-examples` + `agboss.dev` | Deterministic final merge SHA pinning (`FINAL_MAIN_FRAMEWORK_SHA`). | Added `scripts/pin-framework-sha.mjs` (`npm run pin:framework-sha`) to `agboss-examples` and `agboss.dev`, and documented Phase 1 in `RELEASE_CHECKLIST.md`. | `Locally verified` + `GitHub CI verified` |
| **P1-2** | `agboss` + `agboss-examples` + `.github` | Security advisory remediation and reachability disposition. | Upgraded `@modelcontextprotocol/sdk` to `^1.32.2` and `zod` to `^4.6.7` in `packages/agboss` and `packages/agboss-cli` and updated lockfile security patches, achieving **`0` production runtime vulnerabilities** in `packages/agboss`. | `Locally verified` + `GitHub CI verified` |
| **P1-3** | `agboss` | First-time developer experience cleanup (`agboss create` template & interactive CLI star link). | Updated [`packages/agboss-cli/src/cli/star.ts`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/packages/agboss-cli/src/cli/star.ts), [`packages/agboss-cli/templates/project/README.md.tmpl`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/packages/agboss-cli/templates/project/README.md.tmpl), and [`CHANGELOG.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/CHANGELOG.md) so all CLI output and generated project READMEs link to `https://github.com/agbossjs/agboss`. | `Locally verified` + `GitHub CI verified` |

---

## 4. Release-Candidate Tarball, Smoke Harness & Proof Report Evidence

### 4.1. Verified Release-Candidate Tarballs (`agboss` `fe351f66692c5c09ebdb0b7d1fddc578953c96d9`)

- **Framework Commit (`agbossjs/agboss`):** `fe351f66692c5c09ebdb0b7d1fddc578953c96d9`
- **Examples Commit (`agbossjs/agboss-examples`):** `0cbb71ad00d99dc23ec1243f1862103a02a2ab7f`
- **Website Commit (`agbossjs/agboss.dev`):** `743443ab73bf8296628ad737739fee300e720a23`
- **`agboss-0.1.0.tgz`:**
  - `integrity`: `sha512-Azu01jlAwG9bDzXTR/wsORpSZi36Y+/pSkh4/5D1rBKXgIQwTHjq63Wzoircq5LaTTnQJe7jX9AV3bCApUrqYQ==`
  - `sha256`: `b6ea25f6cd702e8c60a6dbe5978a6672ae85317de80f6f2c3c6cd384dcf9242a`
  - `entryCount`: `668` files (`bin/agboss.js`, `dist/`, `schema/profile.schema.json`, `skills/agboss/SKILL.md` + 13 reference guides `01-composition-and-architecture.md` .. `13-migrating-from-langgraph.md`, `README.md`, `LICENSE`, `NOTICE`)
- **`agboss-cli-0.1.0.tgz`:**
  - `integrity`: `sha512-TBwR10r9+X+3Id7FTqXPtLdrkOlgOINhS8x9XTj5dmoFBYMakGUQtwo1azodaMEMyoQfY9b9UwgGZBJrAzvV7g==`
  - `sha256`: `b7ec2927c0ed9f829da7bb90dbfcc4f22449871097b22ff66e8d7fd7a04724e1`
  - `entryCount`: `228` files (`bin/agboss.js`, `dist/cli.js`, `templates/`, `README.md`, `LICENSE`, `NOTICE`)

| Project | Priority | Mode | `npm run check` | `npm test` | `npm run demo` | Project Gate | Primary Proof |
| :--- | :---: | :--- | :---: | :---: | :---: | :---: | :--- |
| `01-hello-agboss` | **P0** | `offline-deterministic` | `PASSED` | `PASSED` (6 tests) | `PASSED` | **PASSED** | Independent installation, DI, workflow assembly, first execution |
| `02-path-scope-proof` | **P0** | `offline-deterministic` | `PASSED` | `PASSED` (6 tests) | `PASSED` | **PASSED** | Static graph safety, data availability, freshness, parallel dataflow |
| `03-support-refund-desk` | **P0** | `offline-deterministic` | `PASSED` | `PASSED` (6 tests) | `PASSED` | **PASSED** | Guardrails, PII, tool interception, memory, evals |
| `04-travel-booking-saga` | **P0** | `fault-injection` | `PASSED` | `PASSED` (12 tests) | `PASSED` | **PASSED** | Transaction compensation, interruption, recovery, quorum, HITL |
| `05-cluster-work-stealing` | **P1** | `fault-injection` | `PASSED` | `PASSED` (8 tests) | `PASSED` | **PASSED** | Distributed scheduling, leases, failure recovery, backpressure |
| `06-enterprise-agent-mesh` | **P1** | `multi-service` | `PASSED` | `PASSED` (8 tests) | `PASSED` | **PASSED** | A2A, MCP, federation, cross-service tracing |
| `07-langgraph-migration` | **P0** | `deterministic-reference` | `PASSED` | `PASSED` (6 tests) | `PASSED` | **PASSED** | Deterministic before/after reference migration from raw LangGraph God State to AGBoss narrow DTOs & Flow DSL |

---

## 5. Complete Security & Dependency Advisory Assessment (P1-2)

### 5.1. Summary by Repository & Package Surface

| Target / Scope | Critical | High | Moderate | Low | Total | Status for `v0.1.0` |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **`packages/agboss` (Production Runtime Tarball `agboss-0.1.0.tgz`)** | `0` | `0` | `0` | `0` | **`0`** | **CLEAN** (`0` runtime vulnerabilities) |
| **`agbossjs/agboss.dev` (Documentation Website)** | `0` | `0` | `0` | `0` | **`0`** | **CLEAN** (`0` vulnerabilities across 311 packages) |
| **`agbossjs/agboss` Monorepo Workspace (`npm audit`)** | `2` | `3` | `2` | `0` | **`7`** | **MITIGATED / NON-RUNTIME** (`3` CLI-only in `create-langgraph`, `4` dev-only in `vitest`) |
| **`agbossjs/agboss-examples` Root Tooling (`npm audit`)** | `2` | `0` | `1` | `0` | **`3`** | **DEV-ONLY** (`vitest@3.2.7` test runner only) |

### 5.2. Remaining Workspace Advisories — Detailed Reachability & Disposition Table

| Advisory ID | Package | Installed Version | Patched Version | Severity | Dependency Path | Exposure Scope | Reachability in AGBoss | Mitigation & Disposition for `v0.1.0` |
| :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `GHSA-jmr9-qjv8-65gv`<br/>`GHSA-7pqw-9j4j-h8q3` | `extract-zip` (via `create-langgraph` & `@langchain/langgraph-cli`) | `extract-zip@2.0.1`<br/>`create-langgraph@1.1.5`<br/>`@langchain/langgraph-cli@1.5.1` | None (`No fix available` upstream) | **High** (3 lockfile nodes) | `agboss-cli` → `@langchain/langgraph-cli@1.5.1` → `create-langgraph@1.1.5` → `extract-zip@2.0.1` | **CLI-only (`devDependencies`)** — never present in `agboss` production runtime (`dependencies`). | **Not reachable.** `packages/agboss-cli/src/cli/studio.ts` only invokes `langgraph dev` to start the local LangGraph Studio bridge; AGBoss never calls `create-langgraph` or `extract-zip` to unpack untrusted `.zip` archives (`agboss create` uses built-in local templates in `packages/agboss-cli/templates/`). | **Accepted for `v0.1.0` (Upstream CLI-only, Unreachable).** Isolated to `agboss-cli` (`devDependencies`). Tracked for removal/upgrade once `@langchain/langgraph-cli` drops `create-langgraph` or updates `extract-zip`. |
| `GHSA-82fw-gwwq-j7x9`<br/>`GHSA-qxg6-89mg-rqv8` | `@vitest/mocker` (via `vitest` & `@vitest/coverage-v8`) | `@vitest/mocker@3.2.7`<br/>`vitest@3.2.7`<br/>`@vitest/coverage-v8@3.2.7` | `vitest >= 5.0.5` (`isSemVerMajor: true`) | **Moderate** (2 lockfile nodes) | Root `devDependencies` → `vitest@3.2.7` / `@vitest/coverage-v8@3.2.7` → `@vitest/mocker@3.2.7` | **Dev-only (Workspace test runner)** — not shipped in `agboss-0.1.0.tgz` or `agboss-cli-0.1.0.tgz`. | **Not reachable in production or CLI.** `@vitest/mocker` runs only during local/CI test execution on trusted repository test files with browser mode disabled. | **Accepted for `v0.1.0` (Dev-only test runner).** Upgrading `vitest` from `3.x` to `5.x` is a semver-major test-runner migration scheduled post-`v0.1.0`; zero impact on published `agboss` or `agboss-cli` packages. |
| `GHSA-5gmw-xhrv-c9v3`<br/>`GHSA-85c8-ppgw-ccpr` | `tinypool` (via `vitest` & `@vitest/coverage-v8`) | `tinypool@1.1.1`<br/>`vitest@3.2.7` | `vitest >= 5.0.5` (`isSemVerMajor: true`) | **Critical** (2 lockfile nodes) | Root `devDependencies` → `vitest@3.2.7` → `tinypool@1.1.1` | **Dev-only (Workspace test runner)** — not shipped in `agboss-0.1.0.tgz` or `agboss-cli-0.1.0.tgz`. | **Not reachable in production or CLI.** `tinypool` worker options are controlled exclusively by Vitest's internal test runner process during local/CI test runs and never accept external or user-supplied input. | **Accepted for `v0.1.0` (Dev-only test runner).** Zero runtime or CLI exposure; will be resolved alongside the `vitest@5` devDependency upgrade post-`v0.1.0`. |

---

## 6. Owner Manual Release Entry Point

See [`agbossjs/agboss/RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md) (**`OWNER MANUAL RELEASE — START HERE`**) for the chronological 8-phase execution runbook.
