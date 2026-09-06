# Unique Capability Reconciliation — `interplanetaryfund1`

## Purpose

This comparison exists to make repository retirement safe. The goal is not to make the archival repository match the live system. The goal is to identify anything this repository has that is genuinely better or uniquely useful, move that improvement into the current live application/backend, verify it there, and only then allow this repository to become historical read-only evidence.

## Current source of truth

- User-facing live application: `interplanetarysister/interplanetary-fund2`
- Authoritative Convex/backend and agent runtime: `interplanetarysister/InterplanetaryFund`
- This repository: historical snapshot / archival candidate

## Difference-handling rule

For every difference found:

1. Identify the capability and the exact source file/commit that introduced it.
2. Determine whether the current live repositories already contain an equivalent or better implementation.
3. Classify the difference:
   - **MIGRATE** — genuine improvement or production capability missing from the live application.
   - **ALREADY SUPERSEDED** — live implementation exists and is equivalent or stronger.
   - **HISTORICAL ONLY** — obsolete/decommissioned/reference material.
   - **DO NOT MIGRATE** — weaker, insecure, fake/simulated, duplicate, test-only, or conflicting behavior.
4. For **MIGRATE**, implement the capability in the appropriate current live repository rather than copying the historical repository wholesale.
5. Preserve current authentication, authorization, security, financial-integrity, provider, Node 24, and deployment boundaries.
6. Validate the migrated implementation on its exact head and send it through the normal Agent 2/3 review path before publication.
7. Record the destination commit/PR here. Only then mark that difference reconciled.

## Commit-level comparison

The archival repository currently has four top-level commits in its independent history:

### `210e7f64` — Initial Vercel snapshot
Classification: **ALREADY SUPERSEDED / HISTORICAL BASELINE**

This is the historical application snapshot from which later repository-specific changes were made. It is not authoritative over the current live repositories. Baseline features are compared by capability, and current implementations win when they are equivalent or stronger. Historical security/auth/payment behavior must never be restored merely because it exists here.

### `a94b7053` — Install Vercel Web Analytics
Classification: **MIGRATE**

Unique improvement found: Vercel Web Analytics was enabled in this archival repository, while the current live application did not contain that capability.

Action taken: migrated the capability into `interplanetarysister/interplanetary-fund2` on branch `solstice/migrate-archive-capabilities`, commit `b51db41511989de747e86fd6830666276bfd88a8`.

The live implementation uses Vercel's first-party Web Analytics bootstrap and `/_vercel/insights/script.js` in `index.html`, avoiding reintroducing this archival repository's package/lockfile state.

Status: **MIGRATED TO LIVE REVIEW BRANCH — pending review/merge**

### `b7b0a269` — Merge Vercel Web Analytics PR
Classification: **SAME CAPABILITY AS ABOVE**

No separate migration required.

### `4ac1c82a` — Align FundForge tooling with Node.js 24
Classification: **HISTORICAL ONLY / ALREADY SUPERSEDED**

The change applies to the embedded FundForge historical tooling. FundForge is decommissioned and must not be revived as a production source. The current live repositories already target Node.js 24, so there is no missing production capability to copy from this commit.

## Retirement decision logic

Archiving is safe only after every newly discovered unique difference is either:

- migrated and validated in the current live system,
- confirmed already present in equal or better form,
- explicitly retained only as historical provenance, or
- rejected from migration because it would regress security, correctness, architecture, or current product decisions.

Any future difference that qualifies as an improvement reopens reconciliation until the improvement is moved to the live application and reviewed.
