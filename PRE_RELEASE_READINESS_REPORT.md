# AGBoss v0.1.0 — Cross-Repository Pre-Release Readiness & Verification Report

> **Release Embargo Status:** Active (`0` packages published, `0` git tags created, `0` GitHub Releases created, `0` merges to `main`, `0` production deployments triggered).

---

## 1. Repository State & Pull Request Matrix

| Repository | Branch | Verified Commit SHA | Open PR (Unmerged) | GitHub Actions Verification Run | CI Status |
| :--- | :--- | :--- | :--- | :--- | :---: |
| [`agbossjs/agboss`](https://github.com/agbossjs/agboss) | `chore/pre-release-remediation` | `55d00ff4646713ebbe61085813e5f37b0c32aa3f` (`55d00ff4`) | [PR #465](https://github.com/agbossjs/agboss/pull/465) | [Run #40228504496](https://github.com/agbossjs/agboss/actions/runs/40228504496) (`CI`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/agboss-examples`](https://github.com/agbossjs/agboss-examples) | `chore/pre-release-remediation` | `3ff9e9047d44bbeb8dc247af9265def229ec5c10` (`3ff9e90`) | [PR #21](https://github.com/agbossjs/agboss-examples/pull/21) | [Run #40228523345](https://github.com/agbossjs/agboss-examples/actions/runs/40228523345) (`push`) · [Run #40228528328](https://github.com/agbossjs/agboss-examples/actions/runs/40228528328) (`pull_request`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/agboss.dev`](https://github.com/agbossjs/agboss.dev) | `chore/pre-release-remediation` | `384530420888b6d7a869f74626d0a4bea22f957c` (`3845304`) | [PR #2](https://github.com/agbossjs/agboss.dev/pull/2) | [Run #40228547028](https://github.com/agbossjs/agboss.dev/actions/runs/40228547028) (`push`) · [Run #40228549970](https://github.com/agbossjs/agboss.dev/actions/runs/40228549970) (`pull_request`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/.github`](https://github.com/agbossjs/.github) | `chore/pre-release-remediation` | *(this commit)* | [PR #2](https://github.com/agbossjs/.github/pull/2) | N/A (static org profile & policy markdown) | **PASSED** (`Locally verified` + `Manually inspected`) |

---

## 2. Final Release Engineering Corrections Summary (Round 3 Audit)

| ID | Repository | Finding | Fix Applied | Verification Level |
| :--- | :--- | :--- | :--- | :--- |
| **P0-1 (A)** | `agboss` + `agboss-examples` | `RELEASE_CHECKLIST.md` Phase C.2 referenced a nonexistent `npm run install:all` script in `agboss-examples`. | Implemented [`scripts/install-published.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/install-published.mjs) (`npm run install:published`) and `npm run collect:published` in `agboss-examples/package.json`. Updated `RELEASE_CHECKLIST.md` Phase C.2 to install exact published versions from npm across all 7 examples (`01`..`07`), verify zero local tarballs/symlinks/workspace links, run `check`/`test`/`demo` under `--provenance published`, and generate `published-proof-results.json` & `published-proof-report.md`. | `Locally verified` + `GitHub CI verified` |
| **P0-1 (B)** | `agboss` + `agboss.dev` | `RELEASE_CHECKLIST.md` referenced `isPreRelease: false`, whereas `agboss.dev/src/config/release.ts` uses `isPublishedToRegistry`; `nav.ts` and `index.mdx` also contained hardcoded `v0.1.0-rc` strings that could mix prerelease and published labels. | Updated [`src/config/release.ts`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/src/config/release.ts), [`src/components/nav.ts`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/src/components/nav.ts), and [`src/components/Head.astro`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/src/components/Head.astro) so all version badges, banners, hero eyebrows, nav links, JSON-LD `softwareVersion`, and install cards derive from `RELEASE_META`. Added [`scripts/verify-release-meta.mjs`](https://github.com/agbossjs/agboss.dev/blob/chore/pre-release-remediation/scripts/verify-release-meta.mjs) (`npm run verify:release-meta`) to `deploy.yml` and aligned `RELEASE_CHECKLIST.md` Phase C.3. | `Locally verified` + `GitHub CI verified` ([Run #40228547028](https://github.com/agbossjs/agboss.dev/actions/runs/40228547028)) |
| **P0-1 (C)** | `agboss` | Review all Phase B and Phase C commands in `RELEASE_CHECKLIST.md` for accuracy and dependency ordering. | Verified every command, path, and script across Phase A (A.1–A.9), Phase B (B.1–B.7, enforcing `agboss` publication before `agboss-cli` which depends on `"agboss": "0.1.0"`), and Phase C (C.1–C.4, switching `agboss.dev` to published mode only after live npm verification passes). | `Locally verified` + `GitHub CI verified` |
| **P0-2** | `agboss-examples` | `scripts/verify-published.mjs` previously defaulted to `usePackedFallback` when `--live-registry` was omitted and did not query `npm view` or verify all 11 subpath exports. | Rewrote [`scripts/verify-published.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/verify-published.mjs) to require an explicit mode flag (`--from-packed` or `--live-registry`) and `--version <semver>`, reject missing/conflicting args with exit code `1`, print `SOURCE: LOCAL_PACKED_RELEASE_CANDIDATE` vs `SOURCE: PUBLIC_NPM_REGISTRY`, query `npm view` & `dist-tags` in `--live-registry` mode, verify all 11 subpath exports (`agboss`, `agboss/dto`, `agboss/units`, `agboss/models`, `agboss/testing`, `agboss/testing/setup`, `agboss/eval`, `agboss/mcp`, `agboss/rag`, `agboss/a2a`, `agboss/eslint`), CLI binary resolution (`agboss --help`, `agboss help --json`, `agboss-cli --help`), `agboss create`, 13 AI skill reference files, `check`, `tsc --noEmit`, `npm test`, and `describe --json`, and write `.packed/published-verification.json`. | `Locally verified` + `GitHub CI verified` ([Run #40228523345](https://github.com/agbossjs/agboss-examples/actions/runs/40228523345)) |
| **P0-3** | `agboss-examples` | Separate published-package evidence from RC evidence so post-publication runs cannot inherit PASS statuses from earlier RC runs. | Updated [`scripts/run-all.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/run-all.mjs) and [`scripts/collect-results.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/collect-results.mjs) with explicit `--provenance rc` vs `--provenance published` modes (`.packed/published-manifest.json` + `.packed/published-verification.json` + `.packed/last-run-published.json` → `published-proof-results.json` & `published-proof-report.md`). Expanded [`scripts/run-all.test.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/run-all.test.mjs) to 24 unit tests covering all negative scenarios. | `Locally verified` (`24/24` unit tests) + `GitHub CI verified` |
| **P1-1** | `agboss` + `agboss-examples` + `agboss.dev` | Make final merge SHA handling explicit when PRs are squash- or rebase-merged into `main`. | Added [`scripts/pin-framework-sha.mjs`](https://github.com/agbossjs/agboss-examples/blob/chore/pre-release-remediation/scripts/pin-framework-sha.mjs) (`npm run pin:framework-sha`) to `agboss-examples` and `agboss.dev`, and documented the 10-step merge and SHA-pinning sequence in `RELEASE_CHECKLIST.md` Phase B & Section 5 below. | `Locally verified` + `GitHub CI verified` |
| **P1-2** | `agboss` + `agboss-examples` + `.github` | Complete the security advisory assessment with exact GHSA IDs, dependency paths, reachability, and `v0.1.0` disposition. | Bumped `@modelcontextprotocol/sdk` to `^1.32.2` and `zod` to `^4.6.7` in `packages/agboss` and `packages/agboss-cli` and updated lockfile security patches (`undici`, `hono`, `fast-uri`, `body-parser`, `ip-address`, `source-map-js`, `vite`), eliminating 9 of 16 advisories and achieving **`0` production runtime vulnerabilities** in `packages/agboss`. Documented the complete advisory table in Section 4 below. | `Locally verified` + `GitHub CI verified` |
| **P1-3** | All 4 repos | Verify documentation against actual scripts and flags across all repositories. | Audited `RELEASE_CHECKLIST.md`, `PRE_RELEASE_READINESS_REPORT.md`, `agboss-examples/README.md`, `installation.md`, `quick-start.md`, and `agboss.dev/src/config/release.ts` against `package.json` scripts across all repositories. | `Locally verified` + `GitHub CI verified` |

---

## 3. Release-Candidate Tarball, Smoke Harness & Proof Report Evidence

### 3.1. Verified Release-Candidate Tarballs (`agboss` `55d00ff4646713ebbe61085813e5f37b0c32aa3f`)

- **Framework Commit (`agbossjs/agboss`):** `55d00ff4646713ebbe61085813e5f37b0c32aa3f`
- **Examples Commit (`agbossjs/agboss-examples`):** `3ff9e9047d44bbeb8dc247af9265def229ec5c10`
- **Website Commit (`agbossjs/agboss.dev`):** `384530420888b6d7a869f74626d0a4bea22f957c`
- **`agboss-0.1.0.tgz`:**
  - `integrity`: `sha512-Azu01jlAwG9bDzXTR/wsORpSZi36Y+/pSkh4/5D1rBKXgIQwTHjq63Wzoircq5LaTTnQJe7jX9AV3bCApUrqYQ==`
  - `sha256`: `b6ea25f6cd702e8c60a6dbe5978a6672ae85317de80f6f2c3c6cd384dcf9242a`
  - `entryCount`: `668` files (`bin/agboss.js`, `dist/`, `schema/profile.schema.json`, `skills/agboss/SKILL.md` + 13 reference guides `01-composition-and-architecture.md` .. `13-migrating-from-langgraph.md`, `README.md`, `LICENSE`, `NOTICE`)
- **`agboss-cli-0.1.0.tgz`:**
  - `integrity`: `sha512-C8yx0JgoVHznn/ARJGIe5QAu29fx64o0UHaH65ZyKMizibIhSjKMLkLDEvs0qRygcNScq4Hw+uABY3FLEUbvAg==`
  - `sha256`: `27aadb30e9a8b1ba7350ac4d77134cc2c81f6fceb12743b0f4dc04a8a4eb5627`
  - `entryCount`: `228` files (`bin/agboss.js`, `dist/cli.js`, `templates/`, `README.md`, `LICENSE`, `NOTICE`)
- **`verify-published.mjs` Verification Modes:**
  - `--from-packed --version 0.1.0`: `PASSED` (`SOURCE: LOCAL_PACKED_RELEASE_CANDIDATE`, `registryVerified: false`, 11 subpath exports, CLI `help --json` & `--help`, `agboss create published-smoke-app`, 13 synced AI skills, `.agboss/types.d.ts`, `agboss check`, `tsc --noEmit`, `npm test`, `agboss describe --json`).
  - `--live-registry --version 0.1.0`: Verified fail-closed pre-publication behavior (`SOURCE: PUBLIC_NPM_REGISTRY`, exits with code `1` and records `status: "FAILED"`, `registryMetadata: "failed"` because `agboss@0.1.0` is not yet published on `registry.npmjs.org`, with zero fallback to `.packed/*.tgz`).
- **Release Readiness Gates (`collect-results.mjs --provenance rc --require-p1`):**
  - **Provenance Gate:** `PASSED`
  - **Isolated Package & Scaffold Gate:** `PASSED`
  - **P0 Release Gate:** `PASSED`
  - **P1 Extended Gate:** `PASSED`
  - **Overall Release Readiness Gate:** `PASSED`

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

## 4. Complete Security & Dependency Advisory Assessment (P1-2)

### 4.1. Summary by Repository & Package Surface

| Target / Scope | Critical | High | Moderate | Low | Total | Status for `v0.1.0` |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **`packages/agboss` (Production Runtime Tarball `agboss-0.1.0.tgz`)** | `0` | `0` | `0` | `0` | **`0`** | **CLEAN** (`0` runtime vulnerabilities) |
| **`agbossjs/agboss.dev` (Documentation Website)** | `0` | `0` | `0` | `0` | **`0`** | **CLEAN** (`0` vulnerabilities across 311 packages) |
| **`agbossjs/agboss` Monorepo Workspace (`npm audit`)** | `2` | `3` | `2` | `0` | **`7`** | **MITIGATED / NON-RUNTIME** (`3` CLI-only in `create-langgraph`, `4` dev-only in `vitest`) |
| **`agbossjs/agboss-examples` Root Tooling (`npm audit`)** | `2` | `0` | `1` | `0` | **`3`** | **DEV-ONLY** (`vitest@3.2.7` test runner only) |

### 4.2. Advisories Remediated in `agboss` (`55d00ff4`)

Upgrading `@modelcontextprotocol/sdk` to `^1.32.2` and `zod` to `^4.6.7` in `packages/agboss/package.json` and `packages/agboss-cli/package.json` and updating lockfile patches eliminated **9 advisories** across the workspace and all production dependencies:

| Remediated Package | Previous Version | Updated Version | Advisories Resolved |
| :--- | :--- | :--- | :--- |
| `@modelcontextprotocol/sdk` | `1.30.1` | `1.32.2` | `GHSA-ap5f-22qg-4pff` (Moderate) |
| `zod` | `4.6.5` | `4.6.7` | `GHSA-cf7v-w76x-p784` (Moderate) |
| `undici` | `7.22.0` / `6.24.0` | `7.25.0` / `6.25.0` | `GHSA-vrm6-8vpv-qv8q`, `GHSA-f269-vfmq-vjvj`, `GHSA-2mjp-6q6p-2qxm`, `GHSA-v9p9-hfj2-hcw8`, `GHSA-phc3-fgpg-7m6h`, `GHSA-4992-7rv2-5pvq` (High/Moderate) |
| `hono` | `4.13.9` | `4.13.14` | `GHSA-xf4j-xp2r-rqqx` (Moderate) |
| `fast-uri` | `3.1.8` | `3.1.9` | `GHSA-j6v8-v6hv-66x2` (High) |
| `body-parser` | `2.2.2` | `2.2.3` | `GHSA-366p-4wfc-8c3v` (Moderate) |
| `ip-address` | `10.1.0` | `10.2.0` | `GHSA-796r-2h7f-2p2m` (High) |
| `source-map-js` | `1.2.1` | `1.2.2` | `GHSA-2g6f-pc52-8964` (High) |
| `vite` | `6.4.1` | `6.4.3` | `GHSA-4w7w-66w2-5vf9`, `GHSA-v2wj-q39q-566r`, `GHSA-p9ff-h696-f583` (High/Moderate) |

### 4.3. Remaining Workspace Advisories — Detailed Reachability & Disposition Table

| Advisory ID | Package | Installed Version | Patched Version | Severity | Dependency Path | Exposure Scope | Reachability in AGBoss | Mitigation & Disposition for `v0.1.0` |
| :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `GHSA-jmr9-qjv8-65gv`<br/>`GHSA-7pqw-9j4j-h8q3` | `extract-zip` (via `create-langgraph` & `@langchain/langgraph-cli`) | `extract-zip@2.0.1`<br/>`create-langgraph@1.1.5`<br/>`@langchain/langgraph-cli@1.5.1` | None (`No fix available` upstream) | **High** (3 lockfile nodes) | `agboss-cli` → `@langchain/langgraph-cli@1.5.1` → `create-langgraph@1.1.5` → `extract-zip@2.0.1` | **CLI-only (`devDependencies`)** — never present in `agboss` production runtime (`dependencies`). | **Not reachable.** `packages/agboss-cli/src/cli/studio.ts` only invokes `langgraph dev` to start the local LangGraph Studio bridge; AGBoss never calls `create-langgraph` or `extract-zip` to unpack untrusted `.zip` archives (`agboss create` uses built-in local templates in `packages/agboss-cli/templates/`). | **Accepted for `v0.1.0` (Upstream CLI-only, Unreachable).** Isolated to `agboss-cli` (`devDependencies`). Tracked for removal/upgrade once `@langchain/langgraph-cli` drops `create-langgraph` or updates `extract-zip`. |
| `GHSA-82fw-gwwq-j7x9`<br/>`GHSA-qxg6-89mg-rqv8` | `@vitest/mocker` (via `vitest` & `@vitest/coverage-v8`) | `@vitest/mocker@3.2.7`<br/>`vitest@3.2.7`<br/>`@vitest/coverage-v8@3.2.7` | `vitest >= 5.0.5` (`isSemVerMajor: true`) | **Moderate** (2 lockfile nodes) | Root `devDependencies` → `vitest@3.2.7` / `@vitest/coverage-v8@3.2.7` → `@vitest/mocker@3.2.7` | **Dev-only (Workspace test runner)** — not shipped in `agboss-0.1.0.tgz` or `agboss-cli-0.1.0.tgz`. | **Not reachable in production or CLI.** `@vitest/mocker` runs only during local/CI test execution on trusted repository test files with browser mode disabled. | **Accepted for `v0.1.0` (Dev-only test runner).** Upgrading `vitest` from `3.x` to `5.x` is a semver-major test-runner migration scheduled post-`v0.1.0`; zero impact on published `agboss` or `agboss-cli` packages. |
| `GHSA-5gmw-xhrv-c9v3`<br/>`GHSA-85c8-ppgw-ccpr` | `tinypool` (via `vitest` & `@vitest/coverage-v8`) | `tinypool@1.1.1`<br/>`vitest@3.2.7` | `vitest >= 5.0.5` (`isSemVerMajor: true`) | **Critical** (2 lockfile nodes) | Root `devDependencies` → `vitest@3.2.7` → `tinypool@1.1.1` | **Dev-only (Workspace test runner)** — not shipped in `agboss-0.1.0.tgz` or `agboss-cli-0.1.0.tgz`. | **Not reachable in production or CLI.** `tinypool` worker options are controlled exclusively by Vitest's internal test runner process during local/CI test runs and never accept external or user-supplied input. | **Accepted for `v0.1.0` (Dev-only test runner).** Zero runtime or CLI exposure; will be resolved alongside the `vitest@5` devDependency upgrade post-`v0.1.0`. |

---

## 5. Exact 10-Step Owner-Approved Final Merge, SHA Pinning & Publication Sequence (P1-1)

See [`agbossjs/agboss/RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md) (Phase B & Phase C) for full command blocks:

1. **Merge `agbossjs/agboss` PR (#465) to `main` first** and record `FINAL_MAIN_FRAMEWORK_SHA="$(git -C ../agboss rev-parse HEAD)"`.
2. **Pin `FINAL_MAIN_FRAMEWORK_SHA` in downstream PRs:** Run `node scripts/pin-framework-sha.mjs --sha "$FINAL_MAIN_FRAMEWORK_SHA" --branch main` in `agbossjs/agboss-examples` and `agbossjs/agboss.dev`, commit, and push to their PR branches.
3. **Verify green GitHub Actions CI on `FINAL_MAIN_FRAMEWORK_SHA`** in `agbossjs/agboss-examples` (`Smoke CI`) and `agbossjs/agboss.dev` (`Build & Verify Docs`).
4. **Merge downstream PRs into `main`:** Merge `agbossjs/agboss-examples` PR (#21), `agbossjs/agboss.dev` PR (#2), and `agbossjs/.github` PR (#2) into `main`.
5. **Rebuild & verify final release tarballs from `main`:** Checkout `main` in `agboss` and `agboss-examples`, run `VERIFY_PINNED_FRAMEWORK_SHA=1 AGBOSS_REPO_DIR=../agboss npm run pack:framework`, `npm run verify:package`, `npm run verify:published -- --from-packed --version 0.1.0`, P0+P1 `run-all.mjs --provenance rc`, and `collect-results.mjs --provenance rc --require-p1`. Record final tarball `sha256` and `integrity` checksums.
6. **Tag `v0.1.0` on `agbossjs/agboss` `main`:** `git tag -a v0.1.0 -m "Release v0.1.0" && git push origin v0.1.0`.
7. **Publish to npm in dependency order (`agboss` first, then `agboss-cli`):**
   - `npm publish --workspace=agboss --access public`
   - Verify `npm view agboss@0.1.0 version` resolves to `0.1.0`
   - `npm publish --workspace=agboss-cli --access public`
   - Verify `dist-tags` (`latest: 0.1.0`) and that `deprecated` is empty on `0.1.0`.
8. **Run live registry consumer & scaffold verification:** `npm run verify:published -- --live-registry --version 0.1.0` (confirms `SOURCE: PUBLIC_NPM_REGISTRY` and `.packed/published-verification.json` `status: "PASSED"`).
9. **Run live registry 7-example verification & collect published proof report:**
   - `npm run install:published -- --live-registry --version 0.1.0`
   - `node scripts/run-all.mjs --provenance published --include-p1 --reset --mode check`
   - `node scripts/run-all.mjs --provenance published --include-p1 --mode test`
   - `AGBOSS_OFFLINE_VERIFY=1 node scripts/run-all.mjs --provenance published --include-p1 --mode demo`
   - `npm run collect:published` (writes `published-proof-results.json` & `published-proof-report.md`).
10. **Switch `agboss.dev` to published mode & create GitHub Release:** Set `isPublishedToRegistry: true`, `displayVersion: 'v0.1.0'`, `releaseChannelLabel: 'latest'`, `softwareVersion: '0.1.0'` in `agboss.dev/src/config/release.ts`, run `npm run verify:release-meta && npm run check:docs && npm run build`, push to `main`, and create the `v0.1.0` GitHub Release on `agbossjs/agboss` attaching `published-proof-report.md`.
