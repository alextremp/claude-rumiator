# Rumiator Quick Reference

One-page cheat sheet for Rumiator commands and workflows.

## 🚀 Essential Commands

```bash
/rumiator-init                    # Initialize project
/rumiator-create-product          # Create product plan
/rumiator-create-tasks            # Generate tasks
/rumiator-analyze-business all    # Create functional specs
/rumiator-analyze-tech all        # Create technical specs
/rumiator-develop-next            # Auto-develop next task
/rumiator-status                  # Show dashboard
```

## 📊 Task States Flow

```
draft → pending-business-analysis → pending-technical-analysis
  → ready-for-development → in-progress → in-review → done
```

## 🎯 Complete Workflow (30 seconds)

```bash
# Setup (once)
/rumiator-init

# Plan (start of project)
/rumiator-create-product
/rumiator-create-tasks

# Analyze (before development)
/rumiator-analyze-business all
/rumiator-analyze-tech all

# Develop (repeat)
/rumiator-develop-next

# Monitor (anytime)
/rumiator-status

# Review (end of iteration)
/rumiator-report
```

## 🛠️ All Commands

| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/rumiator-init` | Once, at start | Creates project structure |
| `/rumiator-create-product` | Once, at start | Generates product plan from idea |
| `/rumiator-update-plan` | When scope changes | Updates product plan |
| `/rumiator-create-tasks` | Start of iteration | Creates task list from plan |
| `/rumiator-analyze-business [id\|all]` | Before development | Creates functional specs |
| `/rumiator-analyze-tech [id\|all]` | After business analysis | Creates technical specs |
| `/rumiator-develop [task-id]` | When ready to code | Implements specific task |
| `/rumiator-develop-next` | When ready to code | Auto-selects and develops next task |
| `/rumiator-status` | Anytime | Shows project dashboard |
| `/rumiator-report` | End of iteration | Generates iteration report |
| `/rumiator-block-task [task-id]` | When stuck | Marks task as blocked |
| `/rumiator-adr [decision]` | For big tech decisions | Creates Architecture Decision Record |

## 🤖 Agents Reference

| Agent | Role | Creates |
|-------|------|---------|
| project-manager | Strategy & planning | product-plan.md |
| functional-analyst | Requirements | functional.md, tasks |
| architect | Technical design | technical.md, architecture.md, ADRs |
| developer-frontend | Frontend code | React/Vue/etc components + tests |
| developer-backend | Backend code | APIs, services, DB + tests |
| devops | Infrastructure | CI/CD, Docker, deployment |
| quality-assurance | Testing & review | QA reports, test validation |

## 📁 File Structure

```
.rumiator/
  config.yml              → Project config
  tasks/TASK-XXX.yml      → Task definitions

docs/
  product/
    product-idea.md       → Initial concept
    product-plan.md       → Full plan with iterations
    architecture.md       → System architecture
  features/[name]/
    functional.md         → Business requirements
    technical.md          → Technical design
  adr/
    ADR-XXX-title.md      → Architecture decisions
  iterations/
    iteration-X-report.md → Iteration reports

repositories/
  XXX/                   → Code repos (frontend, backend, etc)
```

## 💡 Decision-Making Guidelines

Agents ask you when:
- **Business logic** is ambiguous → functional-analyst asks
- **Tech stack** choice is unclear → architect asks
- **Implementation detail** is uncertain → developers ask

Confidence thresholds:
- **>80%**: Agent proceeds, documents decision
- **50-80%**: Agent suggests options, asks you to choose
- **<50%**: Agent asks for clarification

## 📋 Task YAML Quick Reference

```yaml
id: TASK-001
title: "Feature name"
feature: category
status: ready-for-development  # See states above
priority: high                 # critical|high|medium|low
iteration: 1
estimated_complexity: medium   # low|medium|high
acceptance_criteria:
  - "Criterion 1"
  - "Criterion 2"
functional_spec: "docs/features/[name]/functional.md"
technical_spec: "docs/features/[name]/technical.md"
blockers: []
```

## 🎨 Document Templates

### Functional Spec Structure
- Overview (3-5 lines)
- Actors
- User Stories
- Flows (Mermaid diagrams)
- Acceptance Criteria (testable)
- Dependencies
- Business Decisions

### Technical Spec Structure
- Technology Stack
- Architecture (Mermaid diagram)
- API Endpoints (full spec)
- Data Models (TypeScript/etc)
- Database Schema
- Security Considerations
- Testing Strategy
- Complexity Estimate

### ADR Structure
- Context (the problem)
- Decision (what we chose)
- Consequences (pros/cons)
- Alternatives Considered
- Status (Proposed/Accepted/Deprecated)

## 🔄 Common Workflows

### Starting a New Project
```bash
/rumiator-init
/rumiator-create-product
/rumiator-create-tasks
/rumiator-analyze-business all
/rumiator-analyze-tech all
```

### Daily Development
```bash
/rumiator-status              # Check what's next
/rumiator-develop-next        # Develop a task
# ... repeat ...
```

### Handling Blockers
```bash
/rumiator-block-task TASK-005  # Document the blocker
# ... resolve blocker ...
# Edit task YAML to remove blocker
/rumiator-develop TASK-005     # Resume development
```

### End of Iteration
```bash
/rumiator-report              # Generate report
/rumiator-update-plan         # Adjust for next iteration
/rumiator-create-tasks        # Create tasks for iteration 2
```

## 📈 Status Dashboard Symbols

```
✓ Done
⚙ In Progress
⏸ In Review
✋ Blocked
→ Ready for Development
📋 Pending Technical Analysis
📝 Pending Business Analysis
📄 Draft
```

## 🎯 Priority Levels

- 🔴 **Critical**: Must have, blocking everything
- 🟠 **High**: Important for MVP/iteration
- 🟡 **Medium**: Nice to have, can defer
- 🟢 **Low**: Future enhancement

## ⚡ Pro Tips

1. Run `/rumiator-status` frequently
2. Keep iterations small (2-4 weeks)
3. Let agents ask questions - improves quality
4. Review and edit generated docs
5. Commit after each completed task
6. Document WHY decisions were made
7. Break down high-complexity tasks
8. Address blockers immediately

## 🔧 Customization Paths

- **Agents**: `.claude/agents/[name].md`
- **Commands**: `.claude/commands/[name].md`
- **Templates**: `.rumiator/templates/*.md`
- **Config**: `.rumiator/config.yml`

## 📚 Documentation Links

- [README.md](./README.md) - Overview & quick start
- [RUMIATOR-GUIDE.md](./RUMIATOR-GUIDE.md) - Complete guide
- [EXAMPLE-WALKTHROUGH.md](./EXAMPLE-WALKTHROUGH.md) - Real project example

## ⌨️ Copy-Paste Workflows

### First-Time Setup
```bash
/rumiator-init && /rumiator-create-product && /rumiator-create-tasks
```

### Full Analysis Pass
```bash
/rumiator-analyze-business all && /rumiator-analyze-tech all
```

### Development Loop
```bash
while true; do /rumiator-develop-next; /rumiator-status; done
```

---

**Bookmark this page for quick reference!** 📖
