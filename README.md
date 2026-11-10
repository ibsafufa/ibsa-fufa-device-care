Device Care Services
React / Express / tRPC / Drizzle starter, adapted from the Sandbox web-db-user template.
`pnpm dev`: development server; honors `PORT` (default 3000).
`pnpm build` / `pnpm start`: build and serve `dist/index.js` and `dist/public/`.
`pnpm db:migrate`: apply checked-in migrations. `pnpm db:push`: generate and apply new schema changes.
`pnpm check` / `pnpm test`: types and application tests.
Start with the Webdev skill's default-template guide. Platform login, storage, payments and service contracts live in its shared references; read the relevant capability before extending its helper.
`server/_core/publicConfig.ts` exposes only named public runtime values. Private keys stay server-side. The platform serves managed `/manus-storage/` assets; the application does not register a second proxy.
Platform configuration is readable and editable through `webdev.config`. Default settings are initial values, not enforced constraints. The agent may modify the files, commands and configuration or follow the flexible guide for another stack.

Device Care service requests and administrator dashboard
The public inquiry form now saves requests through the server API. The private dashboard is available at `/admin` and requires a signed-in Manus account whose database user has `role = 'admin'`.
Required production configuration
`DATABASE_URL`: MySQL connection string for persistent request storage.
Existing Manus OAuth configuration: `MANUS_PROJECT_ID`, `MANUS_JWT_SECRET`, and `MANUS_OAUTH_API_URL` (provided by the Manus runtime when hosted there).
`OWNER_OPEN_ID`: the OpenID of the owner account; the existing user upsert logic grants this account the `admin` role. Alternatively, after the owner has signed in once, set that user's `users.role` to `admin` in the database.
Database migration
Run `pnpm db:migrate` with `DATABASE_URL` configured to create the `service_requests` table from the checked-in migration. The SQL is also in `drizzle/0001_service_requests.sql` for review.
Verification
Deploy the updated server and client together (a static-only deployment cannot save submissions).
Open the website, submit a test inquiry, and confirm a success message appears.
Sign in using the administrator account and visit `/admin`.
Confirm the test request appears, change its status, refresh, and verify the status remains saved.
The dashboard protects list, update, and delete operations with the server-side `adminProcedure`; customers can only create a request and cannot read other customers' submissions.# ibsa-fufa-device-care
