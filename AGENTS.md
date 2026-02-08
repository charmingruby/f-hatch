# Agent Context

> **Quick start:** skimming `README.md` tells you everything about the batteries already wired in.

---

## Hatch in one sentence

A flat Go starter that ships the boring infrastructure (HTTP server, validation, logging, Postgres, Docker, CI) and lets you model features however you want under `internal/`.

---

## Core Rules

### Always Do

- Work inside the package you touched (usually something under `internal/` or `pkg/`)
- Follow existing naming/import patterns; reuse the shared batteries instead of rebuilding them
- Keep dependencies explicit: pass `*sqlx.DB`, validators, configs through constructors
- Add/adjust tests or probes when behavior changes
- Keep files small and focused; split helpers when they stop being obvious

### Never Do

- Introduce new global layers or frameworks
- Reach across packages implicitly (no hidden singletons, no circular deps)
- Skip the provided middleware/telemetry wiring when exposing HTTP handlers
- Use `panic()`/globals for control flow
- Refactor unrelated areas just to “clean up”

---

## When in doubt

1. Read `README.md` for the latest project map and workflow
2. Inspect the relevant `pkg/` helper before inventing a new one (validator, o11y, db, rest server)
3. Ask: “What’s the simplest change that keeps things explicit and easy to extract later?”

---

**Remember:** Hatch is batteries included, architecture optional. Keep it simple and flat.
