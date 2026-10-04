# CarResale AI — Run Locally in VS Code

Full-stack app: **Next.js 16 (App Router) + PostgreSQL + Drizzle ORM + Tailwind CSS 4**

The current catalog is India-first: INR prices with Indian digit grouping, 25 Indian cities, and local brands/models from Maruti Suzuki, Tata, Mahindra, Hyundai, Kia, Toyota, Honda, Skoda, Volkswagen, Renault, Nissan, MG Motor and more. The seed is idempotent and automatically migrates an older USD database to INR exactly once.

---

## 1. Prerequisites

| Tool | Version | Check |
|------|---------|-------|
| Node.js | 20+ (22 recommended) | `node -v` |
| npm | 10+ | `npm -v` |
| PostgreSQL | 14+ (16 recommended) | `psql --version` |
| Git | any | `git --version` |

> No PostgreSQL installed? Use Docker instead:
> ```bash
> docker run -d --name carresale-pg \
>   -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=app_db \
>   -p 5432:5432 postgres:16
> ```

## 2. Open the project

```bash
git clone <your-repo-url> carresale-ai
cd carresale-ai
code .          # opens VS Code
```

## 3. Configure environment

Copy `.env.example` to `.env` in the project root (next to `package.json`):

```env
DATABASE_URL=postgresql://postgres:postgres@127.0.0.1:5432/app_db
```

Adjust user/password/host if your PostgreSQL differs. (Docker command above matches this URL exactly.)

> **Copy the whole project folder.** The photos live in `public/images/` as
> binary files — they do not transfer if you copy-paste only source code.
> If `public/images/` is empty, fix it with one command:
> `node scripts/download-images.mjs`

## 4. Install dependencies

In the VS Code integrated terminal (**Terminal → New Terminal** / ``Ctrl+` ``):

```bash
npm install
```

## 5. Create tables & seed demo data

```bash
# create all tables from src/db/schema.ts
npx drizzle-kit push --force

# insert demo users, cars, listings, leads, models, market data
npx tsx src/db/seed.ts
```

You should see: `India market seed complete ✔`

## 6. Start the dev server

```bash
npm run dev
```

Open **http://localhost:3000** in your browser. Hot-reload is on — edit files in VS Code and the page updates instantly.

## 7. Sign in — register your own account

There are **no demo/shortcut accounts** on the login screen. Open
**http://localhost:3000/register**, pick **Owner/Buyer** or **Dealer**, and
create your account — you land straight in your dashboard.

The seed loads sample **content** (marketplace cars, leads, market data) owned
by background accounts so the marketplace isn't empty — you interact with them
only as a buyer/seller, you don't sign in as them.

Need the admin console locally? Register normally, then promote yourself:

```bash
psql postgresql://postgres:postgres@127.0.0.1:5432/app_db \
  -c "UPDATE users SET role='admin' WHERE email='you@example.com';"
```

Sign out, sign back in, and you'll land on `/admin`.

## 8. Useful commands

```bash
npm run dev         # development server (hot reload)
npm run build       # production build
npm run start       # serve production build (after build)
npm run typecheck   # TypeScript check
npx drizzle-kit push --force   # re-sync DB tables after schema edits
npx tsx src/db/seed.ts         # re-seed (skips if already seeded)
npx drizzle-kit studio         # visual DB browser (optional)
```

Reset the database to a clean slate:

```bash
psql postgresql://postgres:postgres@127.0.0.1:5432/app_db -c "DROP SCHEMA public CASCADE; CREATE SCHEMA public;"
npx drizzle-kit push --force
npx tsx src/db/seed.ts
```

## 9. Project map

```
src/
├── app/
│   ├── page.tsx                 # landing page
│   ├── (auth)/login|register    # auth pages
│   ├── (app)/                   # protected app (dashboard, predict, sell, …)
│   │   ├── dealer/              # dealer console
│   │   └── admin/               # admin control room
│   ├── market/  listings/       # public market + marketplace
│   └── api/auth|predict|health  # API routes
├── components/                  # UI, charts, forms, shells
├── db/                          # schema.ts · seed.ts · connection
└── lib/                         # auth, valuation engine, catalog, helpers
```

## Troubleshooting

| Symptom | Fix |
|---|---|
| `DATABASE_URL is required` | Create/edit `.env`, then restart `npm run dev` |
| `ECONNREFUSED 127.0.0.1:5432` | PostgreSQL isn't running (start it, or `docker start carresale-pg`) |
| `relation "users" does not exist` | Run `npx drizzle-kit push --force` |
| No demo data | Run `npx tsx src/db/seed.ts` |
| Login loops to /login | Clear cookies for localhost, restart dev server, retry |
| Port 3000 busy | `npx kill-port 3000` or run `npm run dev -- -p 3001` |
| Images not loading | All runtime images are bundled under `public/images/` (13 marketplace photos under `public/images/`) and require no internet at runtime. **If photos are missing in your local copy, run `node scripts/download-images.mjs` in the VS Code terminal** — it restores every photo and verifies each one. A styled placeholder also protects against any missing file. |

### Reports PDF export
The Reports page no longer has the non-functional CSV export stub. Use **Download PDF** to download an authenticated PDF report from `/api/reports/pdf`.
