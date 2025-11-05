# Rumiator

[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/alextremp/claude-rumiator)

**Iterative project development system for Claude Code based on Rational Unified Process (RUP)**

Rumiator helps you build software projects systematically by combining specialized AI agents with structured workflows.

## Quick Start

```bash
# 1. Initialize your project
/rumiator-init

# 2. Create product vision
/rumiator-create-product

# 3. Generate tasks (with business requirements)
/rumiator-create-tasks

# 4. Analyze technical approach
/rumiator-analyze-tech all

# 5. Start developing
/rumiator-develop-next

# 6. Monitor progress
/rumiator-status
```

**⚡ New**: Business requirements now integrated into task creation - faster workflow!

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
- **functional-analyst** - Creates tasks with business requirements (summary, user stories, acceptance criteria)
- **architect** - High-level technical guidance and architectural decisions (ADRs)
- **developer-frontend** - Frontend implementation based on requirements and guidance
- **developer-backend** - Backend implementation based on requirements and guidance
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
| `/rumiator-analyze-business` | ⚠️ DEPRECATED - No longer needed |
| `/rumiator-analyze-tech` | Create high-level technical guidance |
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
│   ├── features/               # Technical specs (high-level guidance)
│   │   └── [feature]/
│   │       ├── technical.md    # ⚡ Simplified: architectural guidance only
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

# Generate tasks for iteration 1 (includes business requirements)
/rumiator-create-tasks
# Created: TASK-001 (Auth), TASK-002 (Tasks CRUD), TASK-003 (Dashboard)
# Each task includes: summary, user stories, acceptance criteria

# Get technical guidance
/rumiator-analyze-tech all
# Agent asks: "Should I create an ADR for authentication approach?"
> "Yes, use JWT"
# Agent creates high-level technical guidance (no code/schemas)

# Develop tasks
/rumiator-develop-next
# Implements TASK-001 (backend + frontend + tests)
# Developer agents make implementation decisions

# Check progress
/rumiator-status
# Progress: ██████░░░░ 33% (1/3 tasks done)

# Continue development
/rumiator-develop-next
# Repeat until iteration complete

# Generate report
/rumiator-report
# Creates docs/iterations/iteration-01/report.md
```

**⚡ Simplified**: 2-phase workflow instead of 3 - faster and more efficient!

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
See [RUMIATOR_CHANGELOG.md](./RUMIATOR_CHANGELOG.md) for detailed version history and migration instructions.

### Manual Updates
If automatic update fails, you can:
1. Check `repositories/claude-rumiator/RUMIATOR_CHANGELOG.md` for migration steps
2. Manually update your `.rumiator/` directory
3. Update `rumiator_version` in your `config.yml`

### For Maintainers: Creating a Release

Use the `/rumiator-workflow-pr` command to create a release:

```bash
/rumiator-workflow-pr
```

This command will:
1. Analyze all changes since the last release
2. Automatically detect new commands, templates, and config changes
3. Generate migration actions based on detected changes
4. Ask you a few questions (version type, summary, etc.)
5. Update RUMIATOR_CHANGELOG.md and config.yml.template
6. Commit changes and create a PR

Then:
1. Review and merge the PR
2. Create a git tag: `git tag -a v1.x.x -m "Release v1.x.x" && git push origin v1.x.x`
3. Create a GitHub Release from the tag

**Much easier than writing CHANGELOG manually!** 🎉

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
