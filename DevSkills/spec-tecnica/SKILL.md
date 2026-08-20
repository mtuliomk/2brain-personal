---
name: spec-tecnica
description: Gerar uma especificação técnica em português a partir de uma especificação funcional disponível, investigando o código existente, classificando a mudança como Nova, Modificação ou Duplicada e definindo uma implementação e um plano de testes. Usar na etapa de especificação técnica, antes da implementação. Não usar para alterar código ou redefinir o escopo funcional.
---

# Especificação técnica

Gere `technical-spec.md` em português. Descreva como implementar o escopo funcional usando o código e o contexto disponíveis. Não implemente código, altere repositórios ou redefina silenciosamente o escopo funcional.

## Idioma

Escreva em português todos os títulos, seções, explicações, decisões, evidências, cenários e histórico de versão. Mantenha em inglês somente nomes de arquivos, caminhos, módulos, classes, funções, rotas, contratos, comandos, identificadores e demais trechos que representem código ou elementos técnicos existentes.

## Entradas e investigação

Leia a especificação funcional como fonte principal do escopo e as user stories como contexto de rastreabilidade. Inspecione documentos relevantes da aplicação, skills, código-fonte, testes, configurações, contratos, integrações, permissões, persistência e funcionalidades equivalentes antes de classificar a demanda.

Pesquise sistematicamente termos da task, das user stories e da especificação funcional. Registre evidências usando referências precisas de repositório, caminho, módulo, classe, função, rota, contrato ou teste. Não classifique uma mudança como `Nova` apenas porque a primeira busca não encontrou correspondências.

## Classificação

Classifique toda demanda como exatamente uma das opções abaixo:

- **Nova:** nenhuma implementação existente entrega o comportamento funcional solicitado.
- **Modificação:** uma funcionalidade existente entrega parte do comportamento ou precisa ser estendida ou alterada.
- **Duplicada:** o sistema atual já entrega todos os comportamentos funcionais e critérios de aceite relevantes, sem diferença material.

A classificação altera o conteúdo da mesma estrutura de documento; ela nunca altera a estrutura.

- Para **Nova**, descreva a abordagem completa de implementação.
- Para **Modificação**, descreva somente o delta sobre a implementação existente.
- Para **Duplicada**, documente onde o comportamento já existe e registre que não há implementação necessária.

## Estrutura obrigatória do documento

Use esta estrutura fixa para todas as classificações. Ela espelha a estrutura da especificação funcional com título, contexto, objetivo, escopo, decisões, rastreabilidade e histórico de versão. Não adicione conteúdo após `## Controle de Versão`.

```markdown
# Especificação Técnica — TAK-{{task_number}}

## Contexto
<Contexto funcional relevante, contexto técnico e descobertas do sistema atual.>

## Objetivo
<O que a implementação deve alcançar tecnicamente.>

## Classificação
**Tipo:** Nova | Modificação | Duplicada

<Justificativa da classificação.>

| Referência Funcional | Evidência no Código | Conclusão |
| --- | --- | --- |
| US-01 / CE-01 / CA-01 | <caminho, módulo, função, rota ou teste investigado> | <nova, estender, modificar ou já atendida> |

## Abordagem Técnica

### Componentes e Módulos Afetados
<Componentes, serviços, rotas, jobs ou configurações a criar ou alterar. Para Duplicada, escreva `Nenhuma implementação necessária` e identifique a implementação existente.>

### Fluxo Principal
<Fluxo de implementação e limites de responsabilidade. Para Modificação, descreva somente o delta.>

### Dados e Contratos
<Persistência, migrações, contratos de API, eventos, validação e tratamento de dados; escreva `Nenhum impacto identificado` quando aplicável.>

### Integrações
<Integrações externas ou internas, tratamento de falhas e considerações de compatibilidade; escreva `Nenhum impacto identificado` quando aplicável.>

## Impacto
<Permissões e segurança, observabilidade, desempenho, dados existentes, compatibilidade, rollout e funcionalidades dependentes. Registre `Nenhum impacto identificado` para cada área não aplicável.>

## Fora do Escopo Técnico
<Trabalho técnico considerado, mas excluído. Escreva `Nenhum identificado` quando não houver exclusão relevante.>

## Divergências com a Especificação Funcional
<Perda de informação, conflito, inviabilidade ou diferença de interpretação identificada durante a análise. Escreva `Nenhuma identificada` quando não houver divergência. Não altere o escopo funcional nesta seção.>

## Premissas e Decisões
- **Ambiguidade:** <informação ausente, ambígua ou conflitante>
- **Decisão:** <decisão mais restrita compatível com as evidências>
- **Fonte:** <documento de contexto, evidência no código, precedente ou raciocínio>

## Plano de Testes
| Referência Funcional | Cenário | Nível de Teste | Resultado Esperado |
| --- | --- | --- | --- |
| US-01 / CE-01 / CA-01 | <cenário> | Unitário / Integração / E2E / Manual | <resultado esperado> |

## Controle de Versão
| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agente | Criação inicial do documento |
```

Use referências `US-xx`, `CE-xx` e `CA-xx` quando estiverem disponíveis nos artefatos funcionais. Preserve todas as entradas existentes no histórico de versão e acrescente uma entrada a cada revisão material.

## Decisões e divergências

Não interrompa a tarefa por informações ausentes, ambíguas ou conflitantes. Use esta ordem de prioridade: contexto técnico explícito, arquitetura ou convenção estabelecida, requisito contextual relevante, precedente direto no código e, por fim, raciocínio documentado. Escolha a opção mais restrita compatível com as evidências.

Registre toda premissa material em `## Premissas e Decisões`. Registre conflitos funcionais, perda de informação ou inviabilidade em `## Divergências com a Especificação Funcional`; não os resolva alterando silenciosamente o escopo funcional.

## Critérios de conclusão

Considere a especificação concluída somente quando estiver escrita em português, exceto pelos nomes de arquivos e elementos de código, usar exatamente a estrutura obrigatória, contiver classificação justificada e tabela de evidências, relacionar abordagem técnica e plano de testes às referências funcionais disponíveis, registrar decisões e divergências, mantiver `## Controle de Versão` como última seção e tiver sido gravada e validada no caminho exigido pelo agente da etapa.
