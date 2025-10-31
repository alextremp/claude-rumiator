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
