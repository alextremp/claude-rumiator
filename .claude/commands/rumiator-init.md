Initialize a new Rumiator project with the complete directory structure and configuration files.

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Check if .rumiator/ already exists. If it does, ask user if they want to reinitialize (will backup existing)
2. Create the complete directory structure:
   - .rumiator/tasks/
   - docs/product/
   - docs/adr/
   - docs/features/
   - docs/iterations/
   - docs/devops/
3. Copy .rumiator/config.yml.template to .rumiator/config.yml
4. Ask the user for basic project information:
   - Project name
   - Brief description
   - Preferred tech stack (if known)
5. Update .rumiator/config.yml with the provided information and current date
6. Create a README.md with project structure explanation if it doesn't exist
7. Create a .gitignore if it doesn't exist, ensuring it includes:
   - repositories/*
8. Display a success message with next steps:
   - Run /rumiator-create-product to start planning your product
   - Or manually create docs/product/product-idea.md with your vision

IMPORTANT: Be friendly and welcoming. This is the user's first interaction with Rumiator.
