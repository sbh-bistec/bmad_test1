---
title: 'Clerk Authentication Integration with Azure AD SSO'
slug: 'clerk-authentication-integration'
created: '2026-02-18T06:01:00.000Z'
status: 'in-progress'
stepsCompleted: [1]
tech_stack: ['Next.js 16', 'Clerk', 'Microsoft Azure AD', 'PostgreSQL', 'Vercel Postgres']
files_to_modify: ['.env.local', 'middleware.ts', 'src/lib/db.ts', 'src/app/layout.tsx']
code_patterns: ['Clerk Provider pattern', 'Middleware authentication', 'Database sync pattern']
test_patterns: ['Authentication flow tests', 'Role-based access tests']
---

# Tech-Spec: Clerk Authentication Integration with Azure AD SSO

**Created:** 2026-02-18T06:01:00.000Z

## Overview

### Problem Statement

Need to integrate Microsoft Azure AD SSO with Clerk authentication and establish proper user sync with role-based access control using email domain mapping.

### Solution

Configure Clerk with Azure AD SSO provider, implement user sync from Clerk to database, assign roles based on email domains, and set up webhook handling for real-time updates.

### Scope

**In Scope:**
- Microsoft Azure AD SSO integration with Clerk
- User sync from Clerk to PostgreSQL database 
- Role assignment based on email domains
- Clerk webhook handling for real-time user updates
- Environment variables setup
- Authentication middleware with proper route protection
- Protection for /dashboard, /tasks, /dashboard/team-lead, and /admin routes

**Out of Scope:**
- Advanced role management UI (manual admin assignment)
- Multi-provider SSO (only Azure AD)
- Custom authentication flows outside Clerk

## Context for Development

### Codebase Patterns

- **Clerk Integration**: Already using @clerk/nextjs with ClerkProvider in layout.tsx
- **Middleware Pattern**: Authentication middleware with role-based route protection
- **Database Pattern**: Vercel Postgres with SQL queries for user management
- **Role System**: 'team_lead' and 'intern' roles with permission-based access

### Files to Reference

| File | Purpose |
| ---- | ------- |
| `src/app/layout.tsx` | Root layout with ClerkProvider setup |
| `middleware.ts` | Authentication middleware with role checks |
| `src/lib/db.ts` | Database functions for user sync |
| `package.json` | Dependencies including @clerk/nextjs and svix |
| `src/lib/permissions.ts` | Role-based permission system |

### Technical Decisions

- Use Clerk's built-in Azure AD SSO integration
- Implement email domain-based role assignment (e.g., @company.com = team_lead, @intern.company.com = intern)
- Consolidate syncUserWithDatabase and syncUserFromClerk into single function
- Use Clerk webhooks with Svix for real-time user updates
- Protect routes at middleware level for security

## Implementation Plan

### Tasks

1. **Environment Configuration**
   - Set up Clerk environment variables for Azure AD
   - Configure database connection string
   - Set up webhook secret for Clerk

2. **Azure AD SSO Configuration**
   - Configure Clerk dashboard with Azure AD provider
   - Set up proper redirect URIs
   - Test SSO flow

3. **Database Schema Updates**
   - Ensure users table supports Clerk user ID mapping
   - Add role assignment logic based on email domain

4. **User Sync Implementation**
   - Consolidate sync functions in db.ts
   - Implement email domain role mapping
   - Add error handling for sync failures

5. **Webhook Handler**
   - Create webhook endpoint for Clerk events
   - Handle user.created, user.updated, user.deleted events
   - Implement proper security validation

6. **Route Protection**
   - Update middleware to protect /dashboard and /tasks routes
   - Ensure proper role-based access control
   - Add comprehensive error handling

### Acceptance Criteria

**Given** a user accesses the application
**When** they click sign-in with Microsoft
**Then** they are redirected to Azure AD login
**And** upon successful authentication, they are redirected back to the application
**And** their user account is created in the database with appropriate role based on email domain

**Given** an existing user with @company.com email
**When** they authenticate via Azure AD SSO
**Then** they are assigned 'team_lead' role in the database
**And** can access /dashboard/team-lead routes

**Given** an existing user with @intern.company.com email
**When** they authenticate via Azure AD SSO
**Then** they are assigned 'intern' role in the database
**And** can access /dashboard and /tasks routes but not /dashboard/team-lead

**Given** Clerk sends a webhook event
**When** the webhook endpoint receives the event
**Then** the user data is synchronized with the database
**And** role assignments are updated if email domain changes

**Given** an unauthenticated user tries to access protected routes
**When** they visit /dashboard, /tasks, /dashboard/team-lead, or /admin
**Then** they are redirected to sign-in page
**And** after authentication, they are redirected to their originally requested page

## Additional Context

### Dependencies

- **@clerk/nextjs**: Already installed, version 6.0.0
- **@vercel/postgres**: Already installed for database operations
- **svix**: Already installed for webhook handling
- **Microsoft Azure AD**: SSO provider configuration required

### Testing Strategy

- Unit tests for user sync functions
- Integration tests for authentication flow
- Role-based access control tests
- Webhook event handling tests
- End-to-end tests for complete SSO flow

### Notes

- Email domain mapping should be configurable for flexibility
- Consider adding fallback role assignment for unknown domains
- Implement proper logging for debugging authentication issues
- Ensure database connection pooling for performance
