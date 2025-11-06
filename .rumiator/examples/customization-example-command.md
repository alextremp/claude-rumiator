# Customization Example: Command

This is an example of how to customize a Rumiator command.

## File Location
Place this file in: `.rumiator/customized-commands/[command-name].md`

For example, to customize the `/rumiator-develop` command:
- Create file: `.rumiator/customized-commands/rumiator-develop.md`

## Format

```markdown
# Customization: rumiator-develop.md

## Customization Instructions

- BEFORE starting the development of a new TASK, send a notification to Discord webhook https://discord.com/api/webhooks/YOUR_WEBHOOK_ID to notify the team that TASK-XXX development is about to start
- AFTER ending the development of a new TASK, send a notification to Discord webhook https://discord.com/api/webhooks/YOUR_WEBHOOK_ID to notify the team that TASK-XXX development has ended
- ALWAYS run `npm run lint:strict` before marking any task as done (in addition to standard linting)
- ADD a step to update the CHANGELOG.md file with the changes made in the task before marking as done
```

## How It Works

1. When you run `/rumiator-develop TASK-001`, the command will:
   - First check if `.rumiator/customized-commands/rumiator-develop.md` exists
   - If it exists, read all the customization instructions
   - Apply those instructions, which will modify or add steps to the standard workflow

2. In this example:
   - Discord notification will be sent before starting development
   - Strict linting will run instead of standard linting
   - CHANGELOG.md will be updated
   - Discord notification will be sent after completion

## Customization Types

### Adding Pre/Post Hooks
```markdown
- BEFORE starting development, run `npm run pre-dev-check`
- AFTER completing development, run `npm run post-dev-notify`
```

### Modifying Existing Steps
```markdown
- INSTEAD OF running standard tests, run `npm run test:e2e` for full end-to-end testing
- REPLACE the standard commit message format with conventional commits format
```

### Adding Additional Checks
```markdown
- ADD a security scan using `npm audit` before marking task as done
- ADD a performance test before completing frontend tasks
```

### Integrating with External Tools
```markdown
- SEND a Slack notification to #dev-team channel when task starts
- CREATE a Jira comment with deployment details when task completes
- UPDATE Linear task status to "In Review" after development
```

## Important Notes

- Customizations OVERRIDE conflicting instructions
- Customizations COMPLEMENT non-conflicting instructions
- Use clear, imperative language (BEFORE, AFTER, ALWAYS, ADD, REPLACE)
- Be specific about URLs, file paths, and command arguments
