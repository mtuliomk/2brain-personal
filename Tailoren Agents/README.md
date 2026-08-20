# Tailoren Agents

Esta pasta reúne os artefatos de configuração usados para iniciar os agentes responsáveis pelas etapas do fluxo de desenvolvimento:

1. especificação funcional;
2. especificação técnica;
3. implementação (*coding*).

Cada subpasta representa uma etapa. Nela, os arquivos `AGENTS.md` e `prompt.md` têm funções complementares e devem ser enviados ao agente da etapa correspondente.

## Papéis dos arquivos

| Arquivo     | Responsabilidade                       | Conteúdo esperado                                                                                                                                            |
| ----------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `AGENTS.md` | Contrato permanente da etapa.          | Objetivo, regras, sequência de trabalho, fontes obrigatórias, restrições, validações e critérios de conclusão. Não deve conter dados de uma task específica. |
| `prompt.md` | Mensagem de inicialização da execução. | Instruções resumidas para a tarefa atual e variáveis interpoladas, como `{{task_id}}`, `{{task_number}}`, objetivos, descrição e *guardrails*.               |

O `prompt.md` não substitui o `AGENTS.md`: o primeiro fornece os dados variáveis; o segundo define o comportamento que se aplica a todas as execuções daquela etapa.

## Autonomia dos agentes

Os agentes devem executar integralmente suas etapas de forma autônoma. Eles não dependem de aprovação, validação, decisão ou outra intervenção humana durante a execução. Quando houver informação ausente, ambígua ou indisponível, o agente deve seguir as regras da sua etapa, registrar as premissas e limitações no artefato final e concluir o trabalho com base nas evidências disponíveis.

## Idioma dos artefatos

Todos os artefatos finais devem ter o conteúdo textual escrito em português. Mantenha em inglês somente nomes de arquivos, caminhos, módulos, classes, funções, rotas, contratos, comandos, identificadores, hashes de commit e demais elementos de código existentes. Os nomes de arquivos e os caminhos contratuais também permanecem em inglês, independentemente do idioma dos dados recebidos no prompt.

| Artefato | Idioma do conteúdo |
| --- | --- |
| `user-stories.md` | Português, exceto nomes de arquivos e elementos de código. |
| `functional-spec.md` | Português, exceto nomes de arquivos e elementos de código. |
| `technical-spec.md` | Português, exceto nomes de arquivos e elementos de código. |
| `implementation-plan.md` | Português, exceto nomes de arquivos e elementos de código. |
| `implementation-result.md` | Português, exceto nomes de arquivos e elementos de código. |

## Tratamento de incertezas

A ausência, ambiguidade ou conflito de informações não bloqueia a execução. O agente deve investigar as fontes disponíveis para sua etapa, adotar a premissa mais restrita compatível com as evidências e com os *guardrails*, e não criar requisitos, módulos ou comportamentos sem respaldo. A premissa, a fonte consultada e qualquer divergência devem ser registradas no artefato final da etapa.

## Estrutura

```text
Tailoren Agents/
├── README.md
├── spec funcional/
│   ├── AGENTS.md
│   └── prompt.md
├── spec technical/
│   ├── AGENTS.md
│   └── prompt.md
└── coding/
    ├── AGENTS.md
    └── prompt.md
```

## Contexto disponível no workspace

Todas as etapas recebem documentos de apoio e histórico; esses materiais são somente leitura e devem ser investigados antes de produzir a entrega.

| Fonte                 | Caminho no worker                              | Uso                                                                                                                                          |
| --------------------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Contexto da aplicação | `/workspace/.taloren-docs-context/application` | Produto, arquitetura, segurança, padrões, decisões e demais documentos da aplicação.                                                         |
| Histórico da task     | `/workspace/tasks/history`                     | Documentos, entregas e decisões relacionados exclusivamente ao escopo da task em execução. Serve de referência; não deve receber alterações. |
| Skills                | `/workspace/.taloren-docs-skills`              | Instruções especializadas que se aplicam à tarefa.                                                                                           |
| Repositórios          | `/workspace/repositories/<repository-id>`      | Código, configurações, testes e contratos da aplicação.                                                                                      |

A etapa **spec funcional** não possui código no workspace e, portanto, não deve depender de inspeção de repositórios. As etapas **spec technical** e **coding** possuem repositórios montados e devem usá-los quando forem relevantes à tarefa.

## Contrato de transição entre etapas

O worker disponibiliza no histórico somente leitura apenas documentos relacionados ao escopo da task em execução, incluindo os artefatos concluídos pela etapa anterior. O encadeamento obrigatório é:

1. **spec funcional → spec technical:** `user-stories.md` e `functional-spec.md` devem estar em `/workspace/tasks/history` quando a etapa técnica iniciar.
2. **spec technical → coding:** `technical-spec.md`, junto com as user stories e a especificação funcional relacionadas, deve estar em `/workspace/tasks/history` quando a implementação iniciar.

Cada agente deve consumir exclusivamente os artefatos montados no seu workspace, sem alterar o histórico. Se algum artefato esperado não estiver presente, o agente segue o tratamento de incertezas e registra a situação em sua entrega final.

## Etapas

### 1. `spec funcional`

**Finalidade:** transformar os dados da task e o contexto da aplicação em uma ou mais user stories e em uma especificação funcional, sem implementar código.

- **Instruções fixas:** `spec funcional/AGENTS.md` determina a leitura do contexto, das skills e do histórico, a ordem obrigatória de produção e a rastreabilidade da resposta.
- **Prompt de início:** `spec funcional/prompt.md` fornece os campos da task e reforça que não há implementação de código.
- **Entradas principais:** documentos da aplicação, histórico e skills disponíveis. Não há repositórios/código para investigar nesta etapa.
- **Saídas obrigatórias:**
  - `/workspace/tasks/{{task_id}}/user-stories.md`;
  - `/workspace/tasks/{{task_id}}/functional-spec.md`.
- **Validação:** ambos os arquivos devem existir e não estar vazios antes da conclusão. A `user-stories.md` deve ser criada e validada antes da `functional-spec.md`.

A especificação funcional é a fonte de escopo para a etapa técnica; ela deve explicar o comportamento esperado.

### 2. `spec technical`

**Finalidade:** converter a especificação funcional disponível em um plano implementável, baseado no contexto e no código efetivamente disponível, sem alterar o repositório.

- **Instruções fixas:** `spec technical/AGENTS.md` torna a skill `spec-tecnica` obrigatória e delega a ela a classificação da mudança, a estrutura da especificação, as evidências, decisões, testes, divergências e controle de versão.
- **Prompt de início:** `spec technical/prompt.md` fornece os dados da task e define o destino da entrega.
- **Entradas principais:** `functional-spec.md` e `user-stories.md` do histórico, documentos da aplicação, skills e repositórios montados.
- **Saída obrigatória:** `/workspace/tasks/{{task_id}}/technical-spec.md`.
- **Validação:** o arquivo deve existir e não estar vazio. Não se implementa código nem se altera qualquer arquivo do repositório nesta etapa.

A `functional-spec.md` é a fonte principal do escopo. Caso ela não esteja no histórico, a ausência deve ser registrada conforme a skill `spec-tecnica`, sem inventar requisitos.

### 3. `coding`

**Finalidade:** implementar a task conforme a especificação técnica disponível nos repositórios montados, trabalhando na branch `TALOREN-{{task_number}}`, validando a alteração e criando os commits necessários — sem *push*.

- **Instruções fixas:** `coding/AGENTS.md` define o uso de skills, a investigação de contexto, a implementação, as validações, os commits locais e o arquivo final.
- **Prompt de início:** `coding/prompt.md` fornece exclusivamente os dados variáveis da task.
- **Entradas principais:** histórico (incluindo as especificações disponíveis), documentos da aplicação, skills aplicáveis e repositórios montados.
- **Saídas obrigatórias:** `implementation-plan.md` gerado e validado antes de qualquer alteração de código, alterações implementadas, validações executadas, commits locais nas branches preparadas e `/workspace/tasks/{{task_id}}/implementation-result.md`.
- **Múltiplos repositórios:** altere somente os repositórios necessários; mantenha cada alteração na branch `TALOREN-{{task_number}}` correspondente e faça commits atômicos em cada repositório modificado.
- **Validações:** execute os testes, *lint*, *build* e demais verificações aplicáveis às alterações. Registre no arquivo final os comandos executados e seus resultados.
- **Sem alteração necessária:** não crie commits vazios. Registre no arquivo final a justificativa e as evidências que levaram a essa conclusão.
- **Conteúdo mínimo dos artefatos:** plano rastreável de itens de implementação, validações e commits; e resultado com status da implementação, escopo realizado, repositórios e commits locais, arquivos alterados, validações, premissas, divergências e limitações.
- **Validação dos artefatos:** crie o diretório da task se necessário e execute `test -s "/workspace/tasks/{{task_id}}/implementation-plan.md"` antes de codificar e `test -s "/workspace/tasks/{{task_id}}/implementation-result.md"` antes de concluir.

## Fluxo e dependências

```text
Contexto + histórico + skills
             │
             ▼
spec funcional ──► user-stories.md + functional-spec.md
             │
             ▼
spec technical ──► technical-spec.md
             │
             ▼
coding ──────────► implementation-plan.md → código validado + commits + implementation-result.md
```

Em qualquer etapa, o agente deve preservar o contexto/histórico como leitura, não inventar informações ausentes e validar o artefato final antes de encerrar a execução. As referências entre etapas devem respeitar o escopo já definido: a especificação técnica não modifica silenciosamente a funcional e a implementação não deve ultrapassar a especificação técnica disponível.
