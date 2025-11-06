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

1. Read .rumiator/config.yml to get current iteration number
2. Parse optional related-task-id from command
3. If related-task-id provided:
   - Validate it exists in docs/iterations/iteration-XX/tasks/ (where XX is current iteration)
   - Read the task to understand context
   - Extract feature name for categorization
4. If no related-task-id, ask user:
   - Which feature is affected? (list existing features from docs/features/)
   - Or is this a new/general bug?
5. Ask user for bug details:
   - **Title**: Brief description of the bug
   - **Severity**: low | medium | high | critical
   - **Steps to reproduce**: How to trigger the bug
   - **Expected behavior**: What should happen
   - **Actual behavior**: What actually happens
   - **Root cause** (if known): Optional, can be determined during analysis
6. Determine next TASK-ID:
   - Scan docs/iterations/iteration-XX/tasks/ (where XX is current iteration) for highest existing ID
   - Increment to get next ID (TASK-XXX)
7. Determine iteration placement based on severity:
   - **Critical/High**: Add to current iteration with high priority
   - **Medium**: Add to current iteration with medium priority
   - **Low**: Add to backlog (current iteration + 1) with low priority
8. Create docs/iterations/iteration-XX/tasks/TASK-XXX.yml (where XX is the target iteration):
   - type: bug
   - Set all bug_info fields
   - Set status: "draft"
   - Set priority based on severity
   - Add related_tasks if applicable
9. Create bug analysis directory:
   - docs/features/[feature]/bugs/ (if doesn't exist)
   - Create placeholder: docs/features/[feature]/bugs/BUG-XXX-analysis.md
10. If related to existing TASK(s):
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
    - **Yes**: Update status to "pending-technical-analysis"
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

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-create-bug.md`
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
