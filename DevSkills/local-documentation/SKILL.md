---
name: local-documentation
description: Produzir uma extração factual e compartilhada do código para fluxos de documentação, reutilizável por agentes que auditarão documentos depois. Use para mapear estrutura, interfaces, fluxos e dependências de um repositório sem viés de documento específico. Não use para auditar, comparar ou editar documentação existente.
---

# Extração compartilhada para documentação

Produza uma única leitura factual do repositório, reutilizável por todos os agentes de documentação de uma mesma rodada. O artefato reduz leituras redundantes e garante que os consumidores trabalhem sobre o mesmo *snapshot* de código. Esta skill extrai evidências genéricas do repositório; documentos relacionados ao assunto são contexto de investigação, não fonte de fatos sobre o comportamento atual do código.

Não interprete se documentação existente está correta, desatualizada ou incompleta. Não sugira edições nem compare o código com qualquer documentação. O único arquivo que esta skill pode criar ou atualizar é o artefato único de evidências solicitado; não altere código de produção, testes, configuração funcional, infraestrutura, dependências, dados ou Git.

## Checklist pré-execução

- [ ] Confirmar que o objetivo é produzir evidência genérica e compartilhada, não auditar ou atualizar um documento específico.
- [ ] Registrar o *snapshot* inicial: repositório, `HEAD` ou revisão disponível, branch, estado do *worktree* e arquivos preexistentes relevantes.
- [ ] Definir o destino do artefato de evidências quando ele for informado. Sem um destino, apresentar um único artefato completo no resultado e não gravar arquivos.
- [ ] Antes de extrair, pesquisar na *workspace* documentos relacionados ao assunto do pedido por caminhos, nomes e termos relevantes; registrar os arquivos encontrados ou a ausência deles.
- [ ] Considerar os documentos encontrados como contexto para terminologia, escopo de investigação e pontos técnicos a verificar, sem usá-los como evidência do estado atual do código.
- [ ] Delimitar as fontes técnicas de evidência: árvore de código, manifestos, configurações, módulos de entrada, contratos, testes e automações.
- [ ] Definir explicitamente a **Definition of Done (DoD)** antes de extrair. Ela deve conter a cobertura verificável de estrutura, interfaces públicas, fluxos observáveis, dependências e limitações estáticas, além da consistência com o *snapshot* registrado.
- [ ] Associar cada item da DoD a evidências factuais com caminhos, símbolos, configurações ou comandos observados.

## Checklist pós-execução — validação da DoD

Revise a DoD definida no pré-execução antes de encerrar. Para cada critério aplicável, registre evidência e o status **concluído**, **não aplicável** (com motivo) ou **pendente/bloqueado**. Considere a extração concluída somente quando:

- [ ] estrutura, interfaces, fluxos e dependências definidos na DoD foram extraídos por categoria;
- [ ] cada registro contém uma referência factual ao código ou à configuração que o sustenta;
- [ ] documentos relacionados ao assunto foram pesquisados na *workspace*, considerados como contexto e registrados sem serem tratados como evidência de código;
- [ ] o artefato não contém comparação com documentação, opinião, recomendação ou proposta de edição;
- [ ] limitações de inferência estática foram omitidas ou marcadas como incertas;
- [ ] o *snapshot* permanece consistente. Se a revisão ou arquivos relevantes mudarem durante a extração, a leitura foi reiniciada ou a divergência foi registrada;
- [ ] nenhum arquivo fora do artefato de evidências foi alterado.

## Protocolo de extração

### 0. Descoberta de documentos de contexto

Antes de analisar o código, use os termos do pedido, os nomes de módulos e a organização da *workspace* para localizar documentos relacionados ao assunto. Considere-os para alinhar terminologia, delimitar o que investigar e identificar interfaces ou fluxos que exigem verificação no código. Registre cada caminho consultado no artefato como contexto documental, ou registre `Nenhum identificado`.

Não extraia fatos de código a partir desses documentos, não avalie sua correção e não os compare com os resultados. Todo fato nas categorias técnicas deve ter evidência própria em código, configuração, manifesto, teste ou automação.

### 1. Estrutura

Mapeie módulos, pacotes, pontos de entrada e organização de diretórios que sejam relevantes para entender a arquitetura. Registre caminhos e símbolos observados; não converta esse inventário em avaliação arquitetural.

### 2. Interfaces públicas

Extraia, conforme existirem, APIs expostas, rotas, contratos entre módulos, esquemas, comandos de CLI e outros pontos públicos de integração. Inclua a origem, a interface, o destino ou consumidor identificável e a evidência correspondente.

### 3. Fluxos observáveis

Extraia como dados e chamadas percorrem os módulos mapeados, somente na medida em que isso for observável ou inferível estaticamente. Informe origem, destino, mecanismo, dados ou chamada e grau de certeza. Não preencha lacunas com comportamento suposto de ambiente, runtime ou dependência externa.

### 4. Dependências e configuração

Extraia bibliotecas externas e versões, integrações com serviços externos, variáveis de ambiente e configurações que afetem o comportamento. Registre de onde cada informação foi obtida e marque como incerto o impacto que não puder ser inferido apenas pelo repositório.

## Artefato de evidências

Estruture a saída em um único documento factual. Use tabelas, listas e referências técnicas; evite prosa interpretativa. Preserve esta estrutura, omitindo categorias sem evidência apenas quando registrar o motivo:

```markdown
# Evidências Compartilhadas — <repositório>

## Snapshot
- **Revisão:** `<hash ou Não identificado>`
- **Branch:** `<nome ou Não identificado>`
- **Estado do worktree:** `<limpo / alterações preexistentes>`
- **Momento da extração:** `<data e hora>`

## Documentos de Contexto Consultados
| Caminho | Relação com o assunto | Uso na investigação |
| --- | --- | --- |

<Registre `Nenhum identificado` quando aplicável. Estes documentos não são evidência de comportamento do código.>

## Estrutura
| Caminho ou módulo | Tipo | Ponto de entrada ou símbolo | Evidência |
| --- | --- | --- | --- |

## Interfaces Públicas
| Origem | Interface | Destino ou consumidor | Tipo | Evidência |
| --- | --- | --- | --- | --- |

## Fluxos Observáveis
| Origem | Destino | Mecanismo ou chamada | Dados ou evento | Certeza estática | Evidência |
| --- | --- | --- | --- | --- | --- |

## Dependências e Configuração
| Elemento | Versão ou valor observado | Finalidade observada | Origem da evidência | Certeza estática |
| --- | --- | --- | --- | --- |

## Limitações e Incertezas Estáticas
| Parte não inferida | Motivo | Tratamento |
| --- | --- | --- |
```

Não introduza recomendações, qualificação de qualidade, comparação com documentos ou linguagem que conclua intenção não sustentada pelas evidências. A responsabilidade de interpretar e comparar os registros para cada documento é do agente consumidor.

## Decisões assumidas

Ao final do artefato, registre as partes do código que não puderam ser inferidas estaticamente com confiança — por exemplo, fluxos dependentes de configuração externa não versionada. Marque-as como omitidas ou incertas e informe a fonte da limitação. Não transforme uma lacuna em hipótese para completar a evidência.

## Resultado

Ao concluir, responda em português com:

- identificação do *snapshot* e destino do artefato;
- categorias extraídas e fontes técnicas percorridas;
- documentos relacionados ao assunto encontrados na *workspace* e como foram considerados como contexto, sem avaliá-los ou editá-los;
- limitações e incertezas estáticas registradas;
- confirmação de que nenhum arquivo fora do artefato de evidências foi modificado.
