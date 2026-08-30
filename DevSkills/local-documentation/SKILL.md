---
name: local-documentation
description: Auditar e atualizar documentação do repositório contra o estado atual do código, sem alterar código de produção. Use quando o pedido for revisar, atualizar ou verificar a consistência de documentação. Não use para implementar comportamento, corrigir defeito funcional ou decidir uma mudança de produto.
---

# Documentação local

Audite a documentação solicitada contra o comportamento atual demonstrado pelo repositório e atualize somente os documentos necessários. O objetivo é reduzir divergências entre documentação e código, sem transformar a revisão documental em implementação ou redesign técnico.

Não altere código de produção, testes, configurações funcionais, infraestrutura, dependências, dados ou Git sem solicitação explícita. Preserve mudanças preexistentes e não as sobrescreva para padronizar o texto.

## Auditoria baseada em evidências

Delimite os documentos e tópicos solicitados. Para cada afirmação relevante, procure as fontes técnicas mais próximas: código de entrada, contratos, configuração, manifestos, comandos, testes, exemplos e automações. Dê precedência ao comportamento efetivamente demonstrado por código e configuração; quando a evidência for insuficiente, preserve a incerteza em vez de inventar detalhes.

Classifique os achados como:

- documentação correta e mantida;
- informação ausente, desatualizada, ambígua ou contraditória;
- afirmação não confirmável com as evidências disponíveis.

Ao encontrar contradição entre documentos, resolva-a conforme o estado atual do código e registre a limitação se o código também não for conclusivo. Não corrija documentação fora do escopo apenas por estilo, salvo se for necessária para tornar a seção alterada consistente.

## Atualização

Escreva em conformidade com o idioma, tom, estrutura, links e convenções já adotados pelo repositório. Seja específico sobre pré-requisitos, comandos, interfaces, variáveis, fluxos e limitações somente quando houver evidência. Mantenha exemplos executáveis e marque valores sensíveis como placeholders seguros; nunca introduza credenciais, tokens ou dados pessoais.

Preserve a navegação e os links existentes. Após editar, verifique links e referências internas afetadas, nomes de arquivos, comandos, blocos de código, âncoras e exemplos contra seus alvos. Não alegue que um procedimento foi executado se a evidência apenas permitiu revisão estática.

## Resultado

Ao concluir, responda em português com:

- documentos auditados e fontes técnicas consultadas;
- alterações realizadas e as divergências que elas corrigem;
- verificações executadas, incluindo links, comandos ou exemplos quando aplicável;
- pontos não confirmados, lacunas e documentação deliberadamente não alterada;
- confirmação de que código de produção não foi modificado.

A tarefa está concluída quando a documentação solicitada estiver alinhada às evidências disponíveis, as afirmações não verificáveis estiverem explicitamente tratadas e as mudanças estiverem limitadas aos documentos necessários.
