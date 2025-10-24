# Rumiator

**Iterative project development system for Claude Code based on Rational Unified Process (RUP)**

Rumiator helps you build software projects systematically by combining specialized AI agents with structured workflows.

## Quick Start

### For New Projects

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

### For Existing Projects

```bash
# 1. Initialize and analyze existing codebase
/rumiator-use-into-existing-project

# 2. Review and refine generated documentation
# 3. Continue with development workflow
/rumiator-develop-next
```

## Features

✅ **Systematic workflow** - From idea to implementation
✅ **Specialized agents** - PM, Analyst, Architect, Developers, DevOps, QA
✅ **Comprehensive documentation** - Auto-generated specs and ADRs
✅ **Iterative development** - Small, manageable iterations
✅ **Progress tracking** - Visual dashboards and reports
✅ **Decision transparency** - All choices documented
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
| Command | Purpose |
|---------|---------|
| `/rumiator-init` | Initialize project structure |
| `/rumiator-use-into-existing-project` | Integrate Rumiator into existing project |
| `/rumiator-create-product` | Create product plan from idea |
| `/rumiator-update-plan` | Update product plan |
| `/rumiator-create-tasks` | Generate tasks from plan |
| `/rumiator-analyze-business` | Create functional specs |
| `/rumiator-analyze-tech` | Create technical specs |
| `/rumiator-develop` | Implement a task |
| `/rumiator-develop-next` | Auto-select and develop next task |
| `/rumiator-status` | Show project dashboard |
| `/rumiator-report` | Generate iteration report |
| `/rumiator-block-task` | Mark task as blocked |
| `/rumiator-adr` | Create architecture decision record |

## Project Structure

```
your-project/
├── .rumiator/
│   ├── config.yml              # Configuration
│   ├── tasks/                  # Task definitions
│   └── templates/              # Document templates
├── docs/
│   ├── product/                # Product plan, architecture
│   ├── features/               # Functional & technical specs
│   ├── adr/                    # Architecture decisions
│   └── iterations/             # Iteration reports
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

## Installation

### Quick Setup with Bash Alias

Add this alias to your `~/.bashrc` or `~/.zshrc`:

```bash
# Replace /path/to/rumiator with the actual path where you cloned this repo
alias init-rumiator='mkdir -p .claude && mkdir -p .rumiator && cp -rf /path/to/rumiator/.claude/* .claude/ && cp -rf /path/to/rumiator/.rumiator/* .rumiator/'
```

Then, in any project directory:

```bash
init-rumiator  # Copies all Rumiator files to current project
```

### Manual Installation

```bash
# From this repository directory:
cp -r .claude /path/to/your/project/
cp -r .rumiator /path/to/your/project/
```

## Requirements

- **Claude Code** (with agent and command support)
- Git repository (recommended)

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
