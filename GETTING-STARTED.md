# Getting Started with Rumiator

Welcome! This guide will help you set up and start using Rumiator in 5 minutes.

## What You Need

- ✅ Claude Code installed and running
- ✅ A project idea (even a vague one!)
- ✅ (Optional) Git repository

## Installation

Rumiator is already installed in this directory! All agents and commands are ready to use.

To use Rumiator in **another project**:

1. Copy the entire `.claude/` directory to your project
2. Copy the `.rumiator/` directory to your project
3. You're done!

```bash
# From this directory:
cp -r .claude /path/to/your/project/
cp -r .rumiator /path/to/your/project/
```

## Your First Project (5 minutes)

### Step 1: Initialize (30 seconds)

In your Claude Code session, run:

```
/rumiator-init
```

Answer the questions:
- Project name: `MyApp`
- Description: `A simple todo app`
- Tech stack: (press Enter to skip, or specify if you know)

**Result:** Project structure is created.

### Step 2: Define Your Product (2 minutes)

```
/rumiator-create-product
```

Answer these questions:
1. **What problem does it solve?**
   - Example: "People forget their daily tasks and need a simple way to track them"

2. **Who are the target users?**
   - Example: "Individuals who want a lightweight todo app without complexity"

3. **Main features?**
   - Example: "Create tasks, mark as done, organize by lists"

4. **Any constraints?**
   - Example: "Must be mobile-friendly, work offline"

**Result:** Product plan created with iterations mapped out.

### Step 3: Create Tasks (30 seconds)

```
/rumiator-create-tasks
```

The agent will ask clarifying questions about business requirements:
- "Should we support multiple lists?" → Your choice!
- "What happens when a task is completed?" → Your decision!

**Result:** Tasks generated with:
- Summary (what the task accomplishes)
- User stories (user perspective)
- Acceptance criteria (testable requirements)
- Status: `pending-technical-analysis`

### Step 4: Get Technical Guidance (1 minute)

```
/rumiator-analyze-tech all
```

The agent will:
- Ask tech preferences: "Preferred stack? React/Vue? Node/Python?"
- Create ADRs for important decisions (with your validation)
- Provide high-level architectural guidance (NO code/schemas)

**Result:** Technical guidance created for each task.

### Step 5: Start Coding! (Ongoing)

```
/rumiator-develop-next
```

The system will:
1. Pick the highest-priority task
2. Show you what it will build
3. Ask for confirmation
4. Implement backend + frontend + tests
5. Ask if you want to continue to next task

**Repeat** until iteration is complete!

---

## What Just Happened?

In 5 minutes, you:
- ✅ Structured your project
- ✅ Created a comprehensive product plan
- ✅ Generated task list with business requirements (summary, user stories, acceptance criteria)
- ✅ Got high-level technical guidance (architecture, design system, considerations)
- ✅ Started implementing features

All with **full documentation** that's automatically generated!

**⚡ New workflow**: Business requirements are now integrated into task creation, making the process faster!

---

## Next Steps

### Monitor Your Progress

```
/rumiator-status
```

Shows a dashboard with:
- Tasks completed vs. remaining
- Current blockers
- Next recommended action

### Complete Iteration

Keep running `/rumiator-develop-next` until all tasks are done.

### Generate Report

```
/rumiator-report
```

Creates a comprehensive report of what was accomplished.

### Plan Next Iteration

```
/rumiator-update-plan
/rumiator-create-tasks
# ... repeat the cycle
```

---

## Understanding the System

### The Flow
```
Idea → Plan → Tasks (with business requirements) → Technical Guidance → Development → Review
```

**⚡ Simplified**: 2-phase analysis instead of 3!

### Key Files You'll See

```
.rumiator/
  config.yml           ← Project configuration

iterations/
  iteration-01/
    tasks/
      TASK-001.yml       ← Task definitions

docs/
  product/
    product-plan.md    ← Your roadmap
    architecture.md    ← System design
  features/
    auth/
      technical.md     ← High-level architectural guidance (no code/schemas)
```

### The Agents

Think of them as your team:
- **PM**: Strategizes the product
- **Analyst**: Creates tasks with business requirements (summary, user stories, criteria)
- **Architect**: Provides high-level technical guidance and manages ADRs
- **Developers**: Make implementation decisions and write code
- **DevOps**: Deploy it
- **QA**: Test it

They all **ask you questions** when they need input!

**⚡ New**: Developers now have more autonomy to make implementation decisions!

---

## Common Questions

### "Do I need to know RUP?"
No! The system guides you through it.

### "Can I skip steps?"
Technically yes, but you'll miss documentation. For quick prototypes, you can go straight to `/rumiator-develop`, but you won't have specs.

### "Can I edit generated files?"
Absolutely! All generated docs are meant to be reviewed and refined by you.

### "What if I disagree with an agent's choice?"
Just edit the document or tell the agent when it asks. You're in control.

### "Can I use this with existing projects?"
Yes! Run `/rumiator-init` in your existing project. You can document existing features retroactively or use it only for new features.

---

## Tips for Success

1. **Answer agent questions honestly** - They improve quality
2. **Keep iterations small** - 2-4 weeks, 5-15 tasks
3. **Review generated docs** - Refine them to match your vision
4. **Run `/rumiator-status` often** - Stay aware of progress
5. **Commit frequently** - After each completed task
6. **Don't rush** - Let the process guide you

---

## Example: Real First Session

Here's what a real first session looks like:

```
You: /rumiator-init
Claude: [asks questions, creates structure]

You: /rumiator-create-product
Claude: What problem does your product solve?
You: "People struggle to track their reading list"
Claude: Who are the target users?
You: "Avid readers who want to track books and notes"
Claude: Main features?
You: "Add books, track reading progress, take notes, get recommendations"
Claude: [generates 3-iteration plan]

You: /rumiator-create-tasks
Claude: Should users be able to share their reading lists publicly?
You: "Yes, but privacy settings should be available"
Claude: [creates 8 tasks with business requirements]

You: /rumiator-analyze-tech all
Claude: Preferred stack?
You: "React + Node.js + PostgreSQL"
Claude: Should I create an ADR for the data model approach?
You: "Yes, please"
Claude: [generates high-level technical guidance + ADRs]

You: /rumiator-develop-next
Claude: Next task is TASK-001: User Authentication. Proceed?
You: "Yes"
Claude: [implements backend + frontend + tests]
       Task complete! Continue?
You: "Yes"
Claude: [continues with next task...]
```

---

## Troubleshooting

### "I don't see the commands"
Make sure `.claude/commands/` exists with the command files.

### "Agents aren't working"
Check that `.claude/agents/` exists with all agent files.

### "I want to customize the workflow"
Edit files in `.claude/commands/` and `.claude/agents/` to change behavior.

### "I made a mistake in planning"
Run `/rumiator-update-plan` to revise your product plan.

---

## Resources

- **[README.md](./README.md)** - Overview and feature list
- **[QUICK-REFERENCE.md](./QUICK-REFERENCE.md)** - Command cheat sheet
- **[RUMIATOR-GUIDE.md](./RUMIATOR-GUIDE.md)** - Complete documentation
- **[EXAMPLE-WALKTHROUGH.md](./EXAMPLE-WALKTHROUGH.md)** - Real project example

---

## Ready to Build?

```bash
# Start now:
/rumiator-init
```

**Happy building! 🚀**

Need help? Check the [RUMIATOR-GUIDE.md](./RUMIATOR-GUIDE.md) for detailed explanations.
