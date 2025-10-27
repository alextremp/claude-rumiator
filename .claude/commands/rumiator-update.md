Update the current Rumiator project to the latest version from the official repository.

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Read `.rumiator/config.yml` and get the `language` field
2. **Use that language for ALL communication** with the user (questions, responses, messages, etc.)
3. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Overview
This command updates the project to the latest Rumiator version by:
1. Cloning/updating the official repository
2. Comparing versions
3. Applying migrations from RUMIATOR_CHANGELOG.md

## Steps

### 1. Verify Current Installation

a. Check if `.rumiator/config.yml` exists
   - If not, inform user to run `/rumiator-init` first

b. Read current `rumiator_version` from `.rumiator/config.yml`
   - If field doesn't exist, assume version "0.0.0" (pre-versioning)

### 2. Clone/Update Official Repository

a. Check if `repositories/claude-rumiator/` exists
   - If not, clone: `git clone https://github.com/alextremp/claude-rumiator.git repositories/claude-rumiator`
   - If exists, update: `cd repositories/claude-rumiator && git pull origin master`

b. Handle git errors gracefully:
   - If repository has local changes, inform user to clean it or remove the directory
   - If network error, inform user to check connection

### 3. Compare Versions

a. Read `rumiator_version` from `repositories/claude-rumiator/.rumiator/config.yml.template`
   - This is the latest available version

b. Compare versions using semantic versioning:
   - If current >= latest: Inform "You're already on the latest version (X.Y.Z)"
   - If current < latest: Proceed with migration

### 4. Read and Parse CHANGELOG

a. Read `repositories/claude-rumiator/RUMIATOR_CHANGELOG.md`

b. Extract all versions between current and latest (inclusive of latest, exclusive of current)
   - Example: If current is 1.0.0 and latest is 1.2.0, get migrations for 1.1.0 and 1.2.0

c. Sort versions in ascending order (oldest to newest)

### 5. Apply Migrations

For each version in order:

a. Display to user:
   ```
   Applying migration to version X.Y.Z...
   ```

b. Parse the migration section in RUMIATOR_CHANGELOG.md for this version

c. Execute migration actions based on type:

   **add_field**:
   - Parse YAML file specified in `file`
   - Add field at `path` with `value`
   - Use dot notation for nested paths (e.g., "settings.new_feature")
   - Preserve existing content and formatting

   **add_file**:
   - Copy file from `repositories/claude-rumiator/{source}` to project `{target}`
   - If file exists, ask user if they want to overwrite
   - Create parent directories if needed

   **modify_file**:
   - Display `instructions` to user
   - Explain this requires manual review
   - Ask user if they want to proceed or skip

   **run_command**:
   - Display the command to be run
   - If `optional: true`, ask user if they want to run it
   - Execute the command and show output

   **message**:
   - Display the `message` to user
   - Wait for user acknowledgment

   **rename_file**:
   - Rename file from `from` to `to`
   - Check if source exists, warn if not

   **delete_file**:
   - Delete file at `file` path
   - Ask for confirmation before deleting

d. After each action, display:
   ```
   ✓ [action_type] completed: {description}
   ```

e. If any action fails:
   - Display error message
   - Ask user if they want to continue or abort
   - If abort, stop migration and display current version

### 6. Update Project Version

a. Update `rumiator_version` in `.rumiator/config.yml` to latest version

b. Display success message:
   ```
   ✓ Successfully updated to Rumiator version X.Y.Z

   Summary:
   - Previous version: A.B.C
   - New version: X.Y.Z
   - Migrations applied: N

   Next steps:
   - Review changes in your .rumiator/ directory
   - Run /rumiator-status to verify everything works
   - Continue with your development workflow
   ```

### 7. Handle Special Cases

**Pre-versioning projects** (version 0.0.0 or missing):
- Inform user this is a pre-versioning project
- Apply ALL migrations from version 1.0.0 onwards
- Be extra careful with breaking changes

**Failed migrations**:
- Keep track of which migrations succeeded
- Update version to last successful migration
- Inform user they can re-run `/rumiator-update` to retry

**No internet connection**:
- If can't clone/pull repository, inform user
- Suggest manual update process

### 8. Migration Log

Create a migration log at `.rumiator/migration-log.txt`:
```
[YYYY-MM-DD HH:MM:SS] Migration from A.B.C to X.Y.Z
- Applied migration 1.1.0 ✓
- Applied migration 1.2.0 ✓
- Success
```

## Error Handling

- **Git errors**: Provide clear instructions to resolve (clean repo, check connection, etc.)
- **Parse errors**: If RUMIATOR_CHANGELOG format is invalid, inform user and abort
- **File errors**: If files are missing or permissions denied, provide helpful message
- **Version conflicts**: If version numbers don't make sense, warn user

## Safety Checks

Before any destructive operation:
1. Warn user about what will be modified
2. Suggest backing up important files
3. Ask for confirmation

For file overwrites:
- Always ask before overwriting existing files
- Show diff if possible
- Allow user to skip or backup first

## User Experience

- Use progress indicators for long operations (cloning, parsing)
- Display clear, actionable messages
- Use the configured language for all communication
- Provide helpful next steps after completion
- Be friendly and encouraging

## Examples

**Example output in Spanish (language: "es-ES")**:
```
🔄 Actualizando Rumiator...

✓ Repositorio oficial actualizado
  Versión actual: 1.0.0
  Versión disponible: 1.2.0

📦 Aplicando migraciones...

→ Migración a 1.1.0
  ✓ add_field: Añadido campo 'nueva_funcionalidad'
  ✓ add_file: Copiado template nuevo

→ Migración a 1.2.0
  ✓ message: Nuevas características disponibles

✅ ¡Actualización completada!
   Rumiator 1.0.0 → 1.2.0

Próximos pasos:
- Revisa los cambios en .rumiator/
- Ejecuta /rumiator-status para verificar
```

**Example output in English (language: "en-US")**:
```
🔄 Updating Rumiator...

✓ Official repository updated
  Current version: 1.0.0
  Available version: 1.2.0

📦 Applying migrations...

→ Migration to 1.1.0
  ✓ add_field: Added field 'new_feature'
  ✓ add_file: Copied new template

→ Migration to 1.2.0
  ✓ message: New features available

✅ Update completed!
   Rumiator 1.0.0 → 1.2.0

Next steps:
- Review changes in .rumiator/
- Run /rumiator-status to verify
```

## Notes

- This command is safe to run multiple times
- If already on latest version, it will just confirm and exit
- Migrations are idempotent where possible
- Always read from official repository, never modify it
- Keep `repositories/claude-rumiator/` as a clean reference
