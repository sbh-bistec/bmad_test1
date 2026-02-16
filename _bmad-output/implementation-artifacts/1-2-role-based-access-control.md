# Story 1.2: Role-Based Access Control

Status: done

<!-- Note: Validation is optional. Run validate-create-story for quality check before dev-story. -->

## Story

As a System,
I want to differentiate between Team Lead and Intern roles,
so that users see appropriate data based on their permissions.

## Acceptance Criteria

1. **Given** Clerk authentication is working from Story 1.1
2. **When** a user logs in through SSO
3. **Then** the system determines if they are Team Lead or Intern
4. **And** Team Leads can see all interns and tasks
5. **And** Interns can only see their own tasks and communications
6. **And** role information is stored and accessible throughout the app

## Tasks / Subtasks

- [x] Task 1: Extend user authentication with role detection (AC: 1, 2, 3)
  - [x] Subtask 1.1: Implement role detection from Clerk user metadata
  - [x] Subtask 1.2: Create role context provider for app-wide access
  - [x] Subtask 1.3: Add role validation middleware
- [x] Task 2: Implement Team Lead visibility permissions (AC: 4)
  - [x] Subtask 2.1: Create Team Lead dashboard with full intern/task visibility
  - [x] Subtask 2.2: Implement GraphQL queries for all interns and tasks
  - [x] Subtask 2.3: Add role-based UI components for Team Leads
- [x] Task 3: Implement Intern visibility restrictions (AC: 5)
  - [x] Subtask 3.1: Create Intern dashboard with filtered task view
  - [x] Subtask 3.2: Implement GraphQL queries filtered by user_id
  - [x] Subtask 3.3: Add role-based UI restrictions for Interns
- [x] Task 4: Role information accessibility (AC: 6)
  - [x] Subtask 4.1: Implement role context throughout app components
  - [x] Subtask 4.2: Add role-based navigation and menu items
  - [x] Subtask 4.3: Create role validation utilities and hooks

## Dev Notes

### Critical Architecture Requirements
- **Clerk Integration**: Extend existing Clerk setup from Story 1.1 with role metadata
- **React Context**: Use RoleContext for app-wide role accessibility (follows architecture patterns)
- **GraphQL Authorization**: Implement role-based query filtering in resolvers
- **Component Organization**: Feature-based in src/components/features/ with role-specific components
- **Database Integration**: Leverage users table from Story 1.1 with role column

### Project Structure Notes
- **Alignment**: Extends Story 1.1 authentication foundation with role-based access
- **Components**: Create role-aware components in src/components/features/dashboard/
- **Context**: Add RoleContext alongside existing AuthContext
- **GraphQL**: Extend schemas with role-based query filtering
- **Database**: Use existing users table with role column for authorization

### Previous Story Intelligence (Story 1.1)
- **Authentication Foundation**: Clerk is fully configured with SSO integration
- **Database Schema**: users table exists with user_id, email, role, created_at columns
- **Middleware**: Authentication middleware is set up and working
- **Component Structure**: Auth components are in src/components/auth/
- **Environment**: Clerk environment variables are configured

### Role-Based Access Control Implementation
- **Role Detection**: Extract role from Clerk user metadata or database users table
- **Context Pattern**: Use React Context + useReducer following architecture decisions
- **Authorization Logic**: Implement at GraphQL resolver level for data filtering
- **UI Adaptation**: Role-based component rendering and navigation
- **Security**: Server-side validation in GraphQL resolvers, client-side UI adaptation

### GraphQL Schema Requirements
```graphql
# Extended user queries with role-based filtering
type Query {
  # Team Lead: Returns all users and tasks
  # Intern: Returns only own data
  users: [User!]!
  tasks(userId: ID): [Task!]!
  assignments(userId: ID): [Assignment!]!
}

type User {
  userId: ID!
  email: String!
  role: UserRole!
  createdAt: DateTime!
}

enum UserRole {
  TEAM_LEAD
  INTERN
}
```

### Component Architecture Requirements
- **RoleContext**: Global context providing user role and permissions
- **Dashboard Components**: Separate TeamLeadDashboard and InternDashboard components
- **Permission Gates**: HOC or hooks for role-based component rendering
- **Navigation**: Role-based menu items and route protection
- **Data Fetching**: Role-aware GraphQL queries and caching

### Security Implementation Details
- **Server-Side Authorization**: GraphQL resolvers enforce role-based data access
- **Client-Side Adaptation**: UI components adapt based on user role
- **Route Protection**: Middleware extends authentication with role validation
- **Data Filtering**: Database queries filtered by user_id for Interns
- **Audit Trail**: All role-based access decisions logged

### Database Integration Requirements
```sql
-- Extend existing users table from Story 1.1
-- Role column already exists: VARCHAR(50) CHECK (role IN ('team_lead', 'intern'))

-- Tasks table for role-based filtering
CREATE TABLE tasks (
  task_id SERIAL PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  description TEXT,
  status VARCHAR(50) NOT NULL,
  created_by INTEGER REFERENCES users(user_id),
  assigned_to INTEGER REFERENCES users(user_id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Assignments table for tracking task assignments
CREATE TABLE assignments (
  assignment_id SERIAL PRIMARY KEY,
  task_id INTEGER REFERENCES tasks(task_id),
  assigned_to INTEGER REFERENCES users(user_id),
  assigned_at TIMESTAMP DEFAULT NOW()
);
```

### WebSocket Event Integration
- **Role-Based Events**: WebSocket events filtered by user role
- **Event Types**: task.created, task.updated, assignment.completed (snake_case past tense)
- **Access Control**: Interns receive events only for their assigned tasks
- **Team Lead Events**: Team Leads receive events for all tasks and interns

### Performance Considerations
- **Query Optimization**: Role-based queries with proper database indexing
- **Caching Strategy**: Apollo Client caching with role-aware cache keys
- **Real-time Updates**: Efficient WebSocket filtering by role
- **Dashboard Loading**: Optimized queries for Team Lead dashboard (potentially large datasets)

### Testing Requirements
- **Unit Tests**: Role detection logic, context providers, permission utilities
- **Integration Tests**: GraphQL resolver authorization, component role rendering
- **E2E Tests**: Complete user journeys for both Team Lead and Intern roles
- **Security Tests**: Attempted access to unauthorized data should fail

### Error Handling Patterns
- **Authorization Errors**: GraphQL errors with user-friendly messages
- **Role Validation**: Clear error messages for missing or invalid roles
- **Fallback UI**: Graceful degradation when role detection fails
- **Logging**: Comprehensive logging for role-based access decisions

### Development Workflow Integration
- **Story Dependencies**: This story builds directly on Story 1.1 authentication foundation
- **Sequential Development**: Complete Story 1.1 before starting this story
- **Shared Components**: Reuse auth components from Story 1.1
- **Database Migration**: Extend existing database schema, don't modify Story 1.1 tables

### References
- [Source: _bmad-output/planning-artifacts/architecture.md#Authentication & Security]
- [Source: _bmad-output/planning-artifacts/architecture.md#API & Communication Patterns]
- [Source: _bmad-output/planning-artifacts/architecture.md#Implementation Patterns & Consistency Rules]
- [Source: _bmad-output/planning-artifacts/prd.md#Access Control]
- [Source: _bmad-output/planning-artifacts/epics.md#Story 1.2]
- [Source: _bmad-output/implementation-artifacts/1-1-clerk-authentication-integration.md]
- [Source: Clerk User Metadata Documentation - https://clerk.com/docs/reference/backend/user-metadata]

## Dev Agent Record

### Agent Model Used

Cascade (Penguin Alpha) - Advanced AI coding assistant

### Debug Log References

### Completion Notes List

✅ **Story 1.2 Implementation Complete** - All role-based access control features implemented:
- RoleContext provider with React hooks for app-wide role access
- Role validation middleware extending Clerk authentication
- Team Lead dashboard with full visibility of interns and tasks
- Intern dashboard with filtered task view
- Role-based permission utilities and guard components
- Database migrations for tasks and assignments tables
- API endpoints for user and users data retrieval
- Comprehensive test coverage for permissions and context
- All acceptance criteria satisfied (AC 1-6)

### File List

- src/contexts/RoleContext.tsx (Role context provider with React hooks)
- src/lib/permissions.ts (Role validation utilities and permission helpers)
- src/components/auth/RoleGuard.tsx (Permission gate component)
- src/components/features/dashboard/TeamLeadDashboard.tsx (Team Lead dashboard)
- src/components/features/dashboard/InternDashboard.tsx (Intern dashboard)
- src/components/features/dashboard/DashboardClient.tsx (Client dashboard wrapper)
- src/components/providers/ClientProviders.tsx (Client providers wrapper)
- src/app/dashboard/page.tsx (Updated to use role-based dashboards)
- src/app/layout.tsx (Updated to include RoleProvider)
- middleware.ts (Extended with role validation)
- src/app/api/user/route.ts (API endpoint for user data)
- src/app/api/users/route.ts (API endpoint for all users data with role authorization)
- src/app/api/tasks/route.ts (API endpoint for tasks with role-based filtering)
- src/app/api/dashboard/stats/route.ts (API endpoint for dashboard statistics)
- src/app/api/graphql/route.ts (GraphQL API endpoint)
- graphql/schema.ts (GraphQL schema definitions)
- graphql/resolvers.ts (GraphQL resolvers with role-based access control)
- db/migrations/002_create_tasks_table.sql (Tasks table migration)
- db/migrations/003_create_assignments_table.sql (Assignments table migration)
- tests/lib/permissions.test.ts (Permission utilities tests)
- tests/contexts/RoleContext.test.tsx (Role context tests)
