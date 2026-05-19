# Project Roadmap

> Backend service for the `docs-helper` Chrome extension.
> Lets teams attach shared notes to features defined in their `doc-map.json`.

---

## Vision

The `docs-helper` extension already maps **feature keys** (e.g. `order_info`, `payment_links_list`) from a team's `doc-map.json` to internal documentation links. This backend extends that with a **shared notes layer** — any team member can leave context, warnings, or links on a feature, and the rest of the team sees them in the extension popup.

In short: **tribal knowledge, attached to the features it belongs to.**

---

## Status

🚧 **In development** — see milestones below.

---

## Milestones

### M1 — Notes MVP

Goal: any user can attach notes to a feature key and read them back.

- [ ] `POST /notes` — create a note for a `featureKey`
- [ ] `GET /notes?featureKeys=...` — batch read for multiple feature keys at once
- [ ] `PATCH /notes/:id`, `DELETE /notes/:id`
- [ ] Input validation
- [ ] OpenAPI/Swagger docs

---

### M2 — Accounts & Team Workspaces

Goal: notes are scoped to a team, not global.

- [ ] Email + password registration and login
- [ ] JWT-based authentication with refresh tokens
- [ ] **Workspaces** — a team has its own space, its own doc-map URL, its own notes
- [ ] **Invitations** — invite teammates by email
- [ ] **Roles** — Owner / Admin / Member
- [ ] All notes are workspace-scoped; no cross-workspace leakage

---

### M3 — Doc-map Integration

Goal: backend understands the `doc-map.json` directly.

- [ ] Workspace can point at a `doc-map.json` URL
- [ ] Backend fetches, validates, and caches the map
- [ ] Manual refresh endpoint
- [ ] **Orphan detection** — if a feature key disappears from a new doc-map version, related notes are flagged (not deleted)
- [ ] Endpoint to list orphaned notes for cleanup

---

### M4 — Discovery & Engagement

Goal: notes are findable and lightweight to interact with.

- [ ] **Full-text search** across notes in a workspace
- [ ] **Emoji reactions** on notes (👍 ❤️ 🎉 etc.)
- [ ] **Pinned notes** — admins can pin important notes to appear first
- [ ] **Activity feed** — recent notes across the workspace, paginated

---

### M5 — Live Collaboration

Goal: the extension reflects teammates' changes without refresh.

- [ ] WebSocket gateway with workspace-scoped rooms
- [ ] Events: `note:created`, `note:updated`, `note:deleted`, `reaction:added`, `reaction:removed`, `doc-map:updated`
- [ ] JWT authentication on socket handshake

---

### M6 — Background Jobs

Goal: things that shouldn't block requests happen in the background.

- [ ] **Periodic doc-map sync** — backend re-fetches every workspace's doc-map on a schedule
- [ ] **Invitation emails** — sent asynchronously
- [ ] Job monitoring dashboard (admin only)

---

### M7 — Production Readiness

Goal: safe to use at work without 3am surprises.

- [ ] Structured JSON logging with request IDs
- [ ] Liveness and readiness health checks
- [ ] Global error handling with consistent error format
- [ ] Rate limiting (auth endpoints + global)
- [ ] Helmet, CORS allowlist, dependency audit
- [ ] Test coverage for critical flows

---

### M8 — Deployment

Goal: shipped, automated, monitored.

- [ ] Dockerized (multi-stage build)
- [ ] CI: lint, typecheck, tests on every PR
- [ ] CD: auto-deploy on merge to `main`
- [ ] Error monitoring via Sentry
- [ ] Published URL + deploy instructions in README

---

### M9 — Extension Wiring

Goal: the existing `docs-helper` extension is no longer single-player.

- [ ] Login flow inside the extension popup
- [ ] Notes panel showing notes per visible feature
- [ ] Inline "add note" UI
- [ ] Reactions UI
- [ ] Live updates via WebSocket while popup is open
- [ ] Workspace picker for users in multiple teams

---

## Out of Scope (for now)

To keep the project shippable, the following are explicitly **not** planned:

- ❌ Editing `doc-map.json` through the API — the source of truth stays in the team's repo
- ❌ A standalone web UI — the extension is the only client
- ❌ Public/shareable notes outside a workspace
- ❌ AI-powered note summarization
- ❌ Mobile app
- ❌ Billing / subscriptions
- ❌ OAuth providers (Google, GitHub) — email + password only initially

These may be reconsidered after M9 ships.

---

## Tech Stack

- **Backend:** Nest.js, TypeScript
- **Database:** PostgreSQL (with full-text search), Prisma ORM
- **Cache & Queues:** Redis + BullMQ
- **Real-time:** Socket.IO via `@nestjs/websockets`
- **Auth:** JWT (access + refresh)
- **Deployment:** Docker, Railway, GitHub Actions
- **Monitoring:** Sentry

---

## Contributing

Currently a personal project, but feedback and ideas are welcome via GitHub Issues.
