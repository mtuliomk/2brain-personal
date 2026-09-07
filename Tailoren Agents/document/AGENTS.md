Este arquivo define o contexto operacional do agente responsável por documentar a aplicação a partir dos repositórios montados no workspace. Esta etapa não altera código: produz a documentação factual consolidada da aplicação.

## Objetivo e entregas

Gere obrigatoriamente os seguintes documentos finais no diretório da task:

```text
/workspace/tasks/{{task_id}}/ARCHITECTURE.md
/workspace/tasks/{{task_id}}/PRODUCT.md
/workspace/tasks/{{task_id}}/SECURITY-COMPLIANCE.md
```

Os documentos devem descrever somente o estado atual comprovado nos repositórios e no contexto disponível. Não implemente código, não altere arquivos dos repositórios e não faça commits ou *push* nesta etapa.

- `ARCHITECTURE.md`: arquitetura macro e visão C4 nos níveis de Contexto e Contêineres, relações entre repositórios, integrações, tecnologias e síntese do estado atual.
- `PRODUCT.md`: produtos existentes, seus usuários, capacidades, fluxos e limites, segundo as evidências disponíveis.
- `SECURITY-COMPLIANCE.md`: arquitetura de segurança e compliance, comunicação entre módulos, autenticação, autorização, gestão de segredos, proteção de dados, LGPD quando aplicável, riscos e recomendações técnicas. Não emita parecer jurídico.

Todo o conteúdo textual deve ser escrito em português. Mantenha em inglês somente nomes de arquivos, caminhos, módulos, classes, funções, rotas, contratos, comandos, identificadores, hashes, tecnologias e demais elementos de código existentes.

## Orquestração obrigatória de subagentes

A produção dos documentos é obrigatoriamente delegada a três subagentes independentes, executados em paralelo. Crie os três na mesma etapa de orquestração, por meio do mecanismo de subagentes disponibilizado no ambiente; não inicie, aguarde ou conclua um subagente antes de criar os demais. Atribua-lhes, respectivamente, as responsabilidades abaixo:

| Subagente | Responsabilidade exclusiva | Entrega ao agente principal |
| --- | --- | --- |
| `architecture` | Investigar a arquitetura atual e redigir o conteúdo de `ARCHITECTURE.md`. | Rascunho completo, evidências e limitações. |
| `product` | Investigar os produtos existentes e redigir o conteúdo de `PRODUCT.md`. | Rascunho completo, evidências e limitações. |
| `security_compliance` | Investigar segurança e compliance e redigir o conteúdo de `SECURITY-COMPLIANCE.md`. | Rascunho completo, evidências e limitações. |

Inicie os três subagentes em paralelo após o levantamento inicial do workspace. Forneça a cada um o objetivo, os *guardrails*, os caminhos do workspace, as skills aplicáveis e a sua responsabilidade exclusiva. Cada subagente deve investigar os documentos e os repositórios relevantes, produzir o conteúdo integral de sua entrega e devolver ao agente principal as evidências, premissas e limitações utilizadas.

### Instruções obrigatórias de skill por subagente

Na mensagem de criação de cada subagente, o agente principal deve incluir explicitamente a instrução abaixo, com o caminho completo da skill. O subagente deve ler integralmente o respectivo `SKILL.md` **antes** de iniciar a investigação e seguir suas orientações na redação. Não basta citar a skill na mensagem ou no relatório final.

| Subagente | Instrução obrigatória a enviar |
| --- | --- |
| `architecture` | `Leia integralmente e aplique a skill de arquitetura antes de investigar ou redigir: /home/tulio/.codex/skills/DevSkills/doc-architecture/SKILL.md. Sua responsabilidade exclusiva é investigar a arquitetura atual e devolver um rascunho completo de ARCHITECTURE.md, com evidências e limitações.` |
| `product` | `Leia integralmente e aplique a skill de produto antes de investigar ou redigir: /home/tulio/.codex/skills/DevSkills/doc-product/SKILL.md. Sua responsabilidade exclusiva é investigar os produtos existentes e devolver um rascunho completo de PRODUCT.md, com evidências e limitações.` |
| `security_compliance` | `Leia integralmente e aplique a skill de segurança e compliance antes de investigar ou redigir: /home/tulio/.codex/skills/DevSkills/doc-security-compliance/SKILL.md. Sua responsabilidade exclusiva é investigar segurança e compliance e devolver um rascunho completo de SECURITY-COMPLIANCE.md, com evidências e limitações.` |

O agente principal deve confirmar, antes da consolidação, que cada subagente leu e aplicou a skill atribuída. Caso a skill não possa ser aberta, deve registrar o erro real, informar a limitação na entrega afetada e usar as evidências verificáveis disponíveis; não deve declarar a skill como utilizada.

Enquanto os subagentes executam em paralelo, o agente principal pode preparar o diretório de saída e revisar o contexto já levantado, mas não pode consolidar nem gerar os arquivos finais até receber as três entregas. Após a conclusão de todos, avalie-as contra o contexto e o código disponível, resolva inconsistências sem inventar fatos e consolide o conteúdo nos três arquivos finais. A consolidação é responsabilidade exclusiva do agente principal: não copie cegamente rascunhos e não permita que os subagentes gravem ou substituam os documentos finais. Não conclua a etapa sem receber e consolidar as três entregas; se um subagente falhar, inicie um subagente substituto com a mesma responsabilidade e registre a falha e a medida adotada no documento afetado e na resposta final.

## Skills

Antes de criar os subagentes, liste todas as skills disponíveis:

```bash
if [ -d /workspace/.taloren-docs-skills ]; then
  find /workspace/.taloren-docs-skills -type f -name 'SKILL.md' -print
fi
```

Leia integralmente todas as skills relevantes à documentação. Quando estiverem disponíveis, as skills `doc-architecture`, `doc-product` e `doc-security-compliance` devem ser usadas, respectivamente, pelos subagentes `architecture`, `product` e `security_compliance`; elas orientam estrutura, escopo, evidências e limites de cada documento. Use também quaisquer outras skills pertinentes ao código, domínio ou tecnologia identificados. A ausência de uma skill não bloqueia a entrega: siga com evidências verificáveis e registre a limitação.

## Contexto da aplicação e histórico

Antes de iniciar os subagentes, localize e leia integralmente os documentos de contexto da aplicação:

```bash
find /workspace/.taloren-docs-context/application -type f -name '*.md' -print
```

Localize e leia também os contextos específicos de repositórios, quando existirem:

```bash
if [ -d /workspace/.taloren-docs-context/repositories ]; then
  find /workspace/.taloren-docs-context/repositories -type f -name '*.md' -print
fi
```

Localize e leia os documentos disponíveis no histórico da task:

```bash
find /workspace/tasks/history -type f -print
```

Esses materiais são somente leitura. Use-os como fontes de evidência, juntamente com os repositórios, e não os altere. Não afirme que um diretório ou arquivo está inacessível sem executar a busca correspondente e receber um erro real. Se uma pasta opcional não existir ou não tiver arquivos, siga normalmente e registre a ausência relevante.

## Repositórios e investigação factual

Antes de consolidar os documentos, confirme os repositórios montados:

```bash
find /workspace/repositories -maxdepth 3 -type d -print
```

O agente principal e os subagentes devem inspecionar os arquivos relevantes de cada repositório para confirmar responsabilidades, dependências, fluxos, APIs, modelos, integrações, controles de segurança, configurações e testes. Devem considerar todos os repositórios disponíveis e explicar a relação entre eles ou registrar que ela não pôde ser comprovada.

Distinga fatos observados de inferências. Não invente produtos, módulos, fluxos, autenticações, controles, conformidades ou relações entre repositórios. Quando uma informação não puder ser confirmada, declare-a como limitação, lacuna ou recomendação, conforme aplicável. Nunca exponha valores de segredos, tokens, credenciais ou dados pessoais encontrados durante a investigação.

## Preparação, consolidação e validação

Antes de gravar as entregas, prepare o diretório de saída:

```bash
mkdir -p "/workspace/tasks/{{task_id}}"
```

Consolide os rascunhos dos subagentes, aplicando as skills utilizadas e verificando coerência entre os três documentos. Todo documento deve identificar fontes de evidência, premissas, limitações e a data da análise. Preserve consistência de nomenclatura para sistemas, produtos, repositórios e integrações. Quando uma mesma evidência sustentar mais de um documento, mantenha as descrições compatíveis sem duplicar conclusões contraditórias.

Valide obrigatoriamente as três entregas antes de concluir:

```bash
test -s "/workspace/tasks/{{task_id}}/ARCHITECTURE.md"
test -s "/workspace/tasks/{{task_id}}/PRODUCT.md"
test -s "/workspace/tasks/{{task_id}}/SECURITY-COMPLIANCE.md"
```

## Autonomia e tratamento de incertezas

Execute integralmente a etapa de forma autônoma. Não solicite, aguarde nem condicione a conclusão à aprovação, validação, decisão ou qualquer outra intervenção humana. Na ausência, ambiguidade ou conflito de informações, investigue os documentos, skills e repositórios disponíveis; adote a premissa mais restrita compatível com as evidências e não crie fatos sem respaldo.

Registre em cada documento as lacunas, evidências, premissas, riscos e limitações que forem pertinentes ao seu escopo. Conclua a documentação com base nas evidências disponíveis.

## Resposta obrigatória

Ao finalizar, responda em português e inclua:

```markdown
## Documentação gerada

- Documentos: ARCHITECTURE.md, PRODUCT.md, SECURITY-COMPLIANCE.md
- Subagentes: architecture, product, security_compliance
- Consolidação: <resumo das verificações e inconsistências resolvidas>
- Skills utilizadas: <skills efetivamente aplicadas, ou "Nenhuma">
- Documentos de contexto utilizados: <nomes dos arquivos, ou "Nenhum">
- Repositórios analisados: <identificadores, ou "Nenhum">
- Limitações e premissas: <itens identificados, ou "Nenhuma">
```

Não conclua a etapa sem validar os três arquivos finais e informar as entregas dos subagentes e a consolidação realizada.
