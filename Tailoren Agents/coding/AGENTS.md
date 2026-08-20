Este arquivo define o contexto operacional do agente responsável por implementar a task. A skill `spec-coding` define o fluxo de implementação, validação, commits e entrega; este arquivo define como a etapa deve ser executada no workspace.

## Skills obrigatórias

Use obrigatoriamente as skills `spec-coding` e `dev-commit` disponibilizadas no worker em `/workspace/.taloren-docs-skills`.

Antes de alterar código, liste todas as skills disponíveis:

```bash
if [ -d /workspace/.taloren-docs-skills ]; then find /workspace/.taloren-docs-skills -type f -name 'SKILL.md' -print; fi
```

Leia integralmente as skills relevantes à linguagem, camada, framework e tipo de alteração. Use as skills de desenvolvimento aplicáveis, como `dev-backend-nodejs`, `dev-backend-golang` e `dev-frontend-nodejs`, quando o código afetado corresponder ao seu domínio. As skills complementam a especificação técnica e não podem ampliar seu escopo.

## Autonomia e tratamento de incertezas

Execute toda a etapa de forma autônoma. Não solicite, aguarde nem condicione a conclusão a aprovação, validação, decisão ou qualquer outra intervenção humana.

Quando uma informação estiver ausente, ambígua ou conflitante, investigue a especificação técnica, a especificação funcional, as user stories, os documentos da aplicação, as skills e o código disponível. Adote a premissa mais restrita compatível com as evidências e os *guardrails*, sem inventar requisitos ou alterar escopo. Registre a ambiguidade, a decisão, a fonte e qualquer limitação em `implementation-result.md`.

## Contexto da aplicação

Antes de implementar, liste e leia integralmente os documentos disponíveis:

```bash
find /workspace/.taloren-docs-context/application -type f -name '*.md' -print
```

Use esse contexto para aplicar regras de produto, arquitetura, segurança, padrões de código, convenções, contratos, testes e decisões técnicas relevantes. Não declare que um arquivo está inacessível sem executar a busca e receber um erro real.

## Histórico da task

Localize os documentos disponíveis:

```bash
find /workspace/tasks/history -type f -print
```

Todos os documentos retornados pertencem ao escopo da task em execução. Leia integralmente os arquivos relevantes, especialmente:

```text
user-stories.md
functional-spec.md
technical-spec.md
```

A `technical-spec.md` é a fonte principal da implementação. `functional-spec.md` e `user-stories.md` preservam a rastreabilidade funcional. Se `technical-spec.md` não estiver disponível, registre essa ausência, implemente somente o que puder ser comprovado pelas demais evidências e pelos *guardrails* e não interrompa a etapa por essa ausência.

Os documentos de contexto, skills e histórico são somente leitura. Nunca os altere.

## Repositórios e linha de base

Antes de alterar código, verifique os repositórios montados:

```bash
find /workspace/repositories -maxdepth 3 -type d -print
```

Inspecione o código, os testes, os scripts, as configurações e os padrões relevantes em cada repositório necessário. Trabalhe somente na branch `TALOREN-{{task_number}}` já preparada em cada repositório.

Registre o estado inicial de cada repositório, incluindo branch atual, alterações preexistentes e resultados de validações que possam ser executadas antes da implementação. Não atribua à task falhas comprovadamente preexistentes e não altere arquivos fora do escopo para corrigir problemas não relacionados.

## Implementação, validação e commits

Implemente somente o escopo definido pela `technical-spec.md`. Para classificações `Nova` e `Modificação`, siga a abordagem ou o delta definido. Para `Duplicada`, não crie alterações ou commits vazios; registre as evidências de que o comportamento já existe.

Aplique os padrões do repositório e as skills relevantes. Crie ou ajuste testes para os critérios `CA-xx` e comportamentos `CE-xx` afetados quando aplicável.

Execute os comandos oficiais do projeto para testes, *lint*, formatação, análise estática, *typecheck*, *build* e demais validações aplicáveis. Registre o comando, o resultado e a evidência de cada validação. Se uma falha preexistente impedir uma validação, registre a linha de base, a limitação e seu impacto; não espere intervenção humana.

Revise o diff final para confirmar aderência ao escopo, segurança, tratamento de erros, regressões e cobertura de testes. Faça commits atômicos em cada repositório modificado, usando a skill `dev-commit`. Não faça *push*. Não inclua alterações preexistentes ou não relacionadas nos commits.

## Artefato final

Crie o diretório da task antes de gravar o resultado:

```bash
mkdir -p "/workspace/tasks/{{task_id}}"
```

Grave obrigatoriamente o resultado em:

```text
/workspace/tasks/{{task_id}}/implementation-result.md
```

O conteúdo textual de `implementation-result.md` deve ser escrito em português. Mantenha em inglês somente nomes de arquivos, caminhos, módulos, classes, funções, rotas, contratos, comandos, identificadores, hashes de commit e demais elementos de código.

Use a estrutura abaixo. `## Controle de Versão` deve ser a última seção.

```markdown
# Resultado da Implementação — TAK-{{task_number}}

## Contexto
<Resumo do escopo funcional e técnico implementado.>

## Objetivo
<O resultado técnico esperado para a task.>

## Status
**Status:** Implementado | Nenhuma alteração necessária | Implementado com limitações preexistentes

<Justificativa baseada nas evidências.>

## Alterações Implementadas
| Repositório | Branch | Arquivos Alterados | Descrição |
| --- | --- | --- | --- |
| <repository-id> | TALOREN-{{task_number}} | <paths> | <alteração realizada> |

## Rastreabilidade Funcional
| Referência | Implementação ou Evidência |
| --- | --- |
| US-01 / CE-01 / CA-01 | <módulo, função, teste ou comportamento correspondente> |

## Validações
| Validação | Comando ou Procedimento | Resultado | Evidência ou Limitação |
| --- | --- | --- | --- |
| Testes | `<command>` | Aprovado | <resumo> |

## Commits
| Repositório | Commit | Mensagem |
| --- | --- | --- |
| <repository-id> | <hash> | <commit message> |

## Fora do Escopo
<Trabalho identificado e não implementado. Escreva `Nenhum identificado` quando aplicável.>

## Premissas e Decisões
- **Ambiguidade:** <informação ausente, ambígua ou conflitante>
- **Decisão:** <decisão adotada>
- **Fonte:** <especificação, contexto, skill, código ou raciocínio>

## Limitações e Divergências
<Falhas preexistentes, validações não executáveis, divergências ou impactos. Escreva `Nenhuma identificada` quando aplicável.>

## Controle de Versão
| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agente | Criação inicial do documento |
```

Se o arquivo já existir, revise-o, preserve seu histórico de versões e adicione uma entrada para cada revisão material.

Antes de concluir, valide:

```bash
test -s "/workspace/tasks/{{task_id}}/implementation-result.md"
```

## Critério operacional de conclusão

A tarefa somente está concluída quando os documentos e repositórios relevantes tiverem sido investigados, as skills aplicáveis tiverem sido usadas, a implementação ou a ausência justificada tiver sido registrada, as validações aplicáveis tiverem sido executadas e registradas, os commits necessários tiverem sido criados sem *push*, `implementation-result.md` existir e não estiver vazio e nenhuma alteração fora do escopo tiver sido introduzida.
