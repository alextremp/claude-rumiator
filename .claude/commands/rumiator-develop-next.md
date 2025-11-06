Automatically select and develop the next highest-priority task that's ready for development.

Prerequisites:
- At least one task with status "ready-for-development" must exist

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Read .rumiator/config.yml to get current iteration number
2. Scan docs/iterations/iteration-XX/tasks/ (where XX is current iteration) for all tasks with status "ready-for-development"
3. If no tasks are ready:
   - Check if there are tasks in earlier stages
   - Display status breakdown (X pending analysis, Y in progress, Z done)
   - Suggest appropriate command:
     * If pending-technical-analysis: /rumiator-analyze-tech all
     * If all done: Congratulate user, suggest next iteration
3. If tasks are ready, select the next task to develop:
   - Sort by:
     1. Priority (critical > high > medium > low)
     2. Dependencies (tasks with no dependencies first)
     3. Complexity (low complexity first for quick wins)
     4. Creation date (older first)
   - Pick the first task from sorted list
4. Display selected task:
   - Task ID, title, priority
   - Complexity estimate
   - Brief overview from functional spec
5. Ask user if they want to proceed with this task or choose a different one
6. If user confirms, execute /rumiator-develop [task-id] internally
7. After development completes:
   - Display summary
   - Ask if user wants to develop the next task immediately
   - If yes, repeat from step 1
   - If no, show progress summary

Progress summary should include:
- Total tasks: X
- Done: Y (Z%)
- In progress: A
- Ready for development: B
- Pending analysis: C

Important:
- Respect dependencies (don't start task if dependency is not done)
- Give user control (always ask before proceeding)
- Show progress to keep user informed
