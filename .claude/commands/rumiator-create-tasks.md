Generate task definitions from the product plan for the current iteration using the Functional Analyst agent.

Prerequisites:
- docs/product/product-plan.md must exist
- Tasks will be located in docs/iterations/iteration-XX/tasks/ (where XX is current iteration of .rumiator/config.yml)

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Check if docs/product/product-plan.md exists. If not, tell user to run /rumiator-create-product first
2. Read .rumiator/config.yml to get current iteration number
3. Read docs/product/product-plan.md to understand features for current iteration
4. Launch the functional-analyst agent in "Task Creation Mode":
   - Identify all features for the current iteration
   - For each feature:
     * Determine next task ID (check existing tasks in docs/iterations/iteration-XX/tasks/)
     * Create docs/iterations/iteration-XX/tasks/TASK-XXX.yml with:
       - Basic metadata (id, type, title, feature, priority, iteration, dates)
       - **summary**: 3-5 line description of what the task accomplishes
       - **user_stories**: List of user stories in format "As a [role], I want to [action], so that [benefit]"
       - **acceptance_criteria**: Specific, testable criteria
       - **status**: `pending-technical-analysis`
       - **business_analyst**: "functional-analyst"
     * Create docs/features/[feature-name]/ directory
5. Display created tasks summary:
   - Task ID, title, summary (first line)
   - Total count
   - Priority breakdown
6. Ask user if any obvious features are missing
7. If user mentions missing features:
   - Create additional tasks following same process
   - Update the count
8. Read current iteration number from .rumiator/config.yml (iterations.current)
9. Update docs/iterations/iteration-[XX]/plan.md:
   - Add list of created tasks to "Tasks" section
   - Update "Scope" section with summary of features
   - Keep status as "In Progress"
10. Display next steps:
   - **Next command**: `/rumiator-analyze-tech all` to create technical specifications
   - Tasks are now ready for technical analysis by the architect

## Important Notes

### What Changed
- Tasks are now created **directly** in status `pending-technical-analysis`
- **Business requirements** (summary, user stories, acceptance criteria) are included in the task YAML during creation
- There is **NO separate business analysis phase** - all business requirements are captured upfront
- The **functional-analyst** agent only runs once (during task creation), not twice
- The next step is **technical analysis**, not business analysis

### Task Quality
- Tasks should be granular (1-3 days of work ideally)
- Each task should have a clear, testable objective
- Priority should reflect business value and dependencies
- Summary, user stories, and acceptance criteria must be complete and clear
- **NO technical details** should be included (APIs, schemas, code)

### Agent Responsibilities
- **functional-analyst**: Creates tasks with complete business requirements (summary, user stories, acceptance criteria)
- **architect**: Will analyze technical approach, architecture impact, and create ADRs if needed
- **developers**: Will implement based on business requirements and technical guidance

## Example Output

```
Reading product plan for iteration 1...
Creating tasks with business requirements...

✓ TASK-001: User Registration and Email Verification
  Feature: authentication
  Priority: high
  Status: pending-technical-analysis
  Summary: Enable users to register for an account using their email address...
  User Stories: 4
  Acceptance Criteria: 10

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

Created 3 tasks for iteration 1.

Priority breakdown:
- High: 3
- Medium: 0
- Low: 0

> Are any obvious features missing? (describe or press Enter to continue)
[Enter]

✓ All tasks created successfully!

Next steps:
→ Run `/rumiator-analyze-tech all` to create technical specifications
→ Or run `/rumiator-analyze-tech TASK-001` for a specific task

Note: Tasks already include business requirements (summary, user stories, acceptance criteria).
The architect will now analyze technical approach and architectural impact.
```

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-create-tasks.md`
2. **If the file exists**:
   - Read the customization file completely
   - Apply ALL customization instructions described in that file
   - Customizations OVERRIDE any conflicting steps or behaviors defined earlier in this command
   - Customizations COMPLEMENT non-conflicting steps
3. **If the file does not exist**:
   - Continue with the standard behavior defined above

**Examples of customizations**:
- Adding pre/post execution hooks (notifications, validations, etc.)
- Modifying specific steps in the workflow
- Adding additional checks or requirements
- Integrating with company-specific tools
- Changing output formats or destinations
