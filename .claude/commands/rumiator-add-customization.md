Create or modify customizations for Rumiator commands and agents interactively.

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Read `.rumiator/config.yml` and get the `language` field
2. **Use that language for ALL communication** with the user (questions, responses, messages, etc.)
3. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Overview
This command helps users create customizations for agents and commands without manually creating files. It provides an interactive interface to:
- Select what to customize (agents or commands)
- Choose from available options
- Describe the desired customization
- Generate a customization proposal using AI
- Handle conflicts with existing customizations

## Steps

### 1. Verify Rumiator Installation

a. Check if `.rumiator/config.yml` exists
   - If not, inform user to run `/rumiator-init` first

b. Ensure customization directories exist:
   - `.rumiator/customized-commands/`
   - `.rumiator/customized-agents/`
   - If they don't exist, create them

### 2. Ask What to Customize

Present the user with two options:
```
¿A qué quieres aplicar la customización?

1. Agentes
2. Comandos
```

Wait for user response (1 or 2).

### 3. List Available Options

**If user selected Agentes (1)**:
- List all available agents from `repositories/claude-rumiator/.claude/agents/`:
  1. architect.md
  2. developer-backend.md
  3. developer-frontend.md
  4. devops.md
  5. functional-analyst.md
  6. project-manager.md
  7. quality-assurance.md

**If user selected Comandos (2)**:
- List all available commands from `repositories/claude-rumiator/.claude/commands/`:
  (Exclude `rumiator-add-customization.md` from the list)

Format the list as:
```
De acuerdo, customizaremos un [agente|comando], ¿cuál de ellos?

1. architect.md
2. developer-backend.md
...
```

### 4. Check for Existing Customization

a. Once user selects an option (by number), determine the file name

b. Check if customization already exists:
   - For agents: `.rumiator/customized-agents/[selected-agent].md`
   - For commands: `.rumiator/customized-commands/[selected-command].md`

c. If file exists:
   - Read the current content
   - Inform the user:
     ```
     ⚠️ Ya existe una customización para [nombre]. Contenido actual:

     [mostrar contenido]

     ¿Qué quieres hacer?
     1. Agregar nuevas instrucciones (mantener las actuales)
     2. Reemplazar completamente la customización
     3. Cancelar
     ```
   - Wait for user response

d. If user selects "Cancelar" (3), exit gracefully

### 5. Gather Customization Requirements

Ask the user to describe the customization:
```
De acuerdo, vamos a customizar [tipo] [nombre], descríbeme qué quieres customizar:
```

Wait for user's detailed description.

### 6. Generate Customization Proposal

a. Use the user's description to generate customization instructions

b. Consider the format from examples:
   - Use imperative language: ALWAYS, NEVER, BEFORE, AFTER, ADD, REPLACE
   - Be specific about tools, paths, and requirements
   - Focus on actionable instructions

c. If appending to existing customization (option 1 from step 4c):
   - Read existing content
   - Integrate new instructions with existing ones
   - Avoid duplicates
   - Maintain consistency

d. Generate a proposal following this format:
   ```markdown
   # Customization: [filename]

   ## Customization Instructions

   - [INSTRUCTION 1]
   - [INSTRUCTION 2]
   ...
   ```

e. Show the proposal to the user:
   ```
   He generado la siguiente propuesta de customización:

   [mostrar propuesta completa]

   ¿Quieres aplicar esta customización?
   1. Sí, aplicar
   2. No, necesito modificarla
   3. Cancelar
   ```

### 7. Handle User Feedback

**If user selects "Sí, aplicar" (1)**:
- Proceed to step 8

**If user selects "No, necesito modificarla" (2)**:
- Ask: "¿Qué cambios necesitas hacer?"
- Wait for user feedback
- Regenerate proposal based on feedback
- Return to step 6e (show proposal again)

**If user selects "Cancelar" (3)**:
- Exit gracefully

### 8. Apply Customization

a. Determine target file path:
   - For agents: `.rumiator/customized-agents/[selected-file].md`
   - For commands: `.rumiator/customized-commands/[selected-file].md`

b. Write the customization file with the approved content

c. Display success message:
   ```
   ✅ Customización aplicada exitosamente!

   Archivo creado: [ruta del archivo]

   La próxima vez que ejecutes [nombre del comando/agente], se aplicarán estas customizaciones automáticamente.

   💡 Consejos:
   - Puedes editar el archivo manualmente en cualquier momento
   - Para ver ejemplos de customizaciones, consulta `.rumiator/examples/`
   - La customización se preservará cuando ejecutes `/rumiator-update`
   ```

### 9. Offer Additional Actions

Ask the user:
```
¿Quieres customizar algo más?
1. Sí, crear otra customización
2. No, terminar
```

**If user selects "Sí" (1)**:
- Return to step 2

**If user selects "No" (2)**:
- End command gracefully

## Important Notes

1. **Language**: Always use the language from config.yml for all interactions
2. **Validation**: Ensure user inputs are valid (numbers in range)
3. **Error Handling**: Handle file read/write errors gracefully
4. **Examples**: Reference the example files at `.rumiator/examples/customization-example-command.md` and `.rumiator/examples/customization-example-agent.md` when generating proposals
5. **Repository**: List commands and agents from `repositories/claude-rumiator/` to always show the latest available options
6. **Preservation**: Remind users that customizations are preserved during `/rumiator-update`

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-add-customization.md`
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
