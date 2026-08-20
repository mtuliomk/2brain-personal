---
name: spec-funcional
description: Gerar uma especificação funcional em português a partir de uma ou mais user stories disponíveis, descrevendo o comportamento e os critérios de aceite sem decisões de implementação. Usar na etapa de especificação funcional, antes da especificação técnica. Não usar para investigar código, classificar a mudança ou implementar.
---

# Especificação funcional

Gere `functional-spec.md` em português usando `user-stories.md` como entrada principal. Descreva o comportamento de negócio e observável pelo usuário que deve ser entregue. Não tome decisões de arquitetura, tecnologia, código, arquivo, classe, rota, estrutura de dados ou implementação.

## Idioma

Escreva em português todos os títulos, seções, explicações, comportamentos, critérios de aceite, decisões e histórico de versão. Mantenha em inglês somente nomes de arquivos, caminhos, módulos, classes, funções, rotas, contratos, comandos, identificadores e demais trechos que representem código ou elementos técnicos existentes.

## Entradas e escopo

Leia todos os documentos disponíveis relacionados ao escopo da task em execução. Use `user-stories.md` como fonte de personas, valor, escopo e identificadores das stories. Use o contexto da aplicação para preservar terminologia de produto, regras de negócio, permissões, restrições conhecidas e precedentes relevantes. Não inspecione código para determinar se a funcionalidade já existe.

Não amplie, reduza ou redefina silenciosamente o escopo registrado nas user stories. Quando uma story sugerir uma solução técnica, registre-a apenas como contexto; ela não é um requisito funcional.

## Estrutura obrigatória do documento

Use esta estrutura fixa. O documento deve ser escrito em português, e `## Controle de Versão` deve ser sua última seção.

```markdown
# Especificação Funcional — TAK-{{task_number}}

## Contexto
<Por que esta task existe: o problema ou oportunidade, com base nas user stories e no contexto disponível.>

## Objetivo
<O que deve ser verdadeiro após a conclusão desta task.>

## Comportamento Esperado
1. **CE-01 — <título curto>**
   - **User Stories relacionadas:** US-01
   - <Comportamento observável e verificável pelo usuário, incluindo permissões, pré-condições, resultados e estados de exceção relevantes.>

## Fora de Escopo
- <Comportamento explicitamente excluído>
- Escreva `Nenhum identificado` quando não houver exclusão relevante.

## Critérios de Aceite
1. **CA-01 — CE-01**
   - Dado <pré-condição>, quando <ação>, então <resultado esperado>.

## Premissas e Decisões
- **Ambiguidade:** <informação ausente, ambígua ou conflitante>
- **Decisão:** <decisão funcional mais restrita compatível com as evidências>
- **Fonte:** <user story, documento de contexto, precedente ou raciocínio>

## Controle de Versão
| Versão | Data | Autor | Alteração |
| --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agente | Criação inicial do documento |
```

Todo `CE-xx` deve referenciar ao menos uma `US-xx` e ter ao menos um `CA-xx` correspondente. Preserve todas as entradas existentes no histórico de versão e acrescente uma entrada a cada revisão material.

## Autonomia e decisões

Não interrompa a tarefa por informações ausentes, ambíguas ou conflitantes. Use esta ordem de prioridade: contexto explícito da aplicação, regra de produto ou terminologia oficial, precedente relevante do escopo da task e, por fim, raciocínio documentado. Escolha o comportamento mais restrito compatível com as evidências.

Registre toda interpretação material em `## Premissas e Decisões`. Quando uma solicitação conflitar com o contexto da aplicação, siga o contexto, registre o conflito e a decisão e prossiga sem aguardar intervenção humana.

## Critérios de conclusão

Considere a especificação concluída somente quando estiver escrita em português, exceto pelos nomes de arquivos e elementos de código, seguir a estrutura obrigatória, relacionar comportamentos esperados e critérios de aceite às user stories, não contiver decisões técnicas de implementação, registrar premissas materiais, mantiver `## Controle de Versão` como última seção e tiver sido gravada e validada no caminho exigido pelo agente da etapa.
