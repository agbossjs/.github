# AGBoss v0.1.0 — Cross-Repository Pre-Release Readiness & Verification Report

> **Release Embargo Status:** Active (`0` packages published, `0` git tags created, `0` GitHub Releases created, `0` merges to `main`, `0` production deployments triggered).

---

## 1. Repository State & Pull Request Matrix

| Repository | Branch | Pushed Commit SHA | Open PR (Unmerged) | GitHub Actions Verification Run | CI Status |
| :--- | :--- | :--- | :--- | :--- | :---: |
| [`agbossjs/agboss`](https://github.com/agbossjs/agboss) | `chore/pre-release-remediation` | `f22c33254a6eeeb30cf0bbcb72097010eb09f2d5` (`f22c3325`) | [PR #465](https://github.com/agbossjs/agboss/pull/465) | [Run #40222501803](https://github.com/agbossjs/agboss/actions/runs/40222501803) (`CI`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/agboss-examples`](https://github.com/agbossjs/agboss-examples) | `chore/pre-release-remediation` | `68bbacc1fde56186194e028b7c11fe69dc85ab3b` (`68bbacc`) | [PR #21](https://github.com/agbossjs/agboss-examples/pull/21) | [Run #40222908577](https://github.com/agbossjs/agboss-examples/actions/runs/40222908577) (`push`) · [Run #40222928959](https://github.com/agbossjs/agboss-examples/actions/runs/40222928959) (`pull_request`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/agboss.dev`](https://github.com/agbossjs/agboss.dev) | `chore/pre-release-remediation` | `ba7d6365b44507fe56f3d7d834df7f3bfab5c699` (`ba7d636`) | [PR #2](https://github.com/agbossjs/agboss.dev/pull/2) | [Run #40223658518](https://github.com/agbossjs/agboss.dev/actions/runs/40223658518) (`Build & Verify Docs`) | **PASSED** (`GitHub CI verified` + `Locally verified`) |
| [`agbossjs/.github`](https://github.com/agbossjs/.github) | `chore/pre-release-remediation` | *(this commit)* | [PR #2](https://github.com/agbossjs/.github/pull/2) | N/A (static org profile & policy markdown) | **PASSED** (`Locally verified` + `Manually inspected`) |

---

## 2. Round 2 Issue Remediation Summary

| ID | Repository | Finding | Fix Applied | Verification Level |
| :--- | :--- | :--- | :--- | :--- |
| **P0-1** | `agboss-examples` | `collect-results.mjs` allowed skipped P0 demos (`r.demoStatus === "skipped"`) and skipped P1 checks to pass, treated `.packed/package-verification.json` existence as PASS without checking contents/provenance, and `run-all.mjs` silently skipped missing projects. | Rewrote `scripts/collect-results.mjs` and `scripts/run-all.mjs` with explicit `PASSED`, `FAILED`, `SKIPPED`, `NOT_RUN`, and `BLOCKED` statuses, fail-closed P0/P1/Package/Provenance gates, and 14 unit tests in `scripts/run-all.test.mjs` covering all 10 required failure/staleness scenarios. | `Locally verified` + `GitHub CI verified` ([Run #40222908577](https://github.com/agbossjs/agboss-examples/actions/runs/40222908577)) |
| **P0-2** | `agboss` + `agboss-examples` | Cross-repository validation required explicit commit-pair pinning (`framework_sha` + `examples_sha`) with no fallback to `main` and uploaded CI proof artifacts. | Added `.agboss-version.json` pinning `f22c33254a6eeeb30cf0bbcb72097010eb09f2d5`. Updated `smoke.yml`, `packed-release.yml`, and `nightly.yml` to assert `git rev-parse HEAD == framework_sha`, execute all 12 steps across P0 + P1 (`check`, `test`, `demo`), verify clean git status after `--restore`, and upload `.packed/manifest.json`, `.packed/package-verification.json`, `.packed/last-run.json`, `proof-results.json`, and `proof-report.md`. | `Locally verified` + `GitHub CI verified` ([Run #40222908577](https://github.com/agbossjs/agboss-examples/actions/runs/40222908577), [Run #40222928959](https://github.com/agbossjs/agboss-examples/actions/runs/40222928959)) |
| **P0-3** | `agboss` | `RELEASE_CHECKLIST.md` had outdated references (`>=20` vs `>=22.13`, `npm run test:coverage` vs `npm run coverage`, `schemas/` vs `schema/profile.schema.json`, `dist/bin.js` vs `bin/agboss.js` + `dist/cli.js`, step numbering, npm per-version deprecation behavior, and `--live-registry` flag). | Rewrote `RELEASE_CHECKLIST.md` into Phase A (Steps A.1–A.8), Phase B (Steps B.1–B.5, Owner-Controlled Manual Publication), and Phase C (Steps C.1–C.3, Post-Publication Smoke Verification with `--live-registry`). | `Locally verified` + `GitHub CI verified` ([Run #40222501803](https://github.com/agbossjs/agboss/actions/runs/40222501803)) |
| **P1-1** | `agboss.dev` | `.github/workflows/deploy.yml` ran `npm ci && npm run build` without checking out `agbossjs/agboss` and running `npm run check:docs`; `installation.md` frontmatter said `Node.js 24+` instead of `Node.js 22.13+`. | Added `.agboss-version.json` pinning `f22c33254a6eeeb30cf0bbcb72097010eb09f2d5`, updated `.github/workflows/deploy.yml` to checkout and build `agbossjs/agboss`, run `npm run check:docs` (`212 snippets compiled in 65 docs`), and run `npm run build` (`66 pages`). Regenerated `package-lock.json` (`0` vulnerabilities) and updated `installation.md` to `Node.js 22.13+`. | `Locally verified` + `GitHub CI verified` ([Run #40223658518](https://github.com/agbossjs/agboss.dev/actions/runs/40223658518)) |
| **P1-2** | `agboss.dev` | Homepage (`index.mdx`) rendered copy-ready `npx agboss create my-app` and `npm install agboss` cards on the first screen before `0.1.0` is published (`0.0.1` on npm is a deprecated placeholder). | Created `src/components/InstallCards.astro` driven by `RELEASE_META.isPublishedToRegistry` in `src/config/release.ts` so pre-release mode displays the runnable `v0.1.0-rc` source/tarball workflow alongside the clearly labeled post-publish npm CLI target. Verified 7 routes × 2 viewports (`1440×900`, `390×844`) × 2 themes (`dark`, `light`) in headless Chromium via Playwright (`28/28` checks passed, `0` console/page errors, `0` horizontal overflow). | `Locally verified` + `GitHub CI verified` ([Run #40223658518](https://github.com/agbossjs/agboss.dev/actions/runs/40223658518)) |
| **P1-3** | All 4 repos | Verification evidence needed to be committed and attached to PRs/CI artifacts for external review. | Committed this `PRE_RELEASE_READINESS_REPORT.md` to `agbossjs/.github`, uploaded CI proof artifacts in `agboss-examples` and `agboss.dev`, and attached verification summaries to all 4 PRs. | `Locally verified` + `GitHub CI verified` |
| **P1-4** | All 4 repos | Document security and dependency audit findings clearly. | Documented `npm audit` across all repositories (`0` vulnerabilities in `agboss.dev`; transitive dev/CLI advisories isolated and documented in Section 4 below). | `Locally verified` + `Manually inspected` |
| **P1-5** | All 4 repos | Ensure public verification claims match evidence and preserve `07-langgraph-migration` as a deterministic reference migration showcase. | Aligned `agboss/package.json`, `agboss/packages/agboss/package.json`, `agboss-examples/README.md`, `agboss.dev`, and `.github/profile/README.md`. | `Locally verified` + `GitHub CI verified` |

---

## 3. Release-Candidate Tarball & Proof Report Evidence

### 3.1. GitHub Actions CI Run (`agboss-examples` Run [#40222908577](https://github.com/agbossjs/agboss-examples/actions/runs/40222908577), Node `24.21.0`, Ubuntu Latest)

- **Framework Commit (`agbossjs/agboss`):** `f22c33254a6eeeb30cf0bbcb72097010eb09f2d5`
- **Examples Commit (`agbossjs/agboss-examples`):** `68bbacc1fde56186194e028b7c11fe69dc85ab3b`
- **Run ID:** `0b3973f1-a249-47ff-9ddb-2018bc298fa4`
- **`agboss-0.1.0.tgz`:**
  - `integrity`: `sha512-mqvNSX1HL7L+4mPxF1m9cPBE7bVKtj6vOWStM/hCUMJGpcz4JvBmjXTY1c+MDILQBkpGedAJ7Hx01yLoXvZeyw==`
  - `sha256`: `62cbae7a82a294fe9a7238a58799057bc8529e51493a83590829598f23f6071d`
  - `entryCount`: `668` files (`bin/agboss.js`, `dist/`, `schema/profile.schema.json`, `skills/agboss/SKILL.md` + 13 reference guides, `README.md`, `LICENSE`, `NOTICE`)
- **`agboss-cli-0.1.0.tgz`:**
  - `integrity`: `sha512-5yhBAIupR/JT6kV9aKYodJ9aBQctx+I18H1+zACmV8rIjiEb+nvV6lpVHU0RJ1T1XHG/QXkZqyLOrjvYSyK37g==`
  - `sha256`: `752084f9e6e1778659b00f0e18300708842a64dd24e988db22b479fbbf663517`
  - `entryCount`: `228` files (`bin/agboss.js`, `dist/cli.js`, `templates/`, `README.md`, `LICENSE`, `NOTICE`)
- **Gates:**
  - **Provenance Gate:** `PASSED`
  - **Isolated Package & Scaffold Gate:** `PASSED` (11 subpath exports, CLI `bin`, clean `agboss create`, 13 synced AI skills, `check`, `tsc --noEmit`, `npm test`, `describe`)
  - **P0 Release Gate:** `PASSED`
  - **P1 Extended Gate:** `PASSED`
  - **Overall Release Readiness Gate:** `PASSED`

| Project | Priority | Mode | `npm run check` | `npm test` | `npm run demo` | Project Gate | CI Duration (ms) | Local Duration (ms) |
| :--- | :---: | :--- | :---: | :---: | :---: | :---: | ---: | ---: |
| `01-hello-agboss` | **P0** | `offline-deterministic` | `PASSED` | `PASSED` (6 tests) | `PASSED` | **PASSED** | 25,997 | 18,393 |
| `02-path-scope-proof` | **P0** | `offline-deterministic` | `PASSED` | `PASSED` (6 tests) | `PASSED` | **PASSED** | 15,266 | 10,435 |
| `03-support-refund-desk` | **P0** | `offline-deterministic` | `PASSED` | `PASSED` (6 tests) | `PASSED` | **PASSED** | 22,004 | 13,005 |
| `04-travel-booking-saga` | **P0** | `fault-injection` | `PASSED` | `PASSED` (12 tests) | `PASSED` | **PASSED** | 9,212 | 6,542 |
| `05-cluster-work-stealing` | **P1** | `fault-injection` | `PASSED` | `PASSED` (8 tests) | `PASSED` | **PASSED** | 8,859 | 6,127 |
| `06-enterprise-agent-mesh` | **P1** | `multi-service` | `PASSED` | `PASSED` (8 tests) | `PASSED` | **PASSED** | 16,076 | 11,327 |
| `07-langgraph-migration` | **P0** | `deterministic-reference` | `PASSED` | `PASSED` (6 tests) | `PASSED` | **PASSED** | 7,433 | 5,669 |

---

## 4. Security & Dependency Audit Summary (P1-4)

- **GitHub Actions Permissions:**
  - `agboss/.github/workflows/ci.yml`: Top-level `permissions: { contents: read }`; `contents: write` is isolated exclusively to the `publish-badge` job when `github.ref == 'refs/heads/main' && github.event_name == 'push'`.
  - `agboss-examples/.github/workflows/{smoke,packed-release,nightly}.yml`: Top-level `permissions: { contents: read }`.
  - `agboss.dev/.github/workflows/deploy.yml`: Top-level `permissions: { contents: read }`; `id-token: write` is isolated to the `deploy` job on `refs/heads/main` (`push` only).
- **Secrets & PII Hygiene:** Zero `.env` files, zero live API keys, zero private keys, and zero real PII exist in git history or working trees across all four repositories.
- **Vulnerability Disclosure Policy (`SECURITY.md`):** Present in `agbossjs/agboss`, `agbossjs/agboss-examples`, `agbossjs/agboss.dev`, and `agbossjs/.github` (`security@agboss.dev` + GitHub Security Advisories).
- **`npm audit` Findings:**
  - **`agbossjs/agboss.dev`:** **`0` vulnerabilities** across all 311 packages (`npm ci` & `npm audit` clean).
  - **`agbossjs/agboss` & `agbossjs/agboss-examples`:**
    - `packages/agboss-cli` (`devDependencies` companion) depends on `@langchain/langgraph-cli@^1.0.0` (for `agboss studio`), which pulls transitive upstream `create-langgraph` / `extract-zip` / `hono` / `body-parser` advisories from `@langchain/langgraph-cli`. Because `agboss-cli` is installed in `devDependencies`, these transitive Studio bridge packages stay out of production container images that install `agboss` (`--omit=dev`).
    - Dev-only test runner dependencies (`vitest`, `@vitest/coverage-v8`, `vite`, `tinypool`) in workspace `devDependencies` do not ship in `agboss-0.1.0.tgz` or `agboss-cli-0.1.0.tgz`.
    - In `packages/agboss`, `@modelcontextprotocol/sdk` (`^1.30.1`), `zod` (`^4.6.5`), and transitive `undici` use caret ranges (`^`) so consumers installing `agboss@0.1.0` receive the latest patched semver-compatible releases from npm.

---

## 5. Remaining Owner-Controlled Manual Release Steps

See [`agbossjs/agboss/RELEASE_CHECKLIST.md`](https://github.com/agbossjs/agboss/blob/chore/pre-release-remediation/RELEASE_CHECKLIST.md) (Phase B & Phase C) for the exact step-by-step commands once the owner approves publication:

1. Merge PRs (`agbossjs/agboss#465`, `agbossjs/agboss-examples#21`, `agbossjs/agboss.dev#2`, `agbossjs/.github#2`) into `main`.
2. Tag `v0.1.0` on `agbossjs/agboss` `main` and publish `agboss@0.1.0` and `agboss-cli@0.1.0` to npm (`npm publish --workspace=agboss --access public && npm publish --workspace=agboss-cli --access public`).
3. Run post-publication live registry verification (`npm run verify:published -- --live-registry --version 0.1.0` in `agboss-examples`).
4. Flip `isPublishedToRegistry: true` and `displayVersion: 'v0.1.0'` in `agboss.dev/src/config/release.ts` and deploy `agboss.dev`.
