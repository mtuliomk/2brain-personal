---
name: local-feature
description: Planejar, implementar e validar autonomamente features novas ou modificações de fluxos existentes no ambiente local. Use quando o pedido introduzir comportamento novo, tocar mais de um módulo ou exigir decisão de design com critério de sucesso verificável. Não use para investigação aberta, correção pontual sem comportamento novo ou auditoria exclusiva de documentação.
---

# Desenvolvimento de feature local

Conduza o ciclo completo de planejamento, execução e validação para entregar a mudança solicitada no ambiente local. Preserve o objetivo, os critérios de sucesso e as escolhas explícitas do usuário; não amplie o escopo com melhorias adjacentes sem necessidade demonstrável.

Não crie commits, faça push, publique, altere dados de produção ou execute ações externas irreversíveis sem solicitação explícita. Alterações preexistentes não relacionadas devem ser preservadas e não devem ser incluídas como parte da entrega.

## Linha de base e entendimento

Antes de editar, registre mentalmente e informe no resultado quando relevante:

- estado do repositório e alterações preexistentes;
- instruções locais, convenções, arquitetura e módulos afetados;
- comportamento atual, comportamento esperado e critérios verificáveis de sucesso;
- comandos oficiais de teste, qualidade, *typecheck*, *build* e execução local;
- contratos, migrações, integrações, dados, permissões ou compatibilidades que a mudança possa afetar.

Use código, testes e configurações como fonte principal do comportamento atual. Quando os requisitos estiverem ambíguos, escolha a interpretação mais restrita compatível com as evidências e declare a decisão; peça esclarecimento somente se a ambiguidade impedir uma implementação segura.

## Planejamento

Antes de modificar o código, formule um plano de execução proporcional à complexidade. Para cada etapa, identifique o módulo ou arquivo provável, a alteração de comportamento, dependências ou contratos impactados e como ela será validada. Para mudanças de design, compare alternativas somente até haver uma decisão justificada pelos critérios de sucesso, pelos padrões do repositório e pelo menor impacto compatível.

Mantenha o plano no raciocínio e apresente-o ao usuário de forma concisa no resultado, salvo se o pedido exigir um artefato de plano. Não crie documentos de planejamento no repositório sem solicitação explícita.

## Implementação

Implemente o menor conjunto coeso de mudanças necessário. Reutilize componentes, padrões, abstrações, contratos e validações existentes antes de criar novos. Mantenha a compatibilidade dos consumidores existentes, a menos que uma quebra seja requisito explícito.

Aplique as skills de desenvolvimento correspondentes à tecnologia e ao tipo de alteração. Atualize testes que representem o comportamento observável e inclua cobertura para novos fluxos, falhas e limites relevantes. Atualize documentação apenas quando ela for afetada pelo comportamento implementado; use `local-documentation` quando a tarefa for exclusivamente documental.

Revise o diff para verificar que não há regressões óbvias, alterações acidentais, segredos, arquivos gerados ou mudanças fora do escopo. Não mascare falhas de testes nem reduza validações para fazer a entrega parecer aprovada.

## Validação e resultado

Execute os comandos oficiais aplicáveis, priorizando testes direcionados e incluindo *lint*, formatação, análise estática, *typecheck* e *build* quando existirem e forem proporcionais à mudança. Se uma validação não puder ser executada, registre o motivo, o impacto e a melhor evidência alternativa. Diferencie falhas preexistentes das introduzidas pela alteração.

Ao concluir, responda em português com:

- resumo do comportamento entregue e decisão de design relevante;
- arquivos ou módulos alterados e o motivo;
- testes e validações executados, com seus resultados;
- limitações, riscos, premissas e validações não executadas;
- estado de commits somente se houver solicitação ou se tiver sido alterado por ordem explícita.

A entrega está concluída quando os critérios de sucesso estiverem atendidos, as alterações forem coesas e as validações aplicáveis tiverem evidência suficiente.
