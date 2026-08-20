---
name: spec-coding
description: Implementar uma mudança de código a partir de uma especificação técnica disponível, aplicando skills de desenvolvimento relevantes, executando validações do projeto, criando commits locais e gerando um resultado de implementação versionado. Usar na etapa de implementação. Não usar para redefinir escopo funcional ou técnico.
---

# Implementação

Implemente o escopo definido na especificação técnica disponível. Use a especificação funcional e as user stories para manter a rastreabilidade dos comportamentos e critérios de aceite. Não altere silenciosamente o escopo, a classificação ou a abordagem técnica.

## Autonomia e linha de base

Execute a etapa de forma autônoma. Não aguarde aprovação, validação, decisão ou intervenção humana. Quando houver informação ausente, ambígua ou conflitante, use a especificação técnica, o contexto, as skills e o código como evidências; adote a decisão mais restrita compatível com elas e registre a decisão no resultado final.

Antes de alterar código, registre a linha de base: branches, alterações preexistentes, comandos oficiais de validação e falhas já existentes. Não atribua falhas preexistentes à task e não altere arquivos fora do escopo para fazer validações globais passarem.

## Implementação por classificação

- **Nova:** implemente a abordagem completa definida na especificação técnica.
- **Modificação:** implemente somente o delta definido sobre o código existente.
- **Duplicada:** não crie alterações ou commits vazios; registre no resultado final as evidências de que o comportamento já existe.

Reutilize padrões, componentes, helpers, contratos e convenções existentes antes de criar novos. Crie ou ajuste testes para os comportamentos `CE-xx` e critérios `CA-xx` afetados quando aplicável.

## Validações

Identifique os comandos e procedimentos oficiais no repositório. Execute as validações aplicáveis após a última alteração, incluindo testes, *lint*, formatação, análise estática, *typecheck*, *build* e revisão do diff.

Para cada validação, registre comando ou procedimento, resultado e evidência. Quando uma falha preexistente impedir uma validação, registre a linha de base, a limitação e o impacto. Não invente comandos, ferramentas ou critérios ausentes.

## Commits

Crie commits atômicos e locais somente nos repositórios modificados. Inclua apenas alterações da task, siga a skill `dev-commit` quando disponível e não faça *push*. Não crie commit vazio quando não houver alteração necessária.

## Resultado final

Gere `implementation-result.md` em português no caminho definido pelo agente da etapa. Mantenha em inglês somente nomes de arquivos, caminhos, módulos, classes, funções, rotas, contratos, comandos, identificadores, hashes de commit e outros elementos de código.

O resultado deve usar a estrutura definida pelo agente da etapa, preservar `## Controle de Versão` como última seção e registrar status, alterações, rastreabilidade, validações, commits, escopo excluído, decisões e limitações. Preserve entradas existentes do histórico de versão e acrescente uma entrada a cada revisão material.

## Critérios de conclusão

Considere a implementação concluída somente quando o escopo técnico tiver sido respeitado, as alterações ou a ausência justificada estiverem registradas, as validações aplicáveis tiverem sido executadas e documentadas, os commits necessários tiverem sido criados localmente sem *push*, o resultado final tiver sido gravado e validado e nenhuma alteração fora do escopo tiver sido introduzida.
