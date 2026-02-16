# UX Design Document - t2

**Author:** ShaeelaHussain  
**Date:** 2026-02-13  
**Version:** 1.0 - Basic Wireframes and Design System

## Overview

This document provides basic UX design specifications for the intern task management system, focusing on core user journeys and component design patterns.

## Design System Basics

### Color Palette
- **Primary Blue:** #3B82F6 (Primary actions, links)
- **Success Green:** #10B981 (Completed tasks, success states)
- **Warning Yellow:** #F59E0B (In progress, pending review)
- **Error Red:** #EF4444 (Returned tasks, errors)
- **Neutral Gray:** #6B7280 (Secondary text, borders)
- **Background:** #F9FAFB (Page background)
- **White:** #FFFFFF (Cards, modals)

### Typography
- **Headings:** Inter, 600 weight
- **Body Text:** Inter, 400 weight
- **Small Text:** Inter, 400 weight, 14px
- **Button Text:** Inter, 500 weight, 16px

### Spacing Scale
- **XS:** 4px
- **SM:** 8px  
- **MD:** 16px
- **LG:** 24px
- **XL:** 32px
- **2XL:** 48px

## Component Library

### Button Styles
- **Primary Button:** Blue background, white text, rounded 6px, padding 12px 24px
- **Secondary Button:** White background, blue border, blue text, rounded 6px, padding 12px 24px
- **Status Button:** Colored background based on status (green/yellow/red), white text

### Card Components
- **Task Card:** White background, subtle border, rounded 8px, padding 16px, shadow-sm
- **User Card:** White background, border, rounded 8px, padding 16px, avatar + name
- **Dashboard Widget:** White background, border, rounded 8px, padding 20px

### Form Elements
- **Input Fields:** White background, border, rounded 6px, padding 12px, focus: blue border
- **Dropdowns:** Same as inputs with arrow indicator
- **Text Areas:** White background, border, rounded 6px, padding 12px, min-height 100px

## Core User Journeys

### Journey 1: Team Lead Dashboard Experience

#### Dashboard Layout
```
┌─────────────────────────────────────────────────────────────┐
│ Header: Logo | Navigation | User Menu                    │
├─────────────────────────────────────────────────────────────┤
│ Overview Cards                                              │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐           │
│ │Total    │ │In       │ │Submitted│ │Approved │           │
│ │Tasks    │ │Progress │ │Tasks    │ │Tasks    │           │
│ │24       │ │8        │ │3        │ │13       │           │
│ └─────────┘ └─────────┘ └─────────┘ └─────────┘           │
├─────────────────────────────────────────────────────────────┤
│ Intern Status Table                                         │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Intern Name │ Assigned │ In Progress │ Submitted │ Done │ │
│ │ Ahmed Ali   │ 5        │ 2            │ 1         │ 2   │ │
│ │ Sara Khan   │ 4        │ 1            │ 2         │ 1   │ │
│ │ Omar Hassan │ 6        │ 3            │ 0         │ 3   │ │
│ └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

#### Key Interactions
- Click intern name to view their detailed task list
- Click task count to filter by status
- Quick actions: "Create Task" button (top right)
- Real-time updates when interns change task status

### Journey 2: Intern Task Management

#### Intern Dashboard Layout
```
┌─────────────────────────────────────────────────────────────┐
│ Header: Logo | My Tasks | User Menu                       │
├─────────────────────────────────────────────────────────────┤
│ Welcome Message                                             │
│ "Hi Ahmed! You have 2 tasks in progress, 1 pending review" │
├─────────────────────────────────────────────────────────────┤
│ My Tasks                                                   │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ ┌─ Task Card: Build Login Page ─────────────────────┐   │ │
│ │ │ Status: In Progress (Yellow)                     │   │ │
│ │ │ Assigned: Feb 10, Due: Feb 15                    │   │ │
│ │ │ [View Details] [Update Status] [Ask Question]    │   │ │
│ │ └───────────────────────────────────────────────────┘   │ │
│ │                                                         │ │
│ │ ┌─ Task Card: API Documentation ────────────────────┐   │ │
│ │ │ Status: Submitted (Green)                         │   │ │
│ │ │ Assigned: Feb 8, Due: Feb 12                     │   │ │
│ │ │ [View Details] [Add Comment]                      │   │ │
│ │ └───────────────────────────────────────────────────┘   │ │
│ └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

#### Task Detail View
```
┌─────────────────────────────────────────────────────────────┐
│ ← Back to Tasks                                            │
├─────────────────────────────────────────────────────────────┤
│ Task: Build Login Page Mockup                               │
│ Status: In Progress ● [Update to Submitted]                 │
├─────────────────────────────────────────────────────────────┤
│ Description                                                 │
│ "Create Figma mockup for login page with email/password..." │
├─────────────────────────────────────────────────────────────┤
│ Requirements                                                │
│ • Use company brand colors                                   │
│ • Include forgot password link                              │
│ • Mobile responsive design                                  │
├─────────────────────────────────────────────────────────────┤
│ Questions & Comments                                        │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Ahmed: Feb 12 - "Should I include social login?"       │ │
│ │ Fatima: Feb 12 - "No, just email/password for MVP"    │ │
│ │ [Add Comment]                                           │ │
│ └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### Journey 3: Task Review Workflow

#### Review Queue Layout (Team Lead)
```
┌─────────────────────────────────────────────────────────────┐
│ Header: Logo | Review Queue | User Menu                   │
├─────────────────────────────────────────────────────────────┤
│ Pending Reviews (3)                                         │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ ┌─ Task Review Card ────────────────────────────────┐   │ │
│ │ │ API Documentation - Sara Khan                   │   │ │
│ │ │ Submitted: Feb 12, 2024                          │   │ │
│ │ │ [View Submission] [Approve] [Return for Revision] │   │ │
│ │ └───────────────────────────────────────────────────┘   │ │
│ └─────────────────────────────────────────────────────────┘ │
├─────────────────────────────────────────────────────────────┤
│ Recent Reviews                                              │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ ✅ Login Page - Ahmed Ali (Approved, Feb 11)          │ │
│ │ 🔴 Database Schema - Omar Hassan (Returned, Feb 10)    │ │
│ └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

#### Review Interface
```
┌─────────────────────────────────────────────────────────────┐
│ Task Review: API Documentation                              │
├─────────────────────────────────────────────────────────────┤
│ Original Requirements                                        │
│ "Document all user endpoints with examples..."              │
├─────────────────────────────────────────────────────────────┤
│ Sara's Submission                                           │
│ [Link to Figma/document]                                    │
│ "I've documented POST /users and GET /users/{id}"         │
├─────────────────────────────────────────────────────────────┤
│ Review Actions                                              │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Feedback:                                               │ │
│ │ [Text area for review comments]                        │ │
│ │                                                         │ │
│ │ Decision:                                               │ │
│ │ ○ Approve with feedback                                │ │
│ │ ● Return for revision                                   │ │
│ │                                                         │ │
│ │ [Submit Review]                                         │ │
│ └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

## Accessibility Guidelines

### Keyboard Navigation
- All interactive elements reachable via Tab
- Clear focus indicators (blue outline)
- Logical tab order through forms and tables

### Screen Reader Support
- Semantic HTML5 elements (header, nav, main, section)
- ARIA labels for custom components
- Alt text for images and icons
- Status announcements for dynamic updates

### Visual Accessibility
- Minimum contrast ratio 4.5:1 for normal text
- Color not the only indicator of status (use icons + text)
- Resizable text up to 200% without breaking layout
- High contrast mode support

## Responsive Design

### Desktop (1024px+)
- Full dashboard with all columns visible
- Side-by-side task and review interfaces
- Hover states and tooltips available

### Tablet (768px-1023px)
- Stacked dashboard cards
- Simplified task list view
- Collapsible navigation

### Mobile (320px-767px)
- Single column layout
- Bottom navigation for key actions
- Swipe gestures for task status changes
- Full-screen task detail views

## Performance Considerations

### Loading States
- Skeleton screens for dashboard data
- Progressive loading of task lists
- Optimized images and icons

### Real-time Updates
- Subtle animations for status changes
- Non-intrusive notifications
- Offline indicators when connection lost

## Next Steps

This basic UX design provides:
1. **Component foundation** for development team
2. **User journey clarity** for implementation priorities  
3. **Accessibility baseline** for inclusive design
4. **Responsive framework** for multi-device support

**Recommendations for next phase:**
- Interactive prototyping for key flows
- Usability testing with actual Team Leads and Interns
- Visual design refinement with brand guidelines
- Component library implementation in code
