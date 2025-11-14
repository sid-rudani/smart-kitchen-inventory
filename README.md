Github Repository: https://github.com/sid-rudani/smart-kitchen-inventory/

A lightweight Next.js app to manage kitchen inventory, suggest recipes, and track item consumption using Supabase for auth and storage.

This repository contains a small Next.js (App Router + TypeScript) project. The app uses Supabase for authentication and storage. The original project integrates with a locally-run LLM via Ollama (the author used the `llama3.1` model). This README explains how to set up the project and the local Ollama model.

## What you'll find here

- `app/` - Next.js pages and API routes (app router)
- `components/` - React UI pieces used across the app
- `utils/supabase/client.ts` - Supabase client factory (reads public env vars)
- `utils/supabase/` - Supabase-related helpers

## Prerequisites

- macOS (instructions use macOS where appropriate)
- Node.js (v18+ recommended)
- pnpm (recommended) or npm/yarn
- Git

Install pnpm (if you don't have it):

```bash
npm install -g pnpm
```

## Quick local setup

1. Clone the repo

```bash
git clone <this-repo-url>
cd smart-kitchen-inventory
```

2. Install dependencies

```bash
pnpm install
```

3. Environment variables

Create a `.env.local` file at the project root. At minimum the app expects the Supabase public URL and anon key (these are used by the client code in `utils/supabase/client.ts`):

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-public-anon-key
# Optional: set the Ollama HTTP API address (if you run Ollama locally)
OLLAMA_API_URL=http://127.0.0.1:11434
```

Notes:
- Create a Supabase project and copy the Project URL and anon/public API key into the two vars above.
- The code uses `createBrowserClient(process.env.NEXT_PUBLIC_SUPABASE_URL, process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY)` (see `utils/supabase/client.ts`).

4. Run the app

```bash
pnpm run dev
```

Open http://localhost:3000 in your browser.

## Ollama + llama3.1 (local LLM)

The original project used a locally-run Ollama model (the author used `llama3.1`). If you want to run the same setup locally, follow these steps.

1. Install Ollama (macOS example)

```bash
# Homebrew (recommended on macOS):
brew install ollama
# Or follow the platform-specific instructions at https://ollama.com
```

2. Pull the model (use the model name you need; `llama3.1` is what the author used):

```bash
ollama pull llama3.1
```

3. Start Ollama's HTTP server so your app can send inference requests (default port is 11434):

```bash
ollama serve
```

4. Confirm/override the API URL

By default Ollama listens on `http://127.0.0.1:11434`. If you need to override that location for the app, set `OLLAMA_API_URL` in `.env.local`.

Important: The project's codebase does not require Ollama to run the Next.js UI — however, features that call the LLM will fail or be disabled if Ollama is not available. Make sure Ollama is running and the model is pulled before trying AI-powered features.

## Scripts

Available npm scripts (from `package.json`):

- `pnpm run dev` — start Next.js in development mode
- `pnpm run build` — build for production
- `pnpm run start` — run production build
- `pnpm run lint` — run eslint

## Project notes and troubleshooting

- Supabase: If you get auth or DB errors, verify `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` in `.env.local` and confirm your Supabase tables exist.
- Ollama: If the LLM features return errors, confirm:
  - Ollama is installed and running (`ollama serve`).
  - The model (e.g. `llama3.1`) is pulled: `ollama list` will show available models.
  - `OLLAMA_API_URL` is set (if not using the default `http://127.0.0.1:11434`).
- Ports: Next.js default is 3000. Ollama default is 11434. Adjust if those ports conflict.

## Short developer notes

- Supabase client factory is small and intentionally reads public env vars from `process.env` so the client can be used inside the browser (see `utils/supabase/client.ts`).
- If you want to wire a remote LLM provider instead of Ollama, add the provider credentials and update the server-side route(s) that call the LLM.

If you'd like, I can also:

- Add an example `.env.example` file with placeholders
- Add a small script or README section showing a quick curl example for the app's LLM API call (once you tell me which internal route or integration you use)

---

If anything needs more detail (for example: exact LLM endpoint usage inside this repo or an `.env.example` file), tell me and I will add it.
# Smart Kitchen Inventory

> A lightweight Next.js app to manage kitchen inventory, suggest recipes, and track item consumption using Supabase for auth and storage.

## Quick summary



- Framework: Next.js 16 (App Router)
- Language: TypeScript + React 19
- UI: Tailwind CSS + shadcn/ui components + Radix + Lucide icons
- Backend: Supabase (auth + database)
- Package manager: pnpm (pnpm-lock.yaml present)

## Features

- Sign up / log in (Supabase auth)
- Add, list and consume items in inventory
- Recipe suggestions based on inventory
- Dashboard view for inventory and suggestions
- API routes under `app/api` for items and recipes

## Prerequisites

- Node.js (v20+ recommended)
- pnpm installed globally (or use `npm`/`corepack`):

```bash
pnpm install -g pnpm
```

## Environment variables

Create a `.env.local` in the project root with these variables (Supabase):

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=public-anon-key
```

Notes:
- The app uses `@supabase/ssr` and server-side helpers — both public URL and anon key are read from `process.env`.

## Install & run (development)

From the project root:

```bash
pnpm install
pnpm dev
```

Open http://localhost:3000

## Build & run (production)

```bash
pnpm build
pnpm start
```

## Available scripts

Taken from `package.json`:

- `pnpm dev` — run Next.js in development mode
- `pnpm build` — build for production
- `pnpm start` — run built app
- `pnpm lint` — run ESLint

## Key files & folders

- `app/` — Next.js App Router routes, UI pages and server API endpoints
  - `app/api/items` — API routes for CRUD on items
  - `app/api/recipes` — recipe suggestion APIs
  - `app/auth` — signup/login pages
  - `app/dashboard` — main dashboard page
- `components/` — UI components and shadcn wrappers
- `components/ui/` — design system primitives and components
- `utils/supabase/client.ts` — Supabase client factory (browser)
- `lib/utils.ts` — assorted helpers used across the app

## Supabase integration

The code uses `@supabase/ssr` helpers for server and `@supabase/ssr`/`@supabase/supabase-js` on the client. The `createClient` helper expects the two env variables listed above. For server-side auth, the app uses cookie helpers via Next's `cookies()` and `createServerClient`.
