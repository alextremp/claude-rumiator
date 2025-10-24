# Getting Started with Rumiator

Welcome! This guide will help you set up and start using Rumiator in 5 minutes.

## What You Need

- ✅ Claude Code installed and running
- ✅ A project idea (even a vague one!)
- ✅ (Optional) Git repository

## Installation

Rumiator is already installed in this directory! All agents and commands are ready to use.

### Option 1: Quick Setup with Bash Alias (Recommended)

Add this alias to your `~/.bashrc` or `~/.zshrc`:

```bash
# Replace /path/to/rumiator with the actual path where you cloned this repo
alias init-rumiator='mkdir -p .claude && mkdir -p .rumiator && cp -rf /path/to/rumiator/.claude/* .claude/ && cp -rf /path/to/rumiator/.rumiator/* .rumiator/'
```

Then, to install Rumiator in any project:

```bash
cd /path/to/your/project
init-rumiator
```

### Option 2: Manual Installation

```bash
# From this directory:
cp -r .claude /path/to/your/project/
cp -r .rumiator /path/to/your/project/
```

### Option 3: For Existing Projects with Code

If your project already has code and you want to generate documentation based on the current state:

1. First install Rumiator (using Option 1 or 2)
2. In Claude Code, run:
   ```
   /rumiator-use-into-existing-project
   ```
3. Answer the questions and review generated documentation

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

**Result:** Tasks generated for first iteration (e.g., TASK-001: User auth, TASK-002: Task CRUD, etc.)

### Step 4: Analyze Requirements (1 minute)

```
/rumiator-analyze-business all
```

The agent will ask clarifying questions. Answer honestly:
- "Should we support multiple lists?" → Your choice!
- "What happens when a task is completed?" → Your decision!

**Result:** Functional specs created for each task.

### Step 5: Design Architecture (1 minute)

```
/rumiator-analyze-tech all
```

The agent will ask tech preferences:
- "Preferred frontend framework?" → React, Vue, or whatever you like
- "Database preference?" → PostgreSQL, MongoDB, etc.

**Result:** Technical specs and architecture diagram created.

### Step 6: Start Coding! (Ongoing)

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
- ✅ Generated task list with priorities
- ✅ Documented business requirements
- ✅ Designed technical architecture
- ✅ Started implementing features

All with **full documentation** that's automatically generated!

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
Idea → Plan → Tasks → Business Analysis → Tech Analysis → Development → Review
```

### Key Files You'll See

```
.rumiator/
  config.yml           ← Project configuration
  tasks/
    TASK-001.yml       ← Task definitions

docs/
  product/
    product-plan.md    ← Your roadmap
    architecture.md    ← System design
  features/
    auth/
      functional.md    ← "What" to build
      technical.md     ← "How" to build
```

### The Agents

Think of them as your team:
- **PM**: Strategizes the product
- **Analyst**: Figures out requirements
- **Architect**: Designs the system
- **Developers**: Write the code
- **DevOps**: Deploy it
- **QA**: Test it

They all **ask you questions** when they need input!

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
Yes! Use `/rumiator-use-into-existing-project` to analyze your codebase and generate documentation based on what you've already built. Or use `/rumiator-init` if you only want to use Rumiator for new features going forward.

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
Claude: [creates 8 tasks for iteration 1]

You: /rumiator-analyze-business all
Claude: Should users be able to share their reading lists publicly?
You: "Yes, but privacy settings should be available"
Claude: [generates functional specs]

You: /rumiator-analyze-tech all
Claude: Preferred stack?
You: "React + Node.js + PostgreSQL"
Claude: [generates technical specs + architecture]

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
