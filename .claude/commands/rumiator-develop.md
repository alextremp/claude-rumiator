Implement a specific task using the appropriate developer agents (frontend, backend, devops).

Usage:
- /rumiator-develop [task-id] - Develop a specific task

Prerequisites:
- Task must have status "ready-for-development"
- Both functional and technical specs must exist

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Validate task-id:
   - Check if task exists
   - Verify status is "ready-for-development"
   - If invalid, show error and list tasks that are ready
2. Read task YAML and check task type (type field):
   - If type not specified, default to "feature"
   - If type is "bug", follow bug development workflow (see below)
   - If type is "architecture-review", follow architecture review workflow (see below)
   - If type is "feature", continue with normal workflow
3. Read functional spec and technical spec (if they exist)
4. Determine implementation type based on technical spec analysis:
   - **Frontend only**: UI/UX, components, no backend changes
   - **Backend only**: API, business logic, database, no UI
   - **DevOps**: CI/CD, infrastructure, deployment
   - **Fullstack**: Both frontend and backend
4. Update task status to "in-progress"
5. Launch appropriate agent(s):

   **If Frontend only**:
   - Launch developer-frontend agent
   - Agent implements feature, writes tests, updates task

   **If Backend only**:
   - Launch developer-backend agent
   - Agent implements feature, writes tests, updates task

   **If DevOps**:
   - Launch devops agent
   - Agent implements infrastructure, updates task

   **If Fullstack** (execute sequentially):
   a. First, launch developer-backend agent
      - Wait for backend completion
      - Check if backend succeeded
   b. Then, launch developer-frontend agent
      - Integrate with backend
      - Complete frontend implementation

6. After development completes:
   - Verify task status is "done" (agents should update this)
   - If status is still "in-progress", check for blockers
   - Display summary:
     * Task ID and title
     * Developer(s) involved
     * Files modified
     * Tests added/modified
     * Any blockers encountered
7. If task is done, suggest next steps:
   - Optionally run QA review
   - Run /rumiator-develop-next for next task
   - Or commit the changes if satisfied

## Bug Development Workflow

If task type is "bug":

1. Read bug_info section from task YAML
2. Display bug details:
   - Severity
   - Steps to reproduce
   - Expected vs actual behavior
   - Related tasks
3. Read bug analysis document: docs/features/[feature]/bugs/BUG-XXX-analysis.md
4. Determine bug type:
   - **Frontend bug**: Launch developer-frontend
   - **Backend bug**: Launch developer-backend
   - **Both**: Launch both sequentially (backend first)
5. Launch appropriate developer agent with specific instructions:
   - Fix the root cause, not just symptoms
   - Add regression tests to prevent recurrence
   - Verify fix against acceptance criteria
   - Update bug analysis with resolution notes
   - If bug was in related_tasks, update those tasks' notes
6. After fix complete:
   - Verify all acceptance criteria met
   - Ensure tests pass
   - Update task status to "done"
   - If bug revealed larger issue, suggest creating architecture-review task

## Architecture Review Workflow

If task type is "architecture-review":

1. Read architecture_review section from task YAML:
   - related_adr: The new ADR to follow
   - affected_tasks: Original task(s) being reviewed
   - reason: Why review is needed
2. Display review context:
   - Which ADR triggered this review
   - What changed in architecture
   - Which original task is being reviewed
3. Read original task's implementation:
   - Source code files (if available)
   - Original technical spec
   - Original functional spec
4. Read new ADR to understand required changes
5. Launch architect agent to:
   - Assess what needs to change
   - Create migration plan
   - Update technical spec if needed
   - Estimate refactoring effort
6. Ask user if changes should be implemented now:
   - **Yes, refactor now**:
     * Launch appropriate developer agent(s)
     * Implement changes per new architecture
     * Run tests
     * Mark as "done"
   - **No, defer**:
     * Document findings in task notes
     * Create new task for actual refactoring
     * Mark review as "done"
     * Add refactor task to backlog
   - **Need more analysis**:
     * Keep in "in-progress"
     * Ask architect for more details
     * Come back to this later
7. Update related original task:
   - Add note that it was reviewed for ADR-XXX
   - Link to this review task

## Feature Development Workflow (Standard)

If task type is "feature" or not specified:

[Previous steps 4-7 apply here, starting with "Determine task type based on technical spec analysis"]

Important:
- **For bugs**: Fix root cause, add regression tests, update bug analysis
- **For architecture reviews**: Assess impact, create migration plan, decide on timing
- **For features**: Follow standard development workflow
- Agents should ask user if they have questions about implementation details
- Backend must be completed before frontend for fullstack tasks
- Agents must run tests and ensure they pass
- Tasks should only be marked "done" if ALL acceptance criteria are met
- If blocked, document clearly in task YAML
- Bug fixes should be verified against original bug report
- Architecture reviews may create follow-up refactoring tasks
