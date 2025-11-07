Analyze tasks and create high-level technical specifications using the Architect agent.

Usage:
- /rumiator-analyze-tech [task-id] - Analyze a specific task
- /rumiator-analyze-tech all - Analyze all tasks pending technical analysis

Prerequisites:
- Tasks must have status "pending-technical-analysis"
- Tasks must already include business requirements (summary, user_stories, acceptance_criteria)

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Parse the command argument:
   - If no argument or "all": find all tasks with status "pending-technical-analysis"
   - If task-id provided: validate it exists and has correct status
   - If invalid task-id or wrong status: show error and list valid tasks
2. For each task to analyze:
   a. Read the task YAML (which already includes business requirements)
   b. Launch the architect agent in "Technical Analysis Mode":
      - Read task business requirements (summary, user_stories, acceptance_criteria)
      - Read docs/product/architecture.md to understand current architecture
      - Read existing ADRs to understand architectural decisions
      - **Evaluate if ADR is needed**:
        * If yes: Create ADR draft and **ASK USER** to validate it
        * If user approves: Save ADR and continue
        * If user rejects: Revise or skip ADR creation
      - Create docs/features/[feature-name]/technical.md with **HIGH-LEVEL guidance**:
        * Technology stack to use
        * Architecture overview (how feature fits into system)
        * Frontend considerations: design system components, state management, routing, impact
        * Backend considerations: API approach, business logic, data access pattern, impact
        * Security considerations
        * Performance considerations
        * Architectural decisions with rationale
        * ADR references
        * Testing strategy (what types of tests needed)
        * Complexity estimate with justification
        * Implementation notes (high-level guidance, constraints, risks)
      - **DO NOT include**:
        * Specific API endpoint paths/methods
        * Request/response JSON examples
        * Data models or TypeScript interfaces
        * Database schemas or SQL
        * Code snippets or implementation examples
      - Update docs/product/architecture.md with new components/patterns if applicable
   c. **VERIFY ARCHITECTURAL DECISIONS** (Critical Step):
      - After technical spec is created, scan it for ADR references
      - For each referenced ADR:
        * Read the ADR file
        * Check its status field
        * If status is "Superseded" or "Superseded by ADR-XXX":
          ⚠️ WARN USER IMMEDIATELY:
          ```
          ⚠️ WARNING: Architecture Decision Outdated

          Technical spec references ADR-XXX which has been SUPERSEDED
          Old ADR: ADR-XXX - [Title]
          Status: Superseded by ADR-YYY
          New ADR: ADR-YYY - [New Title]

          This task may be using an outdated architectural approach.

          Options:
          1. Update spec now to use ADR-YYY (recommended)
          2. Review architecture: /rumiator-review-architecture ADR-XXX
          3. Proceed anyway (not recommended - may cause rework)

          What would you like to do?
          ```
        * If user chooses to update spec:
          - Launch architect again to revise spec with new ADR
          - Update task YAML to reference new ADR
          - Add note about architecture update
        * If user chooses to review architecture:
          - Pause this analysis
          - Suggest running /rumiator-review-architecture ADR-XXX
          - Keep task status as "pending-technical-analysis"
          - Exit this command
        * If user proceeds anyway:
          - Add warning to task YAML notes
          - Mark task with high risk
          - Continue normally
      - If all ADRs are current (status: "Accepted" or "Proposed"):
        ✓ Continue normally
   d. Update task YAML:
      - status: "ready-for-development" (or "pending-architecture-review" if issues found)
      - architect: "architect"
      - technical_spec: path to technical.md
      - estimated_complexity: low|medium|high
      - related_adrs: [list of ADR-XXX referenced in spec]
      - updated: current date
3. Display summary for each analyzed task:
   - Task ID and title
   - Complexity estimate
   - Key technologies involved
   - ADRs created/referenced (if any)
   - Path to technical spec
4. After all tasks processed, display:
   - Total tasks analyzed
   - Complexity breakdown (X low, Y medium, Z high)
   - ADRs created or referenced
   - Next steps: Run /rumiator-develop [task-id] or /rumiator-develop-next to start implementation

## Important Notes

### What Changed
- Technical specs now provide **high-level architectural guidance** only
- **NO implementation details** (API specs, data models, schemas, code) are included
- **ADRs are validated with user** before proceeding
- Developers will make implementation decisions based on guidance and business requirements
- Technical specs are **shorter and more focused** (1-2 pages vs 3-5 pages)

### Architect Responsibilities
- **Identify** which technologies and patterns to use
- **Assess** architectural impact of the feature
- **Create** ADRs for significant decisions (with user validation)
- **Provide** high-level guidance and considerations
- **Estimate** complexity based on architecture
- **DO NOT** write implementation code or detailed specs

### Developer Responsibilities (next phase)
- Read business requirements from task YAML
- Read architectural guidance from technical spec
- Make implementation decisions (API design, data models, schemas)
- Write code following architectural patterns
- Ensure acceptance criteria are met

## Example Output

```
Reading tasks pending technical analysis...
Found 3 tasks to analyze.

Analyzing TASK-001: User Registration and Email Verification
→ Reading business requirements from task YAML...
→ Understanding current architecture...
→ Evaluating if ADR is needed...

Agent: This task introduces authentication to the system.
Should I create an ADR for the authentication approach (JWT vs Session-based)?

> Yes, please create an ADR for JWT-based authentication

Creating ADR-001: JWT-based Authentication
→ Title: Use JWT tokens with refresh token rotation
→ Decision: JWT for stateless authentication
→ Alternatives: Session-based, OAuth only

> Please review this ADR. Proceed? (y/n)
y

✓ ADR-001 validated and saved
→ Creating technical specification...
→ Updating architecture.md...

✓ Technical spec created at docs/features/authentication/technical.md
✓ Task updated to "ready-for-development"
✓ Complexity: Medium

Technologies identified:
- Frontend: React Context for auth state, TanStack Query
- Backend: JWT with bcrypt for password hashing
- Database: PostgreSQL with indexes on email

---

Analyzing TASK-002: Workspace Creation and Management
→ Reading business requirements...
→ Understanding current architecture...
→ No new ADR needed (uses existing patterns)

✓ Technical spec created at docs/features/workspaces/technical.md
✓ Task updated to "ready-for-development"
✓ Complexity: High (multi-tenant architecture)

Technologies identified:
- Frontend: Design system Table and Form components
- Backend: Multi-tenant data isolation pattern
- Database: PostgreSQL with workspace_id in all tables

---

All tasks analyzed!

Summary:
- Tasks analyzed: 2
- Complexity: 0 low, 1 medium, 1 high
- ADRs created: 1 (ADR-001)
- Ready for development: 2

Next steps:
→ Run `/rumiator-develop TASK-001` to start implementing
→ Or run `/rumiator-develop-next` to automatically pick next task

Note: Technical specs provide architectural guidance only.
Developers will make implementation decisions (APIs, schemas, etc.) during development.
```

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-analyze-tech.md`
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
