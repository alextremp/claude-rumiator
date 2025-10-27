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
