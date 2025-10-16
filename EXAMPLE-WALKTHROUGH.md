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

## Day 2: Task Creation & Business Analysis

### Step 3: Create Tasks

```bash
/rumiator-create-tasks
```

**Output:**
```
Reading product plan for iteration 1...
Creating tasks...

✓ TASK-001: User Registration and Login
  Feature: auth
  Priority: high
  Status: draft

✓ TASK-002: Workspace Creation and Management
  Feature: workspaces
  Priority: high
  Status: draft

✓ TASK-003: Basic Task CRUD Operations
  Feature: tasks
  Priority: high
  Status: draft

✓ TASK-004: Task Assignment and Status Tracking
  Feature: tasks
  Priority: medium
  Status: draft

✓ TASK-005: Basic Dashboard UI
  Feature: dashboard
  Priority: medium
  Status: draft

✓ TASK-006: CI/CD Pipeline Setup
  Feature: devops
  Priority: high
  Status: draft

Created 6 tasks for iteration 1.

> Are any obvious features missing? (describe or press Enter to continue)
[Enter]

Updating all tasks to "pending-business-analysis"...

✓ Done! Run /rumiator-analyze-business all to create functional specs.
```

**Result:** 6 task YAML files created in `.rumiator/tasks/`

### Step 4: Business Analysis

```bash
/rumiator-analyze-business all
```

**For TASK-001 (Auth):**
```
Analyzing TASK-001: User Registration and Login
Launching functional-analyst agent...

Agent: I need clarification on the authentication approach.

Question 1: Should we support OAuth (Google, GitHub) or just email/password?

> Just email/password for MVP. We can add OAuth in iteration 2.

Question 2: Password reset flow - email-based or security questions?

> Email-based password reset.

Question 3: Should we implement email verification on registration?

> Yes, users must verify email before accessing the platform.

Creating docs/features/auth/functional.md...

✓ Functional spec created
✓ Task updated to "pending-technical-analysis"

Key acceptance criteria:
- User can register with email, password, and name
- Email must be verified via link sent to inbox
- User can log in with verified credentials
- User can request password reset via email
- Password must be at least 8 characters with 1 number
```

**For TASK-002 (Workspaces):**
```
Analyzing TASK-002: Workspace Creation and Management

Agent: Clarification needed.

Question: Can a user belong to multiple workspaces?

> Yes, users can create or join multiple workspaces.

Question: Who can invite new members to a workspace?

> Only workspace owners and admins can invite members.

Creating docs/features/workspaces/functional.md...

✓ Functional spec created
✓ Task updated to "pending-technical-analysis"
```

**...continues for all tasks...**

**Result:** All 6 tasks now have functional specs and are ready for technical analysis.

---

## Day 3: Technical Analysis

### Step 5: Technical Analysis

```bash
/rumiator-analyze-tech all
```

**For TASK-001 (Auth):**
```
Analyzing TASK-001: User Registration and Login
Launching architect agent...

Agent: Tech stack confirmation.

Question: For JWT tokens, what's the preferred expiry time?

> Access token: 15 minutes, Refresh token: 7 days

Question: Should we use bcrypt or argon2 for password hashing?

> bcrypt is fine for MVP.

Creating docs/features/auth/technical.md...
Updating docs/product/architecture.md...

✓ Technical spec created
✓ Architecture diagram updated
✓ Task updated to "ready-for-development"
✓ Complexity estimate: Medium

API endpoints defined:
- POST /api/auth/register
- POST /api/auth/login
- POST /api/auth/refresh
- POST /api/auth/forgot-password
- POST /api/auth/reset-password
- GET /api/auth/verify-email/:token

Data models: User, RefreshToken, EmailVerification
Database tables: users, refresh_tokens, email_verifications
```

**For TASK-002 (Workspaces):**
```
Analyzing TASK-002: Workspace Creation and Management

Agent: I recommend creating an ADR for multi-tenancy approach.

> Yes, please create one.

Creating ADR-001: Multi-tenant Data Isolation Strategy...
Creating docs/features/workspaces/technical.md...

✓ Technical spec created
✓ ADR-001 created at docs/adr/ADR-001-multi-tenancy.md
✓ Task updated to "ready-for-development"
✓ Complexity estimate: High

Reason for High: Multi-tenant architecture, role-based access control
```

**...continues for all tasks...**

**Final status:**
```
All 6 tasks analyzed!

Complexity breakdown:
- Low: 1 (TASK-005: Dashboard UI)
- Medium: 3 (TASK-001, TASK-003, TASK-004)
- High: 2 (TASK-002, TASK-006)

Ready for development! Run /rumiator-develop-next to start.
```

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
- 📄 6 functional specifications
- 📄 6 technical specifications
- 📄 1 Architecture Decision Record
- 📄 1 Iteration report
- 📄 Updated architecture diagram

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

## Next Steps for Iteration 2

```bash
/rumiator-create-tasks  # Generate tasks for real-time features
/rumiator-analyze-business all
/rumiator-analyze-tech all
/rumiator-develop-next  # Start building chat!
```

---

**This is how Rumiator works in practice!** 🚀

For more details, see:
- [README.md](./README.md) - Quick reference
- [RUMIATOR-GUIDE.md](./RUMIATOR-GUIDE.md) - Complete documentation
