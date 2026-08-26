---
name: doc-security-compliance
description: Analisar um ou mais repositórios que compõem uma aplicação e produzir documentação da arquitetura atual de segurança e compliance, incluindo comunicação entre módulos, autenticação de usuários e entre serviços, LGPD quando aplicável, riscos e recomendações. Usar para documentar o estado atual para times técnicos e agentes de desenvolvimento; não usar para implementar controles ou emitir parecer jurídico.
---

# Documentação de segurança e compliance

Produza a documentação de segurança e compliance no **caminho e nome de arquivo definidos pelo prompt**. Não presuma nem imponha um nome padrão. Se o prompt não definir o destino, apresente a documentação completa no output da execução; não crie nem altere arquivos.

Uma aplicação é a composição de todos os repositórios presentes na workspace ou explicitamente indicados pelo prompt e pelo contexto. Analise a aplicação como um todo. O objetivo principal é documentar a **arquitetura atual de segurança e compliance**: como os módulos se comunicam, as fronteiras de confiança, os protocolos, a autenticação de usuários e entre serviços, os controles e os dados protegidos. A análise de riscos é complementar e deve sustentar a compreensão do estado atual, não transformar o documento em um relatório de auditoria. Quando a aplicação tratar dados de clientes, documente obrigatoriamente como os requisitos e princípios aplicáveis da LGPD aparecem — ou não aparecem — nas evidências disponíveis.

O resultado é destinado a times de segurança, compliance, arquitetura e liderança técnica e servirá futuramente como contexto para agentes de desenvolvimento autônomo com IA. Seja autocontido, preciso e rastreável. Descreva o estado atual sem declarar conformidade legal ou certificação que não esteja comprovada. Esta documentação não substitui avaliação jurídica, DPO/encarregado, auditoria ou *pentest*.

Nunca reproduza segredos, credenciais, chaves privadas, tokens, dados pessoais reais ou outros dados sensíveis encontrados durante a análise. Descreva apenas o tipo, local lógico e controle observado, usando referências seguras.

Não altere código, configurações, infraestrutura, dados ou documentação existente, salvo quando o usuário pedir expressamente. O objetivo é retratar controles, lacunas e riscos atuais, não implementar correções.

## Execução autônoma

Execute a análise integralmente de forma autônoma, dentro das permissões e ferramentas disponíveis. Não apresente plano de trabalho, não peça confirmação para iniciar ou continuar e não aguarde validação humana entre etapas. Inicie a investigação, tome as decisões necessárias com base nas evidências e entregue a documentação final.

Quando faltar informação, houver ambiguidade, conflito entre fontes, repositório inacessível ou evidência insuficiente, prossiga com o que estiver disponível. Registre a limitação, a premissa e a decisão no documento; use `Não identificado` quando aplicável. Grave o documento se o prompt informar um caminho; caso contrário, apresente-o integralmente no output. Não solicite o caminho de saída.

Um plano interno pode orientar a execução, mas não deve ser exposto ao usuário nem condicionar o início ou a conclusão da análise.

## Fontes e investigação

Use em conjunto os dados e instruções do prompt, o contexto em que a skill foi invocada e os artefatos dos repositórios. Inventarie todos os repositórios e módulos analisados. Inspecione, conforme existirem, documentação, manifestos, código-fonte, módulos de entrada, rotas, contratos, clientes, modelos, migrações, configurações, variáveis de ambiente, infraestrutura como código, pipelines, dependências, testes, *logs*, telemetria, filas, *jobs*, eventos e políticas.

Mapeie obrigatoriamente a comunicação entre módulos: origem, destino, finalidade, mecanismo e protocolo, direção, sincronismo, dados trocados, autenticação/autorização, credencial ou tipo de token, criptografia de transporte e evidência. Dê atenção especial aos fluxos de autenticação de usuários e *service-to-service*: identifique, quando evidenciado, o protocolo, emissor e consumidor de tokens, escopos ou *claims*, validações, ciclo de vida, renovação e armazenamento lógico de credenciais. Não exponha valores de tokens ou segredos. Inclua os módulos sem comunicação identificada e declare explicitamente a ausência. Diferencie comunicação interna, integrações externas e acesso a persistência.

Investigue controles de identidade e acesso, protocolos de autenticação, gestão de tokens e segredos, criptografia, validação de entrada, exposição de interfaces, dependências, isolamento, auditoria, monitoramento, retenção, *backup*, continuidade e resposta a incidentes, sem inferir controles inexistentes. Quando houver ambiguidade ou conflito entre documentos, dados de contexto e código, priorize o comportamento demonstrado pelo código e configuração como representação do estado atual. Registre a divergência, a decisão e as evidências.

## Dados de clientes e LGPD

Determine se a aplicação trata dados de clientes, inclusive dados pessoais, identificadores online, dados de contato, dados de uso, dados financeiros ou categorias especiais. Quando houver evidência desse tratamento, inclua obrigatoriamente a seção `## LGPD e Dados de Clientes` completa, mesmo quando a conclusão for que um aspecto não foi identificado.

Documente somente o que as evidências sustentarem sobre: categorias de dados, titulares, finalidade, fluxo e compartilhamento, bases legais, consentimento, minimização, retenção e descarte, direitos dos titulares, operadores e terceiros, transferências internacionais, segurança, registro de operações e resposta a incidentes. Não presuma base legal, papel de controlador/operador, consentimento válido ou conformidade. Para cada aspecto sem evidência, escreva `Não identificado` e registre o risco ou lacuna correspondente quando material.

Se não houver evidência de dados de clientes, registre a investigação e a conclusão em `## Dados e Classificação`, indicando os limites dessa conclusão; não omita a análise.

## Estrutura obrigatória do documento

Use esta estrutura fixa. Mantenha `## Controle de Versão` como a última seção.

```markdown
# Segurança e Compliance da Aplicação — <nome ou `Não identificado`>

## Escopo da Análise
- **Repositórios analisados:**
  - `REP-01` — `<repositório>` — <responsabilidade aparente>
- **Contexto considerado:** <dados do prompt, contexto e documentos relevantes>
- **Limites da análise:** <evidências indisponíveis ou escopo não analisado; escreva `Nenhum identificado` quando aplicável>

## Visão de Segurança
<Resumo das superfícies de ataque, fronteiras de confiança, ativos relevantes e postura observada.>

## Módulos e Comunicação
| ID | Módulo | Repositório | Responsabilidade | Interfaces expostas |
| --- | --- | --- | --- | --- |
| MOD-01 | <nome> | `REP-01` | <responsabilidade> | <API, evento, job, interface ou `Não identificado`> |

```mermaid
flowchart LR
  %% Módulos, dependências externas, fluxos e fronteiras de confiança observados
```

### Matriz de Comunicação entre Módulos
| Origem | Destino | Finalidade | Mecanismo e protocolo | Direção e sincronismo | Dados trocados | Autenticação, autorização e token | Proteção em trânsito | Evidência |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| MOD-01 | MOD-02 | <finalidade> | <API/HTTP, evento, gRPC, biblioteca, banco etc.> | <unidirecional/bidirecional; síncrono/assíncrono ou `Não identificado`> | <categoria ou `Não identificado`> | <controle, tipo de token ou `Não identificado`> | <controle ou `Não identificado`> | <referência segura> |

<Registre cada `MOD-xx` sem comunicação identificada com destino `Nenhum identificado`.>

## Protocolos de Comunicação e Autenticação

### Autenticação de Usuários
| ID | Ponto de entrada | Protocolo ou mecanismo | Credencial ou token | Emissor e validação | Autorização | Ciclo de vida e armazenamento lógico | Evidência |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AUTH-01 | <interface ou API> | OAuth 2.0 / OpenID Connect / Sessão / `Não identificado` | JWT / cookie de sessão / `Não identificado` | <descrição ou `Não identificado`> | <papéis, escopos ou `Não identificado`> | <descrição ou `Não identificado`> | <referência segura> |

### Autenticação *Service-to-Service*
| ID | Serviço de origem | Serviço de destino | Protocolo ou mecanismo | Credencial ou token | Emissor, validação e autorização | Ciclo de vida e gestão de segredo | Evidência |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AUTH-02 | MOD-01 | MOD-02 | OAuth 2.0 Client Credentials / mTLS / API key / `Não identificado` | <tipo ou `Não identificado`> | <descrição ou `Não identificado`> | <descrição ou `Não identificado`> | <referência segura> |

<Descreva somente mecanismos observados. Não exponha tokens, chaves, segredos ou valores de configuração.>

## Controles de Segurança Observados
| ID | Domínio | Controle ou comportamento observado | Cobertura e limite | Evidência |
| --- | --- | --- | --- | --- |
| SEC-01 | Identidade e acesso / Segredos / Dados / Aplicação / Infraestrutura / Observabilidade | <descrição> | <onde se aplica e limitações> | <referência segura> |

## Dados e Classificação
| ID | Categoria de dado | Titulares ou origem | Módulos envolvidos | Classificação | Armazenamento ou trânsito | Evidência |
| --- | --- | --- | --- | --- | --- | --- |
| DAT-01 | <categoria> | <origem ou `Não identificado`> | MOD-01 | Pessoal / Sensível / Confidencial / Operacional / `Não identificado` | <descrição> | <referência segura> |

<Inclua uma conclusão explícita sobre a existência ou não de evidências de dados de clientes.>

## LGPD e Dados de Clientes
<Preencha esta seção obrigatoriamente quando houver dados de clientes. Se a análise não encontrar evidência, registre `Não identificado` em cada item e as lacunas materiais.>

| Aspecto | Situação observada | Evidência | Lacuna ou impacto |
| --- | --- | --- | --- |
| Categorias de dados e titulares | <descrição ou `Não identificado`> | <fonte> | <descrição ou `Nenhuma identificada`> |
| Finalidade do tratamento | <descrição ou `Não identificado`> | <fonte> | <descrição ou `Nenhuma identificada`> |
| Base legal | <descrição ou `Não identificado`> | <fonte> | <descrição ou `Nenhuma identificada`> |
| Consentimento, quando aplicável | <descrição ou `Não identificado`> | <fonte> | <descrição ou `Nenhuma identificada`> |
| Minimização, retenção e descarte | <descrição ou `Não identificado`> | <fonte> | <descrição ou `Nenhuma identificada`> |
| Direitos dos titulares | <descrição ou `Não identificado`> | <fonte> | <descrição ou `Nenhuma identificada`> |
| Compartilhamento, operadores e transferências | <descrição ou `Não identificado`> | <fonte> | <descrição ou `Nenhuma identificada`> |
| Segurança e resposta a incidentes | <descrição ou `Não identificado`> | <fonte> | <descrição ou `Nenhuma identificada`> |

## Operação, Auditoria e Resposta a Incidentes
<Registros, monitoramento, auditoria, alertas, gestão de vulnerabilidades, *backup*, continuidade e processos de resposta evidenciados; escreva `Não identificado` quando aplicável.>

## Riscos, Limites e Lacunas
| ID | Item | Categoria | Impacto potencial | Evidência ou motivo |
| --- | --- | --- | --- | --- |
| RSK-01 | <descrição> | Segurança / LGPD / Compliance / Operação | <impacto> | <evidência> |

<Escreva `Nenhum identificado` quando não houver evidência suficiente.>

## Recomendações
| ID | Recomendação macro | Prioridade sugerida | Justificativa | Riscos, limites ou elementos relacionados |
| --- | --- | --- | --- | --- |
| REC-01 | <melhoria macro de segurança ou compliance> | Crítica / Alta / Média / Baixa | <benefício e evidência que a motivam> | RSK-01 / SEC-01 / DAT-01 |

<Inclua recomendações somente quando sustentadas por evidências e riscos ou limites observados. Priorize pelo impacto, exploração potencial, exposição, obrigação de compliance e abrangência. Não detalhe plano de implementação, tecnologias obrigatórias, cronograma ou aconselhamento jurídico. Escreva `Nenhuma recomendação identificada` quando não houver evidência suficiente.>

## Estado Atual de Segurança e Compliance

<Esta seção é a síntese operacional destinada ao contexto de agentes de desenvolvimento. Descreva o estado atual, e não o estado desejado. Use os identificadores das seções anteriores e registre `Não identificado` onde necessário.>

### Fronteiras e Comunicação a Preservar
- <Módulos, fronteiras de confiança, protocolos e exigências de transporte que a manutenção não deve alterar implicitamente.>

### Autenticação e Autorização Vigentes
- **Usuários:** <AUTH-xx, mecanismo, token/sessão, validações e autorização observados.>
- **Service-to-service:** <AUTH-xx, protocolo, credencial/token, validações e permissões observadas.>
- **Segredos e credenciais:** <local lógico, responsável e restrição relevante; sem valores sensíveis.>

### Dados e Compliance Vigentes
- **Dados de clientes:** <existência, categorias e módulos DAT-xx envolvidos, ou `Não identificado`>.
- **LGPD:** <aspectos observados, lacunas e decisões relevantes, ou `Não aplicável: não há evidência de dados de clientes`>.

### Controles e Restrições de Manutenção
- <Controles SEC-xx, invariantes, dependências e requisitos operacionais que devem ser preservados ao alterar a aplicação.>

### Lacunas Conhecidas para Consideração Futura
- <RSK-xx ou `Nenhuma identificada`>.

## Evidências e Rastreabilidade
| Elemento ou conclusão | Fonte | Interpretação |
| --- | --- | --- |
| MOD-01 / SEC-01 / DAT-01 / RSK-01 | `<repositório>:<caminho, módulo, contrato, configuração ou teste>` | <o que a evidência sustenta> |

## Premissas e Decisões
- **Ambiguidade ou divergência:** <informação ausente, ambígua ou conflitante>
- **Decisão:** <interpretação adotada; registre a prevalência do código quando aplicável>
- **Fonte:** <prompt, contexto, documento, código ou raciocínio>

## Controle de Versão
| Versão | Data | Autor | Alteração |
| --- | --- | --- | --- |
| 1.0 | YYYY-MM-DD | Agente | Criação inicial do documento |
```

Use identificadores estáveis `REP-xx`, `MOD-xx`, `AUTH-xx`, `SEC-xx`, `DAT-xx`, `RSK-xx` e `REC-xx`, referenciando-os consistentemente nos diagramas, tabelas, riscos, recomendações, evidências e decisões. Preserve as entradas existentes do histórico de versão e acrescente uma entrada a cada revisão material.

## Autonomia e decisões

Não interrompa a análise por ausência, ambiguidade ou conflito de informações. Para conclusões sobre o estado atual, priorize comportamento e configuração efetivamente demonstrados pelo código; depois, use dados explícitos do prompt, contexto e documentação. Quando fontes não técnicas divergirem do código, documente a divergência e o impacto, sem alterar silenciosamente a conclusão.

Documente primeiro o estado observado da arquitetura de segurança e compliance; riscos e recomendações são complementares. Declare explicitamente controles não identificados, lacunas, limites, dependências, suposições e comunicações ausentes. Relacione cada recomendação aos riscos, limites ou evidências que a sustentam e mantenha sua prioridade como sugestão, não como fato observado. Não classifique a aplicação como “conforme com a LGPD” sem parecer ou evidência jurídica explícita.

## Critérios de conclusão

Considere a documentação concluída somente quando estiver gravada no caminho definido pelo prompt ou, quando ele não existir, apresentada integralmente no output; estiver escrita em português e orientada a segurança, compliance, arquitetura e liderança técnica; delimitar todos os repositórios como uma única aplicação; documentar obrigatoriamente a comunicação entre todos os módulos analisados, incluindo protocolos e mecanismos de autenticação de usuários e *service-to-service*; identificar controles e dados relevantes sem expor segredos ou dados sensíveis; avaliar e documentar LGPD sempre que houver dados de clientes; incluir uma seção final de estado atual de segurança e compliance voltada à manutenção por agentes; incluir riscos e recomendações macro priorizadas e justificadas por evidências; registrar divergências com prevalência explícita do código; ser autocontida, rastreável e utilizável como contexto por agentes de desenvolvimento autônomo; e manter `## Controle de Versão` como última seção.
