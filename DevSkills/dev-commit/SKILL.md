---
name: dev-commit
description: Convenções e fluxo seguro para revisar alterações, organizar staging e criar mensagens de commit claras e consistentes. Use ao preparar, revisar, corrigir ou criar commits Git em qualquer projeto; segue Conventional Commits quando o repositório não definir outro padrão e evita incluir arquivos ou dados indevidos.
---
# Convenções de commit

Aplicar estas convenções ao revisar alterações e criar commits Git. Seguir primeiro o padrão documentado no repositório, no histórico recente e nas configurações existentes; usar as regras abaixo como padrão quando não houver convenção explícita.

Esta skill trata da preparação, revisão, agrupamento e mensagem de commits. Não define estratégia de branching, processo de release ou política de pull request.

## Checklist antes de criar o commit

- [ ] Entender o objetivo da alteração e o comportamento esperado.
- [ ] Inspecionar o estado e o diff: `git status --short`, `git diff` e, quando necessário, `git diff --cached`.
- [ ] Verificar o padrão de mensagens no histórico com `git log -n 10 --oneline`.
- [ ] Comparar as alterações encontradas com o pedido atual e identificar mudanças que não pertencem à tarefa.
- [ ] Se houver alterações não relacionadas, perguntar explicitamente ao usuário se deseja incluí-las no commit antes de adicioná-las ao staging.
- [ ] Confirmar que não há arquivos gerados, temporários, dependências, credenciais ou segredos no staging.
- [ ] Separar alterações não relacionadas em commits independentes quando isso melhorar revisão e reversão.
- [ ] Executar os testes, lint, typecheck ou build relevantes, conforme scripts e instruções do projeto.
- [ ] Confirmar que o menor conjunto correto de arquivos será incluído.
- [ ] Não usar `git add .`, `git add -A`, `git commit -a`, `--no-verify`, `--amend`, `reset`, `rebase` ou comandos destrutivos sem necessidade clara e autorização implícita no pedido.

## Formato da mensagem

Quando o projeto não definir outro padrão, usar Conventional Commits:

```text
<tipo>(<escopo opcional>): <descrição no imperativo>

<corpo opcional>

<rodapé opcional>
```

Tipos preferidos:

| Tipo | Usar quando | Exemplo |
|---|---|---|
| `feat` | Adicionar capacidade perceptível ao usuário ou consumidor | `feat(auth): permitir login com passkey` |
| `fix` | Corrigir comportamento incorreto | `fix(api): tratar timeout do provedor` |
| `refactor` | Alterar estrutura sem mudar comportamento | `refactor(parser): extrair validação de tokens` |
| `perf` | Melhorar desempenho sem mudar a finalidade | `perf(search): indexar filtros frequentes` |
| `test` | Adicionar ou ajustar testes | `test(cart): cobrir remoção do último item` |
| `docs` | Alterar documentação | `docs(contributing): explicar setup local` |
| `build` | Alterar build, dependências ou empacotamento | `build: atualizar versão do TypeScript` |
| `ci` | Alterar automações de integração/deploy | `ci: executar testes em matriz de versões` |
| `chore` | Manutenção que não se enquadra nos tipos anteriores | `chore: reorganizar arquivos de configuração` |

## Regras da descrição

- Escrever no imperativo e de forma objetiva: `adicionar`, `corrigir`, `remover`, `atualizar`.
- Começar com letra minúscula quando isso for compatível com o padrão do projeto e não terminar com ponto.
- Descrever a intenção e o efeito principal, não listar cada arquivo alterado.
- Manter a primeira linha curta; preferir até 72 caracteres quando não houver regra diferente.
- Usar escopo apenas quando o repositório tiver módulos claros ou já o utilizar consistentemente.
- Não misturar tipos diferentes na mesma mensagem; dividir o trabalho ou escolher o tipo dominante.

## Corpo, rodapé e breaking changes

- Adicionar corpo somente quando explicar contexto, causa, decisão técnica ou impacto ajudar a revisão futura.
- Quebrar linhas do corpo em aproximadamente 72–88 caracteres, seguindo o padrão do repositório.
- Registrar referências de issues no formato já usado pelo projeto.
- Marcar mudança incompatível com `!` ou `BREAKING CHANGE:` apenas quando consumidores precisarem adaptar código, configuração ou fluxo.
- Não usar o corpo para copiar o diff nem para justificar detalhes óbvios.

Exemplo:

```text
fix(cache): evitar reutilização de dados expirados

Validar a data de expiração antes de devolver o valor persistido,
preservando o fallback existente quando a entrada estiver inválida.

Refs: #123
```

## Staging e agrupamento

- Revisar cada arquivo antes de adicioná-lo: `git diff -- path/to/file`.
- Adicionar arquivos explicitamente ou por partes com `git add -p` quando houver mudanças misturadas.
- Nunca incluir automaticamente alterações não relacionadas. Se o usuário optar por incluí-las, confirmar que são intencionais e agrupá-las em um commit separado quando tiverem objetivo próprio.
- Quando um mesmo arquivo misturar mudanças da tarefa e mudanças externas, mostrar essa situação e perguntar se deve incluir o arquivo inteiro, selecionar apenas hunks relevantes ou deixar as mudanças fora do commit.
- Manter cada commit coeso, compilável quando possível e fácil de reverter.
- Não remover mudanças do usuário nem reformatar arquivos não relacionados para “limpar” o commit.
- Não incluir `.env`, chaves privadas, tokens, dumps, logs ou artefatos de build; se já estiverem rastreados, orientar a remoção segura sem expor o conteúdo.
- Preservar alterações existentes que não façam parte da tarefa; perguntar antes de descartá-las.

## Validação final

- [ ] O diff staged contém somente o objetivo do commit.
- [ ] Alterações fora do escopo foram excluídas ou incluídas somente após confirmação explícita do usuário.
- [ ] A mensagem explica a mudança em uma linha e segue o padrão detectado.
- [ ] Testes e verificações relevantes passaram ou a falha foi informada explicitamente.
- [ ] Não há segredo, dado pessoal ou arquivo sensível no commit.
- [ ] O commit não combina correção funcional com refatoração ou formatação sem relação.
- [ ] Após criar o commit, confirmar o resultado com `git show --stat --oneline HEAD` e `git status --short`.

## Segurança operacional

- Nunca publicar, fazer push, alterar histórico remoto ou criar tag apenas por iniciativa própria; executar somente se solicitado.
- Antes de `--amend`, rebase, reset ou force-push, verificar o histórico e confirmar o escopo da operação.
- Se o commit falhar por hook, corrigir a causa e tentar novamente; não ignorar o hook automaticamente.
- Se houver conflito entre esta skill e a documentação do projeto, seguir a documentação local e registrar a exceção quando relevante.
