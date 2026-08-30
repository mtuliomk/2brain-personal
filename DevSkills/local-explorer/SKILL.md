---
name: local-explorer
description: Conduzir troca de ideias e investigação aberta sobre um problema em desenvolvimento local, com ou sem plano de execução ao final. Use quando o pedido for exploratório, não tiver escopo de arquivo definido ou o usuário ainda estiver decidindo se executará uma mudança. Não use para implementar diretamente uma feature, correção ou documentação.
---

# Exploração local

Use esta skill para transformar uma dúvida, hipótese ou oportunidade em entendimento técnico útil, sem pressupor que haverá implementação. Preserve o caráter exploratório do pedido: investigar e propor opções não autoriza alterar código, configuração, dados, dependências, documentação ou Git.

## Checklist pré-execução

- [ ] Confirmar que o pedido é exploratório e que não há autorização para implementar ou alterar artefatos.
- [ ] Delimitar a pergunta, hipótese, decisão ou oportunidade a investigar e os limites conhecidos.
- [ ] Quando não estiverem claros, fazer perguntas objetivas sobre restrições, motivação e critério de sucesso antes de propor uma solução.
- [ ] Identificar o contexto técnico inicial, as fontes de evidência e eventuais alterações preexistentes que não devem ser tocadas.
- [ ] Avaliar se um diagrama Mermaid melhora materialmente a compreensão da solução, dos componentes, do fluxo ou dos *trade-offs*; quando pertinente, incluí-lo na DoD.
- [ ] Definir explicitamente a **Definition of Done (DoD)** da exploração, com critérios verificáveis e proporcionais ao pedido. Ela deve incluir, quando aplicável: hipóteses investigadas, evidências mínimas, incertezas que precisam ser declaradas, alternativas com *trade-offs*, recomendação esperada, diagrama Mermaid e a decisão conjunta sobre encerrar em plano ou apenas em clareza.
- [ ] Confirmar que cada critério da DoD pode ser verificado sem confundir fato observado com inferência.

## Checklist pós-execução — validação da DoD

Revise a DoD definida no pré-execução antes de encerrar. Para cada critério aplicável, registre evidência e o status **concluído**, **não aplicável** (com motivo) ou **pendente/bloqueado**. Considere a exploração concluída somente quando:

- [ ] as hipóteses e o escopo definidos na DoD foram investigados até o nível de evidência acordado;
- [ ] fatos, inferências, lacunas e riscos estão distinguidos;
- [ ] alternativas com *trade-offs* explícitos, recomendação preferida quando houver e plano somente se acordado foram entregues conforme a DoD;
- [ ] o diagrama Mermaid previsto na DoD foi incluído, é consistente com as evidências e facilita a compreensão da solução, ou sua não pertinência foi justificada;
- [ ] nenhuma alteração de código, configuração, documentação, dados ou Git foi realizada;
- [ ] itens pendentes ou bloqueados e suposições de escopo ou prioridade não confirmadas pelo usuário foram explicitamente reportados.

## Condução da investigação

Comece pelo objetivo, pelo comportamento ou pela hipótese trazida pelo usuário. Entenda o problema antes de propor solução: quando restrições, motivação ou critério de sucesso não estiverem claros, faça perguntas objetivas. Não pule diretamente para implementação.

Explore o repositório em modo somente leitura antes de opinar. Use leitura de arquivos, busca textual (*grep*) e listagem de arquivos (*glob*) para compreender o estado atual; inspecione somente os módulos, padrões, configurações, testes e documentação que possam confirmar ou refutar as hipóteses relevantes. Não escreva, edite, gere ou exclua arquivos neste modo.

Diferencie claramente no resultado:

- **Fatos observados** — sustentados por código, testes, configuração ou documentação, com caminhos e símbolos relevantes.
- **Inferências** — conclusões prováveis que dependem de evidência indireta.
- **Lacunas e perguntas em aberto** — informações que o repositório não permite concluir.
- **Opções** — alternativas de solução, seus impactos, riscos, dependências e critérios para escolhê-las.

Não trate uma hipótese do usuário como causa raiz sem evidência. Apresente alternativas com *trade-offs* explícitos, em vez de uma única direção disfarçada de análise. Quando houver uma opção preferida, declare-a e justifique-a com as evidências e os impactos observados. Considere compatibilidade, contratos, dados, observabilidade, segurança, desempenho, testes e esforço somente quando forem relevantes ao problema.

## Diagramas Mermaid

Use um diagrama Mermaid para explicar a solução final quando ele tornar mais claro um fluxo, a relação entre componentes, uma dependência, uma fronteira ou uma alternativa recomendada. Prefira `flowchart` para relações e fluxos gerais e `sequenceDiagram` quando a ordem de interações for essencial. Não inclua um diagrama decorativo nem represente elementos, relações ou comportamentos sem evidência; quando o texto for mais claro ou a solução ainda for imatura, declare que o diagrama não é pertinente.

## Escopo e autonomia

Navegue pelo código de modo progressivo: comece pelos pontos de entrada e módulos mais próximos ao tema, siga referências e expanda a investigação apenas quando isso reduzir uma incerteza relevante. Respeite alterações preexistentes; não as modifique, descarte ou atribua ao problema investigado.

Decida junto com o usuário se a exploração termina em plano ou apenas em clareza. Não force um plano quando a discussão ainda não amadureceu, nem deixe de planejá-lo quando o usuário decidir avançar. Se a conversa terminar em plano, estruture-o como entrada para `local-feature` (modo feature-normal): escopo, critério de validação e módulos afetados. Não execute a partir desta skill; a execução pertence a `local-feature`.

Quando a resposta depender de uma decisão de produto, requisito não definido ou acesso indisponível, apresente as opções e a informação necessária para decidir. Pergunte apenas o que for indispensável para continuar a investigação; caso contrário, avance com as evidências disponíveis e declare os limites.

## Resultado

Apresente em português uma síntese objetiva que contenha:

1. entendimento do problema e escopo investigado;
2. evidências técnicas relevantes, com caminhos e símbolos quando disponíveis;
3. conclusões, incertezas e riscos;
4. alternativas com *trade-offs* e, quando houver, a opção preferida e sua justificativa;
5. diagrama Mermaid da solução final, quando pertinente, com elementos consistentes com as evidências;
6. decisão sobre encerrar em clareza ou em plano; se houver plano, escopo, critério de validação e módulos afetados para `local-feature`;
7. suposições sobre escopo ou prioridade que não tenham sido confirmadas explicitamente pelo usuário.

A conclusão deve deixar explícito se o tema está pronto para seguir para `local-feature`, `local-bug`, `local-documentation` ou se ainda requer decisão ou investigação.
