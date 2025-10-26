Create a bug report that can be tracked alongside feature tasks in the current iteration.

Usage:
- /rumiator-create-bug [related-task-id] (optional)
- /rumiator-create-bug (create standalone bug)

Prerequisites:
- .rumiator/ directory structure must exist

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Parse optional related-task-id from command
2. If related-task-id provided:
   - Validate it exists in .rumiator/tasks/
   - Read the task to understand context
   - Extract feature name for categorization
3. If no related-task-id, ask user:
   - Which feature is affected? (list existing features from docs/features/)
   - Or is this a new/general bug?
4. Ask user for bug details:
   - **Title**: Brief description of the bug
   - **Severity**: low | medium | high | critical
   - **Steps to reproduce**: How to trigger the bug
   - **Expected behavior**: What should happen
   - **Actual behavior**: What actually happens
   - **Root cause** (if known): Optional, can be determined during analysis
5. Determine next TASK-ID:
   - Scan .rumiator/tasks/ for highest existing ID
   - Increment to get next ID (TASK-XXX)
6. Determine iteration placement based on severity:
   - **Critical/High**: Add to current iteration with high priority
   - **Medium**: Add to current iteration with medium priority
   - **Low**: Add to backlog (current iteration + 1) with low priority
7. Create .rumiator/tasks/TASK-XXX.yml:
   - type: bug
   - Set all bug_info fields
   - Set status: "draft"
   - Set priority based on severity
   - Add related_tasks if applicable
8. Create bug analysis directory:
   - docs/features/[feature]/bugs/ (if doesn't exist)
   - Create placeholder: docs/features/[feature]/bugs/BUG-XXX-analysis.md
9. If related to existing TASK(s):
   - Read each related task YAML
   - Add this bug ID to their related_bugs array
   - Add note about the bug
10. Launch functional-analyst agent to:
    - Analyze if it's a bug or missing requirement
    - Determine impact on existing functional specs
    - Fill in bug analysis document with:
      * Root cause analysis
      * Impact assessment
      * Suggested fix approach
      * Whether functional spec needs update
11. Ask user if immediate fix is needed:
    - **Yes, urgent**: Update status to "pending-technical-analysis"
    - **Yes, but can wait**: Keep as "pending-business-analysis"
    - **No, backlog**: Keep as "draft" in next iteration
12. Display bug summary:
    - Bug ID and title
    - Severity and priority
    - Iteration placement
    - Related tasks
    - Status
    - Path to bug analysis document
13. Suggest next steps based on urgency:
    - Critical/High: "Run /rumiator-analyze-tech TASK-XXX to create fix plan"
    - Medium: "Run /rumiator-analyze-business TASK-XXX for detailed analysis"
    - Low: "Bug added to backlog. Review during next /rumiator-triage-bugs"
14. Ask if user wants to:
    - Create another bug report
    - Start fixing this bug immediately
    - Continue with other work

Important:
- Bugs share the same ID sequence as features (TASK-XXX)
- Critical bugs should interrupt current work
- Document root cause if known, but it's OK to determine it during analysis
- Bug severity ≠ priority (severity is impact, priority is urgency)
- Link bugs to related tasks bidirectionally
- Create bug analysis document even if minimal initially
