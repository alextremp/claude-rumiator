⚠️ **DEPRECATED COMMAND** ⚠️

This command is **NO LONGER NEEDED** in the simplified Rumiator workflow.

## Why is this deprecated?

In the new workflow:
- Business requirements (summary, user stories, acceptance criteria) are now created **directly in the task YAML** during task creation
- The `/rumiator-create-tasks` command now handles all business analysis
- Tasks are created in status `pending-technical-analysis` immediately
- There is **NO separate business analysis phase**

## What to use instead

Instead of running this command, simply run:
- `/rumiator-create-tasks` - Creates tasks with complete business requirements
- `/rumiator-analyze-tech all` - Analyzes technical approach (next step)

## Migration Notes

If you have tasks in status `pending-business-analysis`:
1. Manually update the task YAML to include:
   - `summary`: Brief description of what the task accomplishes
   - `user_stories`: List of user stories
   - `acceptance_criteria`: Testable criteria
2. Update status to `pending-technical-analysis`
3. Run `/rumiator-analyze-tech` to continue

---

## OLD DOCUMENTATION (for reference only)

Analyze tasks and create detailed functional specifications using the Functional Analyst agent.

Usage:
- /rumiator-analyze-business [task-id] - Analyze a specific task
- /rumiator-analyze-business all - Analyze all tasks pending business analysis

Prerequisites:
- Tasks must exist in docs/iterations/iteration-XX/tasks/ with status "pending-business-analysis"

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

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-analyze-business.md`
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
