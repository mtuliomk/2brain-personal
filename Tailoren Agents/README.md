# Tailoren Agents

Esta pasta reúne os artefatos de configuração usados para iniciar os agentes
responsáveis pelas etapas do fluxo de desenvolvimento:

1. especificação funcional;
2. especificação técnica;
3. implementação (*coding*).

Cada subpasta representa uma etapa. Nela, os arquivos `AGENTS.md.md` e
`prompt.md.md` têm funções complementares e devem ser enviados ao agente da
etapa correspondente.

> **Nota sobre os nomes:** os arquivos atualmente usam a extensão dupla
> `.md.md`. Ela é intencionalmente mantida nesta documentação para corresponder
> aos nomes existentes no repositório.

## Papéis dos arquivos

| Arquivo | Responsabilidade | Conteúdo esperado |
| --- | --- | --- |
| `AGENTS.md.md` | Contrato permanente da etapa. | Objetivo, regras, sequência de trabalho, fontes obrigatórias, restrições, validações e critérios de conclusão. Não deve conter dados de uma task específica. |
| `prompt.md.md` | Mensagem de inicialização da execução. | Instruções resumidas para a tarefa atual e variáveis interpoladas, como `{{task_id}}`, `{{task_number}}`, objetivos, descrição e *guardrails*. |

O `prompt.md.md` não substitui o `AGENTS.md.md`: o primeiro fornece os dados
variáveis; o segundo define o comportamento que se aplica a todas as execuções
daquela etapa.

## Estrutura

```text
Tailoren Agents/
├── README.md
├── spec funcional/
│   ├── AGENTS.md.md
│   └── prompt.md.md
├── spec technical/
│   ├── AGENTS.md.md
│   └── prompt.md.md
└── coding/
    ├── AGENTS.md.md
    └── prompt.md.md
```

## Contexto disponível no workspace

Todas as etapas recebem documentos de apoio e histórico; esses materiais são
somente leitura e devem ser investigados antes de produzir a entrega.

| Fonte | Caminho no worker | Uso |
| --- | --- | --- |
| Contexto da aplicação | `/workspace/.taloren-docs-context/application` | Produto, arquitetura, segurança, padrões, decisões e demais documentos da aplicação. |
| Histórico da task | `/workspace/tasks/history` | Entregas e decisões anteriores. Serve de referência; não deve receber alterações. |
| Skills | `/workspace/.taloren-docs-skills` | Instruções especializadas que se aplicam à tarefa. |
| Repositórios | `/workspace/repositories/<repository-id>` | Código, configurações, testes e contratos da aplicação. |

A etapa **spec funcional** não possui código no workspace e, portanto, não deve
depender de inspeção de repositórios. As etapas **spec technical** e **coding**
possuem repositórios montados e devem usá-los quando forem relevantes à tarefa.

## Etapas

### 1. `spec funcional`

**Finalidade:** transformar os dados da task e o contexto da aplicação em uma
user story e em uma especificação funcional, sem implementar código.

- **Instruções fixas:** `spec funcional/AGENTS.md.md` determina a leitura do
  contexto, das skills e do histórico, a ordem obrigatória de produção e a
  rastreabilidade da resposta.
- **Prompt de início:** `spec funcional/prompt.md.md` fornece os campos da
  task e reforça que não há implementação de código.
- **Entradas principais:** documentos da aplicação, histórico e skills
  disponíveis. Não há repositórios/código para investigar nesta etapa.
- **Saídas obrigatórias:**
  - `/workspace/tasks/{{task_id}}/user-story.md`;
  - `/workspace/tasks/{{task_id}}/spec-funcional.md`.
- **Validação:** ambos os arquivos devem existir e não estar vazios antes da
  conclusão. A `user-story.md` deve ser criada e validada antes da
  `spec-funcional.md`.

A especificação funcional é a fonte de escopo para a etapa técnica; ela deve
explicar o comportamento esperado, sem tomar decisões de implementação nem
aprovar a própria entrega.

### 2. `spec technical`

**Finalidade:** converter a especificação funcional aprovada em um plano
implementável, baseado no contexto e no código efetivamente disponível, sem
alterar o repositório.

- **Instruções fixas:** `spec technical/AGENTS.md.md` torna a skill
  `spec-tecnica` obrigatória e delega a ela a classificação da mudança, a
  estrutura da especificação, as evidências, decisões, testes, divergências e
  controle de versão.
- **Prompt de início:** `spec technical/prompt.md.md` fornece os dados da task
  e define o destino da entrega.
- **Entradas principais:** `spec-funcional.md` e `user-story.md` do histórico,
  documentos da aplicação, skills e repositórios montados.
- **Saída obrigatória:**
  `/workspace/tasks/{{task_id}}/spec-tecnica.md`.
- **Validação:** o arquivo deve existir e não estar vazio. Não se implementa
  código nem se altera qualquer arquivo do repositório nesta etapa.

A `spec-funcional.md` é a fonte principal do escopo. Caso ela não esteja no
histórico, a ausência deve ser registrada conforme a skill `spec-tecnica`, sem
inventar requisitos. A especificação técnica requer aprovação humana antes do
início da implementação.

### 3. `coding`

**Finalidade:** implementar a task aprovada nos repositórios montados,
trabalhando na branch `TALOREN-{{task_number}}`, validando a alteração e
criando os commits necessários — sem *push*.

- **Prompt de início:** `coding/prompt.md.md` fornece os dados da task, aponta
  para o histórico, contexto e repositórios e exige que as alterações sejam
  commitadas.
- **Entradas principais:** histórico (incluindo as especificações aprovadas),
  documentos da aplicação, skills aplicáveis e repositórios montados.
- **Saída operacional atual:** alterações implementadas, validações executadas
  e commits locais na branch preparada.

> **Ponto de atenção:** `coding/AGENTS.md.md` está vazio e o
> `coding/prompt.md.md` ainda não define o caminho ou o nome de um arquivo de
> resultado. Isso contraria a regra de que toda etapa deve gerar um arquivo
> final. Antes de usar esta etapa em produção, as instruções de *coding* devem
> definir esse artefato (por exemplo,
> `/workspace/tasks/{{task_id}}/resultado-implementacao.md`), seu conteúdo
> mínimo e a validação `test -s`. Enquanto isso não for feito, não há contrato
> de arquivo final para a implementação.

## Fluxo e dependências

```text
Contexto + histórico + skills
             │
             ▼
spec funcional ──► user-story.md + spec-funcional.md
             │
             ▼
spec technical ──► spec-tecnica.md (aprovação humana)
             │
             ▼
coding ──────────► código validado + commits + arquivo final de resultado
```

Em qualquer etapa, o agente deve preservar o contexto/histórico como leitura,
não inventar informações ausentes e validar o artefato final antes de encerrar
a execução. As referências entre etapas devem respeitar o escopo já aprovado:
a especificação técnica não modifica silenciosamente a funcional e a
implementação não deve ultrapassar a especificação técnica aprovada.
