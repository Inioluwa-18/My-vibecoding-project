# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

Faith & Flow is a working Christian spiritual-life prototype built around Bible reading, original instrumental demos, devotionals, and a personal library. The React frontend has been refactored into feature folders matching the Living PRD’s screens. Clerk handles identity; an Express API stores account-owned records in PostgreSQL through Drizzle. Bible chapters come from an external provider, while six audio demos and four original devotionals remain bundled content. Saves, notes, playlists, reading history, and devotional progress are functional. Supabase is not connected: schema, seed, and verification SQL have been prepared, but credentials were not supplied and no remote tables were created. Preserve current identities, content IDs, and URLs. Treat Supabase integration as a controlled database cutover—not a completed backend setup.

## Architecture (plain language)

- **Frontend:** Location: artifacts/faith-flow/src/  React, TypeScript, and Vite render the application. Wouter handles navigation. TanStack Query manages server data and refreshes. App.tsx composes providers and routes rather than implementing screens. features/ groups screen components, content, hooks, and supporting components. Shared layout and UI primitives live in components/. The screen names match the PRD, but existing URLs intentionally remain unchanged:  Screen component	URL HomeScreen	/listen BibleScreen	/bible ListenScreen	/music StudyScreen	/study MyLibraryScreen	/library ProfileScreen	/settings Additional routes cover welcome, sign-in/sign-up, devotionals, and the Bible Library.
- **Backend / data:** Backend and data Identity: Clerk; do not replace it with Supabase Auth during database setup. API: Express in artifacts/api-server, exposed through /api. Current database: PostgreSQL, accessed through lib/db using DATABASE_URL. Contracts: lib/api-spec/openapi.yaml defines request/response formats used by generated client hooks and validators. Bible source: bible-api.com, with configured KJV/WEB availability and attribution. Supabase preparation: supabase/ contains a proposed, separate faith_flow schema with 15 tables, catalog seeds, and verification queries. It is not wired into the running application or integration-tested against Supabase.
- **Key flows:** ey flows
Sign-in: Clerk establishes identity; private screens and API handlers require authentication.
Save/read: The frontend sends changes to authenticated APIs. PostgreSQL stores records under the account’s Clerk identity; library views refresh after changes, on focus, and periodically.
Listening: Playback state and audio resources sit above screen routing, allowing navigation without restarting the track. Account changes stop playback.
Daily devotional: Home selects from four entries by local calendar day. Text, photograph, and reading link share the same entry. Missing or unapproved imagery falls back to a reviewed nature photograph.
Legacy import: Device snapshots are imported idempotently and retained locally; replaying an imported snapshot must not restore subsequently removed saves.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| Real authentication and account-owned PostgreSQL persistence | solid | Development authentication configuration is not a production-readiness guarantee |
| Feature-based screen organization and separated content/data hooks | rough | Some reader interaction and write-coordination logic remains inside BibleReader |

## Risks & assumptions for the team

Risks and assumptions
Supabase is the main unfinished dependency. The prepared SQL uses faith_flow; current Drizzle mappings target the existing database’s public-schema tables. Align connection handling and schema management before switching.
Do not blindly run the existing db push command against Supabase. It does not describe the prepared Supabase namespace.
Protect existing saves. Connecting to an empty database would make current cloud records appear missing. Any cutover needs an approved migration, reconciliation, and rollback plan.
RLS does not replace API authorization. Prepared tables deny browser-role access, but a privileged server connection can bypass RLS. Every personal query must remain account-scoped.
Content rights need ongoing review. Commercial recordings and additional translations require permission. KJV has territorial restrictions; database metadata does not grant rights.
External Scripture availability is a dependency. Preserve explicit failure/retry states rather than substituting invented text.
Account deletion needs review. Clerk account deletion is not proven to remove all application records automatically.
Time With God, plans, Focus Mode, whole-app themes, offline downloads, payments, and communities remain outside the implemented prototype.

## How to run it

```
How to run it
In this workspace
Use pnpm from the repository root. For a fresh dependency checkout:

pnpm install --frozen-lockfile
Start these configured workflows:

Workflow	Command
artifacts/api-server: API Server	pnpm --filter @workspace/api-server run dev
artifacts/faith-flow: web	pnpm --filter @workspace/faith-flow run dev
The workflows supply PORT and frontend BASE_PATH. Bare dev commands without those settings can fail. Open the app through the project preview.

Keep the existing Clerk configuration and database secret in workspace Secrets. Required configuration includes DATABASE_URL, CLERK_SECRET_KEY, CLERK_PUBLISHABLE_KEY, and VITE_CLERK_PUBLISHABLE_KEY; never commit their values.

Checks
pnpm run typecheck:libs
pnpm --filter @workspace/faith-flow run typecheck
pnpm --filter @workspace/faith-flow run test:architecture
pnpm --filter @workspace/faith-flow run test:devotional-artwork
pnpm --filter @workspace/faith-flow run test:local-day
pnpm --filter @workspace/api-server run test:personal
For a standalone frontend build check, provide the required temporary build settings:

PORT=18881 BASE_PATH=/ pnpm --filter @workspace/faith-flow run build
Supabase handover
Read supabase/README.md before applying anything. The prepared files are:

supabase/migrations/202610150001_core.sql
supabase/seed.sql
supabase/verify.sql
Next engineering action: validate these scripts in an isolated PostgreSQL/Supabase environment, inspect the intended destination, and only then plan connection wiring and any migration of existing records.
```
