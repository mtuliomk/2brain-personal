---
name: spec-funcional
description: Gerar a especificação funcional de uma demanda a partir de uma user story, descrevendo o que deve ser entregue sob a ótica do usuário ou negócio, sem decisões de implementação. Usar na primeira etapa do refinamento, antes da spec técnica. Não usar para decidir como implementar, escrever código ou revisar UAT.
---
# Spec funcional

Gerar a especificação funcional de uma demanda a partir da user story:
descrever o que deve ser entregue sob a ótica do usuário ou negócio. Esta é a
primeira etapa do refinamento. Nunca decidir arquitetura, tecnologia ou
abordagem de implementação; isso pertence à skill `spec-tecnica`, executada
após a aprovação humana desta spec.

## Contexto de entrada

Considerar todos os arquivos de contexto já lidos ou disponibilizados na área de
trabalho, independentemente de caminho, pasta ou nome. Priorizar, entre eles,
decisões, regras de negócio, personas, glossários, fluxos, requisitos e
precedentes diretamente relacionados ao assunto da solicitação. Não consultar
código para verificar se a feature já existe.

Quando uma user story ou demanda anterior estiver disponível no contexto, lê-la
por completo e usar sua história, contexto e escopo inicial como fontes
primárias para `## Objetivo`, `## Contexto`, `## Comportamento esperado` e
`## Fora de escopo`. Não redefinir seu valor de negócio silenciosamente.

Se um contexto necessário não estiver disponível, registrar a ausência em
`Decisões assumidas` e seguir com as fontes existentes. Se o contexto
referenciar outra task, feature ou decisão anterior, localizar esse material
quando estiver disponível antes de prosseguir. Não assumir contexto ausente:
registrar a interpretação adotada em `Decisões assumidas`.

## Limites funcionais

Não incluir:

- linguagem, framework, biblioteca, estrutura de dados, nome de arquivo ou classe;
- decisões sobre como implementar;
- verificação, no código, de que a feature já existe.

Tratar toda demanda como candidata a uma feature nova. Classificar a mudança
como nova, modificação ou duplicata é responsabilidade da `spec-tecnica`.

Se a user story ou a demanda sugerir uma solução técnica, registrar a sugestão
apenas como contexto ou motivação e sinalizar que ela deve ser avaliada na spec
técnica — nunca tratá-la como requisito funcional.

## Estrutura da spec funcional

Gerar a spec com estas seções, nesta ordem:

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
repetir fatos explícitos na user story. Usar, para cada item, este formato:

- **Ambiguidade:** ...
- **Decisão:** ...
- **Fonte:** arquivo de contexto relevante / user story ou demanda, quando
  consultada / precedente disponível / raciocínio próprio.
```

Cada item de `Comportamento esperado` deve ter ao menos um critério de aceite
correspondente, identificado pela referência ao mesmo `CE-XX`. Sintetizar no
`Contexto` somente fatos da história, do contexto e do escopo inicial da user
story; colocar qualquer interpretação nova exclusivamente em `Decisões
assumidas`. Tornar explícitos os limites de escopo, especialmente quando a
demanda for ambígua.

## Autonomia e rastreabilidade

Entregar uma spec completa; não parar por causa de ambiguidades. Resolver cada
ponto usando esta ordem de prioridade:

1. decisão explícita em arquivo de contexto disponível;
2. regra de produto, terminologia oficial ou requisito contextual disponível;
3. precedente relevante disponível;
4. raciocínio próprio, explicitando a lógica.

Registrar toda decisão tomada em `Decisões assumidas`, sem exceção. O registro
serve para a revisão humana distinguir o que veio diretamente da user story
do que foi interpretação da skill.

### Contradições com o contexto

Se a solicitação, a user story ou uma demanda anterior contradizer diretamente
um arquivo de contexto aplicável, seguir o contexto e registrar o conflito com
destaque:

> **⚠ Contradição**: indicar o que a demanda pede e qual regra de contexto conflita.
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

A aprovação humana desta primeira etapa do refinamento é obrigatória antes da
`spec-tecnica`. Não aprovar a própria saída nem iniciar a implementação.

## Exemplo

Para a user story “Usuário quer poder exportar relatório em PDF”:

**Bom comportamento esperado:**

> Usuário consegue gerar um PDF do relatório atualmente visível na tela, a
> partir de uma ação disponível na mesma página.

**Ruim — decisão técnica vazada:**

> Sistema usa uma biblioteca de geração de PDF no backend e retorna o arquivo
> por uma rota REST.

**Decisão assumida:**

> **Ambiguidade**: a user story não diz se a exportação respeita os filtros aplicados.
> **Decisão**: respeitar os filtros atuais.
> **Fonte**: arquivos de contexto de produto e precedente disponível.

A `spec-tecnica`, ao consultar o código, classifica a mudança como `nova`,
`modificação` ou `duplicada`; essa classificação não pertence a esta skill.
