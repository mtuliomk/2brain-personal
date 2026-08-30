---
name: local-feature
description: Planejar, implementar e validar autonomamente features novas ou modificações de fluxos existentes no ambiente local. Use quando o pedido introduzir comportamento novo, tocar mais de um módulo ou exigir decisão de design com critério de sucesso verificável. Não use para investigação aberta, correção pontual sem comportamento novo ou auditoria exclusiva de documentação.
---

# Desenvolvimento de feature local

Conduza o ciclo completo de planejamento, execução e validação para entregar a mudança solicitada no ambiente local. Preserve o objetivo, os critérios de sucesso e as escolhas explícitas do usuário; não amplie o escopo com melhorias adjacentes sem necessidade demonstrável.

Não crie commits, faça push, publique, altere dados de produção ou execute ações externas irreversíveis sem solicitação explícita. Alterações preexistentes não relacionadas devem ser preservadas e não devem ser incluídas como parte da entrega.

## Checklist pré-execução

- [ ] Confirmar que a mudança introduz comportamento novo, altera um fluxo ou exige uma decisão de design verificável.
- [ ] Delimitar objetivo, escopo, comportamento atual, comportamento esperado e consumidores ou contratos afetados.
- [ ] Registrar a linha de base: instruções locais, alterações preexistentes, convenções, módulos e comandos oficiais de validação.
- [ ] Declarar em uma linha o **critério de sucesso** antes de planejar. Se ele não estiver claro e depender de informação que somente o usuário possui, pedir esclarecimento antes de prosseguir; não assumi-lo silenciosamente.
- [ ] Definir explicitamente a **Definition of Done (DoD)** antes de editar, alinhada ao critério de sucesso. Ela deve conter critérios de aceite verificáveis para o comportamento entregue, compatibilidades ou migrações necessárias, testes e validações requeridos, documentação afetada e limites de escopo.
- [ ] Associar cada item da DoD a uma forma de evidência: teste, comando, inspeção de diff, revisão de contrato ou procedimento manual reproduzível.
- [ ] Montar e apresentar o plano antes da execução, com arquivos ou módulos afetados, ordem, dependências entre etapas e validação de cada etapa. A apresentação do plano não exige aprovação humana para continuar.

## Checklist pós-execução — validação da DoD

Revise a DoD definida no pré-execução antes de encerrar. Para cada critério aplicável, registre evidência e o status **concluído**, **não aplicável** (com motivo) ou **pendente/bloqueado**. Considere a feature concluída somente quando:

- [ ] o comportamento e os critérios de aceite definidos na DoD foram atendidos;
- [ ] compatibilidades, contratos, migrações e documentação previstos na DoD foram tratados ou justificados;
- [ ] os testes e validações previstos na DoD foram executados, ou sua ausência está explicitamente limitada;
- [ ] o critério de sucesso declarado antes do plano foi validado por evidências, e não por impressão geral de que a alteração parece correta;
- [ ] o diff está limitado ao escopo acordado e preserva alterações preexistentes;
- [ ] riscos, premissas, decisões assumidas e itens pendentes ou bloqueados foram reportados.

## Linha de base e entendimento

Antes de editar, registre mentalmente e informe no resultado quando relevante:

- estado do repositório e alterações preexistentes;
- instruções locais, convenções, arquitetura e módulos afetados;
- comportamento atual, comportamento esperado e critérios verificáveis de sucesso;
- comandos oficiais de teste, qualidade, *typecheck*, *build* e execução local;
- contratos, migrações, integrações, dados, permissões ou compatibilidades que a mudança possa afetar.

Use código, testes e configurações como fonte principal do comportamento atual. Não bloqueie a execução por ambiguidade menor: resolva-a usando, nesta ordem, `decisions.md`, `product.md` ou `glossary.md`, precedentes em outras *tasks* e, por último, raciocínio próprio. Quando uma fonte não existir ou não resolver a ambiguidade, avance para a próxima. Registre ao final a decisão assumida e a fonte da hierarquia utilizada. Peça esclarecimento somente quando o critério de sucesso não estiver claro e a informação necessária só puder vir do usuário.

## Planejamento

Antes de modificar o código, declare em uma linha o critério de sucesso e formule um plano de execução proporcional à complexidade. Para cada etapa, identifique os arquivos ou módulos afetados, a alteração de comportamento, a ordem de execução, as dependências entre etapas e como ela será validada por teste automatizado, *build* ou verificação manual pontual. Para mudanças de design, compare alternativas somente até haver uma decisão justificada pelo critério de sucesso, pelos padrões do repositório e pelo menor impacto compatível.

Apresente o plano ao usuário antes de iniciar a execução, sem solicitar aprovação ou aguardar validação humana entre as iterações. Não crie documentos de planejamento no repositório sem solicitação explícita.

## Implementação

Implemente o menor conjunto coeso de mudanças necessário. Reutilize componentes, padrões, abstrações, contratos e validações existentes antes de criar novos. Mantenha a compatibilidade dos consumidores existentes, a menos que uma quebra seja requisito explícito.

Aplique as skills de desenvolvimento correspondentes à tecnologia e ao tipo de alteração. Atualize testes que representem o comportamento observável e inclua cobertura para novos fluxos, falhas e limites relevantes. Atualize documentação apenas quando ela for afetada pelo comportamento implementado; use `local-documentation` quando a tarefa for exclusivamente documental.

Revise o diff para verificar que não há regressões óbvias, alterações acidentais, segredos, arquivos gerados ou mudanças fora do escopo. Não mascare falhas de testes nem reduza validações para fazer a entrega parecer aprovada.

## Validação e resultado

Valide a implementação contra o critério de sucesso declarado antes do plano e contra a DoD, não contra uma impressão geral de que o resultado parece correto. Execute os testes e verificações definidos no plano, incluindo os comandos oficiais aplicáveis de *lint*, formatação, análise estática, *typecheck* e *build* quando existirem e forem proporcionais à mudança.

Quando uma validação falhar, diagnostique a causa, corrija e valide novamente. Repita o ciclo até o critério de sucesso ser atendido ou até esgotar as abordagens razoáveis. Neste último caso, pare e registre o obstáculo, as tentativas, as evidências e o impacto; não insista indefinidamente. Se uma validação não puder ser executada, registre o motivo, o impacto e a melhor evidência alternativa. Diferencie falhas preexistentes das introduzidas pela alteração.

Ao concluir, responda em português com:

- critério de sucesso declarado, DoD e evidências que comprovam seus itens;
- plano apresentado e sua execução, incluindo arquivos ou módulos alterados e o motivo;
- testes e validações executados, seus resultados e iterações realizadas após falhas;
- decisões de design ou escopo assumidas autonomamente, cada uma com a fonte usada na hierarquia (`decisions.md`, `product.md`/`glossary.md`, precedente em outra *task* ou raciocínio próprio);
- limitações, riscos, obstáculos, premissas e validações não executadas;
- estado de commits somente se houver solicitação ou se tiver sido alterado por ordem explícita.

A entrega está concluída quando os critérios de sucesso estiverem atendidos, as alterações forem coesas e as validações aplicáveis tiverem evidência suficiente.
