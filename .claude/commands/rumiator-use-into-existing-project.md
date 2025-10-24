Integrate Rumiator into an existing project by analyzing current state and creating a retrospective product plan.

Prerequisites:
- Project must not already have .rumiator/ initialized

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If not, read `.rumiator/config.yml.template` to get the `language` field
3. **Use that language for ALL communication**

## Steps

1. Check if .rumiator/ exists. If yes, ask user if they want to backup and reinitialize
2. Initialize Rumiator structure (same as /rumiator-init):
   - Create directories: .rumiator/tasks/, docs/product/, docs/adr/, docs/features/, docs/iterations/, docs/devops/
   - Copy .rumiator/config.yml.template to .rumiator/config.yml
3. Analyze the project:
   - Examine code structure, git history (last 30 commits), package.json/requirements.txt/etc
   - Identify tech stack, main features, and project maturity
   - Ask user to confirm findings:
     * Project name (suggest from repo)
     * Description/purpose
     * Tech stack assessment
     * Key features to document
4. Launch project-manager agent to create:
   - `docs/product/product-state.md`: Current vision, features list, tech stack, evolution from git history
   - `docs/product/product-plan.md`: Vision, stakeholders, 3-5 iterations roadmap, risks, success criteria
   - `docs/product/architecture.md`: Architecture overview with Mermaid diagram
5. Update .rumiator/config.yml with project details
6. Create initial draft tasks in .rumiator/tasks/ for first 2 iterations (status: "draft")
7. Show summary of generated files with key findings
8. Ask user to review the documentation and provide feedback:
   - Is the product vision accurate?
   - Any missing or incorrect features?
   - Tech stack assessment correct?
   - Iteration planning makes sense?
9. If user requests changes, update the relevant documents and config
10. Display success message and next steps:
    - Run `/rumiator-status` to see project dashboard
    - Review draft tasks in .rumiator/tasks/
    - Run `/rumiator-create-tasks` to elaborate task details
    - Run `/rumiator-analyze-business` to create functional specs

Important: Be flexible and collaborative. User knows their project best - iterate on the documentation until they're satisfied.
