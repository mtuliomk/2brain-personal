---
name: spec-userstory
description: Gerar ou revisar um documento em português com uma ou mais histórias de usuário que registrem valor para usuário ou negócio e sirvam de entrada para uma especificação funcional. Usar na etapa de histórias de usuário, antes da especificação funcional. Não usar para decidir implementação, classificar a mudança ou desenvolver.
---

# Histórias de usuário

Gere `user-stories.md` em português. Converta a task em uma ou mais histórias concisas e orientadas a valor. Não inspecione código, defina detalhes de implementação ou gere a especificação funcional.

## Idioma

Escreva em português todos os títulos, seções, histórias, critérios de aceite, decisões e histórico de versão. Mantenha em inglês somente nomes de arquivos, caminhos, módulos, classes, funções, rotas, contratos, comandos, identificadores e demais trechos que representem código ou elementos técnicos existentes.

## Entradas e decomposição

Leia o contexto da aplicação e todos os documentos disponíveis relacionados ao escopo da task em execução. Extraia a persona ou processo afetado, a capacidade desejada, o valor esperado, os limites conhecidos e a terminologia relevante.

Decida autonomamente quantas histórias são necessárias. Crie histórias distintas somente quando personas, resultados desejados, benefícios, fluxos ou critérios de aceite forem independentes. Não divida uma necessidade única por camadas técnicas, componentes ou etapas de implementação. Gere ao menos uma história.

Quando não for possível identificar uma persona ou processo pelas evidências disponíveis, use `usuário do sistema`. Não aguarde esclarecimento humano. Registre ambiguidades, decisões e fontes materiais no documento.

## Estrutura obrigatória do documento

Use esta estrutura fixa. O documento deve ser escrito em português, e `## Controle de Versão` deve ser sua última seção.

```markdown
# Histórias de Usuário — TAK-{{task_number}}

## Contexto da Task
<Problema ou oportunidade, usuários afetados e objetivo da task.>

## US-01 — <título curto>

**Como** <persona ou processo>
**Quero** <capacidade desejada>
**Para que** <benefício ou valor esperado>

### Critérios de Aceite
- Dado <pré-condição>, quando <ação>, então <resultado esperado>.

### Fora de Escopo
- <Escopo explicitamente excluído>
- Escreva `Nenhum identificado` quando não houver exclusão relevante.

### Premissas e Decisões
- **Ambiguidade:** <informação ausente, ambígua ou conflitante>
- **Decisão:** <decisão mais restrita compatível com as evidências>
- **Fonte:** <documento de contexto, dados da task ou raciocínio>

## US-02 — <título curto, quando aplicável>
...

## Controle de Versão
| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agente | Criação inicial do documento |
```

Preserve todas as entradas existentes no histórico de versão e acrescente uma entrada a cada revisão material. Mantenha as histórias focadas em valor e escopo; deixe o detalhamento de comportamentos e o mapeamento de critérios entre histórias para `functional-spec.md`.

## Critérios de conclusão

Considere o documento concluído somente quando estiver escrito em português, exceto pelos nomes de arquivos e elementos de código, contiver uma ou mais histórias justificadamente independentes, incluir escopo e decisões materiais para cada história, mantiver `## Controle de Versão` como última seção e tiver sido gravado e validado no caminho exigido pelo agente da etapa.
