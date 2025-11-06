Initialize a Rumiator project with the complete directory structure and configuration files.

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. **Ask if this is a new or existing project**:
    - Ask the user to determine if the user is starting a new project or continuing an existing one
    - Example: "Is this a new project or you're initializing Rumiator into an existing one?"

2. **If existing project**:
   a. Check if `repositories/` directory exists
   b. List the contents of `repositories/` to show what repositories are present
   c. Ask the user to confirm that all required project repositories are downloaded in this directory
   d. If repositories are missing, guide the user:
    - Explain they need to clone all project repositories into `repositories/`
    - Example: `git clone <repo-url> repositories/<repo-name>`
    - Wait for user confirmation that repositories are ready
      e. Once confirmed, proceed to step 3

3. Check if .rumiator/ already exists. If it does, ask user if they want to reinitialize.
4. Create the complete directory structure:
    - .rumiator/
    - repositories/ (if it doesn't exist yet - for new projects)
    - docs/product/
    - docs/adr/
    - docs/features/
    - docs/iterations/iteration-01/
    - docs/devops/
5. Copy .rumiator/config.yml.template to .rumiator/config.yml
6. Ask the user for basic project information, and if project was existing, provide defaults from existing config (use the READMEs in the repositories to help):
    - Project name
    - Brief description
    - Preferred tech stack (if known)
7. Update .rumiator/config.yml with the provided information and current date
8. Create initial directories and files:
   - Create docs/iterations/iteration-01/tasks/ directory for task files
   - Create docs/iterations/iteration-01/plan.md with template:
   ```markdown
   # Iteration 01 Plan

   **Status**: Planning
   **Start Date**: TBD
   **End Date**: TBD

   ## Goals
   To be defined by /rumiator-create-product

   ## Scope
   To be defined by /rumiator-create-tasks

   ## Tasks
   None yet - run /rumiator-create-tasks to generate
   ```
9. Create a README.md with project structure explanation if it doesn't exist
10. Create a .gitignore if it doesn't exist, ensuring it includes:
    - repositories/*
11. Display a success message with next steps:
- Run /rumiator-create-product to start planning your product
- Or manually create docs/product/product-idea.md with your vision

IMPORTANT: Be friendly and welcoming. For new projects, this may be the user's first interaction with Rumiator. For existing projects, acknowledge that they're setting up Rumiator to manage their ongoing work.

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-init.md`
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
