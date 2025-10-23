# Project Requirements Document (PRD): pasteblink

## 1. Project Overview

pasteblink is a starter template for building modern web applications with user accounts, a polished UI, and a solid back-end foundation. It bundles sign-up/sign-in flows, a protected dashboard, database connectivity, and a suite of reusable UI components. By providing this boilerplate, pasteblink lets development teams skip repetitive setup tasks and focus on delivering real features faster.

The core problem pasteblink solves is “time-to-market” and consistency. New projects often repeat the same setup steps—Next.js routing, authentication, theming, database integration, Docker configs, etc. pasteblink centralizes all these concerns in a well-structured, TypeScript-first codebase. Success is measured by how quickly a new team can clone the repo and have a working app, with secure login, dark-mode UI, and a ready-to-extend dashboard, within minutes.

## 2. In-Scope vs. Out-of-Scope

### In-Scope (v1)
- User authentication: sign-up, sign-in, sign-out flows via Better Auth.
- Protected dashboard: a post-login area with placeholder widgets and navigation sidebar.
- Theming: light/dark mode toggle using `next-themes` and CSS variables.
- UI component library: `shadcn/ui` components styled with Tailwind CSS.
- Database setup: PostgreSQL connection using Drizzle ORM, with schemas for users, sessions, and accounts.
- Project bootstrapping: Docker and Docker Compose configurations for local development.
- Code organization: Next.js App Router folder structure with `/app`, `/components`, `/lib`, and `/db`.

### Out-of-Scope (v1)
- Business-specific data models and dynamic content beyond authentication.
- Automated test suites (unit/integration).
- Role-based access control or multi-tenant logic.
- Advanced performance tuning (beyond basic image optimization).
- Production CI/CD pipelines or cloud deployment scripts.
- Third-party payment or external API integrations.

## 3. User Flow

A new visitor lands on the home page (root `/`). The site detects no active session and prompts the user to either sign up or sign in. Clicking “Sign Up” opens a form with email and password fields; on submission, the user’s credentials are securely sent to the `/app/api/auth/[...all]/route.ts` endpoint. After successful registration, Better Auth creates a session and redirects the user to `/dashboard`.

An authenticated user sees the dashboard layout: a left sidebar for navigation (Dashboard, Settings placeholder) and a main content area showing starter widgets (e.g., "Welcome, [Name]" card). At the top, a theme toggle button switches between light and dark modes instantly. Users can access sign-out from the sidebar. All API calls (if added later) would inherit authentication and pull dynamic data via Drizzle ORM.

## 4. Core Features

- **Authentication Module**: Full sign-up/sign-in/sign-out flows, session management, client- and server-side config using Better Auth.
- **Protected Dashboard**: Post-login landing area with a navigation sidebar and placeholder widgets for future expansion.
- **UI Component Library**: Headless, accessible components from `shadcn/ui`, styled with Tailwind CSS, supporting light/dark mode.
- **Theming**: Dark/light mode toggle backed by `next-themes` and CSS variables in `globals.css`.
- **Database Integration**: Drizzle ORM setup for PostgreSQL, with schema definitions for authentication entities.
- **Dev Containerization**: Docker and Docker Compose for spinning up the app and Postgres locally with a single command.
- **Next.js App Router**: File-based routing for pages, layouts, and colocated API routes in the `/app` directory.
- **TypeScript First**: End-to-end typing in frontend, backend, and database layers.

## 5. Tech Stack & Tools

- **Frontend**: Next.js (App Router), React, TypeScript, Tailwind CSS, shadcn/ui, next-themes.
- **Backend**: Next.js API Routes, TypeScript, Better Auth (authentication provider), Drizzle ORM.
- **Database**: PostgreSQL (via Docker), Drizzle ORM for type-safe schemas and queries.
- **Containerization**: Docker, Docker Compose for local development.
- **Code Editor & Plugins**: VS Code with Tailwind CSS IntelliSense, ESLint, Prettier. (Optional: Cursor or Windsurf for AI-assisted coding.)

## 6. Non-Functional Requirements

- **Performance**: Initial page load under 2 seconds on a standard broadband connection. Subsequent navigations and theme toggles in under 100ms.
- **Security**: HTTPS by default in production, HttpOnly secure cookies for sessions, input sanitization, OWASP Top 10 awareness.
- **Usability**: Accessible to WCAG 2.1 AA standards, keyboard navigable, visible focus states.
- **Scalability**: Architected to add new routes, components, and database models without major refactoring.
- **Maintainability**: Strict TypeScript usage, modular folder structure, and enforced linting/formatting.

## 7. Constraints & Assumptions

- Better Auth service and PostgreSQL must be reachable (container or cloud).
- Assumes developers have Docker installed and basic Git/Node.js environment.
- Drizzle ORM migrations will be managed manually or via custom scripts.
- No external API keys or secrets beyond environment variables for DB and Auth.

## 8. Known Issues & Potential Pitfalls

- **No Test Suite**: Lack of automated tests may hide regressions. Mitigation: add Jest and React Testing Library for core flows.
- **Theme Flicker**: Initial load may flash in wrong theme. Mitigation: preload preferred theme in `_document` or use `next-themes` SSR support.
- **ORM Migrations**: Drizzle migration scripts aren’t included. Mitigation: plan a migration strategy (e.g., drizzle-kit).
- **Docker Port Conflicts**: Developers may have local Postgres on default port. Mitigation: document how to change ports in `docker-compose.yml`.
- **Static Dashboard**: Current widgets are placeholders. Teams must implement their own data fetching and UI logic.

By following this PRD, the AI model or team can generate detailed technical guides (Tech Stack, Frontend Guidelines, Backend Structure, etc.) without ambiguity, ensuring a smooth path from boilerplate to feature-rich application.