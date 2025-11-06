# Changelog

All notable changes to Claude Rumiator will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## Format for Updates

Each version should include:
- **Version number**: Following semver (MAJOR.MINOR.PATCH)
- **Date**: Release date in YYYY-MM-DD format
- **Changes**: Categorized as Added, Changed, Deprecated, Removed, Fixed, Security
- **Migration instructions**: What users need to do to update their projects

## [2.3.0] - 2025-11-06

### Summary
Added support for customizing commands and agents without losing changes during `/rumiator-update`. Users can now create customization files that override or complement default behaviors.

### Added
- Feature #11: Customization system for commands and agents
- New directories: `.rumiator/customized-commands/` and `.rumiator/customized-agents/`
- Example files: `.rumiator/examples/customization-example-command.md` and `.rumiator/examples/customization-example-agent.md`
- CUSTOMIZATION OVERRIDE section added to all 17 commands (except `/rumiator-workflow-pr` which was not modified)
- CUSTOMIZATION OVERRIDE section added to all 7 agents

### Changed
- Updated all commands to check for customization files before executing
- Updated all agents to check for customization files before executing
- Updated command: `/rumiator-update` - Now includes informational section about customizations being preserved automatically

### Migration Instructions
- **Type**: `automatic`
- **Actions**:
```yaml
migrations:
  - type: "add_file"
    description: "Create customized-commands directory"
    source: ".rumiator/customized-commands/.gitkeep"
    target: ".rumiator/customized-commands/.gitkeep"
    optional: true

  - type: "add_file"
    description: "Create customized-agents directory"
    source: ".rumiator/customized-agents/.gitkeep"
    target: ".rumiator/customized-agents/.gitkeep"
    optional: true

  - type: "add_file"
    description: "Add customization example for commands"
    source: ".rumiator/examples/customization-example-command.md"
    target: ".rumiator/examples/customization-example-command.md"
    optional: true

  - type: "add_file"
    description: "Add customization example for agents"
    source: ".rumiator/examples/customization-example-agent.md"
    target: ".rumiator/examples/customization-example-agent.md"
    optional: true

  - type: "message"
    message: |
      🎨 NEW FEATURE: Command and Agent Customization

      **What's new:**
      - You can now customize commands and agents without modifying core files
      - Customizations survive `/rumiator-update` automatically
      - Two new directories created:
        * `.rumiator/customized-commands/` - for command customizations
        * `.rumiator/customized-agents/` - for agent customizations

      **How to use:**
      1. Create a customization file:
         - For commands: `.rumiator/customized-commands/[command-name].md`
         - For agents: `.rumiator/customized-agents/[agent-name].md`
      2. Add customization instructions in the file
      3. See examples in `.rumiator/examples/` for templates

      **Example:**
      To customize `/rumiator-develop`, create:
      `.rumiator/customized-commands/rumiator-develop.md`

      ```markdown
      # Customization: rumiator-develop.md

      ## Customization Instructions

      - BEFORE starting development, send notification to Discord
      - AFTER completing development, update CHANGELOG.md
      - ALWAYS run strict linting before marking task as done
      ```

      **Benefits:**
      - Integrate company-specific workflows
      - Add pre/post execution hooks (notifications, validations)
      - Customize quality checks and standards
      - Integrate with internal tools

      See RUMIATOR-GUIDE.md for full documentation on customizations.
```
- **Description**: Adds customization system that allows users to override or complement command and agent behaviors.

---

## [2.2.0] - 2025-11-06

### Summary
Fixed task file structure to organize tasks by iteration, preventing file conflicts and improving command performance as projects scale.

### Fixed
- Bug #9: Task files now reside in `docs/iterations/iteration-XX/tasks/` instead of `.rumiator/tasks/`
- Commands no longer slow down as more tasks are created across iterations
- Task files no longer conflict when transitioning between iterations

### Changed
- All commands updated to read/write tasks from iteration-specific directories
- All agents updated to use iteration-specific task paths
- `/rumiator-init` now creates `docs/iterations/iteration-01/tasks/` directory

### Migration Instructions
- **Type**: `automatic` for new projects, `manual` for existing projects
- **Actions**:
```yaml
migrations:
  - type: "message"
    message: |
      📁 IMPORTANT: Task file structure has changed

      **What changed:**
      - Task files now reside in `docs/iterations/iteration-XX/tasks/` instead of `.rumiator/tasks/`
      - This ensures tasks are organized by iteration
      - Commands only read tasks from current iteration (faster, fewer tokens)

      **For NEW projects:**
      - No action needed - `/rumiator-init` creates the correct structure

      **For EXISTING projects with tasks:**
      1. Read `.rumiator/config.yml` to get current iteration number
      2. Create directory: `docs/iterations/iteration-XX/tasks/` (where XX is current iteration)
      3. Move task files:
         ```bash
         mkdir -p docs/iterations/iteration-01/tasks
         mv .rumiator/tasks/*.yml docs/iterations/iteration-01/tasks/
         ```
      4. If you have multiple iterations:
         - Separate tasks by iteration number
         - Move each TASK-XXX.yml to its corresponding iteration directory

      **Benefits:**
      - Commands run faster (only scan current iteration's tasks)
      - No file conflicts between iterations
      - Clear organization: each iteration's tasks are self-contained
      - Lower token usage as project scales
```
- **Description**: Reorganizes task files by iteration to improve performance and prevent conflicts.

---

## [2.1.0] - 2025-11-01

### Summary
Fixed `/rumiator-update` command to ensure changes from new Rumiator versions are applied correctly through direct file synchronization.

### Changed
- Updated command: `/rumiator-update` - Added direct file copy mechanism to sync `.claude/`, `.rumiator/`, and markdown files from official repository after migrations

### Migration Instructions
- **Type**: `none`
- **Actions**: No migration needed - update applies automatically
- **Description**: Users can run `/rumiator-update` to get the fixed command behavior.

---

## [2.0.0] - 2025-11-01

### Summary
Simplified workflow integrating business requirements directly into task creation, reducing the process from 3 phases to 2. Technical specifications now provide high-level architectural guidance instead of detailed implementation, giving developers more autonomy.

### Changed
- Updated agent: `functional-analyst` - Now creates tasks with complete business requirements (summary, user stories, acceptance criteria) in single step
- Updated agent: `architect` - Simplified to provide high-level technical guidance only (no code, APIs, or schemas)
- Updated command: `/rumiator-analyze-tech` - Simplified workflow; only technical guidance phase remains
- Updated command: `/rumiator-create-tasks` - Now includes business requirements collection; tasks created in `pending-technical-analysis` status
- Updated template: `task.yml` - Added fields: `summary`, `user_stories`, `acceptance_criteria`
- Updated template: `technical-spec.md` - Simplified format; removed API specs, data models, database schemas, and code examples
- Documentation improvements: README, GETTING-STARTED, QUICK-REFERENCE, RUMIATOR-GUIDE, EXAMPLE-WALKTHROUGH, INDEX

### Deprecated
- Command: `/rumiator-analyze-business` - No longer needed; business requirements integrated into task creation
- Separate functional spec files - Requirements now in task YAML
- Detailed technical specs with code/APIs/schemas - Now high-level guidance only

### Removed
- Business analysis phase (integrated into task creation)
- Functional spec documents (requirements now in task YAML)
- Detailed technical specifications (now high-level guidance only)

### Breaking Changes

⚠️ **This is a MAJOR release with breaking changes**

**What breaks:**
- `/rumiator-analyze-business` command no longer works
- Old workflow (3-phase) no longer supported
- Tasks in `pending-business-analysis` or `draft` status need manual migration
- Functional spec files no longer generated for new tasks
- Technical specs no longer include implementation details

**How to migrate:**

For existing projects with tasks in progress:

1. **Tasks in `pending-business-analysis`:**
   - Add business requirements to task YAML manually:
     ```yaml
     summary: "Brief description..."
     user_stories: ["As a...", ...]
     acceptance_criteria: ["Must...", ...]
     ```
   - Change status to `pending-technical-analysis`

2. **Tasks in `draft`:**
   - Complete the business requirements
   - Change status to `pending-technical-analysis`

3. **Update workflows:**
   - Remove `/rumiator-analyze-business` from scripts
   - Use: `/rumiator-create-tasks` → `/rumiator-analyze-tech all`

New projects automatically use the simplified workflow.

### Migration Instructions
- **Type**: `manual`
- **Actions**:
```yaml
migrations:
  - type: "message"
    message: |
      ⚠️ BREAKING CHANGE: Workflow simplified from 3 phases to 2 phases

      **What changed:**
      - `/rumiator-create-tasks` now collects business requirements directly
      - Tasks are created in `pending-technical-analysis` status (not `draft`)
      - `/rumiator-analyze-business` command is DEPRECATED (no longer needed)
      - Business requirements (summary, user_stories, acceptance_criteria) are now in task YAML
      - Technical specs provide high-level guidance only (no code/APIs/schemas)

      **Migration for existing projects:**

      1. **For tasks in `pending-business-analysis` status:**
         - Manually add to task YAML:
           * `summary`: Brief description of what the task accomplishes
           * `user_stories`: List of user stories
           * `acceptance_criteria`: Testable criteria
         - Update status to `pending-technical-analysis`

      2. **For tasks with functional specs:**
         - Your existing functional specs remain valid
         - New tasks will use the integrated approach

      3. **Update your workflow:**
         - OLD: `/rumiator-create-tasks` → `/rumiator-analyze-business all` → `/rumiator-analyze-tech all`
         - NEW: `/rumiator-create-tasks` → `/rumiator-analyze-tech all`

      4. **No action needed for:**
         - Completed tasks
         - Tasks in development
         - Existing documentation

  - type: "message"
    message: |
      📋 Template updates available (optional)

      The following templates have been updated:
      - `.rumiator/templates/task.yml` - Added business requirement fields
      - `.rumiator/templates/technical-spec.md` - Simplified format

      Existing projects keep their current templates.
      New projects using `/rumiator-init` will get the updated templates.

  - type: "message"
    message: |
      🤖 Agent behavior updated

      **functional-analyst:**
      - Now collects business requirements during task creation
      - No longer creates separate functional spec files

      **architect:**
      - Provides high-level architectural guidance only
      - Validates ADRs with user before proceeding
      - No longer writes implementation details (APIs, schemas, code)

      These changes happen automatically - no migration needed.
```
- **Description**: Major workflow simplification. Projects with tasks in progress require manual migration. See breaking changes section for details.

---

## [1.0.0] - 2025-10-27

### Added
- Initial release of Claude Rumiator
- Complete RUP-based workflow system
- Specialized agents: project-manager, functional-analyst, architect, developer-frontend, developer-backend, devops, quality-assurance
- Core commands:
  - `/rumiator-init` - Initialize project structure
  - `/rumiator-create-product` - Create product plan
  - `/rumiator-create-tasks` - Generate tasks
  - `/rumiator-analyze-business` - Functional specifications
  - `/rumiator-analyze-tech` - Technical specifications
  - `/rumiator-develop` - Implement tasks
  - `/rumiator-develop-next` - Auto-develop next task
  - `/rumiator-status` - Project dashboard
  - `/rumiator-report` - Iteration reports
  - `/rumiator-adr` - Architecture decision records
  - `/rumiator-create-bug` - Bug reporting
  - `/rumiator-triage-bugs` - Bug prioritization
  - `/rumiator-review-architecture` - Architecture reviews
  - `/rumiator-propagate-architecture-change` - Propagate architecture changes
  - `/rumiator-block-task` - Block tasks
  - `/rumiator-update-plan` - Update product plan
- Documentation templates for product, features, architecture, bugs
- Task lifecycle management with multiple states
- Multi-repository support
- Iteration-based development cycle
- Language configuration support (es-ES, en-US, etc.)

### Migration Instructions
- **Type**: `initial`
- **Actions**: None (initial release)
- **Description**: This is the first release of Claude Rumiator. No migration needed.

---

## Migration Format

For future versions, each migration should follow this format:

```yaml
version: "X.Y.Z"
date: "YYYY-MM-DD"
migration:
  type: "update|breaking|minor"  # Type of migration
  actions:
    - type: "add_field"           # Action type
      file: "config.yml"          # Target file
      path: "rumiator_version"    # Field path (dot notation for nested)
      value: "X.Y.Z"              # Value to set
      description: "Added version tracking"

    - type: "add_file"
      source: ".rumiator/templates/new-template.md"
      target: ".rumiator/templates/new-template.md"
      description: "New template for feature X"

    - type: "modify_file"
      file: ".rumiator/config.yml"
      description: "Updated configuration structure"
      instructions: |
        Manual instructions for complex changes that can't be automated

    - type: "run_command"
      command: "/rumiator-status"
      description: "Verify project status after migration"
      optional: true

    - type: "message"
      message: |
        Important information for users about breaking changes
        or manual steps they need to take.
```

### Migration Action Types

- `add_field`: Add a new field to a YAML file
- `add_file`: Copy a new file to the project
- `modify_file`: Modify an existing file (with instructions)
- `run_command`: Execute a command after migration
- `message`: Display a message to the user
- `rename_file`: Rename a file
- `delete_file`: Delete an obsolete file

### Migration Types

- `initial`: First release, no migration needed
- `minor`: Small updates, automatic migration, no breaking changes
- `update`: Regular updates with new features, mostly automatic migration
- `breaking`: Breaking changes requiring user attention and manual steps
