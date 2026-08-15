# Agent Note: Harness-engineering scaffolding layer over the existing docs standard

Status: implemented

English | [中文](2026-08-15-harness-engineering-layer.zh.md)

## Problem

The repository lacked the scaffolding init-repo assumes every session starts from: no execution-plan lifecycle (no `docs/exec-plans/`, no plans index), no concept references for product direction, security posture, reliability, or quality tracking, no `.env.example`, and no `.opencode/` local config. Meanwhile it already owns strong homes for most of that content's neighbors: a tiered documentation standard with word budgets, bilingual pairing, mechanical gates, CI, and a root AGENTS.md map. Adding the missing pieces naively would create second homes for facts those tiers already own — uppercase `ARCHITECTURE.md` beside `architecture.md`, hand-restated catalogs, English-only docs in a universally paired corpus.

## Decision

The harness-engineering layer lands as concept references at the standard's leaf tier, not as parallel standing docs: `docs/PRODUCT_SENSE.md`, `docs/SECURITY.md`, `docs/RELIABILITY.md`, `docs/QUALITY_SCORE.md`, and `docs/PLANS.md` are ordinary bilingual pairs that link every fact they touch to its owning tier and keep only direction, invariants, and tracking tables at home. The quality score derives grades from named gates rather than sentiment; the reliability page states plainly that no service SLOs exist for a developer preview.

Execution plans are durable working artifacts under `docs/exec-plans/active/` and `docs/exec-plans/completed/` with a `_template.md`; plans track work while Agent Notes keep decisions, and PLANS.md indexes the active set. The whole `docs/exec-plans/` subtree is excluded from translation pairing as churn-first planning residue: each plan is logged, superseded, and moved within days, so a reviewed Chinese translation would be stale at merge. `docs/PLANS.md` itself — the durable index and lifecycle definition — remains a pair. The exclusion is recorded in `scripts/translation-pairing.manifest.json` and named in `docs/i18n/README.md`'s Excluded list, keeping the manifest's explicit-exclusions-only contract intact.

Every diagram in the layer is Mermaid inside the owning doc — the steering loop, trust zones, rollback sequence, grading loop, and plan lifecycle — parsed by the `verify-mermaid` gate and byte-identical across each bilingual pair, which the pairing structure signature already enforces. `.env.example` documents the two variables real-API tests read (`DEEPSEEK_API_KEY`, optional `DEEPSEEK_BASE_URL`) and points to docs/testing.md for key policy. `.opencode/AGENTS.md` and `.opencode/config.json` defer entirely to the root map and declare no overrides of their own.

## Alternatives considered

**Adopt init-repo's standing-doc names wholesale (`ARCHITECTURE.md`, `DESIGN.md` at docs root).** The repo's tier taxonomy already owns those jobs (`architecture.md`, `development.md`, subsystems, cookbooks), the case-insensitive filesystem would collide with the existing lowercase files, and the doc-budgets manifest would gain unbudgeted duplicates. Fitting the layer into the existing tiers keeps one home per fact.

**Make execution plans bilingual pairs too.** Plans churn daily and are working state, not published reference; the pairing contract exists to keep reviewed content consistent across languages. Translating them would multiply work with no reader, so the subtree is excluded and the durable index stays paired.

**Put the concept references under a new `docs/concepts/` subdirectory.** The five documents are peers of the existing top-level references (`testing.md`, `defensive-patterns.md`), and a subdirectory would separate them from the navigation surface they belong to.

**Skip `.opencode/` entirely since the repo has AGENTS.md.** The directory is the local-config surface third-party tooling reads; a two-line deferral pointer costs nothing and prevents an agent or tool from inventing project overrides that drift from the root map.

## Consequences

The repository gains the init-repo surface area — concept references, plan lifecycle, env template, local config — with zero second homes and zero new gate surface: existing `doc-sync` gates (mermaid, wrap, links, pairing, budgets, note format) cover every new file as written. Future plans cost one authored English file under `exec-plans/active/` plus one PLANS.md table row. The pairing manifest gains its first directory-prefix exclusion, so the exclusion list must keep discriminating between working state (excluded) and durable reference (paired); that line is now visible in the i18n contract. Quality-score grades are claims about named gates: when a gate changes coverage, the grade row travels in the same change or the table lies.
