# CarResale AI

[![GitHub](https://img.shields.io/badge/GitHub-lingaavasu%2Fcar--resale--ai-181717?logo=github)](https://github.com/lingaavasu/car-resale-ai)
[![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)](https://nextjs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-14%2B-336791?logo=postgresql)](https://www.postgresql.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**CarResale AI** is an India-focused full-stack used-car resale intelligence and marketplace platform built with Next.js, PostgreSQL and Drizzle ORM.

## Features

- Vehicle resale valuation with predicted price, range, confidence, depreciation and valuation factors.
- Vehicle input wizard for brand, model, year, fuel, transmission, kilometres, owners, condition and city.
- Used-car marketplace with featured listings and model-specific vehicle photography.
- Garage, favourites, listing views and valuation history.
- Buyer/owner and dealer workflows.
- Dealer CRM with inventory, customers, leads and analytics.
- Admin tools for users, predictions, market data, model versions and audit logs.
- Reports dashboard with valuation charts and market analytics.
- **Download Report as PDF** for the authenticated user's report.
- India-focused INR pricing and market presentation.

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 16, React 19, Tailwind CSS 4 |
| Backend | Next.js App Router API routes / server actions |
| Database | PostgreSQL |
| ORM | Drizzle ORM |
| Charts | Recharts |
| Icons | Lucide React |
| Authentication | bcryptjs + database-backed sessions |
| Language | TypeScript |
| Runtime | Node.js 20+ |

## Local Setup

### Requirements

- Node.js 20+ (22 recommended)
- npm 10+
- PostgreSQL 14+ (16 recommended)
- Git

### Clone

```bash
git clone https://github.com/lingaavasu/car-resale-ai.git
cd car-resale-ai
```

### Install

```bash
npm install
```

### Configure the database

Create a PostgreSQL database named `app_db` and create a local `.env` file:

```env
DATABASE_URL=postgresql://postgres:YOUR_PASSWORD@127.0.0.1:5432/app_db
```

Never commit `.env` or database credentials.

### Initialize the schema

```bash
npx drizzle-kit push --force
npx tsx src/db/seed.ts
```

### Start development

```bash
npm run dev
```

Open http://localhost:3000.

## Useful Commands

```bash
npm run dev
npm run build
npm run start
npm run lint
npm run typecheck
```

## PDF Reports

The Reports page provides a **Download PDF** action. The report endpoint is:

```text
GET /api/reports/pdf
```

The generated report is scoped to the authenticated user and includes relevant valuation, garage, listing, market and dealer-lead metrics.

## Vehicle Images

The project includes model-specific vehicle images and a source/attribution record in [REAL_IMAGE_SOURCES.md](REAL_IMAGE_SOURCES.md).

Third-party images can have separate licensing requirements. Preserve attribution and review the source license before redistribution.

## Deployment

The application is designed for a Node.js-compatible Next.js host such as Vercel. Production deployment requires a reachable PostgreSQL database and the `DATABASE_URL` environment variable.

Do not place production secrets in GitHub.

## Security

- Passwords are hashed before storage.
- Authentication uses database-backed sessions.
- Keep secrets in environment variables.
- Do not commit `.env`, API tokens, private keys or database passwords.

## Author

**Vamshi Krishna C / lingaavasu**

- GitHub: https://github.com/lingaavasu
- Repository: https://github.com/lingaavasu/car-resale-ai

## License

The project source code is licensed under the MIT License. See [LICENSE](LICENSE).

Third-party photographs and other external assets may have separate licenses; see [REAL_IMAGE_SOURCES.md](REAL_IMAGE_SOURCES.md).
