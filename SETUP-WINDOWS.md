# NIDAN setup on Windows CMD

1. Install Node.js 20+ and enable Corepack:

```cmd
corepack enable
corepack prepare pnpm@10.18.0 --activate
```

2. Create the database and copy the server-only environment template:

```cmd
copy .env.example .env
notepad .env
```

Set `DATABASE_URL`, then add the Bhashini and Gemini keys supplied by the administrator. Never add them to frontend files, `VITE_*` variables, Git, screenshots, or logs.

3. Install and migrate:

```cmd
pnpm install
pnpm db:migrate
```

4. Check and test:

```cmd
pnpm check
pnpm test
```

5. Start development:

```cmd
set PORT=3000
pnpm dev
```

Open `http://localhost:3000`. The first local patient can self-register. Staff and doctor accounts must be provisioned by an authenticated administrator using the admin API/provisioning screen. In production, require HTTPS, a managed MySQL database, secure cookies, backups, restricted storage access, and secret-manager injection.

## Migrations

Checked-in migrations are in `drizzle/`. Apply them with `pnpm db:migrate`. For a schema change, update `drizzle/schema.ts`, run `pnpm db:push` in a disposable development database, review the generated SQL, and commit the reviewed migration. Do not run destructive resets against clinical data.
