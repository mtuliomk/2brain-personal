---
name: dev-backend-nodejs
description: Convenções de código para qualquer projeto backend em Node.js com TypeScript, incluindo APIs, BFFs de aplicações web, serviços, workers, jobs, consumers, scripts e utilitários. Abrange nomenclatura, tipagem, tratamento de erros, logs, concorrência, organização de arquivos e formatação, independentemente do framework ou stack. Use ao escrever, revisar ou refatorar esse código. Não cobre arquitetura de projeto nem código frontend.
---

# Convenções de código

Aplicar estas convenções ao escrever ou revisar código backend em Node.js com TypeScript, independentemente do framework utilizado (Express, Fastify, NestJS etc.).

Esta skill trata apenas de estilo, nomenclatura, tipagem, erros, organização de arquivos e práticas de implementação. Não define arquitetura, estrutura de pastas ou escolha de stack.

## Checklist antes de codificar

- [ ] Entender o comportamento esperado e os critérios de aceite.
- [ ] Inspecionar `package.json`, `tsconfig`, ESLint, Prettier e scripts relevantes.
- [ ] Identificar os padrões existentes no módulo relacionado.
- [ ] Confirmar tipos de entrada, saída e erros esperados.
- [ ] Localizar funções, utilitários, serviços e validações compartilhados antes de criar uma implementação nova.
- [ ] Verificar se a reutilização evita duplicação sem introduzir acoplamento ou comportamento inesperado.
- [ ] Confirmar se novas abstrações têm responsabilidade clara.
- [ ] Identificar operações assíncronas, concorrência e efeitos colaterais.
- [ ] Considerar dados sensíveis, validação e riscos de segurança.
- [ ] Definir o menor conjunto de arquivos que precisa ser alterado.

## Nomenclatura

| Elemento | Convenção | Exemplo |
|---|---|---|
| Variáveis e funções | camelCase | `const userAccount = ...` / `calculateTotal()` |
| Classes | PascalCase | `class UserRepository {}` |
| Interfaces de contratos de classes | PascalCase com prefixo `I` | `interface IUserRepository {}` |
| Propriedades vindas do banco | snake_case | `created_at`, `user_id` |
| Arquivos | conforme a seção [Organização de arquivos](#organização-de-arquivos) | `user.ts`, `user-types.ts` |

## Tipagem

- Prefira `type` a `interface` para modelos, DTOs e outras estruturas de dados.
- Siga primeiro a convenção de interfaces existente no projeto. Quando não houver um padrão definido, use `interface` apenas para contratos que uma classe implementará e adote o prefixo `I`.
- Evite `any`. Quando o tipo não puder ser determinado, use `unknown`, faça o narrowing necessário e documente brevemente o motivo.
- Respeite `strict: true` quando habilitado no projeto.
- Evite non-null assertions (`!`) e type assertions (`as`) sem justificativa; prefira narrowing explícito e validação em runtime.
- Declare tipos de retorno em funções públicas, métodos de classes e callbacks complexos.
- Use `import type` para imports utilizados exclusivamente como tipos.

Consulte [Exemplos de código](examples.md#tipagem) quando precisar de uma implementação de referência.

## Tratamento de erros e logs

- Registre o erro no ponto apropriado, com contexto suficiente para depuração e stack trace preservado. Evite registrar o mesmo erro várias vezes: camadas internas podem adicionar contexto e relançar, enquanto a camada responsável pelo tratamento final deve fazer o log principal. Se o `catch` recuperar, converter ou tratar o erro, registre-o antes da ação.
- Nunca registre tokens, senhas, credenciais, documentos, dados pessoais ou payloads sensíveis; redija esses campos antes do log.
- Prefira erros específicos do domínio, como `NotFoundError`, `ValidationError` e `ConflictError`, quando houver contexto suficiente.
- Não capture um erro apenas para relançá-lo sem adicionar contexto, tratamento ou logging útil.

Consulte [Exemplos de código](examples.md#tratamento-de-erros-e-logs) quando precisar de uma implementação de referência.

## Estilo e implementação

- Prefira `async/await` a cadeias de `.then()`/`.catch()`.
- Execute operações assíncronas independentes em paralelo.
- Use `Promise.all` quando uma falha puder interromper o conjunto; use `Promise.allSettled` quando cada resultado precisar ser avaliado individualmente.
- Limite a concorrência de promises com `p-limit` ou equivalente ao processar coleções potencialmente grandes, chamadas externas, operações custosas ou cenários sujeitos a rate limit. Para coleções pequenas e previamente limitadas, `Promise.all` pode ser suficiente. Preserve a execução sequencial quando houver dependência entre os itens. Verifique primeiro se o projeto já possui uma solução adequada e não adicione dependências sem necessidade.

- Prefira arrow functions para callbacks e funções soltas. Em métodos de classes, use a sintaxe padrão; use arrow functions como propriedades quando o binding léxico for necessário ou quando o projeto já adotar esse padrão.
- Mantenha funções pequenas e focadas; extraia responsabilidades quando uma função começar a fazer coisas demais.
- Para lógica de negócio e serviços, prefira classes com dependências recebidas pelo construtor. Não instancie dependências internamente nem as importe diretamente dentro dos métodos.
- Reserve funções soltas para utilitários puros e pequenos.
- Prefira `const`, propriedades `readonly` e imutabilidade. Evite alterar objetos recebidos como parâmetros sem deixar isso explícito.

Consulte [Exemplos de código](examples.md#estilo-e-implementação) quando precisar de uma implementação de referência.

## Validação de dados externos

- Trate dados de APIs, filas, banco, variáveis de ambiente, entrada HTTP e `JSON.parse` como `unknown` até validá-los.
- Valide dados na borda da aplicação com a biblioteca já adotada pelo projeto.
- Não confie apenas nos tipos TypeScript para garantir validação em runtime.

## Organização de arquivos

Para um componente ou módulo `xxx`, separe o código em até três arquivos quando o tamanho justificar:

- `xxx.ts` — implementação principal.
- `xxx-types.ts` — types e constantes usados pela implementação.
- `xxx-utils.ts` — utilitários puros específicos do componente.

Não crie essa separação para módulos triviais que cabem adequadamente em um único arquivo.

## Linter e Prettier

- Siga a configuração existente de ESLint e Prettier; ela tem prioridade sobre preferências desta skill que não estiverem cobertas pelo projeto.
- Não sugira alterar a configuração de lint ou formatação sem solicitação explícita.
- Evite imports não utilizados e dependências circulares.

## Checklist final

- [ ] A implementação segue os padrões existentes do repositório.
- [ ] Tipos de entrada, saída e retornos estão claros e não há usos injustificados de `any`, `as` ou `!`.
- [ ] Dados externos foram validados antes do uso.
- [ ] Erros foram tratados com contexto e logs sem dados sensíveis.
- [ ] Operações assíncronas usam paralelismo e limites de concorrência adequados.
- [ ] Não há duplicação evitável nem mutações desnecessárias.
- [ ] Funções e classes mantêm responsabilidades focadas.
- [ ] ESLint, typecheck, testes e build foram executados quando disponíveis.
