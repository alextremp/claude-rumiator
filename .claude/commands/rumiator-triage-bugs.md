Review all pending bugs and prioritize them for the current or upcoming iterations.

Usage:
- /rumiator-triage-bugs
- /rumiator-triage-bugs --all (include bugs from all iterations)

Prerequisites:
- At least one bug task (type: bug) must exist

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Read .rumiator/config.yml to get current iteration number
2. Scan docs/iterations/iteration-XX/tasks/ (where XX is current iteration) for all TASK-XXX.yml files with type: bug
3. Filter bugs based on command flag:
   - Default: Only bugs for current iteration
   - --all: All bugs regardless of iteration
4. Group bugs by multiple dimensions:

   **By Severity:**
   - Critical (requires immediate attention)
   - High (should be fixed in current iteration)
   - Medium (can be scheduled)
   - Low (backlog)

   **By Status:**
   - draft (not yet analyzed)
   - pending-technical-analysis
   - ready-for-development
   - in-progress
   - blocked

   **By Feature:**
   - Group by feature name
   - Identify patterns (same feature has many bugs = quality issue)

   **By Related Tasks:**
   - Show which features are affected
   - Identify dependencies

5. Display initial bug inventory:
   ```
   Bug Triage Summary
   ==================

   Total Bugs: X
   - Critical: X (Y in current iteration)
   - High: X
   - Medium: X
   - Low: X

   By Status:
   - Draft: X
   - Pending Analysis: X
   - Ready for Dev: X
   - In Progress: X
   - Blocked: X

   By Feature:
   - auth: X bugs
   - dashboard: X bugs
   - payments: X bugs
   ```

6. Launch functional-analyst AND architect agents in parallel to:

   **functional-analyst:**
   - Review bug descriptions
   - Identify missing functional specs
   - Assess business impact of each bug
   - Recommend whether bug reveals missing requirements

   **architect:**
   - Analyze if bugs indicate architectural issues
   - Identify patterns suggesting design flaws
   - Recommend if any bug needs /rumiator-review-architecture
   - Assess technical debt implications

7. Wait for both agents to complete, then compile findings:
   - List bugs that may indicate architecture problems
   - List bugs that reveal missing requirements
   - List bugs that are quick wins (low effort, high impact)
   - List bugs that should be deferred

8. Ask user to prioritize:
   Present bugs grouped by recommendation:

   **Recommended for Immediate Fix (Current Iteration):**
   - TASK-XXX: [Title] - Severity: Critical - Reason: [why urgent]
   - TASK-YYY: [Title] - Severity: High - Reason: [why urgent]

   **Recommended for Architecture Review:**
   - TASK-ZZZ: [Title] - Indicates: [architectural issue]
   - Suggest: Run /rumiator-review-architecture ADR-XXX

   **Quick Wins (Low effort, high impact):**
   - TASK-AAA: [Title] - Estimated: 1-2 hours

   **Can Wait (Next Iteration):**
   - TASK-BBB: [Title] - Reason: [why can wait]

9. Ask user to confirm prioritization or adjust:
   - Which bugs to fix in current iteration?
   - Which bugs to defer to next iteration?
   - Which bugs need architecture review first?
   - Which bugs to close as "won't fix"?

10. Update tasks based on user decisions:
    - Bugs for current iteration: Update iteration field, set appropriate status
    - Bugs for next iteration: Update iteration field to current + 1
    - Bugs needing architecture review: Add note, keep in draft/blocked
    - Won't fix: Update status to a new state "rejected" with reason in notes

11. Read current iteration number from .rumiator/config.yml (iterations.current)
12. Update current iteration plan:
    - Create docs/iterations/iteration-[XX]/bug-triage-report.md
    - Where XX is the current iteration number padded to 2 digits (01, 02, etc.)
    - Document:
      * Date of triage
      * Number of bugs reviewed
      * Bugs added to iteration
      * Bugs deferred
      * Architecture issues identified
      * Quick wins identified
      * Next steps

13. Display final prioritized bug list:
    ```
    Iteration X Bug Plan
    ====================

    Priority Fixes (X bugs):
    1. TASK-XXX: [Critical] [Title] - Status: ready-for-development
    2. TASK-YYY: [High] [Title] - Status: pending-technical-analysis

    Quick Wins (X bugs):
    1. TASK-AAA: [Medium] [Title] - Estimated: 2 hours

    Deferred to Next Iteration (X bugs):
    1. TASK-BBB: [Low] [Title]

    Need Architecture Review (X bugs):
    1. TASK-ZZZ: [Title] - Related ADR: ADR-XXX
    ```

14. Suggest next steps:
    - For critical bugs: "/rumiator-develop TASK-XXX to fix immediately"
    - For pending analysis: "/rumiator-analyze-business TASK-XXX" or "/rumiator-analyze-tech TASK-XXX"
    - For architecture issues: "/rumiator-review-architecture ADR-XXX"
    - For quick wins: "Batch them together for efficient development"

15. Ask if user wants to:
    - Start fixing bugs now
    - Review architecture issues first
    - Continue with planned work and fix bugs later

Important:
- Triage should happen regularly (e.g., weekly or before each iteration)
- Critical bugs always take priority over new features
- Pattern recognition is key: multiple bugs in same area = deeper issue
- Don't just fix symptoms: identify and address root causes
- Quick wins build momentum and improve quality perception
- Architecture issues should trigger review, not just patches
- Document triage decisions for future reference
- Balance bug fixes with feature development (suggest 70/30 or 80/20 split)
