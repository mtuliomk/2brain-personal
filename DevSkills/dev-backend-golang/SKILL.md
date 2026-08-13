---
name: dev-backend-golang
description: Convenções de código para qualquer projeto backend em Go (Golang), incluindo APIs HTTP, gRPC, workers, jobs, consumers, CLIs e serviços. Abrange nomenclatura, packages, tipos, erros, contexto, concorrência, I/O, persistência, observabilidade, segurança, testes e ferramentas do ecossistema Go. Use ao escrever, revisar ou refatorar código backend em Go. Não cobre arquitetura de produto nem código frontend.
---
# Convenções de código

Aplicar estas convenções ao escrever ou revisar backend em Go, independentemente do framework. Priorizar os padrões já existentes no repositório; esta skill complementa, mas não substitui, as decisões do projeto.

## Checklist antes de codificar

- [ ] Entender comportamento, contratos e critérios de aceite.
- [ ] Inspecionar `go.mod`, `go.sum`, versão do Go, `Makefile`, CI e configurações de lint/teste.
- [ ] Identificar padrões do package afetado e reutilizar interfaces, erros, clientes e validações existentes.
- [ ] Confirmar entradas, saídas, efeitos colaterais, deadlines, cancelamento e erros esperados.
- [ ] Verificar autenticação, autorização, dados sensíveis, SSRF, injeção e abuso de recursos.
- [ ] Definir o menor conjunto de packages e arquivos a alterar; evitar dependências e abstrações sem necessidade.

## Nomenclatura e packages

| Elemento | Convenção | Exemplo |
|---|---|---|
| Packages | minúsculos, curtos, uma palavra quando possível; não usar `util`, `common`, `misc`, `helpers` | `user`, `billing` |
| Exportados | PascalCase | `UserRepository`, `ParseToken` |
| Não exportados | camelCase | `userID`, `parseToken` |
| Acrônimos | forma convencional | `HTTPServer`, `UserID`, `URL`, `JSON` |
| Constantes/variáveis | escopo mínimo e nome idiomático, sem `UPPER_SNAKE_CASE` por padrão | `maxRetries`, `DefaultTimeout` |
| Arquivos | nomes curtos; snake_case quando necessário | `user_handler.go`, `user_test.go` |

- Nomear interfaces pelo comportamento (`Reader`, `Store`, `Clock`), declarar no package consumidor e mantê-las pequenas. Evitar `IUserRepository`.
- Exportar somente o contrato público. Todo símbolo exportado deve ter doc comment iniciado pelo próprio nome; packages públicos devem ter comentário de package.
- Preferir nomes claros e não repetir o nome do package (`user.User`, não `user.UserModel`).

## Tipos e modelagem

- Preferir tipos concretos, structs pequenas e composição. Não criar abstrações ou generics apenas para eliminar repetição trivial.
- Criar tipos para conceitos do domínio (`type UserID string`) quando isso evitar confusão entre primitivos.
- Usar ponteiros somente quando `nil` tiver significado, o valor for grande ou houver mutação compartilhada; diferenciar ausência de valor zero conscientemente.
- Evitar `map[string]any` em contratos internos; usar structs tipadas. Tratar JSON, HTTP, filas, banco e configuração como dados não confiáveis até validar.
- Não usar `init()` para lógica de negócio, I/O ou configuração implícita. Evitar `unsafe`, reflexão ou `panic` para contornar modelagem.

## Erros e observabilidade

- Retornar erros explicitamente e adicionar contexto: `fmt.Errorf("creating user: %w", err)`.
- Usar `errors.Is`/`errors.As`; nunca comparar texto. Expor sentinel errors/tipos somente quando forem parte deliberada do contrato.
- Não embrulhar erros de implementação interna se isso expuser detalhes que deveriam permanecer privados.
- Mapear erros de domínio para HTTP/gRPC na borda; nunca expor stack trace, SQL, tokens ou detalhes internos ao cliente.
- Não capturar e relançar sem contexto, tratamento ou log útil. Usar `panic` apenas para falha irrecuperável de inicialização ou invariantes impossíveis.
- Usar `log/slog` ou o logger estruturado do projeto. Incluir correlação/trace ID e contexto operacional; redigir credenciais, tokens, documentos, PII e payloads sensíveis.

## Contexto, I/O e recursos

- Receber `context.Context` como primeiro argumento de funções que fazem I/O ou podem ser canceladas; nunca armazenar contexto em structs.
- Propagar contexto para HTTP, banco, gRPC e integrações remotas; respeitar deadlines e cancelamento.
- Usar timeouts explícitos em clientes e servidores. Validar limites de tamanho, tempo e quantidade antes de processar dados grandes.
- Usar `defer` imediatamente após adquirir recursos (`Body.Close`, `rows.Close`, unlock). Não ignorar erros de `Close`, `Flush`, `Commit`, `Rollback`, `Write` ou `rows.Err()` sem decisão explícita.

## Concorrência

- Criar goroutines somente com benefício claro; definir quem inicia, cancela, aguarda e encerra cada uma.
- Garantir caminho de término para toda goroutine. Evitar vazamentos por canais sem consumidor, bloqueios ou I/O sem cancelamento.
- Usar canais para transferência de propriedade; usar `sync.Mutex`, `RWMutex`, `Once`, `WaitGroup` e `atomic` para estado compartilhado delimitado.
- Não compartilhar estado mutável sem sincronização. Usar `errgroup` quando já adotado para cancelar trabalho relacionado após o primeiro erro.
- Limitar concorrência em lotes, chamadas externas e tarefas sujeitas a rate limit. Validar com `go test -race` quando aplicável.
- Fechar canais somente pelo produtor responsável; preferir retornar erro explícito a codificar erro no fechamento de canal.

## Serviços, handlers e dependências

- Manter handlers finos: extrair/validar entrada, autenticar e autorizar, delegar, mapear resposta e erro.
- Não colocar regra de negócio, queries extensas ou integrações diretamente no handler.
- Montar dependências explicitamente, geralmente por `New...`; preferir injeção por construtor e evitar globais/singletons.
- Criar interfaces para seams de teste ou contratos reais, não para cada struct.
- Preferir biblioteca padrão (`net/http`, `encoding/json`, `database/sql`, `context`) quando suficiente; avaliar dependências por manutenção e segurança.
- Centralizar middleware de request ID, logging, recovery, autenticação, rate limit e métricas sem esconder efeitos importantes.

## HTTP, gRPC e dados externos

- Definir contratos e erros consistentes; validar método, content type, tamanho, formato e autorização.
- Configurar `http.Client`/`Transport` com timeout, pooling e limites; reutilizar o cliente, não criá-lo por request.
- Fechar response bodies e verificar status antes de decodificar. Fazer retry com backoff apenas quando a operação for idempotente e a política estiver definida.
- Em gRPC, propagar contexto e usar deadlines, status codes, interceptors e limites de mensagem conforme o projeto.
- Prevenir SSRF, path traversal, deserialização insegura e SQL injection; parametrizar queries e validar na borda.

## Persistência e organização

- Usar `QueryContext`, `ExecContext` e `BeginTx`; fechar rows, verificar `rows.Err()` e mapear `sql.ErrNoRows` na camada apropriada.
- Manter transações curtas, com rollback seguro; evitar chamadas remotas dentro de transações sem justificativa.
- Usar placeholders; nunca concatenar input em SQL. Não embutir credenciais.
- Respeitar o layout existente. Em serviços maiores, usar `cmd/<serviço>` para entrypoints e `internal/<domínio>` para código privado quando isso melhorar encapsulamento; não criar `pkg/` por hábito.
- Manter entrypoints pequenos, shutdown gracioso e ordem explícita para encerrar servidores, workers, filas e pools. Evitar packages circulares.

## Formatação, módulos e ferramentas

- Executar `gofmt`/`goimports` conforme o projeto. Manter `go.mod`/`go.sum` consistentes e revisar o diff após `go mod tidy`.
- Respeitar a versão do Go e ferramentas configuradas (`go vet`, `staticcheck`, `golangci-lint`, `govulncheck`). Não alterar configuração sem solicitação.
- Evitar dependências abandonadas, versões sem pinning e bibliotecas que duplicam a biblioteca padrão sem benefício.

## Testes

- Cobrir comportamento observável, entradas inválidas, falhas de dependência, cancelamento, timeout, concorrência e bordas relevantes.
- Na ausência de padrão, usar testes no package, `TestXxx`, table-driven e subtests com `t.Run`.
- Usar doubles simples, `httptest` para HTTP e fakes controláveis para rede/banco; não depender de `sleep`.
- Usar `t.Parallel()` somente com isolamento garantido. Adicionar fuzz tests para parsers/decoders e benchmarks somente para hipóteses mensuráveis.
- Executar os comandos oficiais do projeto; quando disponíveis, `go test ./...`, `go test -race ./...`, cobertura, lint, `go vet ./...` e verificação de vulnerabilidades.

## Checklist final

- [ ] Contratos e comportamento seguem a spec e os padrões existentes.
- [ ] Packages, símbolos e interfaces são idiomáticos e têm escopo mínimo.
- [ ] Dados externos foram validados e limites aplicados.
- [ ] Contexto, deadlines, cancelamento e shutdown foram propagados.
- [ ] Recursos são fechados e erros de I/O não foram ignorados.
- [ ] Erros têm contexto e não vazam detalhes internos; logs não contêm dados sensíveis.
- [ ] Goroutines terminam, estado compartilhado é sincronizado e concorrência é limitada.
- [ ] Handlers são finos, dependências explícitas e queries parametrizadas.
- [ ] Código está formatado; módulos/dependências foram revisados.
- [ ] Testes, lint, vet, race e build foram executados quando disponíveis.

## Fontes de referência

- [Effective Go](https://go.dev/doc/effective_go)
- [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments)
- [Go Doc Comments](https://go.dev/doc/comment)
- [Go Modules Reference](https://go.dev/ref/mod)
- [Contexts and structs](https://go.dev/blog/context-and-structs)
- [Working with Errors in Go 1.13](https://go.dev/blog/go1.13-errors)
