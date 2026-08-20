---
name: spec-userstory
description: Gerar ou revisar um documento em inglês com uma ou mais user stories que registrem valor para usuário ou negócio e sirvam de entrada para uma especificação funcional. Usar na etapa de user stories, antes da especificação funcional. Não usar para decidir implementação, classificar a mudança ou desenvolver.
---

# User stories

Generate `user-stories.md` in English. Convert the task into one or more concise, value-oriented stories. Do not inspect code, define implementation details, or create the functional specification.

## Inputs and decomposition

Read the application context and every available document related to the current task scope. Extract the affected persona or process, desired capability, expected value, known limits, and relevant terminology.

Decide autonomously how many stories are needed. Create separate stories only when personas, desired outcomes, benefits, flows, or acceptance criteria are independent. Do not split a single need by technical layers, components, or implementation steps. Generate at least one story.

When a persona or process cannot be identified from the available evidence, use `system user`. Do not wait for human clarification. Record material ambiguities, decisions, and sources in the document.

## Required document structure

Use this fixed structure. The document must be written in English, and `## Version History` must be its final section.

```markdown
# User Stories — TAK-{{task_number}}

## Task Context
<Problem or opportunity, affected users, and task objective.>

## US-01 — <short title>

**As a** <persona or process>
**I want** <desired capability>
**So that** <expected benefit or value>

### Acceptance Criteria
- Given <precondition>, when <action>, then <expected result>.

### Out of Scope
- <Explicitly excluded scope>
- Write `None identified` when there is no relevant exclusion.

### Assumptions and Decisions
- **Ambiguity:** <missing, ambiguous, or conflicting information>
- **Decision:** <narrowest decision compatible with the evidence>
- **Source:** <context document, task input, or reasoning>

## US-02 — <short title, when applicable>
...

## Version History
| Version | Date | Author | Change |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agent | Initial document creation |
```

Preserve every existing version-history entry and append one entry for each material revision. Keep user stories focused on value and scope; leave detailed expected behavior and cross-story acceptance-criteria mapping to `functional-spec.md`.

## Completion criteria

Consider the document complete only when it is written in English, contains one or more independently justified stories, includes scope and material decisions for each story, keeps `## Version History` as the final section, and has been saved and validated at the path required by the stage agent.
