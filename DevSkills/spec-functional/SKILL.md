---
name: spec-funcional
description: Gerar a especificação funcional de uma TAK (task), descrevendo o que deve ser entregue sob a ótica do usuário ou negócio, sem decisões de implementação. Usar quando uma TAK entrar no refinamento, antes da spec técnica. Não usar para decidir como implementar, escrever código ou revisar UAT.
---

# Spec funcional

Gerar a especificação funcional de uma TAK: descrever o que deve ser entregue
sob a ótica do usuário ou negócio. Nunca decidir arquitetura, tecnologia ou
abordagem de implementação; isso pertence à skill `spec-tecnica`, executada
posteriormente a partir deste documento.

## Entradas obrigatórias

Ler sempre, nesta ordem:

1. `context/product.md`: entender o produto, suas regras, personas e fronteiras de escopo.
2. `context/glossary.md`: usar a terminologia oficial, sem inventar sinônimos.
3. A TAK original, completa, incluindo seus campos de **objetivo** e
   **descrição**. Usar o objetivo como fonte primária de `## Objetivo` e a
   descrição como fonte primária de `## Contexto`, `## Comportamento esperado`
   e `## Fora de escopo`.

Depois das entradas obrigatórias, procurar em `tasks/`, `context/` e nos demais
arquivos Markdown do projeto os documentos relacionados ao mesmo assunto da
TAK. Priorizar decisões, regras de negócio, fluxos e precedentes diretamente
relevantes; não consultar código para verificar se a feature já existe.

Se algum documento de contexto obrigatório não existir, registrar sua ausência
em `Decisões assumidas` e seguir com as fontes disponíveis. Se a TAK referenciar
outra task, feature ou decisão anterior, localizar esse material antes de
prosseguir. Não assumir contexto ausente: registrar a interpretação adotada em
`Decisões assumidas`.

## Limites funcionais

Não incluir:

- linguagem, framework, biblioteca, estrutura de dados, nome de arquivo ou classe;
- decisões sobre como implementar;
- verificação, no código, de que a feature já existe.

Tratar toda TAK como candidata a uma feature nova. Classificar a mudança como
nova, modificação ou duplicata é responsabilidade da `spec-tecnica`.

Se a TAK sugerir uma solução técnica, registrar a sugestão apenas como
contexto ou motivação e sinalizar que ela deve ser avaliada na spec técnica —
nunca tratá-la como requisito funcional.

## Formato de saída

Escrever `tasks/TAK-XXXX/spec-funcional.md` com estas seções, nesta ordem:

```markdown
# Spec funcional — TAK-XXXX

## Contexto
Por que essa task existe: problema ou oportunidade, em 2–4 frases.

## Objetivo
O que deve ser verdadeiro depois da conclusão da task, preferencialmente em uma frase.

## Comportamento esperado
Lista numerada e identificada (`CE-01`, `CE-02`...) do comportamento observável
pelo usuário. Para cada comportamento aplicável, cobrir permissões,
pré-condições, resultado observável e estados de exceção, como ausência de
dados, conteúdo inválido, indisponibilidade ou erro. Cada item deve ser
verificável pelo UAT.

## Fora de escopo
O que a task explicitamente não cobre. Não introduzir decisões técnicas nesta
seção. Se não houver exclusões relevantes, escrever `Nenhum identificado`.

## Critérios de aceite
Lista objetiva, testável, numerada e identificada (`CA-01`, `CA-02`...). Cada
critério deve indicar o `CE-XX` correspondente e servir como checklist do UAT.

## Decisões assumidas
Registrar somente ambiguidades ou contradições resolvidas pela skill; não
repetir fatos explícitos no objetivo ou na descrição da TAK. Usar, para cada
item, este formato:

- **Ambiguidade:** ...
- **Decisão:** ...
- **Fonte:** TAK / `product.md` / `glossary.md` / `decisions.md` / outro
  documento Markdown relevante / precedente em `tasks/` / raciocínio próprio.
```

Cada item de `Comportamento esperado` deve ter ao menos um critério de aceite
correspondente, identificado pela referência ao mesmo `CE-XX`. Sintetizar no
`Contexto` somente fatos do objetivo e da descrição da TAK; colocar qualquer
interpretação nova exclusivamente em `Decisões assumidas`. Tornar explícitos os
limites de escopo, especialmente quando a TAK for ambígua.

## Autonomia e rastreabilidade

Entregar uma spec completa; não parar por causa de ambiguidades. Resolver cada
ponto usando esta ordem de prioridade:

1. `context/decisions.md`;
2. `context/product.md` ou `context/glossary.md`;
3. precedente de TAKs semelhantes em `tasks/`;
4. raciocínio próprio, explicitando a lógica.

Registrar toda decisão tomada em `Decisões assumidas`, sem exceção. O registro
serve para a revisão humana distinguir o que veio diretamente da TAK do que
foi interpretação da skill.

### Contradições com o contexto

Se a TAK contradizer diretamente `product.md`, `decisions.md` ou outro
contexto de produto, seguir o contexto e registrar o conflito com destaque:

> **⚠ Contradição**: indicar o que a TAK pede e qual regra de contexto conflita.
>
> **Decisão**: indicar a regra de contexto seguida e por quê.
>
> **Ação recomendada**: sugerir validação humana ou uma decisão de produto
> separada, sem bloquear o pipeline.

## Critério de conclusão

Considerar a spec pronta para revisão humana quando:

- todas as seções estiverem preenchidas, sem lacunas;
- cada comportamento esperado tiver critério de aceite correspondente;
- toda ambiguidade resolvida estiver documentada com sua fonte;
- nenhuma decisão técnica tiver vazado para o documento.

A revisão humana da etapa 2 continua obrigatória. Não aprovar a própria saída
nem iniciar a implementação.

## Exemplo

Para a TAK “Usuário quer poder exportar relatório em PDF”:

**Bom comportamento esperado:**

> Usuário consegue gerar um PDF do relatório atualmente visível na tela, a
> partir de uma ação disponível na mesma página.

**Ruim — decisão técnica vazada:**

> Sistema usa uma biblioteca de geração de PDF no backend e retorna o arquivo
> por uma rota REST.

**Decisão assumida:**

> **Ambiguidade**: a TAK não diz se a exportação respeita os filtros aplicados.
> **Decisão**: respeitar os filtros atuais.
> **Fonte**: `product.md` e o precedente das demais exportações do produto.

A `spec-tecnica`, ao consultar o código, classifica a mudança como `nova`,
`modificação` ou `duplicada`; essa classificação não pertence a esta skill.
