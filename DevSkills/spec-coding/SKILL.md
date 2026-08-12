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

Usar somente após a aprovação humana de `tasks/TAK-XXXX/spec-tecnica.md`. Não
iniciar se a classificação for `Duplicada`: esse caso encerra o fluxo antes da
implementação.

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

## Implementação por classificação

- **Nova**: seguir integralmente a abordagem da spec técnica e criar apenas os
  módulos e arquivos necessários.
- **Modificação**: alterar o mínimo necessário ao redor do código existente.
  Reaproveitar padrões, helpers e estruturas já presentes no código afetado.
  Não refatorar fora do delta da spec, mesmo que haja algo melhorável.

Registrar uma melhoria identificada, mas fora do escopo, em `Decisões e
observações` como **Fora de escopo — não implementado**. Não converter essa
sugestão em código não solicitado.

## Gates automáticos

Após implementar, executar os gates nesta ordem, usando os comandos e critérios
definidos no projeto:

1. testes unitários, conforme `context/testing.md`;
2. lint;
3. padrão de código, conforme `context/conventions.md`;
4. review automatizado.

Todos os gates são obrigatórios. Só sinalizar a TAK como pronta para UAT humano
quando todos passarem. Registrar o comando executado, o resultado e um resumo
da saída de cada gate em `implementacao-log.md`.

## Loop de correção

Criar ou atualizar `tasks/TAK-XXXX/implementacao-log.md` desde a primeira
implementação. Uma tentativa corresponde a uma rodada completa de implementar
ou corrigir e executar todos os gates, na ordem definida.

Quando qualquer gate falhar:

1. registrar o número da tentativa, o gate, o comando, a causa e a alteração
   realizada para correção;
2. corrigir somente o necessário, respeitando a spec técnica;
3. executar novamente todos os gates, começando pelos testes unitários.

Permitir no máximo cinco tentativas por rodada de implementação, incluindo a
primeira. Se a quinta tentativa ainda falhar, parar: não executar uma sexta,
não avançar para UAT e gerar o resumo com status `Bloqueado`. Escalar para
revisão humana com o log completo. A revisão humana decide entre nova direção
na etapa 3 ou retorno à etapa 2 via `spec-correcao`.

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

## Artefatos de saída

Além do código, sempre produzir:

- `tasks/TAK-XXXX/implementacao-log.md`: tentativas, execução dos gates,
  decisões e observações;
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
Checklist copiada fielmente de `spec-funcional.md`, com um item por linha e as
referências `CA-XX` e `CE-XX`. Não reescrever os critérios.

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
- os comportamentos e critérios de aceite da spec funcional tiverem sido
  verificados contra a implementação e os testes executados;
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
