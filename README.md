# Rumiator

**Iterative project development system for Claude Code based on Rational Unified Process (RUP)**

Rumiator helps you build software projects systematically by combining specialized AI agents with structured workflows.

## Quick Start

```bash
# 1. Initialize your project
/rumiator-init

# 2. Create product vision
/rumiator-create-product

# 3. Generate tasks
/rumiator-create-tasks

# 4. Analyze requirements
/rumiator-analyze-business all
/rumiator-analyze-tech all

# 5. Start developing
/rumiator-develop-next

# 6. Monitor progress
/rumiator-status
```

## Features

✅ **Systematic workflow** - From idea to implementation
✅ **Specialized agents** - PM, Analyst, Architect, Developers, DevOps, QA
✅ **Comprehensive documentation** - Auto-generated specs and ADRs
✅ **Iterative development** - Small, manageable iterations
✅ **Progress tracking** - Visual dashboards and reports
✅ **Decision transparency** - All choices documented
✅ **Bug tracking & management** - Track bugs alongside features
✅ **Architecture evolution** - Review and update architectural decisions
✅ **You're in control** - Agents ask before important decisions

## Architecture

### Agents (Workers)
- **project-manager** - Product vision and iteration planning
- **functional-analyst** - Business requirements and acceptance criteria
- **architect** - Technical design and architecture decisions
- **developer-frontend** - Frontend implementation
- **developer-backend** - Backend implementation
- **devops** - CI/CD and infrastructure
- **quality-assurance** - Testing and validation

### Commands (Workflows)

**Planning & Setup**
| Command | Purpose |
|---------|---------|
| `/rumiator-init` | Initialize project structure |
| `/rumiator-update` | Update to latest Rumiator version |
| `/rumiator-create-product` | Create product plan from idea |
| `/rumiator-update-plan` | Update product plan |
| `/rumiator-create-tasks` | Generate tasks from plan |

**Analysis & Design**
| Command | Purpose |
|---------|---------|
| `/rumiator-analyze-business` | Create functional specs |
| `/rumiator-analyze-tech` | Create technical specs |
| `/rumiator-adr` | Create architecture decision record |

**Development**
| Command | Purpose |
|---------|---------|
| `/rumiator-develop` | Implement a task (feature, bug, or review) |
| `/rumiator-develop-next` | Auto-select and develop next task |

**Bug Management**
| Command | Purpose |
|---------|---------|
| `/rumiator-create-bug` | Create a bug report |
| `/rumiator-triage-bugs` | Review and prioritize bugs |

**Architecture Management**
| Command | Purpose |
|---------|---------|
| `/rumiator-review-architecture` | Challenge and update architectural decisions |
| `/rumiator-propagate-architecture-change` | Update tasks affected by architecture changes |

**Monitoring & Reporting**
| Command | Purpose |
|---------|---------|
| `/rumiator-status` | Show project dashboard (features, bugs, reviews) |
| `/rumiator-report` | Generate iteration report |
| `/rumiator-block-task` | Mark task as blocked |

## Project Structure

```
your-project/
├── .rumiator/
│   ├── config.yml              # Configuration
│   ├── tasks/                  # Task definitions (features, bugs, reviews)
│   │   ├── TASK-001.yml        # type: feature
│   │   ├── TASK-015.yml        # type: bug
│   │   └── TASK-040.yml        # type: architecture-review
│   └── templates/              # Document templates
├── docs/
│   ├── product/                # Product plan, architecture
│   ├── features/               # Functional & technical specs
│   │   └── [feature]/
│   │       ├── functional.md
│   │       ├── technical.md
│   │       └── bugs/           # Bug analysis documents
│   ├── adr/                    # Architecture decisions
│   │   └── reviews/            # Architecture review documents
│   └── iterations/             # Iteration reports & plans
│       ├── iteration-01/       # Current iteration
│       │   ├── plan.md
│       │   ├── report.md
│       │   ├── bug-triage-report.md
│       │   └── architecture-change-*.md
│       └── iteration-02/       # Future iterations
│           └── ...
└── src/                        # Source code
```

## Example Workflow

```bash
# Initialize
/rumiator-init

# Describe your product
/rumiator-create-product
> "A task management app for remote teams..."

# Generate tasks for iteration 1
/rumiator-create-tasks
# Created: TASK-001 (Auth), TASK-002 (Tasks CRUD), TASK-003 (Dashboard)

# Analyze requirements
/rumiator-analyze-business all
# Agent asks: "Should we support OAuth or just email/password?"
> "Just email/password for MVP"

# Design architecture
/rumiator-analyze-tech all
# Agent asks: "Preferred stack? React/Vue? Node/Python?"
> "React + Node.js + PostgreSQL"

# Develop tasks
/rumiator-develop-next
# Implements TASK-001 (backend + frontend + tests)

# Check progress
/rumiator-status
# Progress: ██████░░░░ 33% (1/3 tasks done)

# Continue development
/rumiator-develop-next
# Repeat until iteration complete

# Generate report
/rumiator-report
# Creates docs/iterations/iteration-01-report.md
```

## Key Principles

1. **Iterative** - Deliver value in 2-4 week iterations
2. **Documented** - Every decision is recorded
3. **Interactive** - Agents ask when uncertain
4. **Transparent** - Track progress visually
5. **Quality-focused** - Specs, tests, reviews built-in

## Documentation

📖 **[Complete Guide](./RUMIATOR-GUIDE.md)** - Comprehensive documentation

Topics covered:
- Detailed workflow explanation
- Agent responsibilities
- Task lifecycle
- Best practices
- Customization guide
- FAQ

## Requirements

- **Claude Code** (with agent and command support)
- Git repository (recommended)

## Version Management

Rumiator uses semantic versioning and supports automatic updates:

### Checking Your Version
Your Rumiator version is stored in `.rumiator/config.yml` under `rumiator_version`

### Updating Rumiator
```bash
/rumiator-update
```

This command will:
1. Clone/update the official repository to `repositories/claude-rumiator/`
2. Compare your version with the latest available
3. Apply necessary migrations automatically
4. Update your project structure and configuration

### Version History
See [CHANGELOG.md](./CHANGELOG.md) for detailed version history and migration instructions.

### Manual Updates
If automatic update fails, you can:
1. Check `repositories/claude-rumiator/CHANGELOG.md` for migration steps
2. Manually update your `.rumiator/` directory
3. Update `rumiator_version` in your `config.yml`

## Benefits

### For Solo Developers
- Structured approach to building projects
- Comprehensive documentation without extra effort
- Clear progress tracking
- Reduces decision fatigue

### For Teams
- Shared understanding of requirements
- Clear technical specifications
- Documented architectural decisions
- Easy onboarding for new members

### For Learning
- See how professional projects are structured
- Learn RUP methodology in practice
- Understand architecture decision-making
- Study code organization patterns

## Customization

All agents and workflows are customizable:
- Edit `.claude/agents/*.md` to change agent behavior
- Edit `.claude/commands/*.md` to modify workflows
- Edit `.rumiator/templates/*.md` to adjust document structure

## Tips

💡 Run `/rumiator-status` frequently to track progress
💡 Review generated docs and refine them
💡 Let agents ask questions - it improves quality
💡 Keep iterations small (5-15 tasks)
💡 Commit after each completed task

## What's Next?

After completing an iteration:
1. Review with `/rumiator-report`
2. Update plan if needed with `/rumiator-update-plan`
3. Plan next iteration
4. Repeat the cycle

## Contributing

Improve Rumiator by:
- Enhancing agent prompts
- Adding new workflows
- Creating new templates
- Sharing your improvements

## License

MIT - Use freely for any project

---

**Get Started:** Run `/rumiator-init` in your Claude Code session! 🚀

For questions and detailed documentation, see [RUMIATOR-GUIDE.md](./RUMIATOR-GUIDE.md)
