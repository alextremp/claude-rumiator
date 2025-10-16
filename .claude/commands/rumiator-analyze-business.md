Analyze tasks and create detailed functional specifications using the Functional Analyst agent.

Usage:
- /rumiator-analyze-business [task-id] - Analyze a specific task
- /rumiator-analyze-business all - Analyze all tasks pending business analysis

Prerequisites:
- Tasks must exist in .rumiator/tasks/ with status "pending-business-analysis"

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Parse the command argument:
   - If no argument or "all": find all tasks with status "pending-business-analysis"
   - If task-id provided: validate it exists and has correct status
   - If invalid task-id or wrong status: show error and list valid tasks
2. For each task to analyze:
   a. Read the task YAML file
   b. Launch the functional-analyst agent in "Business Analysis Mode":
      - Read task details
      - Create docs/features/[feature-name]/functional.md following template:
        * Overview (3-5 lines)
        * Actors
        * User stories
        * Flows (main + alternatives) with Mermaid diagrams
        * Acceptance criteria (specific, testable)
        * Dependencies
        * Business decisions with rationale
      - If the analyst has questions about requirements, ask the user
      - Update task YAML:
        * status: "pending-technical-analysis"
        * business_analyst: "functional-analyst"
        * functional_spec: path to functional.md
        * updated: current date
3. Display summary for each analyzed task:
   - Task ID and title
   - Key acceptance criteria count
   - Dependencies identified
   - Path to functional spec
4. After all tasks processed, display:
   - Total tasks analyzed
   - Next steps: Run /rumiator-analyze-tech to create technical specifications

Important:
- The analyst MUST ask the user if requirements are unclear (confidence < 50%)
- Functional specs should be concise (~300 words + diagrams)
- Focus on WHAT and WHY, not HOW (that's for technical spec)
- Acceptance criteria must be verifiable
