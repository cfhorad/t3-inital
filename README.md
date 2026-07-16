# T3-anho-dining 🚀

A cutting-edge, type-safe web application template built with a modern implementation of the **T3 Stack**, utilizing the latest Next.js 15, React 19, and Tailwind CSS v4 features. 

This repository serves as a boilerplate combining end-to-end type-safe APIs, robust authentication, relational database management, and a high-performance developer toolchain.

---

## 🛠️ Modern Tech Stack & Dependencies

The project relies on a state-of-the-art tech stack selected for maximum developer productivity, type safety, and runtime performance:

### 1. Core Framework & Runtime
*   **Next.js 15 (App Router)**: Uses Next.js 15 with Server Components, Client Components, and the optimized React Compiler.
*   **React 19 & React DOM 19**: Leverages modern React capabilities like server/client boundary features, hooks, and suspense.
*   **TypeScript**: Strictly typed development environment across both frontend and backend.
*   **pnpm v10**: Fast, disk-space-efficient package manager utilizing strict node_modules generation.

### 2. API & Data Fetching
*   **tRPC v11**: End-to-end typesafe API layer allowing direct consumption of server procedures on the client with zero code-generation.
*   **TanStack React Query v5**: Powering the asynchronous state management, caching, background refetching, and query hydration (`HydrateClient`).
*   **SuperJSON**: Rich-data serialization engine to seamlessly pass complex JS types (like `Date`, `Map`, `Set`, `BigInt`) over the network without manual parsing.
*   **Zod**: Schema validation for runtime type enforcement and input validation on both client forms and tRPC procedures.

### 3. Database & ORM
*   **PostgreSQL**: High-performance relational database.
*   **Drizzle ORM**: A lightweight, TypeScript-first SQL query builder. Includes full relational queries support, schema definitions in TypeScript, and strict type checking.
*   **Drizzle Kit**: CLI migration runner, schema generator, and Drizzle Studio (web GUI for database management).
*   **Postgres.js**: Blazing fast, full-featured PostgreSQL client for Node.js.

### 4. Authentication (Better Auth)
*   **Better Auth (v1.3)**: A modern, modular, type-safe authentication library.
    *   **Drizzle Adapter**: Integrates directly with Drizzle ORM to store users, sessions, and accounts in PostgreSQL.
    *   **Providers**: Out-of-the-box support for Github OAuth and secure Email/Password authentication.
    *   **tRPC Integration**: Unified authentication middleware (`protectedProcedure`) checks active sessions and exposes session info type-safely.

### 5. UI & Styling
*   **HeroUI v3**: A beautiful, accessible, and highly customizable React component library (formerly known as NextUI).
*   **Tailwind CSS v4**: Next-generation utility-first CSS framework configured natively through CSS `@import` rules and `@theme` configuration.

### 6. Code Quality & Tooling
*   **Biome**: Rust-powered, ultra-fast formatter and linter replacing ESLint, Prettier, and import sorting libraries, keeping code quality high with millisecond response times.

---

## 🗂️ Project Structure

The project code is organized logically under `src/`:

```
├── drizzle.config.ts        # Drizzle ORM migration configuration
├── biome.jsonc              # Biome linting and formatting configuration
├── package.json             # Workspace dependencies and scripts
└── src/
    ├── env.js               # Typesafe environment variable validation (using Zod)
    ├── styles/
    │   └── globals.css      # CSS styling Entrypoint using Tailwind CSS v4 & HeroUI Styles
    ├── app/
    │   ├── api/             # Next.js API Routes (tRPC & Better Auth handlers)
    │   ├── _components/     # Shared application components (e.g. Auth Buttons, Post widgets)
    │   ├── layout.tsx       # Root layout configuring Geist font and tRPC provider wrapper
    │   └── page.tsx         # Main entrypoint landing page
    ├── server/
    │   ├── api/             # tRPC Router definitions, context, and procedures (public vs protected)
    │   ├── better-auth/     # Better Auth configuration, client initialization, and server session helpers
    │   └── db/              # Drizzle PostgreSQL client initialization and relational database schema
    └── trpc/
        ├── query-client.ts  # TanStack React Query configuration
        ├── react.tsx        # tRPC Client Provider for React context
        └── server.ts        # Server-side tRPC caller helpers
```

---

## 🚀 Getting Started

Follow these steps to set up and run the application locally:

### 1. Clone & Install Dependencies

Ensure you have **pnpm v10** installed.

```bash
git clone <repository-url>
cd t3-inital
pnpm install
```

### 2. Configure Environment Variables

Create a `.env` file in the root of the project using the `.env.example` template:

```bash
cp .env.example .env
```

Ensure the variables are populated with your credentials:
```env
# Database connection string
DATABASE_URL="postgresql://postgres:password@localhost:5432/t3-initial"

# Better Auth Secret (Generate using: openssl rand -base64 32)
BETTER_AUTH_SECRET="your-32-byte-secret"

# GitHub OAuth credentials for authentication (Optional but configured)
BETTER_AUTH_GITHUB_CLIENT_ID="your-github-client-id"
BETTER_AUTH_GITHUB_CLIENT_SECRET="your-github-client-secret"
```

### 3. Set Up the Database

Make sure your PostgreSQL database is running, then execute migrations:

```bash
# Generate the migration files from schema
pnpm db:generate

# Apply the migrations to the database
pnpm db:migrate

# (Optional) Open Drizzle Studio to inspect and edit database tables
pnpm db:studio
```

### 4. Run the Development Server

Start the Next.js development server with Turbopack enabled:

```bash
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to view the application.

---

## 📜 Available Scripts

Run the following commands using `pnpm`:

| Command | Description |
| :--- | :--- |
| `pnpm dev` | Start development server with Turbopack (`next dev --turbo`). |
| `pnpm build` | Build the application for production. |
| `pnpm start` | Run the production-built application. |
| `pnpm preview` | Compile and preview the production build locally. |
| `pnpm check` | Run lint checks and code formatting verification using Biome. |
| `pnpm check:write` | Automatically lint, format, and organize imports in the project. |
| `pnpm typecheck` | Perform static TypeScript compilation check. |
| `pnpm db:generate` | Generate Drizzle migrations based on your schema. |
| `pnpm db:migrate` | Apply pending Drizzle migrations to database. |
| `pnpm db:push` | Safe alias for `pnpm db:generate && pnpm db:migrate` (preferred for PostgreSQL 17/18+ compatibility). |
| `pnpm db:studio` | Start the local database explorer GUI. |
