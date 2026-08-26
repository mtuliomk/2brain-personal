---
name: doc-architecture
description: Analisar um ou mais repositórios que compõem uma aplicação e produzir documentação de arquitetura macro e C4 nos níveis de Contexto e Contêineres, incluindo obrigatoriamente as relações entre repositórios. Usar para documentar a arquitetura atual para arquitetos e líderes de tecnologia; não usar para projetar mudanças ou implementar.
---

# Documentação de arquitetura

Produza a documentação de arquitetura no **caminho e nome de arquivo definidos pelo prompt**. Não presuma nem imponha um nome padrão. Se o prompt não definir o destino, apresente a documentação completa no output da execução; não crie nem altere arquivos.

Uma aplicação é a composição de todos os repositórios presentes na workspace ou explicitamente indicados pelo prompt e pelo contexto. Analise a aplicação como um todo, documentando sua arquitetura macro e os dois primeiros níveis do modelo C4: **Contexto do Sistema** (nível 1) e **Contêineres** (nível 2). Use “contêiner” no sentido do C4 — aplicação, serviço, banco de dados, fila ou armazenamento executável/deployável — e não como sinônimo exclusivo de Docker.

O resultado é destinado a arquitetos e líderes de tecnologia e servirá futuramente como contexto para agentes de desenvolvimento autônomo com IA. Seja autocontido, preciso e rastreável. Descreva responsabilidades, limites, comunicações, dados, dependências e riscos arquiteturais com o nível de detalhe necessário para compreender o estado atual e tomar decisões informadas. Não proponha uma arquitetura futura, *roadmap* ou implementação detalhada não solicitada. As recomendações obrigatórias devem ser macro, derivadas das evidências e dos riscos observados, e não podem inventar requisitos.

Não altere código, configurações, infraestrutura, dados ou documentação existente, salvo quando o usuário pedir expressamente. O objetivo é retratar a arquitetura atual com base em evidências.

## Execução autônoma

Execute a análise integralmente de forma autônoma, dentro das permissões e ferramentas disponíveis. Não apresente plano de trabalho, não peça confirmação para iniciar ou continuar e não aguarde validação humana entre etapas. Inicie a investigação, tome as decisões necessárias com base nas evidências e entregue a documentação final.

Quando faltar informação, houver ambiguidade, conflito entre fontes, repositório inacessível ou evidência insuficiente, prossiga com o que estiver disponível. Registre a limitação, a premissa e a decisão no documento; use `Não identificado` quando aplicável. A única regra de destino é: grave o documento se o prompt informar um caminho; caso contrário, apresente-o integralmente no output. Não solicite o caminho de saída.

Um plano interno pode orientar a execução, mas não deve ser exposto ao usuário nem condicionar o início ou a conclusão da análise.

## Fontes e investigação

Use em conjunto os dados e instruções do prompt, o contexto em que a skill foi invocada e os artefatos dos repositórios. Delimite primeiro a aplicação e inventarie todos os repositórios analisados. Para cada um, investigue, conforme existirem, `README`, manifestos, código-fonte, módulos de entrada, rotas, contratos, clientes, modelos, migrações, configurações, variáveis de ambiente, infraestrutura como código, pipelines, testes, telemetria, filas, *jobs*, eventos e documentação.

Mapeie como os repositórios se relacionam e como cada relação opera: origem, destino, responsabilidade, finalidade, direção, mecanismo de comunicação, dados ou eventos trocados, sincronismo e evidência. A seção de relacionamento entre repositórios é obrigatória: inclua todos os repositórios analisados, inclusive os que não tenham relação identificada, e declare explicitamente essa ausência.

Quando houver ambiguidade ou conflito entre documentos, dados de contexto e código, priorize o comportamento demonstrado pelo código como representação do estado atual da aplicação. Registre a divergência, a decisão e as evidências em `## Premissas e Decisões`. Quando o código não oferecer evidência suficiente, use a fonte mais específica disponível e registre a premissa.

Não infira integrações, topologia de execução, dados, protocolos, autenticação, permissões, disponibilidade ou estratégias de implantação sem suporte. Escreva `Não identificado` quando aplicável. Distinga fatos observados, inferências e lacunas.

## Diagramas

Inclua diagramas Mermaid no documento para a arquitetura macro, o C4 de Contexto e o C4 de Contêineres. Os diagramas devem ser consistentes com as tabelas e usar identificadores estáveis. Use `flowchart` quando não houver suporte a uma notação C4 específica. Todo nó e relação deve possuir descrição correspondente no texto ou nas tabelas; não introduza no diagrama um elemento sem evidência.

## Estrutura obrigatória do documento

Use esta estrutura fixa. Mantenha `## Controle de Versão` como a última seção.

```markdown
# Arquitetura da Aplicação — <nome ou `Não identificado`>

## Escopo da Análise
- **Repositórios analisados:**
  - `REP-01` — `<repositório>` — <responsabilidade arquitetural aparente>
- **Contexto considerado:** <dados do prompt, contexto e documentos relevantes>
- **Limites da análise:** <evidências indisponíveis ou escopo não analisado; escreva `Nenhum identificado` quando aplicável>

## Visão Arquitetural Macro
<Resumo da aplicação, seus domínios/responsabilidades e a organização arquitetural observada.>

```mermaid
flowchart LR
  %% Componentes macro e relações observadas
```

## C4 — Contexto do Sistema (Nível 1)

### Sistema em Foco
- **Nome:** <nome>
- **Responsabilidade:** <o que oferece>
- **Usuários e atores:** <pessoas, processos ou `Não identificado`>

### Elementos Externos
| ID | Elemento | Tipo | Responsabilidade | Relação com o sistema |
| --- | --- | --- | --- | --- |
| EXT-01 | <nome> | Pessoa / Sistema externo | <responsabilidade> | <interação> |

```mermaid
flowchart LR
  %% Pessoas, sistema em foco e sistemas externos
```

## C4 — Contêineres (Nível 2)

| ID | Contêiner | Tipo | Repositório(s) | Responsabilidade | Dados gerenciados | Tecnologia ou execução relevante |
| --- | --- | --- | --- | --- | --- | --- |
| CTR-01 | <nome> | Aplicação / Serviço / Banco de dados / Fila / Armazenamento | `REP-01` | <responsabilidade> | <dados ou `Não identificado`> | <somente quando evidenciado> |

```mermaid
flowchart LR
  %% Contêineres, armazenamentos e dependências externas
```

## Componentes e Responsabilidades Relevantes
| ID | Componente | Contêiner | Responsabilidade | Interfaces ou pontos de entrada |
| --- | --- | --- | --- | --- |
| CMP-01 | <nome> | CTR-01 | <responsabilidade> | <API, evento, job, tela ou `Não identificado`> |

<Documente somente componentes que esclareçam limites, fluxos ou decisões arquiteturais relevantes; não faça inventário de classes.>

## Relacionamento entre Repositórios
| Origem | Destino | Relação e finalidade | Mecanismo | Direção e sincronismo | Dados ou eventos envolvidos | Evidência |
| --- | --- | --- | --- | --- | --- | --- |
| REP-01 | REP-02 | <como e por que se relacionam> | <API, biblioteca, evento, banco, pipeline etc.> | <unidirecional/bidirecional; síncrono/assíncrono ou `Não identificado`> | <descrição ou `Não identificado`> | <referência precisa> |

<Registre cada `REP-xx` sem relação identificada como uma linha explícita, com destino `Nenhum identificado`.>

## Fluxos e Integrações Relevantes
1. **FL-01 — <nome do fluxo>**
   - **Origem e gatilho:** <elemento e evento inicial>
   - **Caminho:** <contêineres e integrações na ordem observada>
   - **Dados ou evento:** <descrição>
   - **Resultado:** <efeito observável>
   - **Evidências:** <referências>

## Dados, Segurança e Operação

### Dados e Persistência
<Fontes de dados, proprietários, fronteiras e fluxos relevantes; escreva `Não identificado` quando aplicável.>

### Segurança e Acesso
<Autenticação, autorização, segredos, limites de confiança e exposição identificados; escreva `Não identificado` quando aplicável.>

### Operação e Confiabilidade
<Implantação, observabilidade, escalabilidade, falhas, recuperação e dependências operacionais evidenciadas; escreva `Não identificado` quando aplicável.>

## Riscos, Limites e Lacunas Arquiteturais
| ID | Item | Tipo | Impacto potencial | Evidência ou motivo |
| --- | --- | --- | --- | --- |
| RSK-01 | <descrição> | Risco / Limite / Lacuna | <impacto> | <evidência> |

<Escreva `Nenhum identificado` quando não houver evidência suficiente.>

## Recomendações
| ID | Recomendação macro | Prioridade sugerida | Justificativa | Riscos, limites ou elementos relacionados |
| --- | --- | --- | --- | --- |
| REC-01 | <melhoria arquitetural em nível macro> | Crítica / Alta / Média / Baixa | <benefício e evidência que a motivam> | RSK-01 / CTR-01 / FL-01 |

<Inclua recomendações somente quando sustentadas pelas evidências e pelos riscos ou limites observados. Priorize pelo impacto potencial, urgência, exposição e abrangência; explique a classificação. Não detalhe plano de implementação, tecnologias obrigatórias ou cronograma. Escreva `Nenhuma recomendação identificada` quando não houver evidência suficiente.>

## Evidências e Rastreabilidade
| Elemento ou conclusão | Fonte | Interpretação |
| --- | --- | --- |
| CTR-01 / FL-01 / relação entre repositórios | `<repositório>:<caminho, módulo, contrato, configuração ou teste>` | <o que a evidência sustenta> |

## Premissas e Decisões
- **Ambiguidade ou divergência:** <informação ausente, ambígua ou conflitante>
- **Decisão:** <interpretação adotada; registre a prevalência do código quando aplicável>
- **Fonte:** <prompt, contexto, documento, código ou raciocínio>

## Controle de Versão
| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agente | Criação inicial do documento |
```

Use identificadores estáveis `REP-xx`, `EXT-xx`, `CTR-xx`, `CMP-xx`, `FL-xx`, `RSK-xx` e `REC-xx`, e referencie-os de forma consistente nos diagramas, tabelas, fluxos, evidências e decisões. Preserve as entradas existentes do histórico de versão e acrescente uma entrada a cada revisão material.

## Autonomia e decisões

Não interrompa a análise por ausência, ambiguidade ou conflito de informações. Para conclusões sobre o estado atual, priorize comportamento e configuração efetivamente demonstrados pelo código; depois, use dados explícitos do prompt, contexto e documentação. Quando fontes não técnicas divergirem do código, documente a divergência e o impacto, sem alterar silenciosamente a conclusão.

Declare explicitamente fatos não identificados, lacunas, limites, dependências, suposições e relações ausentes. Não deixe relações arquiteturais ou fronteiras relevantes implícitas. Para habilitar o uso futuro por agentes, explique o propósito e a responsabilidade de cada elemento, não apenas sua tecnologia ou nome técnico. Relacione cada recomendação aos riscos, limites ou evidências que a sustentam e mantenha sua prioridade como sugestão, não como fato observado.

## Critérios de conclusão

Considere a documentação concluída somente quando estiver gravada no caminho definido pelo prompt ou, quando ele não existir, apresentada integralmente no output; estiver escrita em português e orientada a arquitetos e líderes de tecnologia; delimitar todos os repositórios como uma única aplicação; incluir arquitetura macro, C4 de Contexto e C4 de Contêineres com diagramas consistentes; relacionar obrigatoriamente todos os repositórios analisados; documentar fluxos, dados, segurança e operação quando evidenciados; incluir recomendações macro priorizadas e justificadas por evidências; registrar divergências com prevalência explícita do código; ser autocontida, rastreável e utilizável como contexto por agentes de desenvolvimento autônomo; e manter `## Controle de Versão` como última seção.
