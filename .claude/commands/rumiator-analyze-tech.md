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
      - Update task YAML:
        * status: "ready-for-development"
        * architect: "architect"
        * technical_spec: path to technical.md
        * estimated_complexity: low|medium|high
        * updated: current date
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
