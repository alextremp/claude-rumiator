Challenge an architectural decision and propose alternatives when development reveals issues.

Usage:
- /rumiator-review-architecture [ADR-XXX] - Review specific ADR
- /rumiator-review-architecture - Review all blockers/issues for architecture problems

Prerequisites:
- Tasks are located in docs/iterations/iteration-XX/tasks/ (where XX is current iteration of .rumiator/config.yml)

When to use:
- Development reveals current approach is unworkable
- Multiple bugs indicate architectural flaw
- New requirements make current decision obsolete
- Technical debt from original decision is too high
- Performance/scalability issues with current approach

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

### Phase 1: Identify What to Review

1. If ADR-XXX specified:
   - Validate ADR exists in docs/adr/
   - Read the ADR
   - Skip to Phase 2

2. If no ADR specified, scan for candidates:
   - Find all tasks with status "blocked"
   - Find all bugs (type: bug) with high/critical severity
   - Find tasks with notes containing keywords: "architecture", "design issue", "refactor needed"
   - Find ADRs referenced in these tasks

3. Display findings:
   ```
   Potential Architecture Issues Detected
   ======================================

   Blocked Tasks Related to Architecture:
   - TASK-XXX: [Title] - Blocker: [reason]
     References: ADR-001, ADR-003

   High-Severity Bugs Indicating Architecture Issues:
   - TASK-YYY: [Title] - Pattern: [description]
     Related Feature: [feature]

   ADRs Referenced in Problematic Tasks:
   - ADR-001: [Title] - Referenced by X tasks
   - ADR-003: [Title] - Referenced by Y tasks
   ```

4. Ask user which ADR to review, or let them specify a different one

### Phase 2: Understand the Problem

5. Read the selected ADR completely:
   - Original decision
   - Context at time of decision
   - Alternatives that were rejected
   - Expected consequences
   - Current status

6. Ask user detailed questions:
   - **What problem did you encounter?**
     "Describe specifically what went wrong with the current approach"

   - **What makes the current decision unworkable?**
     "Is it a technical limitation, performance issue, complexity, maintenance burden?"

   - **What changed since the original decision?**
     "New requirements? Scale? Technology landscape?"

   - **What have you tried to work around it?**
     "Any workarounds attempted? Why didn't they work?"

   - **What's the impact of NOT changing?**
     "Can the project succeed with current approach? What's the cost?"

7. Document the trigger for review:
   - Create docs/adr/reviews/ADR-XXX-review-YYYY-MM-DD.md
   - Capture user's answers
   - Link to related tasks/bugs

### Phase 3: Identify Affected Components

8. Scan project for dependencies on this ADR:

   **In Technical Specs:**
   - Search docs/features/*/technical.md for ADR-XXX references
   - Extract relevant sections

   **In Task YAMLs:**
   - Find all tasks with related_adrs containing ADR-XXX
   - Group by status (done, in-progress, pending)

   **In Code (if available):**
   - Search for TODO/FIXME comments mentioning the ADR
   - Identify modules/files implementing the decision

   **In Other ADRs:**
   - Find ADRs that reference or build upon this one
   - Check for cascade effects

9. Display dependency analysis:
   ```
   Impact Analysis: ADR-XXX
   ========================

   Technical Specs Affected: X files
   - docs/features/auth/technical.md (lines 45-67)
   - docs/features/api/technical.md (lines 12-34)

   Tasks Affected: Y tasks
   - Done: 3 tasks (may need rework)
   - In Progress: 2 tasks (can be adapted)
   - Pending: 5 tasks (need new specs)

   Dependent ADRs: Z
   - ADR-005: Builds upon ADR-XXX for [topic]

   Estimated Rework: [architect's assessment]
   ```

### Phase 4: Generate Alternatives

10. Launch architect agent in "Architecture Review Mode":
    Provide architect with:
    - Original ADR
    - User's problem description
    - Changed context/requirements
    - Affected components analysis
    - Constraints (time, budget, skills)

11. Architect should generate 2-4 alternative approaches:
    For each alternative:
    - **Description**: What is the proposed approach?
    - **How it solves the problem**: Address user's specific issues
    - **Migration path**: How to get from current to new approach
    - **Impact on existing work**:
      * Tasks that need rework
      * Code that needs refactoring
      * Specs that need rewriting
    - **Effort estimate**: Time and complexity
    - **Pros**: Benefits beyond solving current problem
    - **Cons**: Trade-offs and new challenges
    - **Risk assessment**: What could go wrong?

12. Include option to keep current approach:
    - **Alternative 0: Stay the Course**
      * Work around the issues
      * Accept technical debt
      * Document limitations
      * Estimated workaround effort

### Phase 5: Decision

13. Present alternatives to user in clear comparison:
    ```
    Alternative 1: [Name]
    ---------------------
    Solves: [problem]
    Effort: [estimate]
    Pros: [benefits]
    Cons: [trade-offs]
    Risk: [low/medium/high]

    Affected:
    - X tasks need rework
    - Y specs need updates
    - Estimated migration: [time]

    [Repeat for each alternative]
    ```

14. Ask user to select preferred approach:
    - Present as multiple choice with clear criteria
    - Allow time for user to think/discuss with team
    - Offer to provide more detail on any alternative
    - Can ask architect follow-up questions

15. User selects alternative (or decides to stay with current)

### Phase 6: Document Decision

16. If user chooses to change architecture:

    a. Determine next ADR number:
       - Scan docs/adr/ for highest ADR-XXX
       - Increment for new ADR

    b. Create new ADR:
       - Use template from .rumiator/templates/adr.md
       - Title: Reflects the new decision
       - Status: "Accepted"
       - Context: Include original problem + review findings
       - Decision: The chosen alternative
       - Consequences: From architect's analysis
       - Alternatives Considered: List all options from review
       - References: Link to original ADR being superseded
       - Supersedes: ADR-XXX

    c. Update original ADR:
       - Change status to "Superseded by ADR-YYY"
       - Add note at top: "⚠️ SUPERSEDED: See ADR-YYY for current decision"
       - DO NOT modify anything else (ADRs are historical records)

    d. Update architecture.md:
       - Update "Key Architectural Decisions" section
       - Update any diagrams affected
       - Add timeline entry: "YYYY-MM-DD: Revised [topic] (ADR-XXX → ADR-YYY)"

17. If user chooses to keep current approach:
    - Update original ADR with "Reaffirmed on YYYY-MM-DD"
    - Document the review in notes section
    - Create mitigation plan for original issues
    - Update affected tasks with workaround guidance

### Phase 7: Propagate Changes

18. Launch /rumiator-propagate-architecture-change ADR-YYY
    - This command will handle updating all affected tasks and specs
    - See rumiator-propagate-architecture-change.md for details

19. Display summary:
    ```
    Architecture Review Complete
    ============================

    Original ADR: ADR-XXX ([Title])
    Status: Superseded

    New ADR: ADR-YYY ([New Title])
    Status: Accepted

    Decision: [Brief description]

    Impact:
    - X tasks need review
    - Y specs need updates
    - Z ADRs affected

    Next Steps:
    1. Review propagated changes (/rumiator-status)
    2. Update technical specs for affected tasks
    3. Communicate changes to team
    4. Update iteration plan if needed
    ```

20. Ask if user wants to:
    - Review the propagated changes now
    - Start updating technical specs
    - Communicate the change to stakeholders
    - Continue with other work

Important:
- Never delete or heavily modify old ADRs - they're historical records
- Document why the original decision was wrong (learning opportunity)
- Consider partial adoption of new approach (phased migration)
- Update iteration plans if changes affect timeline
- Communicate architectural changes clearly to all stakeholders
- Review should be thorough but not paralyzing - decide and move forward
- Sometimes the right answer is to keep current approach despite issues
- Create architecture review tasks (type: architecture-review) for rework
- Balance ideal architecture with practical constraints

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-review-architecture.md`
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
