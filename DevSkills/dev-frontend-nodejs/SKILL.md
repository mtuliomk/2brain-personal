---
name: dev-frontend-nodejs
description: Convenções de código para projetos frontend em Node.js com TypeScript, incluindo React, Vue, Svelte e aplicações web com SSR ou SPA. Abrange componentes, hooks, estado, tipagem, formulários, acessibilidade, efeitos assíncronos, segurança, testes, nomenclatura e organização de arquivos. Use ao escrever, revisar ou refatorar código frontend; não cobre arquitetura de projeto nem código backend.
---

# Convenções de código frontend

Aplicar estas convenções ao escrever ou revisar código frontend em Node.js com TypeScript, independentemente do framework utilizado. Seguir primeiro os padrões já existentes no projeto.

Esta skill trata estilo, nomenclatura, componentes, estado, efeitos, acessibilidade, validação, organização e práticas de implementação. Não define arquitetura, estrutura de pastas ou escolha de stack.

## Checklist antes de codificar

- [ ] Entender o comportamento esperado, estados da interface e critérios de aceite.
- [ ] Inspecionar `package.json`, `tsconfig`, ESLint, Prettier, scripts e configuração do framework/build.
- [ ] Identificar os padrões existentes para componentes, estilos, estado, chamadas HTTP e testes.
- [ ] Localizar componentes, hooks, composables, utilitários e validações reutilizáveis antes de criar novos.
- [ ] Confirmar os estados de carregamento, sucesso, vazio, erro, retry e ausência de conexão quando aplicável.
- [ ] Definir tipos de props, eventos, respostas externas e valores de formulário.
- [ ] Verificar limites entre código executado no servidor e no navegador (SSR, RSC ou equivalente).
- [ ] Considerar acessibilidade, responsividade, segurança e exposição de dados no bundle.
- [ ] Definir o menor conjunto de arquivos que precisa ser alterado.

## Nomenclatura

| Elemento | Convenção | Exemplo |
|---|---|---|
| Variáveis, funções e hooks/composables | camelCase | `isLoading`, `formatCurrency()`, `useUser()` |
| Componentes e classes | PascalCase | `UserCard`, `CheckoutForm` |
| Tipos e interfaces | PascalCase; prefira `type` para dados | `type UserCardProps = ...` |
| Constantes globais/configuração | UPPER_SNAKE_CASE | `MAX_VISIBLE_ITEMS` |
| Arquivos de componentes | PascalCase quando esse for o padrão do framework | `UserCard.tsx`, `CheckoutForm.vue` |
| Hooks, utilitários e testes | camelCase ou padrão existente | `useUser.ts`, `formatCurrency.test.ts` |
| Handlers de eventos | prefixo `handle` | `handleSubmit`, `handleKeyDown` |

## Tipagem

- Prefira `type` para props, estado, DTOs e estruturas de dados; use `interface` apenas quando o padrão do projeto ou extensão/implementação justificar.
- Evite `any`. Trate dados vindos de APIs, storage, query string, eventos não tipados e `JSON.parse` como `unknown` até validar e fazer narrowing.
- Declare tipos explícitos para props públicas, retornos de hooks/composables e callbacks complexos.
- Use `import type` para imports exclusivamente de tipos e utility types como `Pick`, `Omit`, `Partial` e `Record`.
- Evite non-null assertions (`!`) e assertions (`as`) sem justificativa; prefira guards, schemas e valores padrão seguros.
- Não duplique tipos de respostas de API: reutilize contratos compartilhados quando isso não criar acoplamento inadequado ao frontend.
- Respeite `strict: true` e regras como `noUnusedLocals`, `noImplicitReturns` e `forceConsistentCasingInFileNames`.

Consulte [Exemplos de código](examples.md#tipagem-e-componentes) para referências.

## Componentes e composição

- Mantenha componentes pequenos, coesos e orientados a uma responsabilidade visual ou de interação.
- Prefira composição por props/slots/children a componentes monolíticos com muitas flags booleanas.
- Mantenha componentes de apresentação separados da coordenação de dados quando o projeto já seguir essa distinção.
- Derive valores com `computed`, `useMemo` ou equivalente apenas quando necessário; não replique estado derivável.
- Não altere props, objetos compartilhados ou estado recebido. Atualize o estado por meio das APIs do framework.
- Estabilize referências e callbacks apenas quando isso evitar renders ou efeitos desnecessários, não por padrão.
- Evite criar componentes genéricos antes de haver comportamento compartilhado real.
- Preserve a identidade de listas com keys estáveis; nunca use o índice quando a lista puder ser reordenada, filtrada ou alterada.

## Estado, efeitos e assíncrono

- Mantenha estado no menor escopo possível: local para interação isolada, compartilhado apenas quando houver necessidade real.
- Separe estado de servidor/cache de estado efêmero da interface e use a solução já adotada pelo projeto.
- Não dispare efeitos assíncronos durante o render. Limpe subscriptions, timers e listeners no teardown/unmount.
- Cancele ou ignore respostas obsoletas para evitar race conditions quando parâmetros mudarem rapidamente.
- Evite waterfalls: carregue em paralelo dados independentes e use cache/deduplicação quando disponível.
- Não esconda erros assíncronos. Exiba feedback acionável e permita retry quando apropriado.
- Evite chamadas HTTP diretamente em muitos componentes; reutilize o cliente e os hooks/composables existentes.

## Formulários e dados externos

- Valide no cliente para feedback rápido, mas nunca trate essa validação como substituta da validação do servidor.
- Mantenha mensagens de erro associadas ao campo e preserve os valores do usuário quando houver falha.
- Evite submeter duas vezes: desabilite ou proteja a ação enquanto uma submissão idempotente estiver em andamento.
- Normalize respostas externas no limite da aplicação e trate estados parciais ou campos ausentes explicitamente.
- Não coloque tokens, segredos ou credenciais em código frontend, variáveis públicas ou logs do navegador.
- Evite XSS: não injete HTML não confiável; quando HTML for inevitável, sanitize-o com a solução aprovada pelo projeto.

## Acessibilidade e interface

- Use elementos HTML semânticos e controles nativos antes de criar widgets customizados.
- Garanta nome acessível, foco visível, navegação por teclado, ordem de foco e mensagens de erro anunciáveis.
- Associe `label` a inputs e forneça texto alternativo significativo para imagens informativas; use alt vazio para imagens decorativas.
- Não comunique estado apenas por cor. Respeite contraste, zoom, reduced motion e tamanhos de toque.
- Mantenha layouts responsivos e evite depender de dimensões fixas que quebrem em telas menores.
- Use ARIA apenas quando a semântica nativa não for suficiente e siga o padrão do componente.

## Estilo, performance e organização

- Siga ESLint, Prettier, design tokens e convenções de CSS existentes; não altere suas configurações sem solicitação.
- Prefira classes/tokens e estilos locais ao uso de estilos inline repetidos ou valores mágicos.
- Evite re-renderizações, listeners globais e recomputações desnecessárias; meça antes de otimizar.
- Use lazy loading/code splitting para rotas ou áreas realmente pesadas, sem prejudicar navegação essencial.
- Não faça otimizações prematuras que reduzam legibilidade.
- Para um componente `xxx`, use no máximo os arquivos necessários: `xxx.tsx`/`xxx.vue`, `xxx.types.ts`, `xxx.utils.ts` e `xxx.test.tsx`; não crie separação artificial em componentes triviais.
- Mantenha testes próximos ao código quando esse for o padrão do projeto.

## Testes

- Teste comportamento observável e fluxos do usuário, não detalhes internos nem implementação do framework.
- Cubra renderização relevante, interação por teclado e mouse, estados de loading/vazio/erro/sucesso, validação e casos de borda.
- Prefira queries por papel, nome acessível e texto visível; evite seletores frágeis e detalhes de classe.
- Mocke rede e tempo apenas nas fronteiras necessárias; não transforme cada teste em uma simulação da implementação.
- Não ignore, desabilite ou torne testes menos rigorosos sem justificativa explícita.

## Checklist final

- [ ] A implementação segue os padrões existentes do repositório.
- [ ] Props, estados, eventos e respostas externas estão tipados sem usos injustificados de `any`, `as` ou `!`.
- [ ] Componentes têm responsabilidades focadas e não duplicam estado derivável.
- [ ] Efeitos têm cleanup e não permitem respostas obsoletas ou submits duplicados.
- [ ] Estados de loading, vazio, erro e retry foram considerados quando aplicável.
- [ ] Formulários validam entradas e exibem erros acessíveis.
- [ ] A interface funciona por teclado, tem semântica adequada e é responsiva.
- [ ] Nenhum segredo ou dado sensível foi exposto no bundle, DOM ou logs.
- [ ] Testes cobrem comportamento relevante e não foram ignorados sem justificativa.
- [ ] ESLint, typecheck, testes e build foram executados quando disponíveis.
