Display a comprehensive status dashboard of the current project and its tasks.

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Read .rumiator/config.yml to get project metadata and current iteration number
2. Scan all tasks in docs/iterations/iteration-XX/tasks/ directory (where XX is the current iteration)
3. Categorize tasks by type:
   - Features (type: feature or missing type field)
   - Bugs (type: bug)
   - Architecture Reviews (type: architecture-review)
4. Calculate statistics for each category:
   - Total tasks by type
   - Tasks by status (draft, pending-technical-analysis, ready-for-development, in-progress, in-review, blocked, done, pending-architecture-review)
   - Tasks by priority (critical, high, medium, low)
   - Tasks by complexity (low, medium, high)
   - Tasks by iteration
   - Completion percentage (for features only)
5. For bugs specifically:
   - Count by severity (critical, high, medium, low)
   - Count by status
   - Identify high-priority bugs
6. For architecture reviews:
   - Group by related ADR
   - Show affected tasks
7. Identify blockers:
   - List all tasks with status "blocked"
   - Show blocker details from task YAML
   - Highlight if blocker is architecture-related
8. Display dashboard in a clear, formatted way:

```
═══════════════════════════════════════════════════
  RUMIATOR PROJECT STATUS
═══════════════════════════════════════════════════

Project: [Name]
Current Iteration: [X] of [Y]
Feature Progress: ██████░░░░ 60% (12/20 features completed)

FEATURES BY STATUS
───────────────────────────────────────────────────
✓ Done                          12
⚙ In Progress                    2
⏸ In Review                      1
✋ Blocked                        1
→ Ready for Development          2
📋 Pending Technical Analysis    1
📝 Pending Business Analysis     1
🏗️ Pending Architecture Review   0
📄 Draft                         0
───────────────────────────────────────────────────
Total Features                  20

BUGS
───────────────────────────────────────────────────
By Severity:
  🔴 Critical                    1
  🟠 High                        2
  🟡 Medium                      3
  🟢 Low                         1

By Status:
  ✓ Fixed (Done)                 2
  ⚙ In Progress                  1
  → Ready to Fix                 2
  📋 Needs Analysis              2
  📄 New (Draft)                 1
───────────────────────────────────────────────────
Total Bugs                       7

Critical Bugs Requiring Attention:
  🔴 TASK-015: Login fails on Safari (ready-to-fix)
  🟠 TASK-016: Data loss on logout (in-progress)
  🟠 TASK-019: Performance degradation (needs-analysis)

ARCHITECTURE REVIEWS
───────────────────────────────────────────────────
→ TASK-040: Review User Auth (ADR-003)
   Status: pending-technical-analysis
   Affects: TASK-012, TASK-015

→ TASK-041: Review API Design (ADR-005)
   Status: ready-for-development
   Affects: TASK-020, TASK-023
───────────────────────────────────────────────────
Total Reviews                    2

TASKS BY PRIORITY (All Types)
───────────────────────────────────────────────────
🔴 Critical                      3 (1 bug, 2 features)
🟠 High                         11 (2 bugs, 9 features)
🟡 Medium                       12 (3 bugs, 9 features)
🟢 Low                           3 (1 bug, 2 features)

BLOCKERS
───────────────────────────────────────────────────
⚠ TASK-005: [FEATURE] User authentication
   Issue: API endpoint specification unclear
   Needs: Clarification on OAuth provider
   Impact: high

⚠ TASK-023: [FEATURE] Payment Integration
   Issue: Architecture change (ADR-003 supersedes ADR-001)
   Needs: Update technical spec with new approach
   Impact: high

NEXT ACTIONS
───────────────────────────────────────────────────
Priority 1: Fix critical bugs
  → TASK-015: Login fails on Safari (/rumiator-develop TASK-015)

Priority 2: Resolve blockers
  → TASK-005: Clarify OAuth provider requirements
  → TASK-023: Update spec for new architecture

Priority 3: Continue development
  → TASK-007: Dashboard UI (/rumiator-develop TASK-007)

Priority 4: Architecture reviews
  → TASK-040, TASK-041 (/rumiator-develop TASK-040)

Suggested commands:
  - /rumiator-develop TASK-015 (fix critical bug)
  - /rumiator-triage-bugs (review all bugs)
  - /rumiator-develop-next (continue with features)
═══════════════════════════════════════════════════
```

9. If there are blockers, highlight them prominently and indicate if architecture-related
10. Suggest next action based on current state (prioritized):
    - **Critical bugs**: Fix immediately (/rumiator-develop TASK-XXX)
    - **High-severity bugs**: Review and plan (/rumiator-triage-bugs)
    - **Architecture blockers**: Review architecture (/rumiator-review-architecture)
    - **Regular blockers**: Address blockers first
    - **Architecture reviews**: Complete reviews (/rumiator-develop TASK-XXX)
    - **Tasks ready**: Continue development (/rumiator-develop-next)
    - **Tasks pending analysis**: Run analysis (/rumiator-analyze-business or /rumiator-analyze-tech)
    - **Iteration complete**: Celebrate and suggest planning next iteration
11. Optionally display quick stats summary:
    ```
    Quick Health Check:
    ✓ Features on track: [percentage]
    ⚠ Bugs need attention: [count]
    🏗️ Architecture stable: Yes/No (based on blockers/reviews)
    📈 Velocity: [tasks completed this iteration]
    ```

Important:
- Keep the display clear and scannable
- Use emojis/symbols for visual clarity
- Separate features, bugs, and architecture reviews clearly
- Show bug severity prominently
- Highlight critical bugs requiring immediate attention
- Indicate architecture-related blockers
- Make it easy to understand project health at a glance
- Prioritize next actions: bugs > blockers > features
- Show relationships between architecture reviews and affected tasks
