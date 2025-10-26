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

## 📊 Task Types & States

**Task Types:**
- `feature` - New functionality
- `bug` - Bug fixes
- `architecture-review` - Review code against new architecture

**Task States Flow:**
```
draft → pending-business-analysis → pending-technical-analysis
  → ready-for-development → in-progress → in-review → done

Special states:
  → blocked (any point)
  → pending-architecture-review (when ADR changes)
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

**Planning & Setup**
| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/rumiator-init` | Once, at start | Creates project structure |
| `/rumiator-create-product` | Once, at start | Generates product plan from idea |
| `/rumiator-update-plan` | When scope changes | Updates product plan |
| `/rumiator-create-tasks` | Start of iteration | Creates task list from plan |

**Analysis & Design**
| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/rumiator-analyze-business [id\|all]` | Before development | Creates functional specs |
| `/rumiator-analyze-tech [id\|all]` | After business analysis | Creates technical specs (checks ADRs) |
| `/rumiator-adr [decision]` | For big tech decisions | Creates Architecture Decision Record |

**Development**
| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/rumiator-develop [task-id]` | When ready to code | Implements task (feature/bug/review) |
| `/rumiator-develop-next` | When ready to code | Auto-selects and develops next task |

**Bug Management** 🆕
| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/rumiator-create-bug [task-id]` | Found a bug | Creates bug report, adds to iteration |
| `/rumiator-triage-bugs` | Weekly/when needed | Reviews and prioritizes all bugs |

**Architecture Management** 🆕
| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/rumiator-review-architecture [ADR]` | Architecture not working | Reviews and updates architectural decisions |
| `/rumiator-propagate-architecture-change ADR` | After ADR change | Updates all affected tasks and specs |

**Monitoring & Reporting**
| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/rumiator-status` | Anytime | Shows dashboard (features, bugs, reviews) |
| `/rumiator-report` | End of iteration | Generates iteration report |
| `/rumiator-block-task [task-id]` | When stuck | Marks task as blocked |

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
    bugs/                 → Bug analysis documents 🆕
      BUG-XXX-analysis.md
  adr/
    ADR-XXX-title.md      → Architecture decisions
    reviews/              → Architecture review docs 🆕
      ADR-XXX-review.md
  iterations/
    iteration-01/                  → First iteration (padded 2 digits)
      plan.md                      → Iteration plan
      report.md                    → Iteration report
      bug-triage-report.md         → Bug triage 🆕
      architecture-change-XXX.md   → Architecture changes 🆕
    iteration-02/                  → Second iteration
      ...

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

**Feature Task:**
```yaml
id: TASK-001
type: feature  # feature|bug|architecture-review 🆕
title: "Feature name"
feature: category
status: ready-for-development
priority: high                 # critical|high|medium|low
iteration: 1
estimated_complexity: medium   # low|medium|high
acceptance_criteria:
  - "Criterion 1"
functional_spec: "docs/features/[name]/functional.md"
technical_spec: "docs/features/[name]/technical.md"
related_adrs: ["ADR-001"]      # 🆕
related_bugs: []               # 🆕
blockers: []
```

**Bug Task:** 🆕
```yaml
id: TASK-015
type: bug
title: "Login fails on Safari"
feature: auth
status: ready-for-development
priority: critical
iteration: 1
bug_info:
  related_tasks: ["TASK-001"]
  severity: critical           # critical|high|medium|low
  steps_to_reproduce: "..."
  expected_behavior: "..."
  actual_behavior: "..."
  root_cause: "..."
```

**Architecture Review Task:** 🆕
```yaml
id: TASK-040
type: architecture-review
title: "Review User Auth against ADR-003"
status: pending-technical-analysis
architecture_review:
  related_adr: "ADR-003"
  affected_tasks: ["TASK-001", "TASK-012"]
  reason: "ADR-003 supersedes ADR-001"
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
