---
name: local-bug
description: Corrigir diretamente um comportamento divergente do esperado no ambiente local, com teste de regressão quando justificável. Use quando a causa raiz for conhecida ou localizável em um módulo e a correção não introduzir comportamento novo. Não use para nova feature, mudança ampla de design, investigação ainda aberta ou atualização exclusivamente documental.
---

# Correção local de bug

Corrija a menor causa raiz que explique a divergência observada, preservando contratos e comportamentos válidos existentes. Esta skill é para restaurar um comportamento esperado, não para adicionar capacidade, redesenhar fluxos ou executar refatorações amplas.

Não crie commits, faça push, publique, altere dados de produção ou execute ações externas irreversíveis sem solicitação explícita. Preserve alterações preexistentes e não as atribua ao bug sem evidência.

## Checklist pré-execução

- [ ] Confirmar que o trabalho restaura comportamento esperado, sem introduzir comportamento novo ou exigir redesign amplo.
- [ ] Delimitar comportamento esperado, divergência observada, impacto conhecido e cenário de reprodução ou evidência estática disponível.
- [ ] Localizar a causa raiz provável, o módulo responsável, contratos próximos e alterações preexistentes que devem ser preservadas.
- [ ] Definir explicitamente a **Definition of Done (DoD)** antes de editar. Ela deve conter critérios de aceite verificáveis para a correção, comportamento que não pode regredir, necessidade ou justificativa de teste de regressão **unitário** e validações obrigatórias do módulo.
- [ ] Associar cada item da DoD a uma evidência: reprodução do cenário, teste unitário, comando oficial, inspeção de contrato ou procedimento manual reproduzível.

## Checklist pós-execução — validação da DoD

Revise a DoD definida no pré-execução antes de encerrar. Para cada critério aplicável, registre evidência e o status **concluído**, **não aplicável** (com motivo) ou **pendente/bloqueado**. Considere o bug corrigido somente quando:

- [ ] a causa raiz e o comportamento esperado definidos na DoD foram comprovadamente tratados;
- [ ] a correção não introduz comportamento novo nem altera contratos fora do escopo;
- [ ] o teste de regressão unitário previsto foi adicionado e passou, ou sua ausência tem justificativa concreta registrada;
- [ ] as validações previstas na DoD foram executadas, ou a limitação foi explicitamente reportada;
- [ ] riscos residuais, falhas preexistentes e itens pendentes ou bloqueados foram reportados.

## Diagnóstico

Antes de editar, confirme o comportamento esperado, o comportamento atual e a origem provável da divergência. Examine o módulo afetado, seus chamadores, contratos, validações, configuração e testes próximos. Reproduza o problema quando houver meio local seguro e proporcional; quando não for possível, use evidência estática e declare a limitação.

Não corrija apenas o sintoma quando a causa raiz for acessível. Se a investigação revelar que a causa é incerta, que afeta múltiplos fluxos ou que exige decisão de design, interrompa a classificação e trate o trabalho como `local-explorer` ou `local-feature`, explicando a razão.

## Implementação e regressão

Implemente uma mudança focalizada no módulo responsável. Reuse as convenções e mecanismos existentes e evite alterações oportunistas fora da causa raiz. Mantenha compatibilidade, tratamento de erros e observabilidade já esperados pelo repositório.

Adicione ou ajuste um teste de regressão **unitário** quando ele puder demonstrar de forma confiável o cenário que falhava, quando houver infraestrutura de testes adequada e quando o custo for proporcional ao risco. Isole dependências por meio dos mecanismos de *mock*, *stub* ou *fake* já adotados pelo projeto. Não construa testes de regressão integrados nem testes que dependam de serviços, bancos, filas, rede ou outras dependências externas. Se não adicionar o teste unitário, registre a justificativa concreta — por exemplo, ausência de ponto unitário testável, cobertura já existente ou alteração exclusivamente configuracional — e execute a melhor validação disponível.

## Validação e resultado

Execute primeiro a validação mais próxima do defeito e, quando aplicável, os comandos oficiais de qualidade, testes, *typecheck* ou *build* que cubram o módulo alterado. Revise o diff para confirmar que a correção é limitada, que não introduz comportamento novo e que não há arquivos ou segredos indevidos.

Ao concluir, responda:

- causa raiz e evidências que a sustentam;
- comportamento corrigido e arquivos ou módulos alterados;
- teste de regressão unitário adicionado ou motivo para sua ausência;
- validações executadas e resultado;
- limitações, riscos residuais ou falhas preexistentes observadas.
