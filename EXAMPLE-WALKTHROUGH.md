# Rumiator Complete Walkthrough Example

This document shows a complete, real-world example of using Rumiator to build a project from scratch.

## Project: "TeamFlow" - Team Collaboration Platform

Let's build a simple team collaboration platform with task management and real-time chat.

---

## Day 1: Initialization & Planning

### Step 1: Initialize the Project

```bash
/rumiator-init
```

**Interaction:**
```
> Project name?
TeamFlow

> Brief description?
A lightweight collaboration platform for small teams with task management and chat

> Preferred tech stack? (optional, press Enter to skip)
React, Node.js, PostgreSQL

✓ Created directory structure
✓ Created config at .rumiator/config.yml
✓ Created templates
✓ Created README.md
✓ Created .gitignore

Next step: Run /rumiator-create-product to start planning!
```

**Result:** Project structure is ready.

### Step 2: Create Product Vision

```bash
/rumiator-create-product
```

**Interaction:**
```
> What problem does your product solve?

"Small teams struggle with fragmented tools. They use Slack for chat,
Trello for tasks, and email for everything else. TeamFlow combines
essential collaboration features in one simple app."

> Who are the target users?

"Small tech teams (5-20 people), startups, and remote teams who want
a simpler alternative to complex enterprise tools."

> What are the main features you envision?

"1. User authentication
2. Workspace/team creation
3. Task management (create, assign, track status)
4. Real-time chat (team channels)
5. Notifications
6. Basic user profiles"

> Any specific requirements or constraints?

"Must be fast and simple. Mobile-responsive. Free tier for up to 10 users.
Must support GDPR compliance for EU users."

Creating docs/product/product-idea.md...
Launching project-manager agent to create product plan...

✓ Product plan created at docs/product/product-plan.md

Summary:
- Vision: Unified collaboration platform for small teams
- Target: 3 iterations over 8 weeks
- Iteration 1: MVP (Auth, Workspaces, Basic Tasks) - Weeks 1-3
- Iteration 2: Collaboration (Real-time chat, Notifications) - Weeks 4-6
- Iteration 3: Polish (User profiles, Settings, Analytics) - Weeks 7-8
- Key Risk: Real-time features complexity - Mitigate with WebSocket library
- Compliance: GDPR data privacy, secure authentication

> Proceed with creating tasks for iteration 1? (y/n)
y
```

**Result:**
- `docs/product/product-idea.md` created
- `docs/product/product-plan.md` created with detailed plan

---

## Day 2: Task Creation with Business Requirements

### Step 3: Create Tasks (with Business Requirements)

```bash
/rumiator-create-tasks
```

**Output:**
```
Reading product plan for iteration 1...
Creating tasks with business requirements...

Agent: I need clarification on the authentication approach.

Question 1: Should we support OAuth (Google, GitHub) or just email/password?

> Just email/password for MVP. We can add OAuth in iteration 2.

Question 2: Password reset flow - email-based or security questions?

> Email-based password reset.

Question 3: Should we implement email verification on registration?

> Yes, users must verify email before accessing the platform.

✓ TASK-001: User Registration and Login
  Feature: auth
  Priority: high
  Status: pending-technical-analysis
  Summary: Enable users to register for an account using their email address...
  User Stories: 4
  Acceptance Criteria: 10

Agent: Can a user belong to multiple workspaces?

> Yes, users can create or join multiple workspaces.

✓ TASK-002: Workspace Creation and Management
  Feature: workspaces
  Priority: high
  Status: pending-technical-analysis
  Summary: Allow users to create workspaces for organizing their work...
  User Stories: 5
  Acceptance Criteria: 12

✓ TASK-003: Basic Task CRUD Operations
  Feature: tasks
  Priority: high
  Status: pending-technical-analysis
  Summary: Implement core task management functionality...
  User Stories: 4
  Acceptance Criteria: 8

✓ TASK-004: Task Assignment and Status Tracking
  Feature: tasks
  Priority: medium
  Status: pending-technical-analysis

✓ TASK-005: Basic Dashboard UI
  Feature: dashboard
  Priority: medium
  Status: pending-technical-analysis

✓ TASK-006: CI/CD Pipeline Setup
  Feature: devops
  Priority: high
  Status: pending-technical-analysis

Created 6 tasks for iteration 1.

Priority breakdown:
- High: 3
- Medium: 3

> Are any obvious features missing? (describe or press Enter to continue)
[Enter]

✓ All tasks created successfully!

Next steps:
→ Run `/rumiator-analyze-tech all` to create technical specifications

Note: Tasks already include business requirements (summary, user stories, acceptance criteria).
The architect will now analyze technical approach and architectural impact.
```

**Result:** 6 task YAML files created with complete business requirements

**⚡ What changed:** Business analysis is now integrated into task creation - one step instead of two!

---

## Day 3: Technical Guidance (Not Implementation)

### Step 4: Get Technical Guidance

```bash
/rumiator-analyze-tech all
```

**For TASK-001 (Auth):**
```
Analyzing TASK-001: User Registration and Login
→ Reading business requirements from task YAML...
→ Understanding current architecture...
→ Evaluating if ADR is needed...

Agent: This task introduces authentication to the system.
Should I create an ADR for the authentication approach (JWT vs Session-based)?

> Yes, please create an ADR for JWT-based authentication

Creating ADR-001: JWT-based Authentication
→ Title: Use JWT tokens with refresh token rotation
→ Decision: JWT for stateless authentication
→ Alternatives: Session-based, OAuth only

> Please review this ADR. Proceed? (y/n)
y

✓ ADR-001 validated and saved
→ Creating technical specification...
→ Updating architecture.md...

✓ Technical spec created at docs/features/auth/technical.md
✓ Task updated to "ready-for-development"
✓ Complexity: Medium

Technologies identified:
- Frontend: React Context for auth state, TanStack Query
- Backend: JWT with bcrypt for password hashing
- Database: PostgreSQL with indexes on email

Technical considerations documented:
- Design system: Use existing Form, Input, Button components
- State management: React Context for auth state
- Security: bcrypt (12 rounds), JWT short-lived tokens
- Performance: Cache user session data
```

**⚡ What changed:** Technical spec now provides HIGH-LEVEL guidance only (no API specs, data models, or SQL schemas)!

**For TASK-002 (Workspaces):**
```
Analyzing TASK-002: Workspace Creation and Management
→ Reading business requirements...
→ Understanding current architecture...

Agent: This introduces multi-tenant architecture.
Should I create an ADR for multi-tenant data isolation?

> Yes, please

Creating ADR-002: Multi-tenant Data Isolation Strategy...
→ Decision: Row-level isolation with workspace_id in all tables

> Review ADR-002. Proceed? (y/n)
y

✓ ADR-002 validated and saved
✓ Technical spec created at docs/features/workspaces/technical.md
✓ Task updated to "ready-for-development"
✓ Complexity: High (multi-tenant architecture)

Technologies identified:
- Frontend: Design system Table and Form components
- Backend: Multi-tenant data isolation pattern
- Database: PostgreSQL with workspace_id in all tables
```

**...continues for all tasks...**

**Final status:**
```
All 6 tasks analyzed!

Summary:
- Tasks analyzed: 6
- Complexity: 1 low, 3 medium, 2 high
- ADRs created: 2 (ADR-001, ADR-002)
- Ready for development: 6

Next steps:
→ Run `/rumiator-develop TASK-001` to start implementing
→ Or run `/rumiator-develop-next` to automatically pick next task

Note: Technical specs provide architectural guidance only.
Developers will make implementation decisions (APIs, schemas, etc.) during development.
```

**⚡ Key difference:** Architect doesn't write implementation details - developers have more autonomy!

---

## Days 4-7: Development Sprint

### Step 6: Start Development

```bash
/rumiator-status
```

**Output:**
```
═══════════════════════════════════════════════════
  RUMIATOR PROJECT STATUS
═══════════════════════════════════════════════════

Project: TeamFlow
Current Iteration: 1 of 3
Progress: ░░░░░░░░░░ 0% (0/6 tasks completed)

TASKS BY STATUS
───────────────────────────────────────────────────
→ Ready for Development          6
✓ Done                           0
───────────────────────────────────────────────────

NEXT ACTIONS
───────────────────────────────────────────────────
High-priority tasks ready:
1. TASK-001: User Registration and Login (Medium complexity)
2. TASK-002: Workspace Creation (High complexity)
3. TASK-006: CI/CD Pipeline Setup (High complexity)

Suggested: /rumiator-develop-next
═══════════════════════════════════════════════════
```

### Develop First Task

```bash
/rumiator-develop-next
```

**Output:**
```
Selecting next task...

Selected: TASK-001 - User Registration and Login
Priority: High
Complexity: Medium
Type: Fullstack (Backend + Frontend)

Overview: Implement user authentication with email verification

> Proceed with this task? (y/n)
y

Developing TASK-001...

==== Backend Development ====
Launching developer-backend agent...

Creating database migration: 001_create_users_table.sql
Creating models: User.ts, RefreshToken.ts, EmailVerification.ts
Creating repositories: UserRepository.ts
Creating services: AuthService.ts, EmailService.ts
Creating controllers: AuthController.ts
Creating routes: auth.routes.ts
Creating middleware: authMiddleware.ts
Writing unit tests...
Writing integration tests...
Running tests... ✓ All pass (23/23)
Running linter... ✓ No errors

✓ Backend implementation complete

==== Frontend Development ====
Launching developer-frontend agent...

Creating components:
- src/components/auth/RegisterForm.tsx
- src/components/auth/LoginForm.tsx
- src/components/auth/ForgotPasswordForm.tsx
- src/pages/Register.tsx
- src/pages/Login.tsx
- src/hooks/useAuth.ts

Creating API client: src/api/auth.ts
Writing tests...
Running tests... ✓ All pass (15/15)

✓ Frontend implementation complete

==== Summary ====
✓ Task TASK-001 completed
Files created: 18
Tests added: 38
Status: done

> Continue to next task? (y/n)
n

Run /rumiator-status to see progress, or /rumiator-develop-next when ready.
```

### Continue Development

Over the next few days, you continue:

```bash
/rumiator-develop-next  # TASK-006 (CI/CD Setup)
/rumiator-develop-next  # TASK-002 (Workspaces)
/rumiator-develop-next  # TASK-003 (Task CRUD)

# Check progress midway
/rumiator-status
```

**Midway status:**
```
Progress: ████████░░ 67% (4/6 tasks completed)

✓ Done: TASK-001, TASK-002, TASK-003, TASK-006
⚙ Ready: TASK-004, TASK-005
```

**Blocker scenario:**

During TASK-004 (Task Assignment), the backend developer encounters an issue:

```
Developer: I need clarification on task assignment logic.

Question: Should a user be able to assign tasks to users who are NOT
in the same workspace?

> No, users can only assign tasks within their workspace.

Question: What should happen if an assigned user leaves the workspace?

> Hmm, I hadn't thought about that. Let me block this task and create
a separate task to handle that edge case.

/rumiator-block-task TASK-004
```

**Blocking TASK-004:**
```
> What is the blocking issue?

"Need to define behavior for edge case: assigned user leaving workspace"

> What is needed to unblock?

"Decision on whether to auto-unassign, keep assignment with warning, or
prevent user from leaving if they have assigned tasks"

> Impact? (Low/Medium/High/Critical)

Medium

✓ TASK-004 marked as blocked
⚠ No dependent tasks affected

Recommendation: Create a task for edge case handling, or decide now.

> Let's decide now. Tasks should auto-unassign if user leaves workspace.

Unblocking TASK-004...
✓ Decision documented in technical spec
✓ Task ready to continue

Resuming development...
```

### Finish Sprint

```bash
/rumiator-develop-next  # Complete TASK-004
/rumiator-develop-next  # Complete TASK-005

/rumiator-status
```

**Final status:**
```
Progress: ██████████ 100% (6/6 tasks completed)

✓ All tasks for iteration 1 complete!
```

---

## Day 8: Iteration Review

### Step 7: Generate Report

```bash
/rumiator-report
```

**Output:**
```
Generating iteration 1 report...
Reading task history...
Analyzing velocity...

✓ Report created at docs/iterations/iteration-01-report.md

Summary:
- Completed: 6/6 tasks (100%)
- Velocity: 0.86 tasks/day
- Complexity delivered: 1 low, 3 medium, 2 high
- Blockers encountered: 1 (resolved)
- ADRs created: 1 (Multi-tenancy)
- Test coverage: 87%

Key achievements:
✓ Full authentication system with email verification
✓ Multi-tenant workspace architecture
✓ Task CRUD with assignment
✓ Basic dashboard UI
✓ CI/CD pipeline deployed to staging

Lessons learned:
→ Edge cases should be discussed during functional analysis
→ High-complexity tasks took longer than estimated

Next iteration recommendations:
→ Allocate more time for high-complexity tasks
→ Add edge case checklist to functional analysis
```

### Step 8: Plan Next Iteration

```bash
# Update plan for iteration 2
/rumiator-update-plan
```

```
> What changed?

"We want to prioritize real-time features in iteration 2.
Let's focus on chat and live task updates."

Updating product-plan.md...

✓ Iteration 2 updated
✓ Changelog entry added

Next: Run /rumiator-create-tasks when ready to start iteration 2.
```

---

## Summary: What We Built

### Iteration 1 Deliverables

**Features:**
- ✅ User authentication (register, login, email verification, password reset)
- ✅ Workspace management (create, invite, members, roles)
- ✅ Task management (CRUD, assignment, status tracking)
- ✅ Basic dashboard UI
- ✅ CI/CD pipeline

**Documentation:**
- 📄 Product plan with 3 iterations
- 📄 6 tasks with business requirements (in YAML) ⚡
- 📄 6 technical guidance documents (high-level) ⚡
- 📄 2 Architecture Decision Records (validated with user) ⚡
- 📄 1 Iteration report
- 📄 Updated architecture diagram

**⚡ Streamlined docs:** No separate functional specs, no detailed API/schema specs!

**Code:**
- 🔧 Backend API (18 endpoints)
- 🎨 Frontend React app (12+ components)
- ✅ 53 tests (87% coverage)
- 🚀 CI/CD with automated testing

**Time:** 8 days

---

## Key Takeaways

1. **Structure helps:** The framework kept us focused and organized
2. **Documentation matters:** Every decision is documented, making onboarding easy
3. **Iteration works:** Small iterations delivered value quickly
4. **Agents ask questions:** Clarifications improved quality
5. **Progress is visible:** Status dashboard kept us motivated
6. **⚡ Simplified workflow:** 2-phase analysis is faster and more efficient
7. **⚡ Developer autonomy:** Developers make implementation decisions, not architects
8. **⚡ User validation:** ADRs are validated with user before proceeding

## Next Steps for Iteration 2

```bash
/rumiator-create-tasks         # Generate tasks with business requirements ⚡
/rumiator-analyze-tech all     # Get technical guidance only ⚡
/rumiator-develop-next         # Start building chat!
```

**⚡ Faster:** One less command - business analysis integrated into task creation!

---

**This is how Rumiator works in practice!** 🚀

For more details, see:
- [README.md](./README.md) - Quick reference
- [RUMIATOR-GUIDE.md](./RUMIATOR-GUIDE.md) - Complete documentation
