---
name: spec-coding
description: Implementar a mudança de código a partir da spec técnica aprovada, executar os gates automáticos — testes, lint, padrão de código e review automatizado — e sinalizar prontidão para UAT somente quando todos passarem. Usar na etapa 3 do pipeline, depois da revisão humana da spec técnica. Não usar para gerar specs nem para classificar uma TAK como nova, modificação ou duplicada.
---

# Implementação

Implementar a mudança de código descrita em uma spec técnica aprovada. Esta é a
única skill do pipeline que altera código de produção; as skills anteriores
produzem documentos. Executar a abordagem definida sem reabrir decisões de
escopo, classificação ou arquitetura.

## Pré-condição e entradas

Usar somente após a aprovação humana de `tasks/TAK-XXXX/spec-tecnica.md`.
Confirmar a aprovação pelo status, marcação ou evidência definida pelo pipeline.
Se não houver evidência verificável, não iniciar: registrar o bloqueio e pedir a
regularização da etapa 2. Não iniciar se a classificação for `Duplicada`: esse
caso encerra o fluxo antes da implementação.

Ler sempre, nesta ordem:

1. `tasks/TAK-XXXX/spec-tecnica.md`: executar a abordagem ou o delta já
   decidido, conforme a classificação `Nova` ou `Modificação`.
2. `tasks/TAK-XXXX/spec-funcional.md`: validar ao final que os comportamentos e
   critérios de aceite observáveis foram atendidos.
3. `context/conventions.md`: obrigatório; aplicar convenções e identificar os
   comandos ou critérios de padrão de código definidos.
4. `context/testing.md`: identificar cobertura e comandos de teste exigidos.
5. `context/architecture.md`: usar como referência de padrão geral; a spec
   técnica já deve ter resolvido conflitos arquiteturais.

Ler também os demais arquivos Markdown do projeto diretamente relacionados à
mudança, incluindo decisões, contratos, integrações e guias operacionais.
Inspecionar os scripts, configurações e padrões do repositório necessários para
executar os gates. Não inventar comandos, critérios ou ferramentas ausentes.
Registrar a ausência de um gate obrigatório como falha e escalar conforme o
loop de correção.

## Skills auxiliares

Antes de alterar código, consultar as skills disponíveis e identificar quais
são aplicáveis ao domínio, linguagem, tipo de mudança e artefatos afetados.
Dar preferência às skills cujo nome começa com `dev-`, por serem as convenções
de desenvolvimento do workspace. Ler e seguir integralmente as instruções das
skills selecionadas.

Selecionar apenas skills relevantes; não carregar uma skill sem relação com a
mudança. Exemplos: usar `dev-backend-nodejs` para alterações de backend Node.js
com TypeScript, `dev-frontend-nodejs` para frontend Node.js com TypeScript e
`dev-commit` somente quando a tarefa incluir preparar ou criar commit. Se não
houver skill `dev-` aplicável, considerar as demais skills disponíveis que
atendam diretamente à necessidade, respeitando suas condições de uso.

Registrar em `implementacao-log.md` as skills auxiliares consultadas e o motivo
da seleção. Quando nenhuma for aplicável, registrar essa conclusão e a fonte da
análise. As skills auxiliares complementam esta skill; não podem ampliar o
escopo definido na spec técnica nem substituir seus gates obrigatórios.

## Linha de base

Antes de alterar código, registrar em `implementacao-log.md`:

- o estado inicial do repositório, incluindo alterações preexistentes;
- os resultados dos gates que puderem ser executados sem a implementação;
- falhas preexistentes ou limitações do ambiente, com evidência.

Não atribuir à TAK uma falha comprovadamente preexistente. Se ela impedir a
validação obrigatória, tratá-la como bloqueio externo e escalar.

## Implementação por classificação

- **Nova**: seguir integralmente a abordagem da spec técnica e criar apenas os
  módulos e arquivos necessários.
- **Modificação**: alterar o mínimo necessário ao redor do código existente.
  Reaproveitar padrões, helpers e estruturas já presentes no código afetado.
  Não refatorar fora do delta da spec, mesmo que haja algo melhorável.

Registrar uma melhoria identificada, mas fora do escopo, em `Decisões e
observações` como **Fora de escopo — não implementado**. Não converter essa
sugestão em código não solicitado.

## Plano de implementação

Antes de alterar código, criar em `tasks/TAK-XXXX/implementacao-log.md` a seção
`## Plano de implementação`. Decompor a abordagem ou o delta aprovado da spec
técnica em um checklist de itens verificáveis, incluindo criação ou alteração de
código, dados, contratos, integrações e testes quando aplicáveis. Incluir também
os gates e a geração do resumo final.

O plano apenas detalha a execução; não redefine a abordagem, classificação ou
escopo da spec técnica. Marcar cada item como concluído somente após a execução
correspondente. Registrar qualquer desvio ou melhoria identificada em `Decisões
e observações`, sem incluir trabalho fora de escopo no checklist. Relacionar os
itens de testes aos `CE-XX` e `CA-XX` aplicáveis.

Exemplo:

```markdown
## Plano de implementação
- [ ] Alterar o módulo responsável por ...
- [ ] Atualizar dados, contratos ou integrações afetados
- [ ] Criar ou ajustar testes para CA-01 e CA-02
- [ ] Executar todos os gates automáticos
- [ ] Gerar `implementacao-resumo.md`
```

## Gates automáticos

Antes de executar cada gate, identificar seu comando ou procedimento oficial no
projeto. Consultar, conforme aplicável, scripts e configurações do repositório,
CI/CD, `README.md`, `CONTRIBUTING.md`, `context/testing.md` e
`context/conventions.md`. Não pressupor linguagem, gerenciador de pacotes,
ferramenta ou sintaxe de comando. Registrar em `implementacao-log.md`, para cada
gate, o procedimento executado, sua fonte e o resultado.

Após implementar, executar os gates nesta ordem, usando os comandos e critérios
definidos no projeto:

1. testes exigidos por `context/testing.md`, incluindo unitários e, quando
   aplicável, integração, contrato, E2E e regressão;
2. lint;
3. padrão de código, conforme `context/conventions.md`;
4. review automatizado.

O review automatizado deve avaliar o diff da TAK quanto à aderência à spec
técnica, alterações fora de escopo, segurança, tratamento de erros, regressões
e cobertura de testes. Todos os gates são obrigatórios. Só sinalizar a TAK como
pronta para UAT humano quando todos passarem.

Quando uma skill auxiliar executar um gate, aceitar seu resultado sem duplicar a
execução somente se o gate tiver sido concluído após a última alteração de código
e houver evidência suficiente. Registrar em `implementacao-log.md` o gate, a
skill responsável, o procedimento, a fonte, o resultado e o estado do código
validado. Qualquer alteração posterior invalida essa evidência para os gates
afetados, que devem ser executados novamente antes do UAT. A `spec-coding`
continua responsável por conferir a cobertura de todos os gates e pelo status
final da TAK.

## Loop de correção

Criar ou atualizar `tasks/TAK-XXXX/implementacao-log.md` desde a primeira
implementação. Uma tentativa corresponde a uma rodada completa de implementar
ou corrigir e executar todos os gates na ordem definida.

Quando qualquer gate falhar:

1. registrar o número da tentativa, o gate, a causa e a alteração realizada
   para correção;
2. corrigir somente o necessário, respeitando a spec técnica;
3. durante o diagnóstico, executar primeiro o gate afetado quando isso reduzir
   o tempo de feedback; antes de concluir a tentativa, executar todos os gates
   novamente, na ordem definida.

Permitir no máximo cinco tentativas por rodada de implementação, incluindo a
primeira. Se a quinta tentativa ainda falhar, parar: não executar uma sexta,
não avançar para UAT e gerar o resumo com status `Bloqueado`.

Se a falha for externa ou de configuração — por exemplo, credencial ausente,
ambiente indisponível, dependência externa indisponível, gate não configurado
ou falha preexistente que impeça validação — registrar como **Bloqueio externo**
com evidências e escalar imediatamente; não consumir tentativas repetindo uma
execução que não pode ser corrigida no código. Nos demais casos, escalar após a
quinta tentativa com o log completo. A revisão humana decide entre nova direção
na etapa 3 ou retorno à etapa 2 via `spec-correcao`.

## Validação de critérios de aceite

Registrar no log, após os testes e antes de gerar o resumo, a evidência de
implementação de cada critério aplicável:

```markdown
## Validação de critérios de aceite
- [x] CA-01 / CE-01 — evidência: teste, fluxo ou artefato correspondente.
- [ ] CA-02 / CE-02 — pendente ou bloqueado: motivo.
```

Não marcar um critério como atendido sem evidência verificável. Critério pendente
ou bloqueado impede o status `Pronto para UAT`.

## Decisões e observações

Registrar no log toda decisão de baixo nível não coberta pela spec técnica, bem
como toda sugestão fora de escopo. Usar este formato:

```markdown
- **Tipo:** Decisão | Fora de escopo — não implementado
- **Contexto/Ambiguidade:** ...
- **Decisão/Observação:** ...
- **Fonte:** `conventions.md` / precedente no código / outro documento do projeto /
  raciocínio próprio.
```

Não usar essa seção para alterar silenciosamente o escopo funcional ou técnico.

## Nova rodada após correção de UAT

A skill `spec-correcao` é responsável por produzir a correção após uma rejeição
no UAT humano. Quando uma spec de correção aprovada for fornecida, tratá-la como
uma nova rodada de spec técnica: ler suas instruções, implementar o delta e
reiniciar o limite de cinco tentativas para essa rodada. Não escrever a spec de
correção nesta skill.

## Revisão final do diff

Antes de gerar o resumo, revisar as alterações efetivas para confirmar que não
há arquivos acidentais, segredos, artefatos gerados indevidos ou mudanças sem
relação com o plano e a spec técnica. Remover ou reverter o que não pertencer à
TAK e registrar qualquer exceção justificada no log.

## Artefatos de saída

Além do código, sempre produzir:

- `tasks/TAK-XXXX/implementacao-log.md`: usar, nesta ordem, as seções
  `Skills auxiliares`, `Linha de base`, `Plano de implementação`, `Tentativas e
  gates`, `Validação de critérios de aceite`, `Decisões e observações` e
  `Bloqueios e escalonamento`; em `Tentativas e gates`, registrar para cada
  gate o procedimento executado, a fonte que o definiu, o resultado e as
  evidências relevantes;
- `tasks/TAK-XXXX/implementacao-resumo.md`: ponto único de partida para o UAT
  humano, inclusive quando a implementação estiver bloqueada.

Usar este formato para o resumo:

```markdown
# Resumo de implementação — TAK-XXXX

## Status
Pronto para UAT | Bloqueado (ver [[implementacao-log.md]])

## Gates
Todos passaram na tentativa N | Listar falhas corrigidas, com referência às
respectivas tentativas em [[implementacao-log.md]].

## Critérios de aceite
Copiar literalmente os critérios de aceite de `spec-funcional.md`, com um item
por linha e as referências `CA-XX` e `CE-XX`. Não resumir, reinterpretar ou
criar critérios novos.

## Alterações fora do escopo original
Listar os itens marcados como **Fora de escopo — não implementado** no log.
Escrever `Nenhuma` se não houver itens.
```

Em caso de bloqueio, gerar o resumo com os gates pendentes e as tentativas
realizadas. A TAK não segue para UAT até ser destravada.

## Critério de conclusão

Considerar a TAK pronta para Human UAT somente quando:

- o código estiver conforme a abordagem ou delta da spec técnica;
- todos os gates automáticos tiverem passado;
- os comportamentos e critérios de aceite da spec funcional tiverem evidência
  registrada na seção `Validação de critérios de aceite`;
- o diff final tiver sido revisado e contiver somente alterações pertinentes à
  TAK, sem segredos ou artefatos indevidos;
- `implementacao-log.md` registrar decisões e todas as tentativas realizadas;
- `implementacao-resumo.md` existir com status `Pronto para UAT`;
- nenhuma alteração fora do escopo da spec técnica tiver sido implementada.

Não aprovar o UAT humano, não alterar a classificação da TAK e não iniciar uma
spec de correção.

## Exemplos

**Decisão de implementação:**

> **Tipo:** Decisão  
> **Contexto/Ambiguidade:** havia helper existente e a spec não definia se ele
> deveria ser reutilizado.  
> **Decisão/Observação:** reaproveitado `formatarData()` com um parâmetro extra,
> em vez de criar utilitário duplicado.  
> **Fonte:** `conventions.md` — evitar duplicação de utilitários equivalentes.

**Tentativa de correção:**

> **Tentativa 2/5**: testes unitários falharam em caso de lista vazia. Corrigida
> a validação antes do processamento principal; todos os gates foram executados
> novamente a partir dos testes unitários.

**Escalonamento:**

> **Bloqueado após 5 tentativas**: testes continuam falhando em escrita
> simultânea. A abordagem da spec técnica pode não ser viável como descrita;
> recomendar revisão humana da spec técnica antes de nova implementação.
