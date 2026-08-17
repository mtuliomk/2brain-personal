---
name: spec-tecnica
description: Gerar a especificação técnica de uma demanda a partir de uma spec funcional aprovada disponível no contexto, descrevendo como implementá-la com base no código existente e classificando a mudança como nova, modificação ou duplicada. Usar na segunda etapa do refinamento, antes da implementação. Não usar para definir comportamento funcional nem para implementar código.
---

# Spec técnica

Gerar a especificação técnica de uma demanda: descrever como implementar o que
a spec funcional aprovou. Esta é a segunda etapa do refinamento, executada
após a aprovação humana da spec funcional. Verificar também, no código
existente, se a feature já existe — decisão que pertence a esta skill, não à
spec funcional.

Não alterar o escopo funcional silenciosamente. A spec técnica deve explicar a
implementação, não repetir o comportamento esperado. A aprovação humana desta
spec é pré-requisito para a `spec-coding`.

## Contexto de entrada

Considerar todos os arquivos de contexto já lidos ou disponibilizados na área de
trabalho, independentemente de caminho, pasta ou nome. Priorizar uma spec
funcional aprovada como fonte do escopo funcional, além de decisões técnicas,
padrões arquiteturais, convenções, requisitos de testes, contratos, integrações
e guias operacionais diretamente relacionados à mudança.

Quando uma user story ou demanda anterior estiver disponível no contexto, lê-la
por completo apenas para preservar contexto e identificar eventual perda de
informação na transição para a spec funcional; não redefinir o escopo a partir
dela.

Inspecionar também o código-fonte relevante para localizar a feature,
equivalentes e pontos de integração existentes. Se um contexto necessário não
estiver disponível, registrar sua ausência em `Decisões assumidas` e seguir com
as fontes existentes.

## Investigação no código

Antes de classificar a demanda, pesquisar sistematicamente os termos da user
story, da spec funcional e da demanda e inspecionar, quando aplicável:

- fluxos, telas, rotas, serviços e integrações equivalentes;
- modelos, persistência, migrações e contratos de API;
- testes, configurações e permissões relacionados;
- pontos de entrada e módulos que chamam ou expõem o comportamento.

Não concluir que uma mudança é **Nova** apenas porque não há evidência no ponto
inicial da busca. Registrar as evidências relevantes na classificação.

## Classificação obrigatória

Abrir toda spec técnica com uma classificação, resultado da busca no código:

- **Nova**: não existir implementação que entregue o comportamento funcional
  solicitado.
- **Modificação**: existir uma feature relacionada, ou o sistema já entregar
  apenas parte dos comportamentos ou critérios de aceite; a mudança altera ou
  estende o que existe.
- **Duplicada**: o sistema já entregar todos os comportamentos e critérios de
  aceite relevantes da spec funcional, sem diferença funcional material.

Justificar a classificação com referências precisas a arquivos, módulos,
classes, funções ou rotas. Incluir uma tabela de evidências que relacione cada
`CE-XX` ou `CA-XX` relevante à evidência encontrada e à conclusão.

## Fluxo por classificação

- **Nova**: produzir a spec técnica completa.
- **Modificação**: produzir apenas o delta sobre o sistema existente.
- **Duplicada**: não descrever uma implementação nova. Produzir uma análise
  curta apontando onde a feature já existe, para que a revisão humana decida se
  encerra a demanda ou se a spec funcional descreveu uma diferença sutil.

## Estrutura da spec técnica: nova

Gerar a spec com esta estrutura:

```markdown
# Spec técnica — TAK-XXXX

## Classificação
Nova — justificar a classificação e incluir uma tabela:

| Referência funcional | Evidência no código | Conclusão |
| --- | --- | --- |
| CE-01 / CA-01 | Arquivos, módulos, funções ou rotas investigados | Não existe equivalente |

## Abordagem

### Componentes e módulos afetados
...

### Fluxo principal
...

### Dados e contratos
...

### Integrações
...

### Compatibilidade e migração
Descrever quando aplicável; caso contrário, indicar que não há impacto identificado.

## Trade-offs
Registrar alternativas razoáveis consideradas e por que a abordagem escolhida
foi preferida, quando aplicável.

## Impacto
Descrever efeitos fora do módulo principal: dados, contratos de API,
dependências e comportamento de outras features. Avaliar explicitamente, quando
aplicável, permissões e segurança, observabilidade, desempenho, dados
existentes, compatibilidade e rollout.

## Fora do escopo técnico
Se necessário, registrar alterações técnicas relacionadas que foram
consideradas, mas não fazem parte da demanda.

## Divergências com a spec funcional
Incluir somente se a análise revelar perda de informação, conflito,
inviabilidade ou diferença de interpretação. Não alterar a spec funcional:
descrever o impacto e recomendar retorno à revisão de refinamento.

## Decisões assumidas
Registrar cada decisão técnica ou ausência de contexto que não esteja coberta
explicitamente pelos documentos de contexto, usando:

- **Ambiguidade:** ...
- **Decisão:** ...
- **Fonte:** arquivo de contexto técnico relevante / código / precedente
  disponível / raciocínio próprio.

## Plano de testes
Aplicar os requisitos de testes definidos no contexto disponível. Relacionar
cada cenário aos `CE-XX` e `CA-XX` da spec funcional e especificar os níveis de
teste relevantes.

## Controle de versão
Este deve ser obrigatoriamente o último conteúdo do documento. Manter todo o
histórico já registrado e adicionar uma linha a cada alteração material do
artefato; para a criação inicial, usar a versão `1.0`. Não incluir seções,
texto ou anexos depois desta tabela.

| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | AAAA-MM-DD | Agente | Criação inicial do documento |
```

## Estrutura da spec técnica: modificação

Não reconstruir o que já existe. Escrever apenas o delta:

```markdown
# Spec técnica — TAK-XXXX

## Classificação
Modificação — indicar exatamente o código existente que será alterado e incluir
a tabela de evidências por `CE-XX` ou `CA-XX`.

| Referência funcional | Evidência no código | Conclusão |
| --- | --- | --- |
| CE-01 / CA-01 | Arquivos, módulos, funções ou rotas investigados | Alterar ou estender |

## O que muda
Descrever somente a alteração sobre o comportamento ou código atual. Incluir
impactos em contratos de API, dados persistidos, integrações, permissões,
segurança, observabilidade, desempenho, compatibilidade ou rollout, quando
houver.

## Fora do escopo técnico
Incluir somente se houver exclusões técnicas relevantes.

## Divergências com a spec funcional
Incluir somente quando aplicável, sem alterar a spec funcional silenciosamente.

## Decisões assumidas
Registrar apenas as decisões e ausências de contexto relevantes para o delta,
no formato: ambiguidade, decisão e fonte.

## Plano de testes
Listar somente a cobertura do delta e relacionar cada cenário aos `CE-XX` e
`CA-XX` afetados. Não repetir testes existentes que continuam válidos.

## Controle de versão
Este deve ser obrigatoriamente o último conteúdo do documento. Manter todo o
histórico já registrado e adicionar uma linha a cada alteração material do
artefato; para a criação inicial, usar a versão `1.0`. Não incluir seções,
texto ou anexos depois desta tabela.

| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | AAAA-MM-DD | Agente | Criação inicial do documento |
```

## Estrutura da spec técnica: duplicada

```markdown
# Spec técnica — TAK-XXXX

## Classificação
Duplicada — incluir uma tabela de evidências demonstrando que todos os `CE-XX`
e `CA-XX` relevantes já são atendidos.

| Referência funcional | Evidência no código | Conclusão |
| --- | --- | --- |
| CE-01 / CA-01 | Arquivo, módulo, função ou rota existente | Já atendido |

## Onde já existe
Indicar a referência exata no código que entrega cada comportamento descrito na
spec funcional.

## Observação
Registrar diferenças sutis entre a spec funcional e o comportamento existente,
se houver. Havendo diferença funcional material ou algum critério não atendido,
reclassificar como `Modificação`.

## Decisões assumidas
Registrar decisões de classificação, ausências de contexto ou interpretações
necessárias, no formato: ambiguidade, decisão e fonte.

## Recomendação
Indicar claramente se a demanda deve ser encerrada ou devolvida ao refinamento
para esclarecer uma diferença funcional.

## Controle de versão
Este deve ser obrigatoriamente o último conteúdo do documento. Manter todo o
histórico já registrado e adicionar uma linha a cada alteração material do
artefato; para a criação inicial, usar a versão `1.0`. Não incluir seções,
texto ou anexos depois desta tabela.

| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | AAAA-MM-DD | Agente | Criação inicial do documento |
```

## Autonomia e decisões

Não parar por causa de uma decisão técnica em aberto. Decidir usando esta ordem
de prioridade:

1. decisão técnica explícita em arquivo de contexto disponível;
2. padrão arquitetural ou convenção técnica disponível;
3. outro requisito ou documento de contexto relevante;
4. precedente direto no código;
5. raciocínio próprio, explicitando a lógica.

Registrar em `Decisões assumidas` toda decisão tomada por qualquer desses
caminhos, sem exceção, no formato `Ambiguidade` / `Decisão` / `Fonte`.

### Contradições com o contexto

Se a abordagem mais razoável colidir com um padrão estabelecido em arquivo de
contexto aplicável, seguir o contexto, não a preferência de implementação.
Registrar o conflito em destaque:

> **⚠ Contradição**: explicar o padrão que entra em conflito e o que a
> abordagem mais óbvia exigiria quebrar.
>
> **Decisão**: indicar o padrão seguido e o que teve de ceder.
>
> **Ação recomendada**: se o padrão precisar mudar, sugerir uma decisão
> separada de arquitetura, convenções ou contexto; não resolver isso
> silenciosamente nesta demanda.

## Critério de conclusão

Considerar a spec pronta para revisão humana quando:

- a classificação estiver definida, justificada e apoiada pela tabela de
  evidências;
- uma mudança **Nova** tiver todas as seções do formato completo preenchidas;
- uma **Modificação** descrever o delta com precisão;
- uma **Duplicada** demonstrar que todos os critérios relevantes já são
  atendidos e trouxer uma recomendação clara;
- o plano de testes cobrir os `CE-XX` e `CA-XX` afetados e os requisitos de
  teste definidos no contexto;
- toda decisão técnica, ausência de contexto ou divergência funcional estiver
  registrada;
- nenhuma mudança de escopo funcional tiver sido feita silenciosamente.

A aprovação humana desta segunda etapa do refinamento é obrigatória antes da
`spec-coding`. Não aprovar a própria saída nem iniciar implementação.

## Exemplo

Para a spec funcional "Usuário consegue gerar um PDF do relatório atualmente
visível na tela":

> **Decisão técnica**: gerar o PDF no cliente, não no servidor.  
> **Fonte**: o contexto arquitetural disponível estabelece que operações que não
> precisam de dados adicionais do backend devem ocorrer no cliente para reduzir
> a carga do servidor.

Exemplo de contradição:

> **⚠ Contradição**: a abordagem mais simples exigiria uma dependência de
> geração de PDF não aprovada pelas convenções disponíveis.
> **Decisão**: usar a biblioteca já presente no projeto, mesmo exigindo mais
> código de adaptação.  
> **Ação recomendada**: avaliar a adoção de uma nova biblioteca em uma
> alteração separada de convenções, não nesta demanda.
