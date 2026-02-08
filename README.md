# F-Hatch

F-Hatch is a batteries-included Go starter. It keeps the tooling you need to ship production services (HTTP server, validation, logging, Postgres, Docker, CI) and leaves everything else flat so you can model features however you like.

---

## Why this template?

- ✅ **Flat by design** – No architecture imposed; structure features the way your domain needs
- ✅ **Ready to run** – Health probes, graceful shutdown, logging, validation, Postgres client, Docker setup, CI workflows
- ✅ **Simple to extend** – Drop new packages under `internal/`, wire them into the HTTP server, and reuse the shared batteries

---

## Repository map

```text
cmd/api/            → Program entry point (config load, DB connect, HTTP server start)
config/             → Environment-backed configuration loader
internal/           → Your features live here (empty playground)
pkg/
  db/postgres/      → sqlx client helper and shared DB errors
  http/rest/        → Gin-based server, middleware, probes, response helpers
  o11y/             → slog logger and context helpers
  validator/        → go-playground validator wrapper with context helpers
pkg/http/rest/
  middleware.go     → telemetry + validator wiring
  probe.go          → /livez and /readyz
  request.go        → JSON binding + validation helpers
  response.go       → Response helpers
db/migration/       → Place your SQL migrations (Makefile + migrate CLI)
Dockerfile          → Production build image (CGO disabled, static binary)
docker-compose.yml  → Local Postgres + app wiring
Makefile            → Build, migrations, mock generation, tests, lint
.github/workflows/  → Build, test, and lint workflows
```

Everything under `pkg/` is safe to import anywhere. Keep feature-specific code in `internal/`—create packages that expose the handlers/services you need and register them in `cmd/api` when ready.

---

## Getting started

1. **Configure environment**
   - Copy `.env.example` to `.env`
   - Adjust `REST_SERVER_PORT` and `POSTGRES_URL` (Docker compose uses `db:5432`)
2. **Run the stack**
   - Using Go: `go run ./cmd/api`
   - Using Air (hot reload): `air`
   - Using Docker compose: `docker compose up --build`
3. **Hit the probes**
   - `GET /livez` for liveness
   - `GET /readyz` for readiness (checks DB)

---

## Batteries

- **Configuration** – `config.Load()` merges `.env` + environment variables.
- **Logging** – `pkg/o11y` exposes a global `slog` logger and context helpers (`o11y.WithLogger`, `o11y.FromContext`).
- **Validation** – `pkg/validator` wraps `go-playground/validator`. Use `validator.WithValidator`/`FromContext` to share instances per request.
- **HTTP server** – `pkg/http/rest.Server` handles graceful shutdown, middleware, probes, and response helpers. Extend `setupRouter` to add routes.
- **Database** – `pkg/db/postgres.Connect` returns an `*sqlx.DB`. Add your repositories near your features.
- **Migrations** – Use `make new-mig NAME=create_users`, then `make mig-up` (ensure `DATABASE_URL` in `Makefile` points to localhost DB).
- **Testing** – `make test` runs `mockery` (adjust paths if you add mocks) and `go test ./... -race`.

---

## CI/CD

GitHub workflows cover build, lint, and tests. They expect the same `REST_SERVER_PORT`/`POSTGRES_URL` env vars, so keep `.env.example` current when adding config.

---

**Flat, explicit, and ready to grow.**
