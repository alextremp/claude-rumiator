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
2. Read task YAML, functional spec, and technical spec
3. Determine task type based on technical spec analysis:
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

Important:
- Agents should ask user if they have questions about implementation details
- Backend must be completed before frontend for fullstack tasks
- Agents must run tests and ensure they pass
- Tasks should only be marked "done" if ALL acceptance criteria are met
- If blocked, document clearly in task YAML
