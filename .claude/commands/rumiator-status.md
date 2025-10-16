Display a comprehensive status dashboard of the current project and its tasks.

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Read .rumiator/config.yml to get project metadata
2. Scan all tasks in .rumiator/tasks/ directory
3. Calculate statistics:
   - Total tasks
   - Tasks by status (draft, pending-business-analysis, pending-technical-analysis, ready-for-development, in-progress, in-review, blocked, done)
   - Tasks by priority (critical, high, medium, low)
   - Tasks by complexity (low, medium, high)
   - Tasks by iteration
   - Completion percentage
4. Identify blockers:
   - List all tasks with status "blocked"
   - Show blocker details from task YAML
5. Display dashboard in a clear, formatted way:

```
═══════════════════════════════════════════════════
  RUMIATOR PROJECT STATUS
═══════════════════════════════════════════════════

Project: [Name]
Current Iteration: [X] of [Y]
Progress: ██████░░░░ 60% (12/20 tasks completed)

TASKS BY STATUS
───────────────────────────────────────────────────
✓ Done                          12
⚙ In Progress                    2
⏸ In Review                      1
✋ Blocked                        1
→ Ready for Development          2
📋 Pending Technical Analysis    1
📝 Pending Business Analysis     1
📄 Draft                         0
───────────────────────────────────────────────────
Total                           20

TASKS BY PRIORITY
───────────────────────────────────────────────────
🔴 Critical                      2
🟠 High                          8
🟡 Medium                        7
🟢 Low                           3

TASKS BY COMPLEXITY
───────────────────────────────────────────────────
Simple (Low)                     6
Moderate (Medium)               10
Complex (High)                   4

BLOCKERS
───────────────────────────────────────────────────
⚠ TASK-005: User authentication
   Issue: API endpoint specification unclear
   Needs: Clarification on OAuth provider

NEXT ACTIONS
───────────────────────────────────────────────────
1. Resolve blocker in TASK-005
2. Develop TASK-007 (ready-for-development)
3. Analyze TASK-012 (pending-technical-analysis)

Suggested command: /rumiator-develop-next
═══════════════════════════════════════════════════
```

6. If there are blockers, highlight them prominently
7. Suggest next action based on current state:
   - If tasks blocked: Address blockers first
   - If tasks ready: /rumiator-develop-next
   - If tasks pending analysis: /rumiator-analyze-business or /rumiator-analyze-tech
   - If iteration complete: Celebrate and suggest planning next iteration

Important:
- Keep the display clear and scannable
- Use emojis/symbols for visual clarity
- Highlight blockers and next actions
- Make it easy to understand project health at a glance
