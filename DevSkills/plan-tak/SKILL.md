---
name: plan-tak
description: Ajudar a criar ou melhorar uma TAK (task) clara, completa e pronta para as skills de spec funcional e spec técnica, estruturando objetivo e descrição sob a ótica de produto. Usar antes do refinamento, quando o usuário quiser registrar uma demanda, esclarecer seu escopo ou preparar uma TAK. Não usar para gerar specs, decidir implementação, classificar a mudança no código ou executar desenvolvimento.
---

# Planejamento de TAK

Criar ou revisar uma TAK que será entrada da `spec-funcional` e, depois, da
`spec-tecnica`. Transformar a necessidade do usuário em uma demanda clara,
observável e delimitada, sem escrever a spec funcional nem tomar decisões de
implementação.

## Contexto a consultar

Antes de escrever ou alterar a TAK, ler nesta ordem, quando existirem:

1. `.taloren_context/product.md`: produto, personas, regras e fronteiras de escopo;
2. `.taloren_context/glossary.md`: terminologia oficial;
3. `.taloren_context/decisions.md`: decisões de produto aplicáveis;
4. TAKs, decisões e outros arquivos Markdown do projeto diretamente
   relacionados ao mesmo assunto.

Usar esses documentos para evitar termos inconsistentes, demandas que conflitem
com decisões existentes ou duplicação de trabalho já planejado. Não consultar o
código para concluir se a funcionalidade já existe: essa classificação pertence
à `spec-tecnica`.

## Como conduzir

Extrair da solicitação do usuário o problema, o resultado esperado, o usuário
ou processo impactado, limites conhecidos e referências relevantes. Fazer
perguntas somente quando a ausência de informação impedir uma TAK minimamente
compreensível ou puder levar a um escopo arriscado. Quando houver informação
suficiente, elaborar a TAK sem antecipar decisões que pertencem às specs.

Não incluir linguagem, framework, biblioteca, arquivos, classes, rotas,
arquitetura ou solução de implementação. Se o usuário trouxer uma sugestão
técnica, preservá-la em `Contexto adicional` como sugestão a ser avaliada na
spec técnica, nunca como requisito da TAK.

Se a demanda conflitar diretamente com documentação de contexto, não ocultar o
conflito. Registrar a regra existente, a solicitação conflitante e a validação
necessária antes do refinamento.

## Formato da TAK

Criar ou atualizar a TAK em `tasks/TAK-XXXX/tak.md`, preservando
frontmatter e campos existentes. Criar a pasta `tasks/TAK-XXXX/` quando ela não
existir. Garantir, no mínimo, esta estrutura:

```markdown
# TAK-XXXX — Título objetivo

## Objetivo
Uma frase que descreve o resultado de negócio ou usuário esperado, sem explicar
como implementar.

## Descrição
Descrever o problema ou oportunidade, quem é afetado e o comportamento ou
resultado esperado. Incluir regras, limites e referências explicitamente
informados pelo usuário ou pelos documentos de contexto.

## Contexto adicional
Incluir links para TAKs, decisões ou documentos relacionados. Registrar
sugestões técnicas apenas como contexto para avaliação posterior.

## Pontos a esclarecer
Listar somente lacunas que não puderam ser resolvidas com o contexto disponível.
Escrever `Nenhum identificado` quando não houver pontos.
```

Manter `Objetivo` conciso e orientado a resultado. Em `Descrição`, usar termos
do glossário e separar fatos fornecidos de hipóteses. Não transformar a TAK em
uma lista de critérios de aceite detalhados: a `spec-funcional` definirá o
comportamento verificável e os critérios de aceite.

## Qualidade antes de concluir

Considerar a TAK pronta para entrar no refinamento quando:

- possui título, objetivo e descrição preenchidos;
- explica a necessidade e o resultado esperado de forma compreensível;
- usa a terminologia do produto;
- referencia contexto relevante e expõe conflitos conhecidos;
- delimita o que já é conhecido sem inventar requisitos ou implementação;
- deixa em `Pontos a esclarecer` apenas lacunas realmente necessárias.

A TAK pronta é entrada da `spec-funcional`; não aprovar a própria demanda, não
gerar a spec e não iniciar implementação.

## Exemplo

```markdown
# TAK-1234 — Exportar relatório visível

## Objetivo
Permitir que usuários exportem o relatório que estão consultando.

## Descrição
Usuários precisam compartilhar o resultado de um relatório sem reproduzir os
dados manualmente. A exportação deve refletir o relatório visualizado no momento
da solicitação. A definição de formato, permissões detalhadas e demais regras
será refinada na spec funcional.

## Contexto adicional
- [[.taloren_context/product]] — regras de acesso a relatórios.
- Sugestão técnica da solicitação: gerar o arquivo no backend; avaliar na spec
  técnica, não tratar como requisito.

## Pontos a esclarecer
Nenhum identificado.
```
