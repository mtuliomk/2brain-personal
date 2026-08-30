---
name: local-explorer
description: Conduzir troca de ideias e investigação aberta sobre um problema em desenvolvimento local, com ou sem plano de execução ao final. Use quando o pedido for exploratório, não tiver escopo de arquivo definido ou o usuário ainda estiver decidindo se executará uma mudança. Não use para implementar diretamente uma feature, correção ou documentação.
---

# Exploração local

Use esta skill para transformar uma dúvida, hipótese ou oportunidade em entendimento técnico útil, sem pressupor que haverá implementação. Preserve o caráter exploratório do pedido: investigar e propor opções não autoriza alterar código, configuração, dados, dependências, documentação ou Git.

## Condução da investigação

Comece pelo objetivo, pelo comportamento ou pela hipótese trazida pelo usuário. Quando o contexto estiver incompleto, faça uma investigação inicial de baixo risco no repositório antes de pedir esclarecimentos. Inspecione somente os módulos, padrões, configurações, testes e documentação que possam confirmar ou refutar as hipóteses relevantes.

Diferencie claramente no resultado:

- **Fatos observados** — sustentados por código, testes, configuração ou documentação, com caminhos e símbolos relevantes.
- **Inferências** — conclusões prováveis que dependem de evidência indireta.
- **Lacunas e perguntas em aberto** — informações que o repositório não permite concluir.
- **Opções** — alternativas de solução, seus impactos, riscos, dependências e critérios para escolhê-las.

Não trate uma hipótese do usuário como causa raiz sem evidência. Não proponha uma única solução como obrigatória quando houver alternativas legítimas. Considere compatibilidade, contratos, dados, observabilidade, segurança, desempenho, testes e esforço somente quando forem relevantes ao problema.

## Escopo e autonomia

Navegue pelo código de modo progressivo: comece pelos pontos de entrada e módulos mais próximos ao tema, siga referências e expanda a investigação apenas quando isso reduzir uma incerteza relevante. Respeite alterações preexistentes; não as modifique, descarte ou atribua ao problema investigado.

Não produza um plano detalhado por padrão. Ao final, inclua um plano de execução breve somente se o usuário pedir, se a decisão estiver suficientemente madura ou se ele solicitar recomendação para avançar. Esse plano deve conter objetivo, módulos prováveis, validações e incertezas remanescentes, sem executar as mudanças.

Quando a resposta depender de uma decisão de produto, requisito não definido ou acesso indisponível, apresente as opções e a informação necessária para decidir. Pergunte apenas o que for indispensável para continuar a investigação; caso contrário, avance com as evidências disponíveis e declare os limites.

## Resultado

Apresente em português uma síntese objetiva que contenha:

1. entendimento do problema e escopo investigado;
2. evidências técnicas relevantes, com caminhos e símbolos quando disponíveis;
3. conclusões, incertezas e riscos;
4. opções ou recomendação condicionada às evidências;
5. próximos passos, incluindo um plano breve somente quando cabível.

A conclusão deve deixar explícito se o tema está pronto para seguir para `local-feature`, `local-bug`, `local-documentation` ou se ainda requer decisão ou investigação.
