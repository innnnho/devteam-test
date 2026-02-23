# Project Conventions

## Workflow Principles

### Plan Before Building
- For ANY non-trivial task (3+ steps or architectural decisions), write a plan to `tasks/todo.md` with checkable items BEFORE writing code
- If something goes sideways mid-implementation, STOP and re-plan immediately — don't keep pushing a broken approach
- Write detailed specs upfront to reduce ambiguity
- Check in on the plan before starting implementation

### Verification Before Done
- Never consider a task complete without proving it works
- Run tests, check logs, demonstrate correctness
- Diff behavior between main and your changes when relevant
- Ask: "Would a staff engineer approve this?"
- If tests exist, they MUST pass before committing

### Autonomous Bug Fixing
- When given a bug report: just fix it — don't ask for hand-holding
- Point at logs, errors, failing tests — then resolve them
- Go fix failing CI tests without being told how
- Zero context switching required from the user

### Core Principles
- **Simplicity First**: Make every change as simple as possible. Impact minimal code.
- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards.
- **Minimal Impact**: Changes should only touch what's necessary. Avoid introducing bugs.
- **Demand Elegance (Balanced)**: For non-trivial changes, pause and ask "is there a more elegant way?" Skip this for simple, obvious fixes — don't over-engineer.

### Task Tracking
1. **Plan First**: Write plan to `tasks/todo.md` with checkable items
2. **Track Progress**: Mark items complete as you go
3. **Explain Changes**: High-level summary at each step
4. **Document Results**: Add a review section to `tasks/todo.md`
5. **Capture Lessons**: After any correction, update `tasks/lessons.md` with the pattern so the same mistake is never repeated

---

## Tech Stack (MUST follow — do not deviate)

- **Language**: TypeScript (strict mode, no `any` types)
- **Runtime**: Node.js 22+
- **Package Manager**: pnpm (never npm or yarn)
- **Backend**: Hono framework on Node.js
- **Frontend**: Next.js 14+ with App Router (no Pages Router)
- **Database**: Supabase (PostgreSQL via Supabase JS client)
- **ORM**: Drizzle ORM with `drizzle-kit` for migrations
- **Auth**: Supabase Auth (email/password + OAuth)
- **Styling**: Tailwind CSS v4 (utility classes only, no custom CSS files)
- **Testing**: Vitest for unit/integration, Playwright for E2E
- **Linting**: Biome (not ESLint or Prettier)
- **Validation**: Zod for all input validation and API schemas

## Architecture Rules

- Use Server Components by default; only add "use client" when needed
- All API routes go in `src/app/api/` using Next.js Route Handlers
- Database queries go through Drizzle ORM, never raw SQL strings
- All environment variables accessed via `src/lib/env.ts` with Zod validation
- Error handling: use Result pattern (never throw in business logic)

## File Organization

```
src/
  app/              # Next.js App Router pages and layouts
    api/            # API route handlers
  components/       # Reusable UI components
    ui/             # Primitive UI components (buttons, inputs)
  lib/              # Shared utilities and configurations
    db/             # Drizzle schema, client, and migrations
    supabase/       # Supabase client setup
    env.ts          # Environment variable validation
  types/            # Shared TypeScript types
tasks/
  todo.md           # Current task plan with checkable items
  lessons.md        # Accumulated lessons from past corrections
tests/
  unit/             # Vitest unit tests
  e2e/              # Playwright E2E tests
  fixtures/         # Test data
```

## Naming Conventions

- Files: `kebab-case.ts` (e.g., `user-service.ts`)
- Components: `PascalCase.tsx` (e.g., `UserProfile.tsx`)
- Functions and variables: `camelCase`
- Types and interfaces: `PascalCase` with descriptive names
- Database tables: `snake_case` (e.g., `user_profiles`)
- API routes: `kebab-case` (e.g., `/api/user-profiles`)

## Code Quality Rules

- Every exported function MUST have JSDoc comments
- Every API endpoint MUST validate input with Zod
- Every new feature MUST include at least one test
- Maximum function length: 30 lines (extract helpers)
- No default exports except for Next.js pages/layouts
- Use `const` by default; `let` only when reassignment is needed
- Prefer early returns over deep nesting
- Challenge your own work before presenting it — if a fix feels hacky, implement the elegant solution

## Git Commit Messages

Follow Conventional Commits:
- `feat:` new feature
- `fix:` bug fix
- `refactor:` code restructuring
- `test:` adding/updating tests
- `docs:` documentation only
- `chore:` build/config changes

## Supabase Integration

- Use `@supabase/supabase-js` for client-side queries
- Use `@supabase/ssr` for server-side in Next.js
- Row Level Security (RLS) MUST be enabled on all tables
- Never expose `service_role` key to the client
- Database schema changes go through Drizzle migrations

## Testing Requirements

- Unit tests: test business logic in isolation (mock Supabase client)
- Integration tests: test API routes with real (test) database
- Test file naming: `*.test.ts` colocated with source or in `tests/`
- Minimum: every public function gets a happy-path test
- Tests MUST pass before any task is considered complete
