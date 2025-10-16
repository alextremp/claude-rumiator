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
   - Update .rumiator/config.yml with iteration count
5. Display a summary of the product plan to the user
6. Ask if they want to proceed with creating tasks for the first iteration

Next steps suggestion:
- Review the product plan and make manual edits if needed
- Run /rumiator-create-tasks to generate task list for current iteration
