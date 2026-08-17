---
name: spec-userstory
description: Gerar ou revisar um documento de user story que registre uma demanda sob a ótica de valor para usuário ou negócio e sirva de entrada para uma especificação funcional completa. Usar ao registrar ou esclarecer uma demanda, antes da spec funcional. Não usar para definir critérios de aceite detalhados, tomar decisões de implementação, classificar mudanças no código ou desenvolver.
---

# User story

Gerar ou revisar uma user story que será a entrada da `spec-funcional`.
Transformar a necessidade informada em uma narrativa curta, clara e rastreável,
centrada na pessoa ou processo beneficiado e no valor esperado. O documento não
substitui a especificação funcional: esta definirá depois os comportamentos
verificáveis e os critérios de aceite.

Neste pipeline, `TAK-XXXX` identifica a demanda; a user story é o documento
que registra seu contexto e valor de negócio.

## Contexto a consultar

Antes de escrever ou alterar a user story, ler nesta ordem, quando existirem:

1. `.taloren_context/product.md`: produto, personas, regras e limites de escopo;
2. `.taloren_context/glossary.md`: terminologia oficial;
3. `.taloren_context/decisions.md`: decisões de produto aplicáveis;
4. user stories, demandas anteriores, decisões e outros arquivos Markdown diretamente
   relacionados ao mesmo assunto.

Usar esses documentos para manter a terminologia consistente, identificar
conflitos e evitar registrar uma demanda já descrita. Não consultar o código
para concluir se a funcionalidade existe: essa análise pertence à
`spec-tecnica`.

## Como conduzir

Extrair da solicitação o problema ou oportunidade, a persona ou processo
impactado, a necessidade, o benefício esperado, limites conhecidos e
referências relevantes. Quando a informação for suficiente, elaborar a user
story sem antecipar comportamentos detalhados ou decisões das próximas etapas.

Fazer perguntas somente quando não for possível identificar a pessoa ou
processo impactado, a necessidade ou o benefício. Registrar as demais lacunas
em `Pontos a esclarecer`; não inventar regras ou resolver ambiguidades que
competem à `spec-funcional`.

Não incluir linguagem, framework, biblioteca, arquivos, classes, rotas,
arquitetura ou solução de implementação. Se houver sugestão técnica na
solicitação, registrá-la em `Contexto adicional` como hipótese para avaliação
posterior, nunca como requisito.

Quando a demanda conflitar diretamente com contexto disponível, destacar o
conflito, a regra existente e a validação necessária. Não ocultar o conflito nem
alterar silenciosamente o pedido.

## Estrutura da user story

Gerar a user story com, no mínimo, esta estrutura:

```markdown
# User story — TAK-XXXX — Título objetivo

## História
Como **[persona ou processo]**, quero **[necessidade ou capacidade]** para
**[benefício ou resultado esperado]**.

## Contexto
Descrever o problema ou oportunidade e quem é afetado. Registrar somente fatos
fornecidos ou confirmados nos documentos de contexto.

## Escopo inicial
Resumir o resultado pretendido e os limites já conhecidos, sem listar
comportamentos detalhados nem critérios de aceite.

## Contexto adicional
Incluir links para demandas, decisões e documentos relacionados. Registrar
sugestões técnicas apenas como contexto para avaliação posterior.

## Pontos a esclarecer
Listar lacunas que precisarão ser tratadas na spec funcional. Escrever `Nenhum
identificado` quando não houver pontos.
```

Manter a seção `História` em uma frase orientada a valor, usando a terminologia
do produto. Quando não existir uma persona humana, usar o processo ou papel que
recebe o benefício. Em `Contexto` e `Escopo inicial`, diferenciar fatos de
hipóteses; hipóteses devem ir para `Pontos a esclarecer`.

Não transformar a user story em uma spec funcional: não criar `CE-XX`, `CA-XX`,
fluxos completos, regras detalhadas, cenários de erro ou decisões assumidas.

## Qualidade antes de concluir

Considerar a user story pronta para servir de entrada da `spec-funcional`
quando:

- possui título e a frase `Como..., quero..., para...` preenchidos;
- explica o problema, o público ou processo afetado e o valor esperado;
- registra limites, contexto e referências relevantes sem inventar requisitos;
- usa a terminologia oficial e explicita conflitos conhecidos;
- separa as lacunas remanescentes em `Pontos a esclarecer`;
- não contém critérios de aceite, detalhamento funcional ou decisões técnicas.

Não gerar a spec funcional, não aprovar a própria demanda e não iniciar
implementação.

## Exemplo

```markdown
# User story — TAK-1234 — Exportar relatório visível

## História
Como **analista**, quero **exportar o relatório que estou consultando** para
**compartilhar seus resultados sem reproduzir os dados manualmente**.

## Contexto
Analistas precisam compartilhar resultados de relatórios com outras pessoas. O
relatório consultado é a referência para a exportação solicitada.

## Escopo inicial
A demanda contempla disponibilizar uma forma de exportar o relatório visualizado.
Formato, permissões, filtros e demais regras serão definidos na spec funcional.

## Contexto adicional
- [[.taloren_context/product]] — regras de acesso a relatórios.
- Sugestão técnica da solicitação: gerar o arquivo no backend; avaliar na spec
  técnica, não tratar como requisito.

## Pontos a esclarecer
- Quais formatos de exportação devem ser suportados?
- A exportação deve respeitar filtros aplicados?
```
