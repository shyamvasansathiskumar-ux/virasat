# Virasat

**Financial continuity, made clear.** When someone dies or a family loses track of its paperwork, money gets stranded across bank deposits, provident funds, insurance policies, mutual funds and LIC covers, each held by a different regulator (RBI, EPFO, IRDAI, SEBI, LIC). Virasat brings those scattered records into one calm workspace: what was found, what needs a document, and what you can claim next.

**Live demo:** [virasatweb-seven.vercel.app](https://virasatweb-seven.vercel.app) · Built at PEC Hacks 4.0 (August 2026) as a team.

> This is a frontend prototype running on sample data. No real financial or regulator connection is made, nothing you type leaves your browser, and the amounts shown belong to a fictional family.

## What you can do in the demo

- **Home:** a continuity record across five regulators, with discovered, claimed and unclaimed value.
- **Claims:** every claim in motion, from *documents pending* through *AI verification* to *ready to submit*.
- **Claim Assist:** walk a claim through upload, review and preparation. Virasat prepares the packet; you submit it yourself on the official portal.
- **FinTwin:** an illustrative what-if for a recovered amount (FD, PPF or mutual fund rates). A projection, not advice.
- **Documents:** what each claim still needs.

## How it's built

- React 19 + Vite + Tailwind, tRPC client, Radix UI.
- Express + tRPC server with Drizzle ORM (MySQL/MariaDB) and self-hosted auth (bcrypt, JWT, rate limiting).
- A `DataService` boundary keeps the UI independent of persistence: `MockDataService` is the default for the demo, and `VITE_USE_BACKEND=true` switches every page to the real API, falling back to the demo data if a call fails.
- Without `DATABASE_URL`, the server also answers from sample data instead of erroring.

## Run it

Demo only (no database):

```bash
pnpm install
pnpm exec vite build && npx serve -s dist/public
```

Full stack with a local database: see [SETUP.md](SETUP.md). `start.sh` creates the database user with a password you choose:

```bash
export VIRASAT_DB_PASSWORD='pick-a-strong-local-password'
./start.sh
```

Tests: `pnpm test` (5 tests).

## Deploy

`vercel.json` builds only the client (`vite build` → `dist/public`) and rewrites every route to `index.html`, so the static demo works on any path. The Express server is not deployed; run it yourself with `pnpm build && pnpm start` if you need the real API.

## Notes

Scaffolded from a Manus web-app template; the Manus editor runtime and debug collector are only loaded in development. See [docs/backend-architecture.md](docs/backend-architecture.md) and [docs/verification-engine.md](docs/verification-engine.md) for the design, and [AUDIT_FOLLOWUP.md](AUDIT_FOLLOWUP.md) for the security pass.

