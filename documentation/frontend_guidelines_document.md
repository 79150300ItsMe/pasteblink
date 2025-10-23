# Frontend Guidelines Document for Pasteblink

This document outlines the frontend architecture, design principles, and technologies used in the Pasteblink starter template. It aims to give a clear, everyday-language overview so that anyone—designer, product manager, or developer—can understand the setup without a deep technical background.

## 1. Frontend Architecture

### 1.1 Overview
- **Framework:** Next.js (App Router) provides a file-based routing system, server-side rendering, and a clear separation between pages and API endpoints.  
- **Language:** TypeScript ensures type safety across the entire stack—from database schemas to UI components.  
- **Containerization:** Docker and Docker Compose simplify local setup by bundling the app and PostgreSQL database in containers.

### 1.2 Key Libraries & Services
- **Authentication:** Better Auth handles sign-up, sign-in, session management, and integrates with Drizzle ORM for user data in PostgreSQL.  
- **Database ORM:** Drizzle ORM offers a type-safe way to define schemas and run queries against PostgreSQL.  
- **Styling:** Tailwind CSS (utility-first) speeds up UI development and enforces consistent design.  
- **UI Components:** shadcn/ui (built on Radix UI + Tailwind) provides accessible, headless building blocks.  
- **Theming:** next-themes with CSS variables enables seamless light/dark mode switching.

### 1.3 Scalability, Maintainability & Performance
- **Component-Based:** Small, reusable React components live in `/components`, making it easy to add or modify features without touching unrelated code.  
- **File-Based Routing:** Adding a new page is as simple as creating a folder under `/app`.  
- **Type Safety:** TypeScript plus Drizzle’s type-safe ORM minimizes runtime errors and eases refactoring.  
- **Server Components:** Next.js’s server components offload heavy data processing to the server, resulting in faster initial loads.

## 2. Design Principles

### 2.1 Usability
- Clear, consistent navigation via a sidebar and header.  
- Familiar forms for sign-in/sign-up flows with in-field labels and immediate validation feedback (planned).  

### 2.2 Accessibility
- shadcn/ui follows WAI-ARIA standards.  
- Semantic HTML tags (e.g., `<nav>`, `<main>`, `<button>`) ensure screen-reader compatibility.  
- Keyboard-navigable UI elements and focus indicators.

### 2.3 Responsiveness
- Mobile-first design: breakpoints in Tailwind (`sm`, `md`, `lg`, `xl`) adjust layouts automatically.  
- Flexbox and CSS Grid utilities ensure content adapts gracefully to different screen sizes.

## 3. Styling and Theming

### 3.1 Styling Approach
- **Tailwind CSS:** Utility-first classes keep CSS close to markup, reducing context switching.  
- **No additional preprocessors:** All styling is handled through Tailwind’s configuration and CSS variables.

### 3.2 Theming
- **next-themes** toggles CSS variables defined in `globals.css` for light and dark modes.  
- Theme context is provided at the root (`/app/layout.tsx`), so all components automatically pick up the current theme.

### 3.3 Visual Style
- **Design Style:** Modern, flat design—clean edges, minimal shadows, and intuitive interactions.  
- **Glassmorphism Accents (optional):** Subtle translucent panels can be applied to modals or cards for a stylish overlay effect.

### 3.4 Color Palette
| Role          | Light Mode HEX | Dark Mode HEX |
|---------------|----------------|---------------|
| Primary       | #2563EB (blue-600)  | #3B82F6 (blue-500) |
| Secondary     | #6B7280 (gray-500) | #D1D5DB (gray-300) |
| Accent        | #14B8A6 (teal-500)  | #2DD4BF (teal-400) |
| Background    | #F9FAFB (gray-50)  | #1F2937 (gray-800) |
| Surface       | #FFFFFF            | #111827 (gray-900) |
| Error         | #DC2626 (red-600)   | #F87171 (red-400) |

### 3.5 Typography
- **Font Family:** Inter (sans-serif) for a clean, modern look.  
- **Font Weights:** 400 (regular), 500 (medium), 700 (bold) to ensure clear hierarchy.

## 4. Component Structure

### 4.1 Organization
- `/app`: Contains page folders (e.g., `sign-in`, `sign-up`, `dashboard`) and shared layout (`layout.tsx`).  
- `/components/ui`: Generated shadcn/ui components customized for this project.  
- `/components`: Project-specific components (sidebar, auth buttons, theme toggle).

### 4.2 Reusability & Maintenance
- Each component has a single responsibility (e.g., `AuthButtons` only handles sign-in/out UI).  
- Props are well-typed with TypeScript, making it easy to see what data a component expects.  
- Common logic lives in `/lib` (e.g., `auth-client.ts`), so UI code remains focused on presentation.

## 5. State Management

### 5.1 Local State
- React’s `useState` for component-specific state (e.g., form inputs, toggle states).

### 5.2 Shared State
- React Context API (wrapped in `/app/layout.tsx`) shares theme and user session data across the app.  
- For larger data needs (e.g., shopping carts, multi-step wizards), consider adding a lightweight store like Zustand or Jotai.

## 6. Routing and Navigation

### 6.1 Routing
- **Next.js App Router:** File-based. Place a `page.tsx` in `/app/dashboard` for the dashboard route. API routes live under `/app/api`.

### 6.2 Navigation
- The `AppSidebar` component in `/components/app-sidebar.tsx` renders links using Next.js’s `Link` component for client-side transitions.  
- Active link highlighting and nested menus can be added as needed.

## 7. Performance Optimization

### 7.1 Built-In Next.js Features
- **Automatic Code Splitting:** Only the code for the current page loads.  
- **Server Components:** Runs heavy logic on the server, reducing client bundle size.  
- **Image Optimization:** Next.js `<Image>` component auto-optimizes images for size and format.

### 7.2 Additional Strategies
- **Dynamic Imports:** Use `next/dynamic` to lazy-load rarely used components.  
- **Tree Shaking:** Ensure unused code is removed—Tailwind’s JIT mode does this for CSS.  
- **Caching & CDN:** Serve static assets from a CDN and leverage HTTP caching headers.

## 8. Testing and Quality Assurance

### 8.1 Unit Testing
- **Tools:** Jest + React Testing Library.  
- **Scope:** Test individual components, utility functions in `/lib`, and form validation logic.

### 8.2 Integration Testing
- **Tools:** React Testing Library + MSW (Mock Service Worker).  
- **Scope:** Test authentication flows, API routes in `/app/api/auth`, and interactions between components.

### 8.3 End-to-End Testing
- **Tools:** Cypress or Playwright.  
- **Scope:** Full sign-up → sign-in → dashboard flow, theme toggle, and navigation.

### 8.4 Linters & Formatters
- **ESLint:** Enforce code style and catch common errors.  
- **Prettier:** Auto-format code for consistency.  
- **Pre-commit Hooks:** Use Husky to run lint and tests before commits.

## 9. Conclusion and Overall Frontend Summary

The Pasteblink frontend is built on a solid, modern stack—Next.js with TypeScript, Tailwind CSS, shadcn/ui, Better Auth, and Drizzle ORM. Its component-driven architecture, utility-first styling, and server components ensure it’s scalable, maintainable, and performant. Clear design principles (usability, accessibility, responsiveness) guide every decision, while Docker offers a smooth developer experience. Testing strategies and linting tools keep code quality high. Together, these guidelines ensure that Pasteblink is not only a great starting point for any web app but also a foundation that teams can confidently build upon and extend.