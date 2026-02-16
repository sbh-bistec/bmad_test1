# Story 1.1: Clerk Authentication Integration

Status: done

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a User,
I want to authenticate using Company SSO through Clerk,
so that I can securely access the system without managing separate credentials.

## Acceptance Criteria

1. **Given** the Next.js project is initialized with `npx create-next-app@latest t2 --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"`
2. **When** I integrate Clerk authentication package
3. **Then** Clerk is configured with environment variables for SSO
4. **And** authentication middleware is set up
5. **And** login/logout functionality is available
6. **And** user sessions are properly managed
7. **And** protected routes redirect to authentication when needed
8. **And** users table is created with user_id, email, role, created_at columns

## Tasks / Subtasks

- [x] Task 1: Initialize Next.js project (AC: 1)
  - [x] Subtask 1.1: Run `npx create-next-app@latest t2 --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"`
  - [x] Subtask 1.2: Verify project structure matches architecture requirements
- [x] Task 2: Install and configure Clerk (AC: 2, 3)
  - [x] Subtask 2.1: Install `@clerk/nextjs` package
  - [x] Subtask 2.2: Set up Clerk environment variables (CLERK_PUBLISHABLE_KEY, CLERK_SECRET_KEY)
  - [x] Subtask 2.3: Configure ClerkProvider in app/layout.tsx
- [x] Task 3: Implement authentication middleware (AC: 4)
  - [x] Subtask 3.1: Create middleware.ts with Clerk authentication
  - [x] Subtask 3.2: Configure protected routes
- [x] Task 4: Create authentication components (AC: 5, 6)
  - [x] Subtask 4.1: Set up SignIn and SignUp components
  - [x] Subtask 4.2: Implement UserButton for profile/logout
  - [x] Subtask 4.3: Add SignedIn/SignedOut wrappers
- [x] Task 5: Database integration (AC: 8)
  - [x] Subtask 5.1: Create users table schema with snake_case naming
  - [x] Subtask 5.2: Set up user sync between Clerk and database
  - [x] Subtask 5.3: Implement role assignment logic

## Dev Notes

### Critical Architecture Requirements
- **Next.js 15 with App Router**: Must use exact initialization command from architecture
- **Clerk Integration**: Latest stable version with enterprise SSO support
- **Database Naming**: snake_case for tables (users, tasks, assignments)
- **Component Organization**: Feature-based in src/components/features/auth/
- **Middleware**: Authentication middleware for protected routes

### Project Structure Notes
- **Alignment**: Follows unified project structure with src/ directory
- **Components**: Create auth components in src/components/auth/
- **Middleware**: Place middleware.ts at project root
- **Environment**: Use .env.local for Clerk keys
- **Database**: PostgreSQL with users table schema

### Security Requirements
- **SSO Integration**: Company SSO through Clerk enterprise features
- **Role-Based Access**: Team Lead vs Intern permissions
- **Protected Routes**: Middleware-based route protection
- **Session Management**: Clerk handles session tokens

### Clerk Configuration Details
- **Provider Setup**: `<ClerkProvider>` wraps entire app in layout.tsx
- **Environment Variables**: CLERK_PUBLISHABLE_KEY, CLERK_SECRET_KEY
- **SSO Providers**: Configure in Clerk dashboard for enterprise
- **User Metadata**: Store role information in Clerk user metadata

### Database Schema Requirements
```sql
-- users table with snake_case naming
CREATE TABLE users (
  user_id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  role VARCHAR(50) NOT NULL CHECK (role IN ('team_lead', 'intern')),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### References
- [Source: _bmad-output/planning-artifacts/architecture.md#Technical Stack]
- [Source: _bmad-output/planning-artifacts/prd.md#Authentication Requirements]
- [Source: _bmad-output/planning-artifacts/epics.md#Story 1.1]
- [Source: Clerk Next.js Documentation - https://clerk.com/docs/nextjs/getting-started/quickstart]

## Dev Agent Record

### Agent Model Used

Cascade (Penguin Alpha) - Advanced AI coding assistant

### Debug Log References

### Completion Notes List

- **Middleware Fix**: Fixed critical Clerk middleware API issue by updating from `auth().protect()` to `await auth.protect()` with async function
- **Authentication Flow**: Complete Clerk integration with SignIn, SignUp, UserButton components
- **Database Integration**: User sync between Clerk and PostgreSQL database with role management, including webhook handler
- **Protected Routes**: Middleware properly configured to protect dashboard and other routes with redirect logic
- **Architecture Compliance**: Moved auth components to correct location (/src/components/features/auth/) per architecture decisions
- **Dependencies**: Added missing @clerk/nextjs and svix packages to package.json
- **Environment Setup**: Created .env.local with proper Clerk and database environment variable templates including webhook secret
- **Build Status**: Project builds successfully (requires Clerk environment variables for production)
- **All Acceptance Criteria Met**: All 8 acceptance criteria have been satisfied
- **Code Review Fixes Applied**: Fixed all HIGH and MEDIUM issues found during adversarial code review:
  - Fixed middleware import error and API usage
  - Created missing UserButton component
  - Added proper redirect logic for protected routes
  - Added database connection validation
  - Updated environment variables with webhook secret
  - Fixed generic metadata in layout.tsx
  - Updated File List to reflect actual implementation

### File List

- src/app/layout.tsx (ClerkProvider setup)
- middleware.ts (Authentication middleware with redirect logic)
- src/components/features/auth/ (Auth components - SignInPage.tsx, SignUpPage.tsx, UserButton.tsx)
- src/app/sign-in/page.tsx (Sign-in page)
- src/app/sign-up/page.tsx (Sign-up page)
- src/app/dashboard/page.tsx (Protected dashboard)
- src/lib/db.ts (Database connection, user sync, and validation)
- src/app/api/webhooks/clerk/route.ts (Clerk webhook handler)
- .env.local (Environment variables template with webhook secret)
- package.json (Dependencies including @clerk/nextjs, svix)
- db/migrations/001_create_users_table.sql (Database schema)
- src/components/providers/ClientProviders.tsx (Client-side providers)
- src/components/auth/RoleGuard.tsx (Role-based access control)
- src/components/features/dashboard/ (Dashboard components - DashboardClient.tsx, InternDashboard.tsx, TeamLeadDashboard.tsx)
