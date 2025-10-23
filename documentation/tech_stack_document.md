# Tech Stack Document for pasteblink

This document explains the technology choices behind **pasteblink** in everyday language. It shows which tools we use, why we picked them, and how they all work together to build a fast, reliable, and easy-to-maintain web application.

## 1. Frontend Technologies

Our frontend (the part users see and interact with) is built using:  

- **Next.js (App Router)**  
  A modern React framework that gives us server-side rendering, file-based routing, and a clear folder structure. This means pages load faster and our code stays organized.  
- **TypeScript**  
  A superset of JavaScript that adds types. It catches mistakes early and makes the code easier to understand and refactor.  
- **React**  
  The core library for building user interfaces with reusable components.  
- **Tailwind CSS**  
  A utility-first styling tool. Instead of writing custom CSS for every element, we apply tiny, reusable classes directly in our HTML. This speeds up design and ensures a consistent look.  
- **shadcn/ui**  
  A collection of ready-made, accessible UI components built on top of Radix UI and Tailwind CSS. We customize these to fit pasteblink’s style, saving design and accessibility work.  
- **next-themes**  
  A simple way to switch between light and dark mode. It reads the user’s preference and applies the right color scheme instantly.  

Why these choices enhance user experience:  
- Server-side rendering (Next.js) leads to faster page loads and better SEO.  
- Tailwind and shadcn/ui let us build polished, consistent interfaces quickly.  
- Dark-mode support via next-themes meets modern user expectations.  
- TypeScript and React ensure our UI code is maintainable and scalable.

## 2. Backend Technologies

The backend (the part that handles data, authentication, and business logic) uses:  

- **Better Auth**  
  A hosted authentication service that manages sign-up, sign-in, password resets, and sessions. It gives us a secure, battle-tested system without building it from scratch.  
- **Drizzle ORM**  
  A lightweight, type-safe way to define and query our database. Drizzle checks our queries at compile time, reducing runtime errors.  
- **PostgreSQL**  
  A powerful, reliable relational database. It stores user accounts, session data, and any custom application data we add.  
- **TypeScript (server-side)**  
  Extends type safety to our backend code, making it easier to maintain and refactor.  
- **API Routes in Next.js**  
  We colocate backend routes inside the same project folder structure (`/app/api/auth/[...all]/route.ts`). This keeps related code together and streamlines full-stack development.

How these components work together:  
1. User submits login or sign-up form in the frontend.  
2. The request hits a Next.js API route, which delegates to Better Auth.  
3. Better Auth stores or verifies credentials in PostgreSQL via the Drizzle adapter.  
4. On success, a session cookie is issued and the user is redirected to the dashboard.  
5. All data fetching (user profile, future application data) goes through Drizzle queries to PostgreSQL.

## 3. Infrastructure and Deployment

To make development and deployment smooth, we use:  

- **Docker & Docker Compose**  
  We containerize the application and the PostgreSQL database. This ensures everyone on the team works in the same environment and simplifies deployment.  
- **Git** and **GitHub**  
  Version control for tracking code changes, collaboration, and code review.  
- **(Optional) CI/CD Pipelines (e.g., GitHub Actions)**  
  When code is pushed, automated tests and builds can run, and successful builds can be deployed to a hosting platform.  
- **Hosting Platform (e.g., Vercel, DigitalOcean)**  
  A cloud service to run the Docker containers or directly host the Next.js app. Vercel integrates seamlessly with Next.js for fast deployments and global content delivery.

These decisions ensure:  
- Consistent environments with Docker.  
- Easy collaboration and code history via Git/GitHub.  
- Fast, automated deployments with CI/CD.  
- High availability and performance using modern cloud hosting.

## 4. Third-Party Integrations

pasteblink relies on a few third-party services to extend functionality without reinventing the wheel:  

- **Better Auth** (authentication as a service)  
- **PostgreSQL** (hosted or managed database)  
- **(Optional) Analytics Tools** like Google Analytics or Plausible for tracking user behavior.  

Benefits of these integrations:  
- Better Auth removes the complexity and security risks of home-grown authentication.  
- A managed PostgreSQL service ensures backups, updates, and scaling are handled for us.  
- Analytics tools provide insight into user engagement, helping us improve the product.

## 5. Security and Performance Considerations

Security measures implemented:  

- **Secure Authentication** with Better Auth, including encrypted password storage, session management, and CSRF protection.  
- **HTTPS Everywhere** by default on hosting platforms to encrypt data in transit.  
- **Type Safety** (TypeScript + Drizzle ORM) reduces runtime errors that could expose vulnerabilities.  
- **Environment Variables** for secrets (API keys, database credentials), never checked into source control.  

Performance optimizations:  

- **Server-Side Rendering** for faster first-load times and SEO benefits.  
- **Automatic Code Splitting** by Next.js so users only download code they need.  
- **Image Optimization** using Next.js’s Image component to serve properly sized formats (WebP, etc.).  
- **Tailwind CSS Purge** to remove unused styles and keep CSS bundles small.  
- **Docker Caching** in CI pipelines to speed up rebuilds.  

## 6. Conclusion and Overall Tech Stack Summary

pasteblink brings together a set of modern, developer-friendly technologies that achieve three main goals:  

1. **Fast, polished user experiences** with Next.js, React, Tailwind CSS, and shadcn/ui.  
2. **Secure, scalable backend** using Better Auth, Drizzle ORM, and PostgreSQL.  
3. **Reliable and consistent deployments** through Docker, GitHub/Git, and cloud hosting (e.g., Vercel).  

Unique strengths of this tech stack:  

- **End-to-end Type Safety:** From the database schema to API routes to UI components.  
- **Integrated Full-Stack Framework:** Next.js App Router blurs the line between frontend and backend for seamless development.  
- **Rapid UI Development:** Utility-first styling and headless UI components mean less custom CSS and more focus on user experience.  

Together, these choices give you a solid foundation to build any modern web application—quickly, securely, and with a delightful developer experience.