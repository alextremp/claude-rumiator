# Customization Example: Agent

This is an example of how to customize a Rumiator agent.

## File Location
Place this file in: `.rumiator/customized-agents/[agent-name].md`

For example, to customize the `frontend-developer` agent:
- Create file: `.rumiator/customized-agents/frontend-developer.md`

## Format

```markdown
# Customization: frontend-developer.md

## Customization Instructions

- ALWAYS use our company's design system from `@acme-corp/design-system` instead of creating custom components
- ALWAYS run Storybook tests using `npm run test:storybook` before marking any task as done
- NEVER use Material-UI components - use our custom component library instead
- ADD an accessibility audit using `axe-core` for every UI component created
- BEFORE completing any frontend task, ensure the component is documented in Storybook with at least 3 examples (default, loading, error states)
- ALWAYS check with the design team in #design-review Slack channel before implementing any UI that deviates from the design system
```

## How It Works

1. When the `frontend-developer` agent is launched (e.g., via `/rumiator-develop TASK-001`):
   - The agent first checks if `.rumiator/customized-agents/frontend-developer.md` exists
   - If it exists, reads all the customization instructions
   - Applies those instructions, which modify the agent's behavior and checklist

2. In this example, the frontend developer will:
   - Use only @acme-corp/design-system components
   - Run Storybook tests as part of the completion checklist
   - Avoid Material-UI entirely
   - Run accessibility audits
   - Document components in Storybook
   - Consult design team for deviations

## Customization Types

### Enforcing Company Standards
```markdown
- ALWAYS follow the coding standards defined in `.rumiator/standards/frontend-standards.md`
- NEVER commit code without running `prettier --write` first
- USE ESLint with our company config from `@acme-corp/eslint-config`
```

### Adding Mandatory Checks
```markdown
- BEFORE marking task as done, ensure test coverage is at least 90% (not 80%)
- ADD bundle size check - fail if frontend bundle exceeds 250KB gzipped
- RUN Lighthouse CI and ensure performance score > 95
```

### Integrating with Company Tools
```markdown
- AFTER creating any API endpoint, automatically update API documentation in Postman workspace "Acme Corp API"
- SEND code coverage report to Codecov using company token
- UPDATE Confluence page "API Documentation" with new endpoints
```

### Modifying Development Flow
```markdown
- BEFORE starting backend development, create database migration file
- ALWAYS use our custom logger from `@acme-corp/logger` instead of console.log
- AFTER completing feature, deploy to dev environment and post URL in #dev-deploys channel
```

### Adding Notification Workflows
```markdown
- WHEN task is marked as done, send message to #engineering Slack channel with summary
- IF task is blocked, create Jira ticket and assign to tech lead
- AFTER deployment, post release notes to #releases channel
```

## Agent-Specific Examples

### Backend Developer
```markdown
# Customization: developer-backend.md

## Customization Instructions

- ALWAYS use Prisma for database operations (never raw SQL)
- BEFORE creating any endpoint, ensure OpenAPI spec is updated
- ADD database query performance test for any new repository method
- NEVER expose internal error details in API responses - use error codes
- AFTER adding new environment variable, update .env.example and deployment docs
```

### DevOps
```markdown
# Customization: devops.md

## Customization Instructions

- ALWAYS use our company's AWS account 123456789 for all infrastructure
- USE Terraform Cloud workspace "acme-production" for production deployments
- BEFORE deploying to production, get approval in #ops-approvals Slack channel
- ADD cost estimation using `terraform cost estimate` before applying changes
- NEVER deploy on Fridays after 2 PM (deployment freeze)
```

### Quality Assurance
```markdown
# Customization: quality-assurance.md

## Customization Instructions

- ALWAYS test on IE11 in addition to modern browsers (company requirement)
- RUN visual regression tests using Percy before approving any UI changes
- ADD performance testing using k6 for any API endpoint changes
- BEFORE approving, verify WCAG 2.1 Level AAA compliance (not just AA)
- CREATE detailed QA report in Jira for every feature tested
```

## Important Notes

- Customizations OVERRIDE conflicting agent instructions
- Customizations COMPLEMENT non-conflicting behaviors
- Use clear, imperative language (ALWAYS, NEVER, BEFORE, AFTER, ADD)
- Be specific about tools, paths, and requirements
- Consider your team's workflow and tooling when creating customizations
