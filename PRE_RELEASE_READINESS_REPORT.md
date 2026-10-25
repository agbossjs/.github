# AGBoss v0.1.0 — Cross-Repository Pre-Release Readiness & Verification Report

> **Release Embargo Status:** Active (`0` packages published, `0` git tags created, `0` GitHub Releases created, `0` merges to `main`, `0` production deployments triggered).
>
> **Release Readiness Verdict:** **`READY FOR OWNER MANUAL RELEASE`** (`v0.1.0`)

---

## 1. Repository State & Pull Request Matrix

| Repository | Branch | Verified Commit SHA | Open PR (Unmerged) | GitHub Actions Verification Run | CI Status |
| :--- | :--- | :--- | :--- | :--- | :---: |
| [`agbossjs/agboss`](https://github.com/agbossjs/agboss) | `chore/pre-release-remediation` | `781c2052cb1cc2a564c21c3e3fdc807e2890ebf9` (`781c2052`) | [PR #465](https://github.com/agbossjs/agboss/pull/465) | [Run #40234994721](https://github.com/agbossjs/agboss/actions/runs/40234994721) (`CI`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/agboss-examples`](https://github.com/agbossjs/agboss-examples) | `chore/pre-release-remediation` | `2b8268307923100b264583b9eedecfaf352a979d` (`2b82683`) | [PR #21](https://github.com/agbossjs/agboss-examples/pull/21) | [Run #40235046260](https://github.com/agbossjs/agboss-examples/actions/runs/40235046260) (`push`) · [Run #40235050323](https://github.com/agbossjs/agboss-examples/actions/runs/40235050323) (`pull_request`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/agboss.dev`](https://github.com/agbossjs/agboss.dev) | `chore/pre-release-remediation` | `656ca373f1413a5520b0f997589dd04d1f5feb61` (`656ca37`) | [PR #2](https://github.com/agbossjs/agboss.dev/pull/2) | [Run #40235051109](https://github.com/agbossjs/agboss.dev/actions/runs/40235051109) (`push`) · [Run #40235055314](https://github.com/agbossjs/agboss.dev/actions/runs/40235055314) (`pull_request`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/.github`](https://github.com/agbossjs/.github) | `chore/pre-release-remediation` | *(this commit)* | [PR #2](https://github.com/agbossjs/.github/pull/2) | N/A (static org profile & policy markdown) | **PASSED** (`Locally verified` + `Manually inspected`) |

---

## 2. Release Version Decision (`v0.1.0` vs. Historical `0.0.1` / `v0.0.2`)

- **Selected Release Version:** **`v0.1.0`** (`agboss@0.1.0` and `agboss-cli@0.1.0`).
- **Actual npm Registry State (`registry.npmjs.org`):** Only `0.0.0-stage` and `0.0.1` have ever been published (`agboss@0.0.1`, `agboss-cli@0.0.1`, `@agboss/core@0.0.1`, `@agboss/cli@0.0.1`). Those `0.0.1` packages were initial package-name reservation placeholders containing no framework code.
- **Why `v0.0.2` Appeared Earlier:** Early documentation drafts on `agboss.dev` (`origin/main`) used `0.0.2` as a working label before the complete v2 architecture (`@Application` DI, Path-Scoped SSA Flow DSL, 13 bundled AI coding skills, 10 unit runners, `@EvalSuite`, and Mesh Hub) was consolidated into the first substantive minor release **`0.1.0`**. `0.0.2` was never published to npm or tagged on GitHub.
- **Current Consistency:** Across all four remediation branches (`agboss`, `agboss-examples`, `agboss.dev`, `.github`), there are **`0` remaining references to `0.0.2` or `v0.0.2`** outside third-party transitive dependencies in `package-lock.json`.

---

## 3. Final Release Preparation & Runbook Engineering Summary

| ID | Repository | Finding / Preparation Item | Fix Applied | Verification Level |
| :--- | :--- | :--- | :--- | :--- |
| **Fix 1** | `agboss.dev` + `agboss` | Stale `dist/` could fail `verify:release-meta` before `npm run build` when switching from pre-release to published mode. | Separated [`scripts/verify-release-meta.mjs`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/scripts/verify-release-meta.mjs) into `--source` (pre-build, ignores `dist/`) and `--dist` (post-build, requires fresh `dist/` matching active mode), added `data-release-mode` to [`src/components/Header.astro`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/src/components/Header.astro), added 6 regression tests in [`scripts/verify-release-meta.test.mjs`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/scripts/verify-release-meta.test.mjs) (`npm run test:release-meta`), and updated [`.github/workflows/deploy.yml`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/.github/workflows/deploy.yml) and [`RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md). | `Locally verified` (`6/6` tests + live Astro build switch) + `GitHub CI verified` |
| **Fix 2** | `agboss` | Field-name precision in `RELEASE_CHECKLIST.md` for `.agboss-version.json` and `.packed/manifest.json`. | Updated [`RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md) Step 1.2 to assert `node -p "require('./.agboss-version.json').frameworkSha"` and Step 2.2 to assert `node -p "require('./.packed/manifest.json').sourceCommit"`, verifying both equal `$FINAL_MAIN_FRAMEWORK_SHA`. | `Locally verified` + `GitHub CI verified` |
| **Fix 3** | `agboss` | Publish exact verified `.tgz` tarballs instead of re-packing via `npm publish --workspace=...`. | Updated Phase 4 in [`RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md) to verify `.packed/manifest.json` (`sourceCommit`, `examplesSourceCommit`, `name`, `version`, `sha256`, `integrity`), run `npm publish "$WORKSPACE_ROOT/agboss-examples/.packed/agboss-0.1.0.tgz" --access public --tag latest` first and `agboss-cli-0.1.0.tgz` second, and assert `npm view <pkg>@0.1.0 dist.integrity` matches `.packed/manifest.json`. | `Locally verified` (`npm publish ... --dry-run` on both `.tgz` files) + `GitHub CI verified` |
| **Fix 4** | `.github` + `agboss` | Clarify CI artifact contents (`smoke.yml` vs `packed-release.yml`) vs. local verified tarballs. | Documented in [`RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md) and Section 4.2 below that `Smoke CI` (`smoke.yml`) uploads lightweight JSON/Markdown proof (`agboss-release-proof-<sha>` without `.packed/*.tgz`), `Packed Release Candidate Verification` (`packed-release.yml`) uploads `packed-release-proof-<sha>` including `.packed/*.tgz`, and the owner publishes the local verified `.packed/*.tgz` files produced in Phase 2 Step 2.2. | `Locally verified` + `GitHub CI verified` |
| **Fix 5** | `agboss` | Fail-fast shell execution and guarded temporary directory cleanup in manual release runbook. | Added `set -euo pipefail` and `&&` chaining to every manual command block in [`RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md) and guarded `TMP_PUBLIC_TEST` (`mktemp -d "${TMPDIR:-/tmp}/agboss-public-verify.XXXXXX"` + non-empty/prefix check) before `rm -rf` in Phase 8. | `Locally verified` + `GitHub CI verified` |
| **P0-1..P1-3** | All 4 repos | Post-publication verification scripts (`install:published`, `verify:published`, `collect:published`), provenance separation (`--provenance rc` vs `published`), security patches (`0` runtime vulnerabilities in `packages/agboss`), and CLI/template links. | Preserved and verified across all 4 repositories. | `Locally verified` + `GitHub CI verified` |

---

## 4. Release-Candidate Tarball, Smoke Harness & Proof Report Evidence

### 4.1. Verified Release-Candidate Tarballs (`agboss` `781c2052cb1cc2a564c21c3e3fdc807e2890ebf9`)

- **Framework Commit (`sourceCommit` in `.packed/manifest.json`):** `781c2052cb1cc2a564c21c3e3fdc807e2890ebf9`
- **Examples Commit (`examplesSourceCommit` in `.packed/manifest.json`):** `2b8268307923100b264583b9eedecfaf352a979d`
- **Website Commit (`agbossjs/agboss.dev`):** `656ca373f1413a5520b0f997589dd04d1f5feb61`
- **`agboss-0.1.0.tgz` (`packages.agboss`):**
  - `integrity`: `sha512-Azu01jlAwG9bDzXTR/wsORpSZi36Y+/pSkh4/5D1rBKXgIQwTHjq63Wzoircq5LaTTnQJe7jX9AV3bCApUrqYQ==`
  - `sha256`: `b6ea25f6cd702e8c60a6dbe5978a6672ae85317de80f6f2c3c6cd384dcf9242a`
  - `shasum`: `5a713e1699abcb482fc83f14ca4be244a0364ce6`
  - `entryCount`: `3748` archive entries (`668` packed files across `bin/agboss.js`, `dist/`, `schema/profile.schema.json`, `skills/agboss/SKILL.md` + 13 reference guides `01-composition-and-architecture.md` .. `13-migrating-from-langgraph.md`, `README.md`, `LICENSE`, `NOTICE`)
- **`agboss-cli-0.1.0.tgz` (`packages.agbossCli`):**
  - `integrity`: `sha512-1RvNx6IDojRYlogHymzp7A1mlk5MROGeRNsZYskz2p46b+9sfzIsJ0Wd/cbkO1NW3YeGn/VJLi+BGmgcVzsaXQ==`
  - `sha256`: `3294e7e0bbffb22f11c5678b251a0f91153b09567e0a83de054e43abc4f60a3f`
  - `shasum`: `78ee9085d1a1838a3e7a2c83a049a8671cf856cc`
  - `entryCount`: `577` archive entries (`228` packed files across `bin/agboss.js`, `dist/cli.js`, `templates/`, `README.md`, `LICENSE`, `NOTICE`)

### 4.2. CI Workflow & Artifact Provenance (`agbossjs/agboss-examples`)

| Workflow | File | Uploaded Artifact Name | Contents | Role in Release Process |
| :--- | :--- | :--- | :--- | :--- |
| **Smoke CI (Release Candidate & Examples Gate)** | [`.github/workflows/smoke.yml`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/.github/workflows/smoke.yml) | `agboss-release-proof-<framework_sha>` | `.packed/manifest.json`, `.packed/package-verification.json`, `.packed/published-verification.json`, `.packed/last-run.json`, `proof-results.json`, `proof-report.md` *(lightweight; excludes `.packed/*.tgz`)* | Automatically validates the pinned `agboss` commit across all 7 examples (`01`..`07`), `verify:package`, and `verify:published -- --from-packed` on every PR and push. |
| **Packed Release Candidate Verification** | [`.github/workflows/packed-release.yml`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/.github/workflows/packed-release.yml) | `packed-release-proof-<framework_sha>` | All proof JSON/Markdown files above **plus** `.packed/agboss-0.1.0.tgz` and `.packed/agboss-cli-0.1.0.tgz` | Provides downloadable `.tgz` release-candidate archives from GitHub Actions when needed. |
| **Local Manual Release Pack (Phase 2 Step 2.2 → Phase 4)** | [`RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md) | Local `$WORKSPACE_ROOT/agboss-examples/.packed/*.tgz` | `.packed/agboss-0.1.0.tgz`, `.packed/agboss-cli-0.1.0.tgz`, `.packed/manifest.json` | Owner packs and verifies the final `main` commit (`FINAL_MAIN_FRAMEWORK_SHA`) locally in Phase 2 Step 2.2 and publishes those exact `.packed/*.tgz` files in Phase 4. |

### 4.3. 7-Project Executable Verification Matrix

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
