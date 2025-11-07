Propagate architecture changes to all affected tasks, specs, and documentation.

Usage:
- /rumiator-propagate-architecture-change ADR-XXX
- Called automatically by /rumiator-review-architecture

This command handles the cascading updates needed when an architectural decision changes.

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

### Phase 1: Understand the Change

1. Read the new ADR (ADR-XXX):
   - Extract the decision
   - Identify what's changing from previous approach
   - Note key implementation details
   - Check if it supersedes another ADR

2. If ADR supersedes another (ADR-YYY):
   - Read the old ADR for comparison
   - Document what's different
   - Understand migration path

3. Read docs/product/architecture.md:
   - Understand overall system context
   - See how this decision fits

### Phase 2: Discover Affected Components

4. Search for direct references:

   **In Technical Specs:**
   ```
   Search: docs/features/*/technical.md
   Pattern: "ADR-YYY" or specific technical details from old ADR
   ```

   **In Task YAMLs:**
   ```
   Read .rumiator/config.yml to get current iteration
   Search: docs/iterations/iteration-XX/tasks/*.yml (where XX is current iteration)
   Pattern: related_adrs containing old ADR-YYY
   Also search: tasks with related features
   ```

   **In Other ADRs:**
   ```
   Search: docs/adr/*.md
   Pattern: References to ADR-YYY
   ```

5. Launch architect agent to identify implicit dependencies:
   - Tasks that might be affected even without explicit reference
   - Components that implement the old approach
   - Features that will benefit from the new approach
   - Potential conflicts with existing design decisions

6. Compile comprehensive affected items list:
   ```
   Affected Components Report
   ==========================

   Direct References:
   - 5 technical specs
   - 8 tasks
   - 2 dependent ADRs

   Implicit Dependencies (architect analysis):
   - 3 additional tasks (implement old pattern)
   - 1 feature (could leverage new approach)

   Total Impact:
   - 11 tasks to review
   - 5 specs to update
   - 2 ADRs to update
   ```

### Phase 3: Categorize and Prioritize

7. Group affected tasks by status:

   **Group A: Tasks Already Completed (status: done)**
   - These were built with old approach
   - May need rework depending on severity
   - Create architecture-review tasks for each

   **Group B: Tasks In Progress**
   - Currently being implemented
   - Need to decide: continue or pivot?
   - High risk of wasted work

   **Group C: Ready for Development**
   - Have technical specs but not started
   - Specs need updating before development

   **Group D: Pending Analysis**
   - Pending-technical-analysis or earlier
   - Can incorporate new approach cleanly
   - Update during normal analysis flow

   **Group E: Blocked Tasks**
   - May be unblocked by new approach
   - Priority for review

8. Display categorized impact:
   ```
   Task Impact Analysis
   ====================

   Group A - Completed (3 tasks):
   - TASK-012: User Authentication
     Impact: Medium - May need to refactor session handling
     Action: Create review task

   - TASK-015: API Endpoints
     Impact: Low - Mostly compatible, minor updates
     Action: Create review task

   Group B - In Progress (2 tasks):
   - TASK-023: Payment Integration [Developer: Jane]
     Impact: High - Using old pattern extensively
     Recommendation: Pause and redesign

   Group C - Ready for Dev (5 tasks):
   - TASK-025 through TASK-029
     Impact: High - All specs need revision
     Action: Update specs before development

   Group D - Pending Analysis (2 tasks):
   - TASK-030, TASK-031
     Impact: Medium - Will incorporate during analysis
     Action: Note in task for analyst/architect

   Group E - Blocked (1 task):
   - TASK-018: Real-time Updates
     Impact: Positive - New approach may unblock
     Action: Re-analyze with new architecture
   ```

### Phase 4: User Decisions

9. For Group A (Completed tasks):
   Ask user:
   ```
   3 completed tasks may need rework based on new architecture.

   Options:
   1. Create architecture-review tasks for all (recommended for critical changes)
   2. Create review tasks only for high-impact items
   3. Document as technical debt, address in future refactor
   4. Leave as-is (old approach still valid for these)

   Which option do you prefer?
   ```

10. For Group B (In Progress):
    For each task, ask:
    ```
    TASK-023 is currently being developed with old approach.

    Options:
    1. Pause task, update spec, restart with new approach
    2. Complete with old approach, create follow-up refactor task
    3. Try to adapt in-flight (risky)

    What should we do?
    ```

11. For Group C (Ready but not started):
    Ask user:
    ```
    5 tasks are ready for development but specs use old architecture.

    Shall I:
    1. Auto-update all specs now (launches architect for each)
    2. Update on-demand as tasks are picked up
    3. Update critical tasks now, others later

    Which approach?
    ```

12. For Group D (Pending Analysis):
    ```
    2 tasks are still in analysis phase.
    I'll add notes to ensure new architecture is used.
    No action needed from you.
    ```

13. For Group E (Blocked):
    ```
    TASK-018 was blocked due to limitations in old approach.

    The new architecture may resolve this.

    Shall I:
    1. Re-analyze immediately with new architecture
    2. Keep blocked, review later
    ```

### Phase 5: Execute Updates

14. For each decision made, execute:

    **Creating Architecture Review Tasks:**
    ```
    For each completed task needing review:
    1. Read .rumiator/config.yml to get current iteration
    2. Get next TASK-ID
    3. Create new docs/iterations/iteration-XX/tasks/TASK-XXX.yml (where XX is current iteration):
       - type: architecture-review
       - title: "Review [original-task] against ADR-XXX"
       - architecture_review:
           related_adr: "ADR-XXX"
           affected_tasks: ["TASK-YYY"]
           reason: "[Brief explanation]"
       - status: "pending-technical-analysis"
       - priority: based on impact (high/medium/low)
       - iteration: current or next
    4. Add reference in original task's related_bugs/notes
    ```

    **Pausing In-Progress Tasks:**
    ```
    1. Read task YAML
    2. Update status to "blocked"
    3. Add to blockers:
       - issue: "Architecture change (ADR-XXX supersedes ADR-YYY)"
         needed: "Update technical spec with new approach"
         workaround: "None"
         impact: "high"
         reported: [today]
         owner: "architect"
    4. Notify user to inform developer
    ```

    **Updating Pending Tasks:**
    ```
    1. Read task YAML
    2. Add note: "⚠️ Use ADR-XXX (new architecture), not ADR-YYY"
    3. Add ADR-XXX to related_adrs
    4. Keep status as-is
    ```

15. Update Technical Specs (if auto-update chosen):

    For each spec in Group C:
    1. Read current technical spec
    2. Launch architect agent for that specific task:
       - Provide: current spec + new ADR + task YAML
       - Ask: "Update this spec to use new architecture from ADR-XXX"
       - Architect should:
         * Mark sections to change with <!-- UPDATED: [reason] -->
         * Rewrite affected sections
         * Add reference to ADR-XXX
         * Remove or update ADR-YYY references
    3. Save updated spec
    4. Update task YAML:
       - Update related_adrs
       - Add note about spec update
       - Set updated: [today]

16. Update Dependent ADRs:

    For each ADR that referenced old ADR:
    1. Read dependent ADR
    2. Check if it's still valid with new decision
    3. Options:
       - Update to reference ADR-XXX instead
       - Mark as "Needs Review" if conflict
       - Leave as-is if orthogonal
    4. Ask user to confirm approach for each

### Phase 6: Communication and Documentation

17. Read current iteration number from .rumiator/config.yml (iterations.current)
18. Create architecture change report:
    ```
    File: docs/iterations/iteration-[XX]/architecture-change-ADR-XXX.md
    Where XX is the current iteration number padded to 2 digits (01, 02, etc.)

    Content:
    ---
    # Architecture Change: ADR-XXX

    **Date:** YYYY-MM-DD
    **Previous ADR:** ADR-YYY
    **New ADR:** ADR-XXX

    ## Change Summary
    [What changed and why]

    ## Impact Assessment

    ### Tasks Updated
    - X tasks marked for architecture review
    - Y tasks blocked pending spec updates
    - Z tasks noted for future analysis

    ### Specifications Updated
    - List of updated technical specs

    ### Estimated Impact
    - Time: [estimate]
    - Complexity: [low/medium/high]
    - Risk: [assessment]

    ## Actions Taken
    1. [Action 1]
    2. [Action 2]
    ...

    ## Next Steps
    1. [Step 1]
    2. [Step 2]
    ...

    ## Communication
    - Team notified: [date]
    - Stakeholders informed: [who]
    - Documentation updated: [what]
    ---
    ```

19. Update iteration plan:
    - Add architecture review tasks to current iteration
    - Adjust timeline if needed
    - Mark potentially affected deliverables

20. Generate stakeholder communication:
    ```
    Draft Email/Message:

    Subject: Architecture Decision Update - [Brief Topic]

    Team,

    We've updated our architectural approach for [topic].

    Previous: [Old approach - ADR-YYY]
    New: [New approach - ADR-XXX]

    Reason: [Why we changed]

    Impact:
    - X tasks need review
    - Y tasks need updated specs
    - Z features will benefit

    Affected team members: [list]

    See full details: docs/adr/ADR-XXX.md

    Questions? [contact]
    ```

### Phase 7: Final Summary

21. Display comprehensive summary:
    ```
    Architecture Change Propagation Complete
    ========================================

    ADR: ADR-XXX ([Title])
    Supersedes: ADR-YYY

    Changes Propagated:
    ✓ 3 architecture-review tasks created
    ✓ 2 in-progress tasks blocked for redesign
    ✓ 5 technical specs updated
    ✓ 2 pending tasks noted for new approach
    ✓ 1 blocked task re-analyzed
    ✓ 2 dependent ADRs updated
    ✓ Architecture.md updated
    ✓ Iteration plan adjusted
    ✓ Change report created

    New Tasks Created:
    - TASK-040: Review TASK-012 (User Auth)
    - TASK-041: Review TASK-015 (API Endpoints)
    - TASK-042: Review TASK-020 (Data Layer)

    Blocked Tasks:
    - TASK-023: Payment Integration (needs redesign)
    - TASK-024: Notification System (needs redesign)

    Updated Specs:
    - docs/features/payments/technical.md
    - docs/features/notifications/technical.md
    - docs/features/analytics/technical.md

    Next Steps:
    1. Review created architecture-review tasks
    2. Communicate changes to team
    3. Update technical specs for blocked tasks
    4. Continue with other work

    Commands you can run:
    - /rumiator-status (see updated task list)
    - /rumiator-analyze-tech TASK-040 (start review)
    - /rumiator-develop-next (continue development)
    ```

22. Ask user what to do next:
    - Start architecture reviews immediately?
    - Update specs for blocked tasks?
    - Communicate to team first?
    - Continue with current work?

Important:
- Be thorough but not disruptive - balance quality with momentum
- Completed work should only be revisited if truly necessary
- In-progress work is highest risk - handle carefully
- Clear communication prevents confusion and rework
- Document everything - architecture changes are significant
- Some tasks may benefit from new approach even if not strictly "affected"
- Consider creating a migration guide if change is complex
- Update onboarding docs if architecture changes are fundamental
- Schedule follow-up review to assess if change was successful

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-propagate-architecture-change.md`
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
