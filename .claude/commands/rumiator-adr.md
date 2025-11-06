Create an Architecture Decision Record (ADR) to document an important technical decision.

Usage:
- /rumiator-adr [brief-description]

ADRs should be created for:
- Choice of major technologies/frameworks
- Significant architectural patterns
- Data storage decisions
- Security approach changes
- Integration strategies
- Any decision with long-term implications

## Language Configuration
**IMPORTANT**: Before starting, determine the user's language:
1. Check if `.rumiator/config.yml` exists and read the `language` field
2. If it doesn't exist, read `.rumiator/config.yml.template` and get the `language` field
3. **Use that language for ALL communication** with the user (questions, responses, documentation, reports, etc.)
4. Examples: If language is "es-ES", communicate in Spanish. If "en-US", communicate in English, etc.

## Steps

1. Check how many ADRs already exist (scan docs/adr/)
2. Determine next ADR number (ADR-001, ADR-002, etc.)
3. Parse the brief description from user or ask for it
4. Ask user for decision details:
   - **Context**: What is the issue or situation prompting this decision?
   - **Decision**: What is the change we're proposing/doing?
   - **Consequences**: What becomes easier or harder?
   - **Alternatives Considered**: What other options were evaluated and why were they rejected?
   - **Status**: Proposed | Accepted | Deprecated | Superseded
   - **Deciders**: Who was involved in this decision?
5. Launch the architect agent to help structure the ADR:
   - Use template from .rumiator/templates/adr.md
   - Create docs/adr/ADR-XXX-[slug-from-title].md
   - Fill in all sections thoroughly
   - Include links to relevant docs or references
6. Update docs/product/architecture.md:
   - Add reference to this ADR in "Key Architectural Decisions" section
   - Ensure architecture diagram reflects the decision if applicable
7. If this ADR relates to a specific task:
   - Update task YAML to reference the ADR
   - Add ADR path to technical spec
8. Display ADR summary:
   - ADR number and title
   - Decision made
   - Key consequences
   - Path to full document
9. Ask user to review the ADR and confirm it's complete

ADR Template Structure:
```markdown
# ADR-XXX: [Decision Title]

**Date**: YYYY-MM-DD
**Status**: Proposed | Accepted | Deprecated | Superseded
**Deciders**: [Names/roles]

## Context
[What's the issue/situation?]

## Decision
[What are we doing?]

## Consequences

### Positive
- Benefit 1
- Benefit 2

### Negative
- Trade-off 1
- Trade-off 2

## Alternatives Considered
1. **Alternative 1**: Why rejected
2. **Alternative 2**: Why rejected

## References
- [Links to relevant docs]
```

Important:
- ADRs are immutable once accepted (create new ADR to supersede)
- Focus on WHY the decision was made, not just WHAT
- Document tradeoffs explicitly
- Keep it concise (1-2 pages max)
- Link to other relevant ADRs
- Update architecture.md to reflect the decision

## CUSTOMIZATION OVERRIDE

**CRITICAL - CHECK FOR CUSTOMIZATIONS**:

1. **Check if customization file exists**: `.rumiator/customized-commands/rumiator-adr.md`
2. **If the file exists**:
   - Read the customization file completely
   - Apply ALL customization instructions described in that file
   - Customizations OVERRIDE any conflicting steps or behaviors defined earlier in this command
   - Customizations COMPLEMENT non-conflicting steps
3. **If the file does not exist**:
   - Continue with the standard behavior defined above

**Examples of customizations**:
- Adding pre/post execution hooks (notifications, validations, etc.)
- Modifying specific steps in the workflow
- Adding additional checks or requirements
- Integrating with company-specific tools
- Changing output formats or destinations
