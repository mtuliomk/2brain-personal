---
name: spec-tecnica
description: Gerar uma especificação técnica em inglês a partir de uma especificação funcional disponível, investigando o código existente, classificando a mudança como Nova, Modificação ou Duplicada e definindo uma implementação e um plano de testes. Usar na etapa de especificação técnica, antes da implementação. Não usar para alterar código ou redefinir o escopo funcional.
---

# Technical specification

Generate `technical-spec.md` in English. Describe how to implement the functional scope using the available codebase and context. Do not implement code, alter repositories, or silently redefine functional scope.

## Inputs and investigation

Read the functional specification as the primary scope source and user stories as traceability context. Inspect relevant application documents, skills, source code, tests, configurations, contracts, integrations, permissions, persistence, and equivalent features before classifying the demand.

Search systematically for terms from the task, user stories, and functional specification. Record evidence using precise repository, path, module, class, function, route, contract, or test references. Do not classify a change as `New` merely because the first search result has no match.

## Classification

Classify every demand as exactly one of the following:

- **New:** no existing implementation delivers the requested functional behavior.
- **Modification:** an existing feature delivers part of the behavior or requires an extension or change.
- **Duplicate:** the current system already delivers all relevant functional behavior and acceptance criteria without material difference.

The classification changes the content of the same document structure; it never changes the structure itself.

- For **New**, describe the complete implementation approach.
- For **Modification**, describe only the delta over the existing implementation.
- For **Duplicate**, document where the behavior already exists and state that no implementation is required.

## Required document structure

Use this fixed structure for every classification. It mirrors the functional specification structure: title, context, objective, scope, assumptions, traceability, and version history. Do not add content after `## Version History`.

```markdown
# Technical Specification — TAK-{{task_number}}

## Context
<Relevant functional scope, technical context, and current-system findings.>

## Objective
<What the implementation must achieve technically.>

## Classification
**Type:** New | Modification | Duplicate

<Rationale for the classification.>

| Functional Reference | Code Evidence | Conclusion |
| --- | --- | --- |
| US-01 / CE-01 / CA-01 | <repository path, module, function, route, or test investigated> | <new, extend, modify, or already covered> |

## Technical Approach

### Affected Components and Modules
<Components, services, routes, jobs, or configurations to create or change. For Duplicate, state `No implementation required` and identify the existing implementation.>

### Main Flow
<Implementation flow and responsibility boundaries. For Modification, describe only the delta.>

### Data and Contracts
<Persistence, migrations, API contracts, events, validation, and data handling; state `No impact identified` when applicable.>

### Integrations
<External or internal integrations, failure handling, and compatibility considerations; state `No impact identified` when applicable.>

## Impact
<Permissions and security, observability, performance, existing data, compatibility, rollout, and dependent features. State `No impact identified` for each non-applicable area.>

## Technical Out of Scope
<Technical work considered but excluded. Write `None identified` when there is no relevant exclusion.>

## Functional Specification Divergences
<Information loss, conflict, infeasibility, or interpretation difference found during analysis. Write `None identified` when there is no divergence. Do not change functional scope here.>

## Assumptions and Decisions
- **Ambiguity:** <missing, ambiguous, or conflicting information>
- **Decision:** <narrowest decision compatible with the evidence>
- **Source:** <context document, code evidence, precedent, or reasoning>

## Test Plan
| Functional Reference | Scenario | Test Level | Expected Result |
| --- | --- | --- | --- |
| US-01 / CE-01 / CA-01 | <scenario> | Unit / Integration / E2E / Manual | <expected result> |

## Version History
| Version | Date | Author | Change |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agent | Initial document creation |
```

Use `US-xx`, `CE-xx`, and `CA-xx` references when they are available in the functional artifacts. Preserve all existing version-history entries and append a new entry for every material revision.

## Decisions and divergences

Do not stop for missing, ambiguous, or conflicting information. Apply this priority order: explicit technical context, established architecture or convention, relevant contextual requirement, direct code precedent, then documented reasoning. Choose the narrowest option compatible with the evidence.

Record every material assumption in `## Assumptions and Decisions`. Record functional conflicts, information loss, or infeasibility in `## Functional Specification Divergences`; do not resolve them by silently changing functional scope.

## Completion criteria

Consider the specification complete only when it is written in English, has exactly the required structure, contains a justified classification and evidence table, maps the technical approach and test plan to available functional references, records decisions and divergences, keeps `## Version History` as the final section, and has been saved and validated at the path required by the stage agent.
