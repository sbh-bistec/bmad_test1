---
stepsCompleted: [1, 2, 3]
inputDocuments: ['_bmad-output/planning-artifacts/prd.md', '_bmad-output/planning-artifacts/architecture.md']
---

# t2 - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for t2, decomposing the requirements from the PRD, UX Design if it exists, and Architecture requirements into implementable stories.

## Requirements Inventory

### Functional Requirements

FR1: Team Lead must be able to create tasks with descriptions and expected outcomes
FR2: Team Lead must be able to assign tasks to specific interns
FR3: Interns must be able to view their assigned tasks with clear priorities
FR4: Interns must be able to update task status (To Do → In Progress → Done)
FR5: Team Lead must be able to view dashboard showing all interns' task status at a glance
FR6: System must send email notifications when tasks are completed
FR7: Team Lead must be able to add and remove intern accounts
FR8: Interns must only see their own tasks and communications
FR9: Team Leads must have full visibility across all tasks and interns
FR10: System must support Company SSO authentication
FR11: Intern data must be stored permanently in database for future reference
FR12: System must support cohort transitions with data archival
FR13: Multiple Team Leads must share visibility across all interns
FR14: System must provide task history and audit trails
FR15: System must support real-time task status updates without page reloads
FR16: System must handle WebSocket connections for instant updates
FR17: System must support role-based access control (Team Lead vs Intern)
FR18: System must provide progressive enhancement for older browser support
FR19: Users must be able to ask questions and provide comments on tasks
FR20: Team Leads must be able to review and approve submitted tasks with inline feedback
FR21: Team Leads must be able to return tasks for revision with specific feedback
FR22: Interns must be able to view detailed task information with expectations and deadlines
FR23: System must support granular status transitions (To Do → In Progress → Submitted → Approved/Returned)

### NonFunctional Requirements

NFR1: Dashboard must load under 2 seconds for optimal user experience
NFR2: Real-time task status updates must occur within 2 seconds across users
NFR3: System must achieve minimal unplanned downtime with fast recovery
NFR4: System must handle graceful error handling and database backups
NFR5: Initial page load time must be under 3 seconds
NFR6: Navigation between views must be instant (no page reloads)
NFR7: System must support modern browsers (Chrome, Edge, Firefox, Safari)
NFR8: System must provide basic WCAG compliance for internal tools
NFR9: All functionality must be accessible via keyboard navigation
NFR10: System must support screen readers with semantic HTML and ARIA labels
NFR11: Color contrast must meet minimum ratios for text readability
NFR12: System must maintain reliable data persistence with no lost tasks or progress
NFR13: System must be work-critical with careful uptime management

### Additional Requirements

- Project must use Next.js 15 with App Router as starter template: `npx create-next-app@latest t2 --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"`
- Database must be PostgreSQL 17.7 with snake_case naming conventions
- Authentication must use Clerk for enterprise SSO integration
- API must use GraphQL with Apollo Server for flexible data fetching
- Real-time communication must use Next.js WebSocket API
- State management must use React Context + useReducer pattern
- Deployment must use Vercel platform
- Project structure must follow feature-based organization in src/components/features/
- GraphQL schemas must be organized by feature in separate files
- WebSocket events must use snake_case past tense naming (task.created, user.updated)
- Components must use PascalCase naming with feature-based contexts
- Database tables must use snake_case plural naming (users, tasks, assignments, cohorts)
- GraphQL types must use PascalCase (User, Task, Assignment)
- GraphQL fields must use camelCase (firstName, taskStatus, createdAt)
- Code must use camelCase for functions and variables
- Tests must be co-located with components (*.test.ts)
- Error handling must use GraphQL errors with React Error Boundaries
- Loading states must follow isLoading, isError, isSuccess pattern

### FR Coverage Map

FR1: Epic 2 - Team Lead task creation with descriptions
FR2: Epic 2 - Team Lead task assignment to specific interns  
FR3: Epic 2 - Intern view of assigned tasks with priorities
FR4: Epic 3 - Intern task status updates (To Do → In Progress → Done)
FR5: Epic 3 - Team Lead dashboard view of all intern status
FR6: Epic 4 - Email notifications for task completion
FR7: Epic 5 - Team Lead intern account management (add/remove)
FR8: Epic 1 - Intern visibility restriction to own tasks only
FR9: Epic 1 - Team Lead full visibility across all tasks and interns
FR10: Epic 1 - Company SSO authentication integration
FR11: Epic 5 - Permanent intern data storage in database
FR12: Epic 5 - Cohort transition support with data archival
FR13: Epic 5 - Multiple Team Lead shared visibility
FR14: Epic 6 - Task history and audit trail maintenance
FR15: Epic 4 - Real-time task status updates without page reloads
FR16: Epic 4 - WebSocket connection handling for instant updates
FR17: Epic 1 - Role-based access control (Team Lead vs Intern)
FR18: Epic 3 - Progressive enhancement for older browser support
FR19: Epic 2 - Question and comment threads on tasks
FR20: Epic 3 - Review and approval workflow with inline feedback
FR21: Epic 3 - Task return for revision with specific feedback
FR22: Epic 2 - Detailed task view with expectations and deadlines
FR23: Epic 3 - Granular status transitions (Submitted → Approved/Returned)

## Epic List

### Epic 1: Foundation & Authentication
Team Leads and Interns can securely access the system with appropriate roles and permissions
**FRs covered:** FR8, FR9, FR10, FR17

### Epic 2: Task Creation & Assignment
Team Leads can create tasks with clear descriptions, assign them to specific interns, and manage task communication
**FRs covered:** FR1, FR2, FR3, FR19, FR22

### Epic 3: Task Status Management & Dashboard
Interns can update task progress, Team Leads can review work, and all users can view status at a glance
**FRs covered:** FR4, FR5, FR18, FR20, FR21, FR23

### Epic 4: Real-time Communication & Notifications
Users receive instant updates and email notifications for task changes
**FRs covered:** FR6, FR15, FR16

### Epic 5: User Administration & Cohort Management
Team Leads can manage intern accounts and handle cohort transitions with data preservation
**FRs covered:** FR7, FR11, FR12, FR13

### Epic 6: Task History & Audit Trail
Complete task history and audit trails are maintained for compliance and performance tracking
**FRs covered:** FR14

## Epic 1: Foundation & Authentication

Team Leads and Interns can securely access the system with appropriate roles and permissions
**FRs covered:** FR8, FR9, FR10, FR17

### Story 1.1: Clerk Authentication Integration

As a User,
I want to authenticate using Company SSO through Clerk,
So that I can securely access the system without managing separate credentials.

**Acceptance Criteria:**

**Given** the Next.js project is initialized
**When** I integrate Clerk authentication package
**Then** Clerk is configured with environment variables for SSO
**And** authentication middleware is set up
**And** login/logout functionality is available
**And** user sessions are properly managed
**And** protected routes redirect to authentication when needed
**And** users table is created with user_id, email, role, created_at columns

---

### Story 1.2: Role-Based Access Control

As a System,
I want to differentiate between Team Lead and Intern roles,
So that users see appropriate data based on their permissions.

**Acceptance Criteria:**

**Given** Clerk authentication is working
**When** a user logs in through SSO
**Then** the system determines if they are Team Lead or Intern
**And** Team Leads can see all interns and tasks
**And** Interns can only see their own tasks and communications
**And** role information is stored and accessible throughout the app

---

### Story 1.3: User Profile Management

As a User,
I want to view and manage my basic profile information,
So that I can confirm my identity and role in the system.

**Acceptance Criteria:**

**Given** I am authenticated and have a role assigned
**When** I access my profile
**Then** I can see my name, email, and role (Team Lead/Intern)
**And** Team Leads see additional system status information
**And** Interns see only their personal information
**And** profile data respects role-based visibility rules

## Epic 2: Task Creation & Assignment

Team Leads can create tasks with clear descriptions, assign them to specific interns, and manage task communication
**FRs covered:** FR1, FR2, FR3, FR19, FR22

### Story 2.1: Task Creation Interface

As a Team Lead,
I want to create tasks with clear descriptions and expected outcomes,
So that I can assign meaningful work to interns.

**Acceptance Criteria:**

**Given** I am authenticated as a Team Lead
**When** I access the task creation form
**Then** I can enter task title and description
**And** I can specify expected outcomes and requirements
**And** I can set deadlines and priority levels
**And** the form validates required fields
**And** tasks table is created with task_id, title, description, status, created_by, created_at columns
**And** tasks are saved to the database with my user_id as created_by
**And** I receive confirmation when task is created successfully

---

### Story 2.2: Task Assignment System

As a Team Lead,
I want to assign created tasks to specific interns,
So that work is distributed appropriately across the team.

**Acceptance Criteria:**

**Given** I have created tasks and am authenticated as a Team Lead
**When** I view the task assignment interface
**Then** I can see all unassigned tasks
**And** I can see all available interns with their current workload
**And** I can assign a task to a specific intern
**And** assignments table is created with assignment_id, task_id, assigned_to, assigned_at columns
**And** the assignment is recorded in the assignments table
**And** the intern receives notification of the new assignment

---

### Story 2.3: Intern Task View

As an Intern,
I want to view my assigned tasks with clear priorities,
So that I know what work I need to complete and in what order.

**Acceptance Criteria:**

**Given** I am authenticated as an Intern
**When** I access my task dashboard
**Then** I can see all tasks assigned to me
**And** tasks are displayed with title, description, and current status
**And** tasks are prioritized by assignment date or priority level
**And** I can only see my own assigned tasks (FR8 compliance)
**And** task status shows To Do, In Progress, Submitted, Approved, or Returned options

---

### Story 2.4: Task Question Thread

As an Intern,
I want to ask questions and provide comments on tasks,
So that I can get clarification and communicate about my work.

**Acceptance Criteria:**

**Given** I am authenticated as an Intern and have assigned tasks
**When** I view a task detail
**Then** I can see a question/comment thread for the task
**And** I can post questions about requirements or progress
**And** I can provide status updates in comments
**And** Team Leads can see and respond to my questions
**And** all comments are timestamped and attributed to the author
**And** I receive notifications when Team Leads respond to my questions

---

### Story 2.5: Detailed Task View

As an Intern,
I want to view detailed task information with expectations and deadlines,
So that I understand exactly what is expected and when it's due.

**Acceptance Criteria:**

**Given** I am authenticated as an Intern
**When** I click on an assigned task
**Then** I can see the complete task description and requirements
**And** I can see the expected outcomes and deliverables
**And** I can see the deadline and priority level
**And** I can see who assigned the task and when
**And** I can view the question/comment thread for the task
**And** I can see the current status and available status transitions

## Epic 3: Task Status Management & Dashboard

Interns can update task progress, Team Leads can review work, and all users can view status at a glance
**FRs covered:** FR4, FR5, FR18, FR20, FR21, FR23

### Story 3.1: Task Status Update System

As an Intern,
I want to update my task status through the complete workflow,
So that my Team Lead can track my progress and review my work.

**Acceptance Criteria:**

**Given** I am authenticated as an Intern and have assigned tasks
**When** I view my task list and click on a task status
**Then** I can change status from To Do to In Progress to Submitted
**And** I cannot change status to Approved or Returned (only Team Leads can)
**And** the status change is immediately saved to the database
**And** status changes include timestamps for audit trail
**And** the interface clearly shows the current status and available transitions

---

### Story 3.2: Team Lead Dashboard View

As a Team Lead,
I want to view a comprehensive dashboard showing all interns' task status,
So that I can see the overall progress of the team at a glance.

**Acceptance Criteria:**

**Given** I am authenticated as a Team Lead
**When** I access the main dashboard
**Then** I can see all interns and their current task assignments
**And** each intern shows their task count by status (To Do, In Progress, Submitted, Approved, Returned)
**And** I can filter by intern name or task status
**And** I can sort tasks by creation date, assignment date, or status
**And** the dashboard loads in under 2 seconds (NFR1 compliance)
**And** I have full visibility across all tasks and interns (FR9 compliance)

---

### Story 3.3: Task Review and Approval

As a Team Lead,
I want to review submitted tasks and provide approval with inline feedback,
So that I can ensure quality work and guide intern development.

**Acceptance Criteria:**

**Given** I am authenticated as a Team Lead and interns have submitted tasks
**When** I access the review queue or click on a submitted task
**Then** I can see the complete task with intern's work and comments
**And** I can approve the task with positive feedback
**And** I can return the task with specific feedback for revision
**And** I can add inline comments on specific parts of the work
**And** the intern receives immediate notification of the decision
**And** the task status changes to Approved or Returned accordingly
**And** all review actions are logged in the task history

---

### Story 3.4: Task Return for Revision

As a Team Lead,
I want to return submitted tasks for revision with specific feedback,
So that interns can improve their work based on clear guidance.

**Acceptance Criteria:**

**Given** I am reviewing a submitted task as a Team Lead
**When** I determine the task needs revision
**Then** I can select "Return for Revision" and provide specific feedback
**And** I can highlight specific areas that need improvement
**And** I can attach references or examples to guide the revision
**And** the task status changes to Returned with timestamp
**And** the intern receives detailed notification with feedback
**And** the intern can resubmit the task after making revisions
**And** the complete review cycle is tracked in task history

---

### Story 3.5: Progressive Enhancement

As a User with an older browser,
I want the dashboard to function correctly with basic features,
So that I can access the system regardless of my browser capabilities.

**Acceptance Criteria:**

**Given** I am using any modern browser (Chrome, Edge, Firefox, Safari)
**When** I access any page in the system
**Then** all core functionality works without JavaScript errors
**And** the interface is accessible via keyboard navigation (NFR9 compliance)
**And** screen readers can interpret all content with semantic HTML (NFR10 compliance)
**And** color contrast meets WCAG minimum ratios (NFR11 compliance)
**And** navigation between views is instant without page reloads (NFR6 compliance)

## Epic 4: Real-time Communication & Notifications

Users receive instant updates and email notifications for task changes
**FRs covered:** FR6, FR15, FR16

### Story 4.1: Real-time Task Status Updates

As a User,
I want to see task status changes instantly in my browser,
So that I always have the most current information without manual refresh.

**Acceptance Criteria:**

**Given** I am authenticated and viewing any task-related page
**When** any user updates a task status or submits work
**Then** WebSocket endpoint at /api/websocket handles the event
**And** all connected users see the change within 2 seconds (NFR2 compliance)
**And** UI components update without page reloads (FR15 compliance)
**And** multiple browser tabs for the same user stay synchronized
**And** connection status is indicated to users (online/offline)
**And** event types use snake_case past tense (task.created, task.updated, user.updated)
**And** WebSocket connections are properly authenticated
**And** graceful reconnection logic handles connection drops
**And** real-time updates work across all modern browsers

---

### Story 4.2: Email Notification Service

As a Team Lead,
I want to receive email notifications when interns complete or submit tasks,
So that I can stay informed about progress without constantly checking the dashboard.

**Acceptance Criteria:**

**Given** task status changes are working in Epic 3
**When** an intern submits a task or marks it as completed
**Then** an email notification is sent to the assigned Team Lead
**And** email includes task title, intern name, and completion timestamp
**And** email delivery is reliable with error handling and retry logic
**And** email templates are professional and informative
**And** notification failures are logged for debugging (FR6 compliance)
**And** interns receive email notifications when tasks are returned for revision

---

### Story 4.3: Notification Management

As a User,
I want to manage my notification preferences and view notification history,
So that I can control how I receive updates and review past communications.

**Acceptance Criteria:**

**Given** I am authenticated in the system
**When** I access my notification settings
**Then** I can enable/disable email notifications for different event types
**And** I can view a history of all notifications sent to me
**And** notification history shows timestamps, event type, and status
**And** preferences are saved and persist across sessions
**And** Team Leads can see notifications for all interns they manage
**And** I can mark notifications as read or unread

## Epic 5: User Administration & Cohort Management

Team Leads can manage intern accounts and handle cohort transitions with data preservation
**FRs covered:** FR7, FR11, FR12, FR13

### Story 5.1: User Account Management

As a Team Lead,
I want to add and remove intern accounts,
So that I can manage team composition as interns join or leave the program.

**Acceptance Criteria:**

**Given** I am authenticated as a Team Lead
**When** I access the user management interface
**Then** I can add new intern accounts with name and email
**And** I can deactivate intern accounts when they leave the program
**And** deactivated accounts preserve their historical task data permanently
**And** new interns receive welcome emails with login instructions
**And** account creation integrates with Clerk authentication (FR7 compliance)
**And** all user data is stored permanently with no automatic deletion (FR11 compliance)
**And** database backups are configured to ensure data safety

---

### Story 5.2: Cohort Transition Management

As a Team Lead,
I want to handle cohort transitions with data archival,
So that I can efficiently manage new intern onboarding while preserving historical data.

**Acceptance Criteria:**

**Given** I have completed intern cohorts in the system
**When** I start a new cohort transition process
**Then** I can archive completed cohorts while preserving all data
**And** archived cohorts are separated from active workspace views
**And** archived data remains queryable for reference and reporting
**And** new cohort setup copies templates and configurations from previous cohorts
**And** the transition process maintains data integrity (FR12 compliance)
**And** data retention policies comply with organizational requirements

---

### Story 5.3: Multi-Team Lead Coordination

As a Team Lead,
I want to share visibility of interns and tasks with other Team Leads,
So that we can collaborate effectively on intern management.

**Acceptance Criteria:**

**Given** I am authenticated as a Team Lead
**When** I access the system
**Then** I can see all interns and tasks managed by other Team Leads
**And** other Team Leads can see my interns and tasks
**And** all Team Leads have equal visibility permissions
**And** role-based access control maintains proper data boundaries
**And** collaborative features don't compromise data security (FR13 compliance)

## Epic 6: Task History & Audit Trail

Complete task history and audit trails are maintained for compliance and performance tracking
**FRs covered:** FR14

### Story 6.1: Task History Tracking

As a Team Lead,
I want to see complete history of all task changes and status updates,
So that I can track progress and understand the full lifecycle of each task.

**Acceptance Criteria:**

**Given** tasks have been created and status changes have occurred
**When** I view a task's history
**Then** I can see all status changes with timestamps and user information
**And** I can see assignment history including who assigned the task and when
**And** I can see any modifications to task descriptions or requirements
**And** history is displayed in chronological order with clear event descriptions
**And** history cannot be deleted or modified by regular users (FR14 compliance)

---

### Story 6.2: Audit Trail System

As a System Administrator,
I want a complete audit trail of all user actions and data changes,
So that we have security and compliance records for the system.

**Acceptance Criteria:**

**Given** users are performing actions in the system
**When** any user action occurs (login, task creation, status change, etc.)
**Then** the action is logged with user ID, timestamp, and action details
**And** all data modifications are tracked with before/after values
**And** audit logs are stored securely and cannot be modified
**And** audit trail includes failed login attempts and permission violations
**And** logs are retained according to data retention policies

---

### Story 6.3: Performance Analytics

As a Team Lead,
I want to view intern performance analytics across tasks and time periods,
So that I can identify patterns and provide better guidance and support.

**Acceptance Criteria:**

**Given** task history and audit trail data is available
**When** I access the performance analytics dashboard
**Then** I can see task completion rates for each intern
**And** I can view average time to complete tasks by intern
**And** I can filter analytics by date range, task type, or intern
**And** performance data is presented in clear charts and tables
**And** analytics are calculated from historical task data

---

### Story 6.4: Historical Data Querying

As a Team Lead,
I want to search and export historical task and performance data,
So that I can generate reports and analyze trends over time.

**Acceptance Criteria:**

**Given** comprehensive task history and audit data exists
**When** I use the historical data search interface
**Then** I can search tasks by date range, intern, status, or keywords
**And** I can filter results by multiple criteria simultaneously
**And** I can export search results to CSV or PDF format
**And** exports include all relevant task details and history
**And** search performance is optimized for large datasets
