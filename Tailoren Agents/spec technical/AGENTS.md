
Este arquivo define o contexto operacional do agente responsável por gerar uma especificação técnica. A skill `spec-tecnica` define as regras de conteúdo, estrutura e classificação; este arquivo define como a etapa deve ser executada no workspace.

## Skill obrigatória

Use obrigatoriamente a skill `spec-tecnica`.

A skill canônica está em:

`/home/tulio/Documents/tulio-personal/DevSkills/spec-tecnica/SKILL.md`

No ambiente do worker, utilize a cópia disponível em:

`/workspace/.taloren-docs-skills/spec-tecnica/SKILL.md`

A skill `spec-tecnica` é a autoridade para:

- classificação como Nova, Modificação ou Duplicada;
- estrutura da especificação técnica;
- tabela de evidências;
- decisões técnicas;
- plano de testes;
- divergências com a especificação funcional;
- controle de versão;
- critérios de conclusão.

Não duplique as regras de conteúdo definidas pela skill. Este arquivo complementa a skill exclusivamente com regras operacionais do workspace, incluindo a autonomia da execução.

A especificação técnica deve explicar como implementar o que está definido na especificação funcional disponível. Não altere o escopo funcional silenciosamente.

## Autonomia e tratamento de incertezas

Execute toda a etapa de forma autônoma. Não solicite, aguarde nem condicione a conclusão a aprovação, validação, decisão ou qualquer outra intervenção humana. Referências a revisão ou aprovação humana em skills ou documentos de contexto não são pré-requisito para gerar e concluir esta especificação.

Quando uma informação estiver ausente, ambígua ou conflitante, investigue os documentos da aplicação, o histórico, as skills e o código disponível. Adote a premissa mais restrita compatível com as evidências e os *guardrails*, sem inventar requisitos, módulos, arquivos, rotas ou integrações. Registre a ambiguidade, a decisão e a fonte no documento, conforme a skill `spec-tecnica`, e conclua a entrega com as evidências disponíveis.

Não implemente código nem altere arquivos do repositório nesta etapa.

## Contexto da tarefa

Os valores variáveis da tarefa são recebidos no prompt de execução:

- `{{task_id}}`: identificador da task;
- `{{task_number}}`: número da task;
- objetivo do job;
- objetivo e descrição da subtask;
- objetivo, descrição e guardrail da task.

Use esses valores como complemento dos arquivos disponíveis no workspace.

## Contexto da aplicação

Antes de elaborar a especificação, liste e leia integralmente os documentos:

```bash
find /workspace/.taloren-docs-context/application \
  -type f -name '*.md' -print
```

Esse contexto pode conter arquitetura, segurança, padrões de código, design system, produto e outras decisões técnicas relevantes.

Não declare que um arquivo está inacessível sem executar a busca e receber um erro real.

## Repositórios da aplicação

Verifique se existem repositórios clonados:

```bash
find /workspace/repositories -maxdepth 3 -type d -print
```

Quando existirem, inspecione o código-fonte relevante dentro de:

```text
/workspace/repositories/<repository-id>
```

Use o código para localizar telas, componentes, fluxos, rotas, serviços, contratos de API, modelos, persistência, integrações, configurações, permissões e testes relacionados.

Não invente módulos, arquivos, rotas ou integrações que não possam ser confirmados no contexto ou no código disponível.

## Skills adicionais

Liste as skills disponíveis no ambiente:

```bash
if [ -d /workspace/.taloren-docs-skills ]; then
  find /workspace/.taloren-docs-skills \
    -type f -name 'SKILL.md' -print
fi
```

Leia integralmente as skills relevantes para a tarefa. A skill `spec-tecnica` continua sendo obrigatória e permanece como autoridade da especificação técnica.

## Especificação funcional

A especificação técnica usa a especificação funcional disponível como fonte principal do escopo.

Os documentos da task são disponibilizados em:

```text
/workspace/tasks/history
```

Localize os arquivos existentes:

```bash
find /workspace/tasks/history -type f -print
```

Leia integralmente os arquivos encontrados, especialmente:

```text
user-stories.md
user-story.md (legado)
spec-funcional.md
```

O worker monta esse diretório já filtrado para a task em execução. Portanto, não exija que `task_id` ou `task_number` estejam presentes no nome dos arquivos.

Se `spec-funcional.md` não estiver disponível, registre essa ausência em `Decisões assumidas` conforme as regras da skill `spec-tecnica`. Use apenas as evidências restantes e os *guardrails* para elaborar a especificação; não invente requisitos funcionais nem interrompa a etapa por essa ausência.

A especificação técnica deve usar a especificação funcional como fonte principal do escopo. As user stories servem para preservar contexto, identificar eventuais perdas de informação e manter a rastreabilidade dos itens `US-xx`.

Se já existir alguma espcificação téncina nessa pasta, sua missão é revisá-la.

## Histórico adicional

Considere outros documentos em `/workspace/tasks/history` somente quando for possível identificar que pertencem a tarefas anteriores.

Use especificações técnicas anteriores apenas como referência de nível de detalhamento, nomenclatura, estilo e padrões técnicos. Não copie escopo, decisões ou requisitos de outra task.

## Classificação e conteúdo

Siga integralmente a skill `spec-tecnica` para investigar o código, classificar a demanda, justificar a classificação com evidências, escolher a estrutura adequada, registrar decisões, elaborar o plano de testes, tratar divergências com a spec funcional e controlar versões.

Não crie uma estrutura alternativa de documento neste arquivo.

## Diretório e arquivo de saída

Antes de gravar o documento, crie o diretório da task:

```bash
mkdir -p "/workspace/tasks/{{task_id}}"
```

Grave obrigatoriamente a especificação em:

```text
/workspace/tasks/{{task_id}}/spec-tecnica.md
```

O arquivo deve ser criado pelo agente e não apenas apresentado na resposta.

Antes de concluir, valide:

```bash
test -s "/workspace/tasks/{{task_id}}/spec-tecnica.md"
```

Se a validação falhar, corrija a gravação antes de concluir.

O arquivo `spec-tecnica.md` será coletado pelo worker e publicado como attachment da task.

## Critério operacional de conclusão

A tarefa só pode ser considerada concluída quando:

1. a skill `spec-tecnica` tiver sido utilizada;
2. o contexto disponível tiver sido investigado;
3. a especificação funcional tiver sido lida, quando disponível;
4. o código relevante tiver sido inspecionado;
5. a especificação tiver sido classificada conforme a skill;
6. o documento tiver sido gravado no caminho obrigatório;
7. o comando `test -s` tiver sido executado com sucesso;
8. nenhuma implementação de código tiver sido iniciada.

A seção `Controle de versão` definida pela skill `spec-tecnica` deve permanecer como o último conteúdo do documento gerado.
