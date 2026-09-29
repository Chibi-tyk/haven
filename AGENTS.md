# Haven workspace boundaries and constraints

This workspace contains three related projects: `app/`, `api/`, and `mobile/`. This file is the shared source for their stable boundaries, chosen stacks, architecture, and constraints. Feature behavior belongs in `SPEC.md`; setup and user-facing project information belongs in `README.md`. `CLAUDE.md` points Claude-based tools to this guidance.

## Shared boundaries

- The API is authoritative for persistence, authentication, validation, authorization, and moderation.
- Web and mobile clients use the versioned HTTP JSON API. They must not connect directly to the database or treat client checks as security enforcement.
- Coordinate breaking API contract changes with both clients.
- Keep credentials, secrets, and local database files out of version control.
- Update this file only when a lasting project boundary, stack choice, or architecture/security constraint changes.

## `app/` — browser client

- TypeScript, React, Vite, and React Router.
- API access and shared domain types live in `src/lib/api.ts` and `src/lib/types.ts`.
- Global and responsive presentation is in `src/styles.css`; account and admin styles are scoped beside their components.
- `VITE_API_URL` configures the API base URL. Never place secrets in browser code.
- Preserve accessible labels, keyboard navigation, visible focus, responsive layouts, and the illustrative listing fallback.
- Keep the existing framework and state patterns unless an explicit architecture decision changes them.

## `api/` — service and data

- TypeScript on Node.js with Express 5; REST routes are mounted under `/api/v1`.
- SQLite via `better-sqlite3`; `src/db.ts` owns database access and `src/seed.ts` provides development data.
- Validate inputs at the API boundary with Zod. Enforce ownership and admin permissions server-side.
- Browser sessions use opaque HTTP-only cookies; store only session hashes.
- Configure port, database path, allowed web origin, environment, and session lifetime via environment variables.
- Keep Helmet, credentialed CORS, bounded JSON size, and authentication rate limits. Use transactions for multi-row listing/photo changes.
- Schema changes need a migration approach before production use.

## `mobile/` — Expo client

- TypeScript, React Native, and Expo Router.
- Screen routes live under `app/`. Extract shared API and presentation code into local modules as the app grows.
- `EXPO_PUBLIC_API_URL` configures the API base URL; public-prefixed values are bundled into the app and must not contain secrets.
- Keep UI native and safe-area aware. For physical devices, configure an API hostname reachable over the local network; device `localhost` is not the development computer.
