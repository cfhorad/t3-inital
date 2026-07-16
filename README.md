# RadCases: Radiology Teaching Cases & Image Library 🩻

A high-performance, type-safe web application designed for medical students to study radiologic cases and teaching images. Built with Next.js 16, React 19, Tailwind CSS v4, tRPC, Drizzle ORM, Better Auth, and UploadThing.

---

## 🚀 Key Features

1.  **Role-Based Access Control (RBAC)**
    *   **Student (Default):** Can browse cases, search/filter, write comments, toggle likes, and give 1-3 star quality ratings.
    *   **Teacher:** All Student features + upload radiologic images, write detailed teaching notes, assign difficulty levels, and classify cases using ACR Codes.
    *   **Admin:** All Teacher features + manage users and edit their roles via the Admin Dashboard.
2.  **Test vs. Answer Images**
    *   Each case contains multiple images categorized as either **Test Images** (shown immediately for self-assessment) or **Answer Images** (revealed alongside the diagnosis and notes).
3.  **Strict ACR Categorization**
    *   Dropdown-based classification system mapping Cases to standard **ACR Anatomy** (e.g., *06 Lung, Mediastinum and Pleura*) and **ACR Pathology** (e.g., *Neoplasm*, *Inflammation/Infection*).
4.  **High-Performance Image Storage**
    *   Uses **UploadThing** for seamless, type-safe direct-to-cloud file uploads, removing the need for manual S3 or R2 bucket configuration.
5.  **Quick Search & Filter**
    *   Filter by ACR Anatomy, ACR Pathology, Difficulty (`Beginner`, `Intermediate`, `Advanced`), or type a keyword to instantly query case notes and titles.

---

## 🛠️ Stack & Dependencies

*   **Frontend:** Next.js 16 (App Router), React 19.2, HeroUI v3 (UI components), Tailwind CSS v4.
*   **Backend API:** tRPC v11 (typesafe API endpoints), TanStack React Query v5.
*   **Database:** PostgreSQL (Neon Serverless / local instance) with Drizzle ORM.
*   **Auth:** Better Auth (with Drizzle adapter).
*   **Storage:** UploadThing.
*   **Tooling:** Biome (formatter & linter).

---

## 📦 Getting Started

### 1. Installation
Install dependencies using `pnpm`:
```bash
pnpm install
```

### 2. Configure Environment Variables
Copy `.env.example` to `.env` and fill in the required variables:
```bash
cp .env.example .env
```

Your `.env` should contain:
```env
# Database
DATABASE_URL="postgresql://postgres:password@localhost:5432/rad_cases"

# Better Auth Secret
BETTER_AUTH_SECRET="your-32-byte-secret"
BETTER_AUTH_GITHUB_CLIENT_ID="your-github-client-id"
BETTER_AUTH_GITHUB_CLIENT_SECRET="your-github-client-secret"

# UploadThing
UPLOADTHING_SECRET="your-uploadthing-secret"
UPLOADTHING_APP_ID="your-uploadthing-app-id"
```

*Tip: Generate the `BETTER_AUTH_SECRET` by running:*
```bash
openssl rand -base64 32
```

### 3. Initialize the Database
If you use Docker, run the script to spin up a PostgreSQL instance:
```bash
chmod +x start-database.sh
./start-database.sh
```

Apply database migrations:
```bash
pnpm db:push
```

Seed initial ACR Anatomy and Pathology data:
```bash
pnpm db:seed
```

### 4. Run Development Server
```bash
pnpm dev
```
Open [http://localhost:3000](http://localhost:3000) to view the application.

---

## 📜 Available Scripts

| Command | Description |
| :--- | :--- |
| `pnpm dev` | Start development server with Turbopack. |
| `pnpm build` | Build the application for production. |
| `pnpm check` | Run lint checks and code formatting verification using Biome. |
| `pnpm check:write` | Automatically lint, format, and organize imports. |
| `pnpm db:generate` | Generate Drizzle migrations. |
| `pnpm db:migrate` | Apply pending Drizzle migrations. |
| `pnpm db:push` | Fast-sync database schema with codebase definitions. |
| `pnpm db:studio` | Open database explorer GUI. |
