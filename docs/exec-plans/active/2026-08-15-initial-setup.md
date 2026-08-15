# Plan: Initial Project Setup

> Area: Global — project foundation
>
> Risk: low
>
> Created: 2026-08-15

## Intent

Complete the repository's agent-first scaffolding with the concept references and planning lifecycle it lacked, without duplicating homes the existing documentation standard already owns.

## Acceptance criteria

- [x] Concept references exist as bilingual pairs: docs/PRODUCT_SENSE.md, docs/SECURITY.md, docs/RELIABILITY.md, docs/QUALITY_SCORE.md, docs/PLANS.md, each with a Mermaid diagram for its core concept.
- [x] Execution-plan lifecycle exists: docs/exec-plans/_template.md plus active/ and completed/.
- [x] `.env.example` documents the environment variables real-API tests and demos read.
- [x] `.opencode/` local config points agents at the root map.
- [ ] `pnpm run doc-sync` passes with the new documents.
- [ ] First feature plan authored from the template.

## Non-goals

- Does not implement product features.
- Does not add CI, build tooling, or a README: the repository already owns them.
- Does not modify runtime code or package manifests.

## Task order

| # | Task | Depends on |
|---|---|---|
| 1 | Concept references (EN+ZH, Mermaid) | — |
| 2 | Execution-plan lifecycle and this plan | 1 |
| 3 | `.env.example` and `.opencode/` | — |
| 4 | Pairing records, doc-sync green, commit | 1, 2, 3 |

## Progress log

| Date | Update |
|---|---|
| 2026-08-15 | Scaffold authored: five concept pairs, exec-plan lifecycle, root config files; doc-sync pending. |
