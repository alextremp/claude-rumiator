Generate a comprehensive progress report for the current iteration.

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Read .rumiator/config.yml to get project and iteration info
2. Read docs/product/product-plan.md to get iteration goals
3. Scan all tasks for the current iteration
4. Calculate detailed metrics:
   - Iteration goals vs completed work
   - Velocity (tasks completed per time period)
   - Complexity delivered (sum of completed task complexities)
   - Blockers encountered and resolved
   - Quality metrics (if QA was performed)
5. Create iteration directory if it doesn't exist: docs/iterations/iteration-[XX]/
   - Where XX is the current iteration number (padded to 2 digits: 01, 02, etc.)
6. Generate report document at docs/iterations/iteration-[XX]/report.md:

```markdown
# Iteration [X] Progress Report

**Generated**: [Date]
**Iteration Period**: [Start] - [End]
**Status**: In Progress | Completed

## Iteration Goals
- [Goal 1 from product plan]
- [Goal 2 from product plan]

## Progress Summary
- **Completion**: 12/15 tasks (80%)
- **Velocity**: 2.4 tasks/week
- **Complexity Delivered**: 8 low, 3 medium, 1 high

## Completed Tasks
| Task ID | Title | Complexity | Completed |
|---------|-------|------------|-----------|
| TASK-001 | User Auth | Medium | 2025-10-05 |
| ... | ... | ... | ... |

## In Progress
| Task ID | Title | Status | Assignee |
|---------|-------|--------|----------|
| TASK-008 | Dashboard | in-progress | developer-frontend |

## Blocked Tasks
| Task ID | Title | Blocker | Impact |
|---------|-------|---------|--------|
| TASK-010 | Payment | API access pending | High |

## Risks & Mitigation
- **Risk**: [Description]
  - **Status**: Mitigated | Ongoing | Escalated
  - **Action taken**: [What was done]

## Decisions Made
- **ADR-001**: Choice of React framework - [summary]
- **Business Decision**: Postponed social login to iteration 2

## Lessons Learned
- What went well: [...]
- What to improve: [...]
- Adjustments for next iteration: [...]

## Metrics
- **Test Coverage**: 85%
- **Code Quality**: [Linter score, etc.]
- **Bugs Found**: 3 (2 resolved, 1 open)

## Next Iteration Planning
- Carry over: [X tasks]
- New features: [To be planned]
- Focus areas: [Based on learnings]
```

7. Display summary to user:
   - Completion percentage
   - Highlight achievements
   - Note any concerns
   - Suggest actions for next iteration
8. Ask if user wants to:
   - Review the full report
   - Plan next iteration
   - Address blockers

Important:
- Be honest about progress (don't sugarcoat issues)
- Highlight both successes and areas for improvement
- Make recommendations actionable
- Keep the report factual and data-driven

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-report.md`
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
