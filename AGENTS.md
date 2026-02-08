# Agent Instructions for Link Shortener Project

## Overview

This document serves as the main entry point for AI agents working on this Next.js-based link shortener application. All coding standards, architectural patterns, and best practices are documented here and in the `/docs` directory.

## Project Stack

- **Framework**: Next.js 16.1.6 (App Router)
- **Language**: TypeScript 5 (strict mode)
- **Styling**: Tailwind CSS 4
- **Authentication**: Clerk
- **Database**: Neon PostgreSQL (serverless)
- **ORM**: Drizzle ORM
- **Runtime**: React 19.2.3

## Important

For detailed instructions on specific topics, refer to these the modular documentation inside /docs directory . ALWAYS refer to the relvant .md file BEFORE generating any code .

## Quick Reference

- [Routing & Auth Flow](./docs/routing-auth-flow.md) - Route protection, redirects, and Clerk modal configuration
- [TypeScript & Code Standards](./docs/typescript-standards.md) - TypeScript configuration, naming conventions, and code style
- [Next.js Architecture](./docs/nextjs-architecture.md) - App Router patterns, server/client components, routing
- [Database & ORM](./docs/database-orm.md) - Drizzle ORM patterns, schema design, migrations
- [Authentication](./docs/authentication.md) - Clerk integration patterns and user management
- [Styling Guidelines](./docs/styling.md) - Tailwind CSS usage, theming, and component styling
- [API & Server Actions](./docs/api-server-actions.md) - Server Actions, API routes, and data fetching
- [Testing & Quality](./docs/testing-quality.md) - Testing standards and code quality requirements

## Core Principles

### 1. Type Safety First
- **NEVER** use `any` type without explicit justification
- Prefer interfaces over types for object shapes
- Use strict TypeScript configuration (already enabled)
- Validate all external data with proper type guards

### 2. Server-First Architecture
- Default to Server Components unless interactivity is required
- Use Server Actions for data mutations
- Minimize client-side JavaScript bundle
- Leverage React Server Components for data fetching

### 3. Performance & Optimization
- Use Next.js Image component for all images
- Implement proper loading states
- Optimize database queries (avoid N+1 problems)
- Use Suspense boundaries appropriately

### 4. Security
- Never expose sensitive environment variables to client
- Validate and sanitize all user inputs
- Use Clerk's authentication checks consistently
- Implement proper CSRF protection for Server Actions

### 5. Code Quality
- Follow ESLint configuration (eslint-config-next)
- Write self-documenting code with clear naming
- Keep components small and focused
- Extract reusable logic into custom hooks or utilities

## File Structure Conventions

```
app/
  ├── (routes)/          # Route groups for organization
  ├── api/              # API routes (if needed)
  ├── components/       # Shared components
  │   ├── ui/          # UI primitives
  │   └── features/    # Feature-specific components
  ├── lib/             # Utility functions, helpers
  ├── actions/         # Server Actions
  └── types/           # Shared TypeScript types
db/
  ├── schema.ts        # Drizzle schema definitions
  ├── index.ts         # Database client initialization
  └── migrations/      # Database migration files
public/              # Static assets
docs/                # Agent instruction files
```

## Development Workflow

1. **Before Making Changes**: Always review relevant documentation in `/docs`
2. **Code Changes**: Follow the patterns established in existing code
3. **Database Changes**: Create migrations using Drizzle Kit
4. **Testing**: Test both server and client components appropriately
5. **Commit**: Ensure ESLint passes before committing

## Path Aliases

Use the `@/` alias for imports:

```typescript
// ✅ Good
import { db } from '@/db';
import { Button } from '@/app/components/ui/button';

// ❌ Avoid
import { db } from '../../../db';
```

## Environment Variables

Required environment variables:

- `DATABASE_URL` - Neon PostgreSQL connection string
- `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` - Clerk publishable key
- `CLERK_SECRET_KEY` - Clerk secret key

**Important**: Only prefix with `NEXT_PUBLIC_` if the variable must be exposed to the browser.

## Common Tasks

### Adding a New Feature
1. Read [Next.js Architecture](./docs/nextjs-architecture.md)
2. Read [Database & ORM](./docs/database-orm.md) if database changes are needed
3. Create necessary schema updates
4. Implement Server Actions for mutations
5. Build UI components following styling guidelines

### Modifying Database Schema
1. Read [Database & ORM](./docs/database-orm.md)
2. Update `db/schema.ts`
3. Generate migration: `npm run db:generate`
4. Review generated migration
5. Apply migration: `npm run db:push` or `npm run db:migrate`

### Adding Authentication Logic
1. Read [Authentication](./docs/authentication.md)
2. Use Clerk hooks/components as documented
3. Implement proper auth checks in Server Actions
4. Handle both authenticated and unauthenticated states

## Error Handling

- Use proper error boundaries for React errors
- Return typed errors from Server Actions
- Log errors appropriately (avoid logging sensitive data)
- Provide user-friendly error messages

## Performance Guidelines

- Minimize use of `'use client'` directive
- Use dynamic imports for heavy client components
- Implement pagination for large data sets
- Cache database queries when appropriate
- Use Next.js built-in optimizations (Image, Font, etc.)

## Accessibility

- Use semantic HTML elements
- Provide proper ARIA labels
- Ensure keyboard navigation works
- Test with screen readers when possible
- Maintain sufficient color contrast

## Questions or Clarifications

If you encounter ambiguity or need clarification on any coding standard:
1. Check the relevant documentation in `/docs`
2. Look for existing patterns in the codebase
3. Prefer consistency with existing code
4. When in doubt, favor simplicity and readability

## Updates to These Instructions

These instructions are living documents. When patterns evolve:
1. Update the relevant documentation file
2. Ensure consistency across all instruction files
3. Update this main file if new categories are added
