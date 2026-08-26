---
name: doc-product
description: Analisar um ou mais repositórios de código que compõem uma aplicação e gerar, em português, documentação da visão de produto, identificando e descrevendo todos os produtos existentes. Usar para compreender o produto a partir do código e artefatos do repositório; não usar para especificar uma mudança ou decidir implementação.
---

# Documentação da visão de produto

Gere `product-vision.md` em português a partir da análise dos repositórios fornecidos. Uma aplicação é a composição de todos os repositórios presentes na workspace ou explicitamente indicados no contexto. Identifique todos os produtos que a aplicação oferece e documente cada um; não presuma que cada repositório corresponde a um produto.

Não altere código, configurações, dados ou documentação existente, salvo quando o usuário pedir expressamente. O objetivo é descrever o produto atual com base em evidências, não propor uma estratégia, *roadmap* ou implementação futura.

## Idioma

Escreva em português todos os títulos, seções, explicações, decisões e histórico de versão. Mantenha em inglês somente nomes de repositórios, arquivos, caminhos, módulos, classes, funções, rotas, contratos, comandos, identificadores e outros elementos técnicos existentes.

## Investigação

Delimite primeiro a aplicação: liste os repositórios analisados e a responsabilidade aparente de cada um. Inspecione sistematicamente, conforme existirem, `README`, documentação, manifestos, configurações, rotas, telas, componentes, serviços, contratos de API, modelos de dados, eventos, testes, integrações, variáveis de ambiente, textos de interface e exemplos de uso.

Investigue também como os repositórios se relacionam. Diferencie aplicações entregues a usuários, superfícies administrativas, APIs e serviços de suporte, bibliotecas compartilhadas, infraestrutura e ferramentas internas. Esses elementos podem habilitar um produto sem constituir um produto independente.

Identifique um produto quando houver uma proposta de valor, público ou processo atendido e conjunto coerente de capacidades observáveis. Não fragmente um único produto por camada técnica, repositório, tela ou API. Documente produtos separados quando as evidências indicarem públicos, propostas de valor, jornadas, domínios ou ofertas independentes. Quando a separação for incerta, adote a interpretação mais conservadora e registre a ambiguidade.

Baseie afirmações em evidências observadas. Para cada afirmação material, registre ao menos uma referência precisa de repositório, caminho, módulo, rota, contrato, tela, teste ou documento. Não infira regras de negócio, personas, preços, métricas, permissões ou integrações sem evidência; registre `Não identificado` quando aplicável.

## Estrutura obrigatória do documento

Use esta estrutura fixa. Ela deve documentar todos os produtos identificados, com uma seção `## Produto N` para cada produto. Mantenha `## Controle de Versão` como a última seção.

```markdown
# Visão de Produto — <nome da aplicação ou `Não identificado`>

## Escopo da Análise
- **Repositórios analisados:**
  - `<repositório>` — <responsabilidade aparente>
- **Critério de composição:** <como os repositórios foram considerados uma única aplicação>
- **Limites da análise:** <repositórios, arquivos ou evidências indisponíveis; escreva `Nenhum identificado` quando aplicável>

## Visão da Aplicação
<Propósito geral, problema ou oportunidade atendida e relação entre os produtos. Use somente evidências disponíveis.>

## Mapa de Produtos
| Produto | Público ou processo atendido | Proposta de valor | Repositórios e evidências principais |
| --- | --- | --- | --- |
| <nome do produto> | <público/processo ou `Não identificado`> | <valor ou `Não identificado`> | `<repositório>:<caminho ou referência>` |

## Produto 1 — <nome>

### Resumo
<Propósito, problema atendido e proposta de valor observável.>

### Público e Contexto de Uso
- **Público ou processo atendido:** <descrição ou `Não identificado`>
- **Necessidade atendida:** <descrição ou `Não identificado`>
- **Contexto e canais de acesso:** <web, API, aplicação móvel, processo interno etc., ou `Não identificado`>

### Capacidades Principais
1. **CP-01 — <nome da capacidade>**
   - <O que o usuário ou processo consegue realizar e o resultado observável.>
   - **Evidências:** `<repositório>:<caminho, rota, módulo, contrato ou teste>`

### Jornada e Fluxos Relevantes
1. **JF-01 — <nome do fluxo>**
   - **Início:** <gatilho ou pré-condição>
   - **Fluxo:** <etapas observáveis>
   - **Resultado:** <resultado entregue>
   - **Evidências:** `<repositório>:<referência>`

### Regras, Permissões e Dados Relevantes
- <Regra, permissão ou dado de produto observado, com evidência.>
- Escreva `Nenhum identificado` quando não houver evidência suficiente.

### Integrações e Dependências de Produto
- <Integração ou dependência que afete a experiência, com evidência.>
- Escreva `Nenhuma identificada` quando aplicável.

### Limites e Lacunas Conhecidas
- <Comportamento ausente, limitação ou incerteza sustentada por evidência.>
- Escreva `Nenhum identificado` quando aplicável.

## Relação entre Produtos
<Como os produtos se complementam, compartilham usuários, dados, capacidades ou dependências. Escreva `Não aplicável: um único produto identificado` quando houver somente um.>

## Evidências e Rastreabilidade
| Afirmação ou elemento | Evidência | Interpretação |
| --- | --- | --- |
| <produto, capacidade, fluxo ou relação> | `<repositório>:<caminho, módulo, rota, contrato ou teste>` | <o que a evidência sustenta> |

## Premissas e Decisões
- **Ambiguidade:** <informação ausente, ambígua ou conflitante>
- **Decisão:** <interpretação conservadora adotada>
- **Fonte:** <evidência, documento de contexto ou raciocínio>

## Controle de Versão
| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agente | Criação inicial do documento |
```

Inclua ao menos uma capacidade principal e uma evidência para cada produto identificado. Não invente seções adicionais para produtos sem evidência suficiente: trate-os como hipótese ou lacuna em `## Premissas e Decisões`, sem promovê-los a produto identificado. Preserve as entradas existentes no histórico de versão e acrescente uma entrada a cada revisão material.

## Autonomia e decisões

Não interrompa a análise por ausência, ambiguidade ou conflito de informações. Use esta ordem de prioridade: documentação e linguagem explícita voltada ao usuário, comportamento observável em interfaces e contratos, testes e exemplos de uso, implementação e configuração e, por fim, raciocínio documentado. Quando as fontes divergirem, registre o conflito e privilegie a evidência mais próxima do comportamento atual.

Não trate uma biblioteca, serviço de suporte, repositório de infraestrutura ou componente técnico como produto sem evidência de proposta de valor e uso próprio. Não omita produtos apenas porque compartilham um repositório, interface ou dados com outro produto.

## Critérios de conclusão

Considere a documentação concluída somente quando `product-vision.md` estiver em português, exceto pelos elementos técnicos existentes, delimitar todos os repositórios analisados como uma única aplicação, identificar e documentar todos os produtos sustentados pelas evidências, diferenciar produtos de componentes técnicos, incluir rastreabilidade para afirmações materiais, registrar lacunas e decisões relevantes, preservar `## Controle de Versão` como última seção e tiver sido gravado e validado no caminho exigido pelo contexto da tarefa.
