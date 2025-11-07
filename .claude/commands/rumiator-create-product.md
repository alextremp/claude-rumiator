Create a comprehensive product plan from an initial product idea using the Project Manager agent.

Prerequisites:
- Project must be initialized with /rumiator-init

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Check if .rumiator/config.yml exists. If not, tell user to run /rumiator-init first
2. Check if docs/product/product-idea.md exists:
   - If it doesn't exist, ask the user to describe their product idea:
     * What problem does it solve?
     * Who are the target users?
     * What are the main features they envision?
     * Any specific requirements or constraints?
   - Create docs/product/product-idea.md with the user's input (keep it concise, 1 page max)
3. Launch the project-manager agent to analyze product-idea.md and create product-plan.md
4. The project-manager should:
   - Read docs/product/product-idea.md
   - Create docs/product/product-plan.md with:
     * Vision (2-3 paragraphs)
     * Stakeholders
     * Iteration breakdown (table format)
     * Initial risks with mitigation strategies
     * Legal & compliance considerations
     * Success criteria
   - Update .rumiator/config.yml with iteration count (iterations.total)
5. Read current iteration number from .rumiator/config.yml (iterations.current)
6. Update docs/iterations/iteration-[XX]/plan.md with iteration goals from product plan:
   - Extract goals for current iteration from product-plan.md
   - Update the plan.md file with:
     * Status: "Planning" → "In Progress"
     * Start Date: current date
     * End Date: estimated (based on velocity or default 2 weeks)
     * Goals: from product plan iteration breakdown
     * Scope: will be filled by /rumiator-create-tasks
7. Display a summary of the product plan to the user
8. Ask if they want to proceed with creating tasks for the first iteration

Next steps suggestion:
- Review the product plan and make manual edits if needed
- Run /rumiator-create-tasks to generate task list for current iteration

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-create-product.md`
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
