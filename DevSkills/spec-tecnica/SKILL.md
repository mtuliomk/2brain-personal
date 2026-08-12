---
name: spec-tecnica
description: Gerar a especificação técnica de uma TAK a partir da spec funcional, descrevendo como implementá-la com base no código existente e classificando a mudança como nova, modificação ou duplicada. Usar depois de `tasks/TAK-XXXX/spec-funcional.md` estar pronta, durante o refinamento e antes de qualquer implementação. Não usar para definir comportamento funcional nem para implementar código.
---

# Spec técnica

Gerar a especificação técnica de uma TAK: descrever como implementar o que a
spec funcional definiu. Verificar também, no código existente, se a feature já
existe — decisão que pertence a esta skill, não à spec funcional.

Não alterar o escopo funcional silenciosamente. A spec técnica deve explicar a
implementação, não repetir o comportamento esperado.

## Entradas obrigatórias

Ler sempre, nesta ordem:

1. `tasks/TAK-XXXX/spec-funcional.md`: identificar o que deve ser verdadeiro
   depois da mudança; não questionar o *quê*, apenas decidir o *como*.
2. Código-fonte relevante: procurar a feature, equivalentes e pontos de
   integração existentes.
3. `context/architecture.md`: aplicar os padrões arquiteturais estabelecidos.
4. `context/conventions.md`: aplicar convenções de código, nomenclatura e
   estrutura.
5. `context/decisions.md`: recuperar decisões técnicas aplicáveis.
6. `context/testing.md`: identificar a cobertura de testes exigida.

Se algum documento de contexto não existir, registrar essa ausência em
"Decisões assumidas" e seguir com as demais fontes disponíveis.

## Classificação obrigatória

Abrir toda spec técnica com uma classificação, resultado da busca no código:

- **Nova**: não existir nada equivalente; a mudança criar algo novo.
- **Modificação**: existir uma feature relacionada; a mudança alterar ou
  estender o que já existe.
- **Duplicada**: a spec funcional descrever, essencialmente, algo que já existe.

Justificar a classificação com referências precisas a arquivos, módulos,
classes, funções ou rotas.

## Fluxo por classificação

- **Nova**: produzir a spec técnica completa.
- **Modificação**: produzir apenas o delta sobre o sistema existente.
- **Duplicada**: não descrever uma implementação nova. Produzir um documento
  curto apontando onde a feature já existe, para que a revisão humana decida se
  encerra a TAK ou se a spec funcional descreveu uma diferença sutil.

## Formato de saída: nova

Escrever em `tasks/TAK-XXXX/spec-tecnica.md`:

```markdown
# Spec técnica — TAK-XXXX

## Classificação
Nova — justificar por que não há nada equivalente no código.

## Abordagem
Descrever como implementar a mudança, com detalhe suficiente para orientar a
implementação sem ambiguidade: módulos afetados, pontos de integração e
decisões principais de design.

## Trade-offs
Registrar alternativas razoáveis consideradas e por que a abordagem escolhida
foi preferida, quando aplicável.

## Impacto
Descrever efeitos fora do módulo principal: dados, contratos de API,
dependências e comportamento de outras features.

## Decisões assumidas
Registrar toda decisão técnica não coberta explicitamente por
architecture.md, conventions.md ou decisions.md. Usar o formato:
ambiguidade, decisão e fonte — ou raciocínio próprio quando não houver fonte.

## Plano de testes
Aplicar a esta mudança o que testing.md exigir, especificando os cenários e
níveis de teste relevantes.
```

## Formato de saída: modificação

Não reconstruir o que já existe. Escrever apenas o delta:

```markdown
# Spec técnica — TAK-XXXX

## Classificação
Modificação — indicar exatamente o código existente que será alterado
(arquivo, módulo, classe, função ou rota).

## O que muda
Descrever somente a alteração sobre o comportamento ou código atual. Incluir,
em uma linha, impactos em contratos de API, dados persistidos ou outras
features, quando houver.

## Decisões assumidas
Registrar apenas as decisões relevantes para o delta, no mesmo formato da
spec técnica de uma mudança nova.

## Plano de testes
Listar somente a cobertura do delta. Não repetir testes existentes que
continuam válidos.
```

## Formato de saída: duplicada

```markdown
# Spec técnica — TAK-XXXX

## Classificação
Duplicada

## Onde já existe
Indicar a referência exata no código — arquivo, módulo, classe, função ou
rota — que já entrega o comportamento descrito na spec funcional.

## Observação
Registrar diferenças sutis entre a spec funcional e o comportamento existente,
se houver, para a revisão humana decidir entre encerrar a TAK ou fazer um
ajuste pontual.
```

## Autonomia e decisões

Não parar por causa de uma decisão técnica em aberto. Decidir usando esta
ordem de prioridade:

1. decisão técnica aplicável em `context/decisions.md`;
2. padrão de `context/architecture.md` ou `context/conventions.md`;
3. precedente direto no código;
4. raciocínio próprio, explicitando a lógica.

Registrar em "Decisões assumidas" toda decisão tomada por qualquer desses
caminhos, sem exceção.

### Contradições com o contexto

Se a abordagem mais razoável colidir com um padrão estabelecido em
`architecture.md`, `conventions.md` ou `decisions.md`, seguir o contexto,
não a preferência de implementação. Registrar o conflito em destaque:

> **⚠ Contradição**: explicar o padrão que entra em conflito e o que a
> abordagem mais óbvia exigiria quebrar.
>
> **Decisão**: indicar o padrão seguido e o que teve de ceder.
>
> **Ação recomendada**: se o padrão precisar mudar, sugerir uma decisão
> separada de arquitetura, convenções ou contexto; não resolver isso
> silenciosamente nesta TAK.

## Critério de conclusão

Considerar a spec pronta para revisão humana quando:

- a classificação estiver definida, justificada e apoiada por referências;
- uma mudança **Nova** tiver todas as seções do formato completo preenchidas;
- uma **Modificação** descrever o delta com precisão;
- uma **Duplicada** identificar o código existente com precisão suficiente
  para verificação rápida;
- o plano de testes cobrir o que muda e o que `testing.md` exigir;
- toda decisão técnica não coberta pelo contexto estiver registrada.

A revisão humana continua obrigatória na etapa 2 do pipeline. Não aprovar a
própria saída nem iniciar implementação.

## Exemplo

Para a spec funcional "Usuário consegue gerar um PDF do relatório atualmente
visível na tela":

> **Decisão técnica**: gerar o PDF no cliente, não no servidor.  
> **Fonte**: `architecture.md` estabelece que operações que não precisam de
> dados adicionais do backend devem ocorrer no cliente para reduzir a carga do
> servidor.

Exemplo de contradição:

> **⚠ Contradição**: a abordagem mais simples exigiria uma dependência de
> geração de PDF não aprovada em `conventions.md`.  
> **Decisão**: usar a biblioteca já presente no projeto, mesmo exigindo mais
> código de adaptação.  
> **Ação recomendada**: avaliar a adoção de uma nova biblioteca em uma
> alteração separada de convenções, não nesta TAK.

