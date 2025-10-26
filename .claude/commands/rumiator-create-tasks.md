Generate task definitions from the product plan for the current iteration using the Functional Analyst agent.

Prerequisites:
- docs/product/product-plan.md must exist

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
     * Determine next task ID (check existing tasks in .rumiator/tasks/)
     * Create .rumiator/tasks/TASK-XXX.yml with status "draft"
     * Create docs/features/[feature-name]/ directory
     * Set basic task metadata (title, feature, iteration, priority)
5. Display created tasks summary:
   - Task ID, title, feature name
   - Total count
6. Ask user if any obvious features are missing
7. If user mentions missing features:
   - Create additional tasks
   - Update the count
8. Update all created tasks to status "pending-business-analysis"
9. Read current iteration number from .rumiator/config.yml (iterations.current)
10. Update docs/iterations/iteration-[XX]/plan.md:
    - Add list of created tasks to "Tasks" section
    - Update "Scope" section with summary of features
    - Keep status as "In Progress"
11. Display next steps:
   - Run /rumiator-analyze-business to create functional specifications
   - Or run /rumiator-analyze-business [task-id] for a specific task

Important:
- Tasks should be granular (1-3 days of work ideally)
- Each task should have a clear, testable objective
- Priority should reflect business value and dependencies
