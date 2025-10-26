Analyze tasks and create detailed technical specifications using the Architect agent.

Usage:
- /rumiator-analyze-tech [task-id] - Analyze a specific task
- /rumiator-analyze-tech all - Analyze all tasks pending technical analysis

Prerequisites:
- Tasks must have status "pending-technical-analysis"
- Functional specs must exist for the tasks

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
   a. Read the task YAML and functional spec
   b. Launch the architect agent in "Technical Analysis Mode":
      - Read functional spec to understand requirements
      - Create docs/features/[feature-name]/technical.md following template:
        * Technology stack needed
        * Architecture diagram (Mermaid)
        * API endpoints with full specification (request/response)
        * Data models (TypeScript interfaces or equivalent)
        * Database schema (SQL or schema definition)
        * Security considerations
        * Performance considerations
        * Testing strategy
        * Complexity estimate (low/medium/high) with justification
        * Technical decisions with rationale
      - Update docs/product/architecture.md with new components/flows
      - If significant architectural decision, create ADR in docs/adr/
      - If the architect has questions about technology choices, ask the user
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
   - Path to technical spec
   - ADR created (if any)
4. After all tasks processed, display:
   - Total tasks analyzed
   - Complexity breakdown (X low, Y medium, Z high)
   - Next steps: Run /rumiator-develop [task-id] or /rumiator-develop-next to start implementation

Important:
- The architect MUST ask the user about tech preferences if unclear (confidence < 50%)
- Technical specs should be detailed but concise (~1.5 pages)
- Maintain consistency with overall architecture
- Document tradeoffs explicitly
- Prefer simple, proven solutions
- Update architecture.md for every task
