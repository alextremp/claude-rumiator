Update the product plan based on changes to product-idea.md or product-plan.md.

Prerequisites:
- docs/product/product-plan.md must exist

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Check if docs/product/product-plan.md exists. If not, tell user to run /rumiator-create-product first
2. Ask the user what changed:
   - Changes to product scope?
   - New features to add?
   - Features to remove?
   - Timeline adjustments?
   - Other strategic changes?
3. Read current docs/product/product-idea.md and docs/product/product-plan.md
4. Launch the project-manager agent to update the plan:
   - Incorporate the changes described by the user
   - Update relevant sections (iterations, risks, etc.)
   - Add a changelog entry at the top of product-plan.md with date and changes
   - Maintain consistency across the document
5. Display a summary of the changes made
6. Check if there are existing tasks that might be affected:
   - Read .rumiator/config.yml to get current iteration number
   - Scan docs/iterations/iteration-XX/tasks/ directory (where XX is current iteration)
   - Warn user if changes might impact existing tasks
   - Suggest running /rumiator-create-tasks to add new tasks if scope increased

Important:
- Document WHY the changes were made
- Keep the changelog updated
- Preserve existing information unless explicitly changing it
