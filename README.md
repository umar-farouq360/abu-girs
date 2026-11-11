# ABU GIRS — Campus Navigator

**GIRS (Geographical Information Retriever System)** is a campus information and navigation application for **Ahmadu Bello University, Zaria**. It helps students, staff, and visitors find campus facilities, explore locations on an interactive map, and search for places by name, category, or proximity.

## Overview

The application covers ABU's Main Campus and Kongo Campus. It was developed as a final-year Computer Science project to make campus location information easier to discover and use.

## Features

- Search campus locations by name and category.
- Explore locations on an interactive map powered by Leaflet and OpenStreetMap.
- Find nearby facilities using latitude, longitude, and a configurable search radius.
- Browse locations across categories including academic, administrative, health, accommodation, recreation, worship, transport, food, and security.
- Admin portal for managing location records.
- Admin sign-in and protected administration routes.
- Seed dataset containing ABU campus locations.

## Technology

- **Application:** Next.js 15, React 19, TypeScript
- **Styling and UI:** Tailwind CSS, Radix UI, Lucide React
- **Maps:** Leaflet, React Leaflet, OpenStreetMap
- **Database:** PostgreSQL with PostGIS, Drizzle ORM
- **Authentication:** Auth.js / NextAuth
- **Forms and validation:** React Hook Form, Zod
- **Package manager:** pnpm

## Getting Started

### Prerequisites

- Node.js 20 or newer
- pnpm
- Docker and Docker Compose

### 1. Clone the repository and install dependencies

```bash
git clone https://github.com/umar-farouq360/abu-girs.git
cd abu-girs
corepack enable
pnpm install
```

### 2. Configure environment variables

Create the local environment file:

```bash
cp .env.example .env.local
```

Set the following values in `.env.local`:

| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `AUTH_SECRET` | Auth.js secret (at least 32 characters) |
| `ADMIN_EMAIL` | Email address for the initial admin account |
| `ADMIN_PASSWORD` | Password for the initial admin account (at least 8 characters) |
| `NODE_ENV` | Usually `development` locally |

For the supplied Docker Compose database, use:

```env
DATABASE_URL=postgresql://girs_user:girs_password@localhost:5434/girs_db
```

Generate an Auth.js secret with:

```bash
openssl rand -hex 32
```

### 3. Start the database

```bash
docker compose up -d
```

The compose file starts PostgreSQL with the PostGIS image and exposes it on port `5434`.

### 4. Apply migrations and seed the database

```bash
pnpm db:migrate
pnpm db:seed
```

The seed command creates the initial admin account from `ADMIN_EMAIL` and `ADMIN_PASSWORD`, and inserts the campus location dataset.

### 5. Run the development server

```bash
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000).

## Useful Commands

| Command | Description |
|---|---|
| `pnpm dev` | Start the Next.js development server |
| `pnpm build` | Build the application |
| `pnpm start` | Start the production server |
| `pnpm db:generate` | Generate Drizzle migrations |
| `pnpm db:migrate` | Apply database migrations |
| `pnpm db:push` | Push schema changes directly (development use) |
| `pnpm db:studio` | Open Drizzle Studio |
| `pnpm db:seed` | Seed the admin account and campus locations |

## Project Structure

```text
.
├── app/
│   ├── (auth)/       # Authentication pages
│   ├── (public)/     # Public search and map pages
│   ├── admin/        # Admin dashboard
│   └── api/          # Route handlers
├── db/               # Drizzle schema and migrations
├── lib/              # Auth, environment, and validation utilities
├── scripts/          # Database seed and maintenance scripts
├── public/
└── docker-compose.yml
```

## Notes

- Proximity search uses PostGIS geography functions and returns distances in metres.
- The map uses OpenStreetMap data; map tile availability and usage are subject to the provider's terms.
- Keep `.env.local` out of version control and use strong, unique credentials outside local development.

## Author

**Umar Farouq Ibrahim** — [GitHub](https://github.com/umar-farouq360)
