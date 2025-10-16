Mark a task as blocked and document the blocker details.

Usage:
- /rumiator-block-task [task-id]

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Validate task-id:
   - Check if task exists
   - If invalid, show error and list existing tasks
2. Read current task YAML
3. Ask user for blocker details:
   - What is the blocking issue?
   - What is needed to unblock?
   - Is there a workaround available?
   - What's the impact (Low, Medium, High, Critical)?
   - Who needs to resolve this? (team member, external party, decision needed, etc.)
4. Update task YAML:
   - status: "blocked"
   - Add to blockers array:
     ```yaml
     blockers:
       - issue: "[Description of the blocking issue]"
         needed: "[What's needed to unblock]"
         workaround: "[Workaround if available, or 'None']"
         impact: "[low|medium|high|critical]"
         reported: "[date]"
         owner: "[who needs to resolve]"
     ```
   - updated: current date
5. Check if this blocker affects other tasks:
   - Scan for tasks that depend on this task
   - Warn user about potential cascade effects
6. Display blocker summary:
   - Task ID and title
   - Blocker description
   - Impact level
   - Suggested action
7. Create a reminder/action item:
   - Add to project notes or suggest creating a task for resolving the blocker
8. Update project status to reflect the blocker
9. Suggest next steps:
   - Address the blocker immediately if critical
   - Or move to another task: /rumiator-develop-next

Important:
- Document blockers clearly and completely
- Assess impact honestly
- Identify who can resolve the blocker
- Keep stakeholders informed of critical blockers
- Regularly review and update blocker status
