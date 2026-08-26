---
name: doc-product
description: Analisar um ou mais repositórios que compõem uma aplicação e produzir uma visão de produto, em português, identificando e descrevendo todos os produtos existentes. Usar para documentar o produto atual para pessoas de Produto; não usar para especificar mudanças ou decidir implementação.
---

# Documentação da visão de produto

Produza a documentação de visão de produto no **caminho e nome de arquivo definidos pelo prompt**. Não presuma nem imponha um nome padrão. Se o prompt não definir o destino, pergunte onde gravar o documento antes de criar ou alterar qualquer arquivo.

Uma aplicação é a composição de todos os repositórios presentes na workspace ou explicitamente indicados pelo prompt e pelo contexto. Analise essa aplicação como um todo, identifique todos os produtos que ela oferece e documente cada um. Não presuma que um repositório, uma tela, uma API ou uma camada técnica seja necessariamente um produto.

O resultado é destinado a uma pessoa de Produto. Descreva o que é oferecido, para quem, qual problema atende, seus fluxos, capacidades, regras e limites. Mencione detalhes técnicos somente se forem necessários para sustentar uma decisão, esclarecer uma dependência que afete a experiência ou fortalecer um ponto de vista de produto. Prefira linguagem de negócio e comportamento observável; não transforme o documento em inventário técnico.

Não altere código, configurações, dados ou documentação existente, salvo quando o usuário pedir expressamente. O objetivo é retratar o produto atual com base em evidências e no contexto recebido, não propor estratégia, *roadmap* ou implementação futura.

## Fontes e investigação

Use, em conjunto, os dados e instruções fornecidos no prompt, o contexto em que a skill foi invocada e os artefatos dos repositórios. Trate dados explícitos de produto — como público, objetivos, terminologia, posicionamento, restrições e métricas — como fontes relevantes; valide-os contra o código quando isso for necessário para evitar contradições, sem descartá-los apenas por não aparecerem na implementação.

Delimite primeiro a aplicação: registre os repositórios analisados e a contribuição de cada um para a oferta. Investigue, conforme existirem, documentação, textos de interface, telas, fluxos, contratos, testes, exemplos de uso, modelos de dados, integrações e configurações. Use artefatos técnicos para compreender o comportamento, mas traduza-os em impacto, capacidade e experiência de produto no documento final.

Identifique um produto quando houver proposta de valor, público ou processo atendido e um conjunto coerente de capacidades observáveis. Documente produtos separados quando as evidências indicarem públicos, propostas de valor, jornadas, domínios ou ofertas independentes. Não fragmente um produto por repositório ou arquitetura. Bibliotecas, APIs de suporte, infraestrutura e ferramentas internas só são produtos quando houver evidência de uso e valor próprios.

Baseie afirmações materiais em evidências. Não infira regras de negócio, personas, preços, métricas, permissões ou integrações sem suporte; escreva `Não identificado` quando aplicável. Guarde referências técnicas precisas apenas na seção de rastreabilidade, e somente quando elas forem úteis para comprovar uma conclusão de produto.

## Estrutura obrigatória do documento

Use esta estrutura fixa. Documente todos os produtos identificados, com uma seção `## Produto N` para cada um. Mantenha `## Controle de Versão` como a última seção.

```markdown
# Visão de Produto — <nome da aplicação ou `Não identificado`>

## Escopo da Análise
- **Repositórios analisados:**
  - `<repositório>` — <contribuição para a aplicação, em linguagem de produto>
- **Contexto considerado:** <dados fornecidos pelo prompt, contexto e documentos relevantes>
- **Limites da análise:** <evidências indisponíveis ou escopo não analisado; escreva `Nenhum identificado` quando aplicável>

## Visão da Aplicação
<Propósito geral, problema ou oportunidade atendida e como os produtos se relacionam.>

## Mapa de Produtos
| Produto | Público ou processo atendido | Proposta de valor | Capacidades centrais |
| --- | --- | --- | --- |
| <nome do produto> | <público/processo ou `Não identificado`> | <valor ou `Não identificado`> | <capacidades observáveis> |

## Produto 1 — <nome>

### Resumo
<Propósito, problema atendido e proposta de valor observável.>

### Público e Contexto de Uso
- **Público ou processo atendido:** <descrição ou `Não identificado`>
- **Necessidade atendida:** <descrição ou `Não identificado`>
- **Canais e contexto de uso:** <descrição ou `Não identificado`>

### Capacidades Principais
1. **CP-01 — <nome da capacidade>**
   - <O que a pessoa ou processo consegue realizar e o resultado percebido.>

### Jornada e Fluxos Relevantes
1. **JF-01 — <nome do fluxo>**
   - **Início:** <gatilho ou pré-condição>
   - **Fluxo:** <etapas observáveis>
   - **Resultado:** <resultado entregue>

### Regras, Permissões e Informações Relevantes
- <Regra, permissão ou informação que afete a experiência ou a decisão de produto.>
- Escreva `Nenhum identificado` quando não houver evidência suficiente.

### Dependências que Afetam a Experiência
- <Dependência, integração ou condição externa e seu efeito para o produto.>
- Escreva `Nenhuma identificada` quando aplicável.

### Limites e Lacunas Conhecidas
- <Limitação, comportamento ausente ou incerteza relevante para Produto.>
- Escreva `Nenhum identificado` quando aplicável.

## Relação entre Produtos
<Como os produtos se complementam, compartilham públicos, jornadas ou informações. Escreva `Não aplicável: um único produto identificado` quando houver somente um.>

## Evidências e Rastreabilidade
| Conclusão de Produto | Fonte | Como sustenta a conclusão |
| --- | --- | --- |
| <produto, capacidade, fluxo ou relação> | <dado do prompt, documento, tela ou, quando necessário, referência técnica> | <interpretação> |

## Premissas e Decisões
- **Ambiguidade:** <informação ausente, ambígua ou conflitante>
- **Decisão:** <interpretação conservadora adotada>
- **Fonte:** <dado do prompt, contexto, evidência ou raciocínio>

## Controle de Versão
| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agente | Criação inicial do documento |
```

Inclua ao menos uma capacidade principal para cada produto identificado. Não promova uma hipótese a produto sem evidência suficiente: registre-a como lacuna ou ambiguidade. Preserve as entradas existentes no histórico de versão e acrescente uma entrada a cada revisão material.

## Autonomia e decisões

Não interrompa a análise por ausência, ambiguidade ou conflito de informações, exceto pela ausência do caminho de saída. Para conclusões de produto, priorize: dados explícitos do prompt e do contexto, documentação e linguagem voltada ao usuário, comportamento observável em interfaces e jornadas, testes e exemplos de uso, e por fim implementação e configuração. Quando fontes divergirem, registre o conflito e privilegie a fonte mais específica e atual sobre o comportamento ou intenção de produto.

Não inclua nomes de arquivos, tecnologias, módulos, rotas, contratos ou estruturas de dados no corpo principal apenas por estarem disponíveis. Inclua-os somente quando forem indispensáveis para explicar um limite, uma dependência, uma decisão ou uma evidência; nesse caso, contextualize seu impacto para Produto.

## Critérios de conclusão

Considere a documentação concluída somente quando estiver no caminho definido pelo prompt, escrita em português e orientada a pessoas de Produto; delimitar todos os repositórios analisados como uma aplicação única; identificar e documentar todos os produtos sustentados por evidências; diferenciar produtos de componentes técnicos; incorporar dados relevantes fornecidos pelo prompt e pelo contexto; registrar rastreabilidade, lacunas e decisões materiais; e manter `## Controle de Versão` como última seção.
