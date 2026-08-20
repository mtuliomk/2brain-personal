---
name: spec-funcional
description: Gerar uma especificação funcional em inglês a partir de uma ou mais user stories disponíveis, descrevendo o comportamento e os critérios de aceite sem decisões de implementação. Usar na etapa de especificação funcional, antes da especificação técnica. Não usar para investigar código, classificar a mudança ou implementar.
---

# Functional specification

Generate `functional-spec.md` in English using `user-stories.md` as the primary input. Describe the business and user-visible behavior that must be delivered. Do not make architecture, technology, code, file, class, route, data-structure, or implementation decisions.

## Inputs and scope

Read every available document that relates to the current task scope. Use `user-stories.md` as the source of personas, value, scope, and story identifiers. Use application context to preserve product terminology, business rules, permissions, known constraints, and relevant precedents. Do not inspect code to determine whether the feature already exists.

Do not silently expand, reduce, or redefine the scope captured in the user stories. When a story suggests a technical solution, record it only as context; it is not a functional requirement.

## Required document structure

Use this fixed structure. The document must be written in English, and `## Version History` must be its final section.

```markdown
# Functional Specification — TAK-{{task_number}}

## Context
<Why this task exists: the problem or opportunity, based on the user stories and available context.>

## Objective
<What must be true after this task is complete.>

## Expected Behavior
1. **EB-01 — <short title>**
   - **Related User Stories:** US-01
   - <User-visible and verifiable behavior, including relevant permissions, preconditions, results, and exception states.>

## Out of Scope
- <Explicitly excluded behavior>
- Write `None identified` when there is no relevant exclusion.

## Acceptance Criteria
1. **AC-01 — EB-01**
   - Given <precondition>, when <action>, then <expected result>.

## Assumptions and Decisions
- **Ambiguity:** <missing, ambiguous, or conflicting information>
- **Decision:** <narrowest functional decision compatible with the evidence>
- **Source:** <user story, context document, precedent, or reasoning>

## Version History
| Version | Date | Author | Change |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agent | Initial document creation |
```

Every `EB-xx` must reference at least one `US-xx` and have at least one matching `AC-xx`. Preserve every existing version-history entry and append one entry for each material revision.

## Autonomy and decisions

Do not stop for missing, ambiguous, or conflicting information. Decide using this priority order: explicit application context, product rule or official terminology, relevant task-scope precedent, then documented reasoning. Choose the narrowest behavior compatible with the evidence.

Record every material interpretation in `## Assumptions and Decisions`. When a request conflicts with application context, follow the context, record the conflict and decision, and continue without waiting for human intervention.

## Completion criteria

Consider the specification complete only when it is written in English, follows the required structure, maps expected behavior and acceptance criteria to the user stories, contains no technical implementation decisions, records material assumptions, keeps `## Version History` as the final section, and has been saved and validated at the path required by the stage agent.
