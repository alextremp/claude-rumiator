Create a new Rumiator release by analyzing changes, updating CHANGELOG, and creating a PR.

**IMPORTANT**: This command should only be run from the claude-rumiator repository by maintainers.

## Overview

This command automates the release process by:
1. Analyzing git changes since last release
2. Detecting modified files and generating migration actions automatically
3. Asking a few questions to fill in what can't be inferred
4. Updating RUMIATOR_CHANGELOG.md with the new version entry
5. Updating rumiator_version in config.yml.template
6. Creating a commit and PR ready to merge

## Steps

### 1. Validate Environment

a. Check if we're in the claude-rumiator development repository (not a project using rumiator):
   - Check if `.rumiator/config.yml` does NOT exist (only template should exist)

   If validation fails, inform user:
   - If `.rumiator/config.yml` exists: "This appears to be a project using rumiator, not the rumiator repository itself. This command is only for rumiator maintainers."
   - Otherwise: "Could not find rumiator repository structure. This command only works in the claude-rumiator repository."

   Note: This validation works with forks too, distinguishing the rumiator repo from projects using rumiator

b. Get current branch:
   ```bash
   git branch --show-current
   ```
   - If on master, warn user to create a feature branch first
   - If on a feature branch, continue

c. Check git status:
   ```bash
   git status --porcelain
   ```
   - If there are uncommitted changes, warn user to commit them first
   - Only proceed if git is clean

### 2. Get Current Version and Last Tag

a. Get current version from `.rumiator/config.yml.template`:
   ```bash
   grep -oP 'rumiator_version:\s*"\K[^"]+' .rumiator/config.yml.template
   ```

b. Get the last version tag:
   ```bash
   git describe --tags --abbrev=0
   ```
   Expected format: `v1.0.0`

c. Verify the tag matches the current version in the file

### 3. Analyze Changes Since Last Release

a. Get all commits since last tag:
   ```bash
   git log v{last_version}..HEAD --oneline
   ```

b. Get changed files since last tag:
   ```bash
   git diff --name-status v{last_version}..HEAD
   ```

c. Categorize changed files:

   **Commands** (`.claude/commands/rumiator-*.md`):
   - Status A (added) → New command
   - Status M (modified) → Updated command
   - Status D (deleted) → Removed command

   **Agents** (`.claude/agents/*.md`):
   - Status A → New agent
   - Status M → Improved agent

   **Templates** (`.rumiator/templates/*.md`):
   - Status A → New template
   - Status M → Updated template

   **Config** (`.rumiator/config.yml.template`):
   - Status M → Configuration updated

   **Docs** (`*.md`, `docs/**`):
   - Group as "Documentation updates"

   **Other files**:
   - List separately

### 4. Generate Change Description

Based on categorized files, auto-generate:

**Added** section:
- For each new command: `- New command: \`/rumiator-{name}\``
- For each new agent: `- New agent: \`{name}\``
- For each new template: `- New template: \`{name}\``

**Changed** section:
- For each modified command: `- Updated command: \`/rumiator-{name}\``
- For each modified agent: `- Improved agent: \`{name}\``
- For each modified template: `- Updated template: \`{name}\``
- If config modified: `- Configuration template updated`
- If docs modified: `- Documentation improvements`

**Removed** section:
- For each removed command: `- Removed command: \`/rumiator-{name}\``

### 5. Generate Migration Actions

Based on detected changes, auto-generate migration actions:

**For new command**:
```yaml
- type: "message"
  message: |
    New command available: /rumiator-{name}

    Run `/rumiator-{name}` to try it out.
```

**For new template**:
```yaml
- type: "add_file"
  source: ".rumiator/templates/{name}.md"
  target: ".rumiator/templates/{name}.md"
  description: "Add {name} template"
```

**For modified template**:
```yaml
- type: "message"
  message: "Template {name} has been updated. Existing projects keep their current version."
```

**For removed command** (breaking change):
```yaml
- type: "message"
  message: |
    ⚠️ Command /rumiator-{name} has been removed.

    Update your workflows if you were using this command.
```

**For config changes**:
Try to detect new fields by comparing old and new versions:

- **Note:** Config files may contain complex nested structures or arrays. New fields can be added at any depth, not just the top level.
- Use a YAML-aware diff tool or script to recursively compare the old and new config files. This helps detect additions inside nested objects or arrays.
- When generating migration actions, specify the full path to the new field using dot notation (e.g., `parent.child.new_field`) or array indices as appropriate (e.g., `items[2].subfield`).
- For example, if a new field is added under `notifications.email.enabled`, the path would be `notifications.email.enabled`.

Example diff:
```bash
git show v{last_version}:.rumiator/config.yml.template > /tmp/old_config.yml
diff -u /tmp/old_config.yml .rumiator/config.yml.template
```

### 6. Suggest Version Type

Based on detected changes:

**MAJOR** (X.0.0) if:
- Any commands removed
- Any agents removed
- Breaking changes in config

**MINOR** (0.X.0) if:
- New commands added
- New agents added
- New templates added
- New features

**PATCH** (0.0.X) if:
- Only bug fixes
- Only documentation
- Only minor improvements

### 7. Ask User for Input

Present analysis and ask questions:

**Question 1: Version Type**
```
Based on the changes I've detected:

Added:
{list auto-detected additions}

Changed:
{list auto-detected changes}

Removed:
{list auto-detected removals}

Suggested version type: {MAJOR/MINOR/PATCH}

What type of release is this?
```

Options:
- MAJOR (X.0.0) - Breaking changes
- MINOR (0.X.0) - New features, backwards compatible
- PATCH (0.0.X) - Bug fixes, backwards compatible

**Question 2: Release Summary**
```
Provide a brief summary (1-2 sentences) describing this release:
```
[Wait for user input]

**Question 3: Review Changes**
```
I've auto-detected these changes:

Added:
{list}

Changed:
{list}

Removed:
{list}

Do you want to:
1. Use these as-is
2. Add/edit descriptions
3. Add manual entries (like "Fixed" section)
```

If user chooses 2 or 3, ask for additional input.

**Question 4: Review Migration Actions**
```
I've generated these migration actions:

{show YAML}

Migration type: {automatic/manual/none}

Do you want to:
1. Use these as-is
2. Edit/modify them
3. Add more actions
4. No migration needed
```

**Question 5: Breaking Changes** (only if MAJOR)
```
This is a MAJOR release with breaking changes.

Please describe:
1. What breaks
2. How users should migrate
3. Any deprecated features
```

### 8. Calculate New Version

Based on user's selected type, calculate new version:

Current: 1.2.3
- MAJOR → 2.0.0
- MINOR → 1.3.0
- PATCH → 1.2.4

### 9. Update RUMIATOR_CHANGELOG.md

a. Generate the new entry:

```markdown
## [{new_version}] - {YYYY-MM-DD}

### Added
{items from Added section}

### Changed
{items from Changed section}

### Removed
{items from Removed section}

### Fixed
{items from Fixed section if provided}

### Migration Instructions
- **Type**: `{automatic/manual/none}`
- **Actions**:
```yaml
{generated migration actions}
```
- **Description**: {migration description}

{If MAJOR, add Breaking Changes section}

---

```

b. Read current RUMIATOR_CHANGELOG.md

c. Find insertion point (after format section, before version 1.0.0)

d. Insert new entry at the top

e. Write updated RUMIATOR_CHANGELOG.md

### 10. Update config.yml.template

a. Read current config.yml.template

b. Update the rumiator_version line:
   ```
   rumiator_version: "{old_version}"
   ```
   Replace with:
   ```
   rumiator_version: "{new_version}"
   ```

c. Write updated config.yml.template

### 11. Commit Changes

```bash
git add RUMIATOR_CHANGELOG.md .rumiator/config.yml.template
git commit -m "chore: release v{new_version}"
```

### 12. Push and Create PR

a. Push to origin:
   ```bash
   git push origin {current_branch}
   ```

b. Create PR using `gh` CLI:
   ```bash
   gh pr create \
     --title "Release v{new_version}" \
     --body "Release v{new_version}

## Summary
{user_summary}

## Changes

### Added
{added_items}

### Changed
{changed_items}

### Removed
{removed_items}

### Fixed
{fixed_items}

## Migration
Type: {migration_type}

See RUMIATOR_CHANGELOG.md for full details.

---

*Generated by /rumiator-workflow-pr*" \
     --base master
   ```

### 13. Display Success

```
✅ Release v{new_version} ready!

Created:
- ✅ Updated RUMIATOR_CHANGELOG.md with version {new_version}
- ✅ Updated config.yml.template to version {new_version}
- ✅ Committed changes
- ✅ Created PR: {pr_url}

Next steps:
1. Review the PR on GitHub
2. Make any final adjustments if needed
3. Merge to master
4. Manually create a git tag:
   git tag -a v{new_version} -m "Release v{new_version}"
   git push origin v{new_version}
5. Create a GitHub Release from the tag

Users can then update with: /rumiator-update
```

## Error Handling

**Not in rumiator repository**:

If `.rumiator/config.yml` exists:
```
❌ Error: This appears to be a project using rumiator, not the rumiator repository itself.

This command is only for rumiator maintainers creating new releases.

If you want to update your project to the latest rumiator version, use:
  /rumiator-update
```

Otherwise:
```
❌ Error: This command only works in the claude-rumiator repository.

Could not find the expected rumiator development structure.
This command is for maintainers creating rumiator releases.
```

**On master branch**:
```
⚠️ Warning: You're on the master branch.

Please create a feature branch first:
  git checkout -b release/v{suggested_version}

Then run this command again.
```

**Uncommitted changes**:
```
⚠️ Warning: You have uncommitted changes.

Please commit or stash them first:
  git status

Then run this command again.
```

**No changes since last release**:
```
⚠️ Warning: No changes detected since last release (v{last_version}).

Are you sure you want to create a release?
```

**gh CLI not available**:
```
⚠️ Warning: GitHub CLI (gh) not found.

I've updated the files and committed the changes.
Please create the PR manually:

1. Push your branch: git push origin {branch}
2. Go to: https://github.com/alextremp/claude-rumiator/compare/master...{branch}
3. Create the PR with the information shown above
```

## Tips

- Run from a clean feature branch
- Commit all changes before running
- Review generated migration actions carefully
- Test migrations locally before releasing
- Use conventional commit messages for better detection

## Output Format

Use clear formatting:
- ✅ Checkmarks for completed steps
- 📋 Lists for detected changes
- 💡 Highlights for suggestions
- ⚠️ Warnings for issues
- 🎉 Success messages

## Notes

- This is an interactive command - it will ask questions
- Most content is auto-generated from git diff
- You only need to confirm/adjust and provide a summary
- The command does all the file updates and git operations
- No GitHub Actions or external scripts needed
- Everything happens in a single command execution
