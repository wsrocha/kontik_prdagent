---
name: features-to-user-stories
description: >
  Skill para quebrar Features em User Stories estruturadas de produto.
  Realiza análise de personas/jornadas sobre artefato epics-*.md e gera user-stories-*.md
  com gate de aprovação humano (Analista + PO) antes de publicar no Jira.
  Use quando Analytics/Product precisar decompor Features em histórias de usuário executáveis.
license: MIT
compatibility: Requer acesso ao artefato epics-*.md gerado por prd-to-epics-features. Ideal após publicação de Features no Jira.
metadata:
  category: product-analysis
  version: 1.0.0
  language: pt-BR
  use-cases: "user-story-breakdown, product-analysis, granularity-refinement, jira-story-creation"
allowed-tools: Read Write Edit mcp__tasks-toolkit__jira_create_issue mcp__tasks-toolkit__jira_search_issues
---
<!-- Porte Kiro -> Claude Code: allowed-tools original era "readFile readMultipleFiles fsWrite strReplace mcp_mcp-toolkit_tool_create_jira_issue mcp_mcp-toolkit_tool_search_jira_issues". Os nomes de tools MCP são placeholders — dependem do MCP server "tasks-toolkit" (ou equivalente Jira) ser configurado no Claude Code; ajuste o nome exato conforme o server ficar disponível. Ver PORTING-NOTES.md. Nenhum outro conteúdo desta skill foi alterado. -->

# Features to User Stories Skill

Skill especializado em analisar Features de produto e decompô-las em User Stories estruturadas,
seguindo padrões de product analysis e rastreabilidade obrigatória.

---

## Quando Usar Este Skill

- Após Features serem publicadas no Jira (`prd-to-epics-features` concluído)
- Analista precisa quebrar Features em personas × jornadas
- Time prepara backlog para Dev usar com spec-kit
- Necessidade de sincronizar análise de produto com estrutura Jira

## Pré-requisitos

- Artefato `output-files/epics-*.md` finalizado e Features já publicadas no Jira
- MCP `tasks-toolkit` ativo (para publicar Stories)
- Acesso ao projeto Jira para criar Stories com epic_key

---

## Referências Operacionais

| Arquivo | Uso |
|---------|-----|
| `.kiro/steering/features-to-user-stories-orchestrator.md` | Contrato: granularidade, personas, jornadas |
| `.kiro/steering/multi-stage-traceability.md` | Rastreabilidade obrigatória PRD↔Feature↔Story↔Task |
| `.kiro/skills/features-to-user-stories/references/user-stories-template.md` | Template do artefato intermediário |
| `.kiro/skills/features-to-user-stories/references/story-structure-guide.md` | Exemplos e padrões "As a...I want...So that" |

## Steering Files ativos

| Steering | Responsabilidade |
|----------|-----------------|
| `features-to-user-stories-orchestrator.md` | Análise, granularidade, critérios de qualidade |
| `multi-stage-traceability.md` | Rastreabilidade PRD→Code (labels, origin fields) |
| `epics-features-orchestrator.md` | Contrato de Features (origem dos dados) |

---

## Fluxo de Trabalho

### Fase 1 — Discovery e Leitura do Artefato

**Objetivo:** Carregar contexto e preparar análise.

1. Extrair do prompt do Analista tudo que já estiver explícito (caminho do epics-*.md, projeto Jira, etc.).
2. Para cada campo abaixo ainda ausente, perguntar em **um único bloco**:

   ```
   Por favor, confirme as seguintes informações:

   1. Qual o artefato de Features? (ex: output-files/epics-sugestao-inteligente.md)
   2. Qual o Project Key do Jira? (ex: ZUPPER)
   3. Qual PO será contatado para aprovação? (username Jira, ex: thiago.soares)
   4. Há personas predefinidas no projeto? (opcional, ex: viajante,gestor-viagem,atendente)
   5. Há filtro de prioridade MoSCoW? (opcional, ex: Must-Have,Should-Have)
   6. Há labels padrão para todas as Stories? (opcional, ex: sprint-23,product-core)
   ```

3. Ler `output-files/epics-*.md` com foco em:
   - Cabeçalho: Produto, versão, Épicos identificados
   - Cada Feature: título, contexto, Critérios de Aceite, RF origem, prioridade MoSCoW
   - Seção Won't Have: garantir que NÃO quebrar em Stories

4. Confirmar ao Analista: "Artefato carregado. [N] Features encontradas, Epic: [E001]. Pronto para análise."

---

### Fase 2 — Análise de Personas e Jornadas

**Objetivo:** Para cada Feature, identificar combinações personas × jornadas.

#### Para cada Feature:

1. **Discovery Questions** (askers de análise, não para o usuário):
   ```
   a) Quantos tipos de usuários interagem com esta Feature?
   b) Cada tipo tem necessidade/fluxo diferente?
   c) Há operações distintas (read vs write, happy path vs error)?
   d) Alguma necessidade não agregada em uma única história?
   e) Há personas que podem trabalhar independentemente?
   ```

2. **Estruturar Personas Identificadas:**
   ```
   Feature F012: "Sugestão automática de transação"
   
   Personas:
   - Contador (usa sugestão)
   - Gerente Fiscal (monitora acurácia)
   - Admin (configura regras)
   
   Cada persona → 1 User Story
   ```

3. **Identificar Jornadas:**
   ```
   Contador:
     Jornada: "Receber sugestão, validar, aplicar ou descartar"
     Deve estar separada? Sim (fluxo coeso)
     
   Gerente:
     Jornada: "Consultar dashboard de acurácia"
     Deve estar separada? Sim (outra necessidade de valor)
     
   Admin:
     Jornada: "Cadastrar novas regras para motor"
     Deve estar separada? Sim (configuração, não execução)
     
   Resultado: 3 User Stories
   ```

4. **Validar Dependências:**
   ```
   S01 (Contador vê sugestão):
     - Prereq: Motor de Regras (S00, outra Feature?) deve estar funcional
     - Bloqueia: S02, S03 (ambas precisam de dados de S01)
   
   S02 (Gerente vê dashboard):
     - Prereq: S01 (histórico de sugestões)
     - Bloqueia: nenhuma
   
   S03 (Admin configura):
     - Prereq: S01 (motor base)
     - Bloqueia: performance optimization
   ```

---

### Fase 3 — Estruturação de User Stories

**Objetivo:** Criar stories estruturadas no padrão **As a...I want...So that**.

Para cada Story identificada:

1. **Estrutura Obrigatória:**
   ```
   Title: [S###] As a [role] I want [action] so that [benefit]
   
   Exemplo:
   "[S001] As a contador I want to see suggested transaction 
           so that I can validate and apply it quickly"
   ```

2. **Critérios de Aceite (derivados da Feature AC's):**
   ```
   Feature AC: "Sugestão exibida em < 500ms"
   Story AC: "Quando contador salva PV, modal com sugestão 
             aparece em < 500ms no navegador"
   
   Feature AC: "Indicador de confiança [80-100%]"
   Story AC: "Modal exibe confiança % ao lado da sugestão (80%, 95%, etc)"
   ```

3. **Critérios específicos por Story:**
   ```
   S01 AC's:
     - [ ] Modal exibe: transação sugerida + confiança % + botões
     - [ ] Modal pode ser fechado (X)
     - [ ] Ao clicar "Usar", transação é aplicada
     - [ ] Ao clicar "Descartar", Jira Comment é criado com timestamp
   
   S02 AC's:
     - [ ] Dashboard mostra: total de sugestões, aceitas, descartadas
     - [ ] Filtro por período (dia, semana, mês)
     - [ ] Exportar relatório em CSV
   
   S03 AC's:
     - [ ] Tela de cadastro exibe: nome da regra, condição, transação
     - [ ] Validar que condição não duplica regra existente
     - [ ] Ativar/desativar regra (soft delete)
   ```

4. **Notas Técnicas (se houver):**
   ```
   S01:
     "Usar regra de cache da Feature F012 para evitar latência"
     "Considerar offline-first para mobile"
   ```

5. **Dependências Explícitas:**
   ```
   S01:
     - Depende de: Feature F010 (Motor de Regras base)
     - Aviso: Admin deve ter S03 pronto antes (regras deve estar funcional)
   
   S02:
     - Depende de: S01 (histórico de dados)
     - Depende de: Dashboard infra (versão 2+)
   ```

6. **Rastreabilidade Obrigatória:**
   ```
   Feature: [F012] Alerta automático de queda de preço
   Jira Feature Key: ZUPPER-450
   RF: RF03 (Requisito Funcional)
   PRD: prd-alerta-preco-passagens.md
   Épico: E001 (Fase 1 — Motor de Regras)
   Prioridade MoSCoW: Must Have → Jira Priority: Alta
   ```

---

### Fase 4 — Geração do Artefato Intermediário

**Objetivo:** Salvar análise estruturada e revisável.

1. Usar template: `.kiro/skills/features-to-user-stories/references/user-stories-template.md`
2. Preencher:
   - Cabeçalho de origem (qual epics-*.md alimentou)
   - Matriz de User Stories (Feature → Stories quebradas)
   - Seção detalhada por Story (As a... + AC's + Dependencies)
   - Seção de Dependências Globais (grafo)
   - Status: `Estruturado`
   - Coluna "Jira Story Key" vazia (será preenchida após publicação)
3. Salvar em: `output-files/user-stories-<nome-base>.md`
   - Nome base: remover `epics-` do início e `.md` do final
   - Exemplo: `output-files/epics-sugestao-inteligente.md` → `output-files/user-stories-sugestao-inteligente.md`

---

### Fase 5 — Gate de Revisão Humana (OBRIGATÓRIO)

**Objetivo:** Garantir aprovação explícita (Analista + PO) antes de publicação.

1. Apresentar sumário ao Analista:
   ```
   📋 USER STORIES GERADAS — Aguardando revisão

   Features analisadas: [N]
   User Stories criadas: [M]
   Dependências internas: [D1]
   Dependências externas: [D2]

   Artefato salvo em: output-files/user-stories-[nome].md

   Próximas ações:
   1. Revisar cada Story (granularidade OK?)
   2. Validar AC's (testáveis e coerentes?)
   3. Confirmar dependências (nenhuma surpresa?)
   4. Solicitar aprovação do PO
   5. Escolher ação (abaixo)
   ```

2. **Analista escolhe uma ação:**
   ```
   ✅ APROVAR e publicar Stories no Jira
   ✏️  AJUSTAR (editar artefato) e re-analisar
   🚫 CANCELAR (manter mas não publicar)
   ```

3. Se **AJUSTAR**: incorporar feedback, voltar à Fase 3 (restruturar)
4. Se **CANCELAR**: registrar status `Cancelado`, encerrar
5. Se **APROVAR**: 
   - PO confirma aprovação (não apenas Analista)
   - Atualizar status para `Aprovado`
   - Prosseguir para Fase 6

---

### Fase 6 — Publicação no Jira via MCP

**Objetivo:** Criar Stories no Jira com rastreabilidade completa.

#### 6.1 — Preparação

1. Para cada Story aprovada, preparar payload validando:
   - `summary` ≤ 255 caracteres (truncar com `…` se necessário)
   - `issue_type` = `Story` (fixo)
   - `epic_key` = Epic vindo de Feature (ex: ZUPPER-450)
   - `priority` = mapeada de MoSCoW (Must→Alta, Should→Média, Could→Baixa)
   - `labels` incluem obrigatórios: `prd:*`, `rf:*`, `feature:*`, `persona:*`
   - Description carrega seção `## Origin` com rastreabilidade completa

2. Ordenar criação: por Épico, depois por persona (contador → gerente → admin)

#### 6.2 — Criação Sequencial

Para cada Story (ordenado):

```
Criar Story: [S###] As a [role] I want...
  project_key: [ZUPPER]
  issue_type: Story
  summary: [título ≤255]
  description: [template com Origin + AC's + Dependencies]
  epic_key: [ZUPPER-450 — da Feature pai]
  priority: [Alta|Média|Baixa — derivado de MoSCoW]
  assignee: [deixar vazio — Analista ou Dev decidirá]
  labels: [prd:sugestao-transacao,rf:RF03,feature:F012,persona:contador]
```

- Registrar `issue_key` retornado no artefato após cada criação bem-sucedida
- Se erro: registrar e **continuar** com próximas Stories
- Reportar erros no sumário final

#### 6.3 — Atualização do Artefato

1. Preencher coluna "Jira Story Key" para cada Story criada
2. Preencher tabela "Resultado da Publicação" (issue keys + URLs)
3. Atualizar status do artefato para `Publicado`
4. Adicionar entrada no "Histórico do Artefato"
5. Salvar o artefato atualizado

#### 6.4 — Sumário Final

```
✅ PUBLICAÇÃO CONCLUÍDA

Stories criadas com sucesso: [N]
Erros: [M] (ver "Resultado da Publicação")

Links rápidos:
- [ZUPPER-461] Story S01: "As a viajante..." → https://jira.zupper.com.br/browse/ZUPPER-461
- [ZUPPER-462] Story S02: "As a gestor-viagem..." → https://jira.zupper.com.br/browse/ZUPPER-462
- ...

Artefato atualizado: output-files/user-stories-[nome].md

Próximos passos sugeridos:
1. Dev team recebe Stories no Jira
2. Dev executa /speckit.specify em cada Story
3. Criar spec.md, plan.md, tasks.md por Story (paralelo)
4. Iniciar implementação via /speckit.implement
```

---

## Regras de Qualidade

Aplicar em Fase 5 (gate) e antes de publicar (Fase 6):

- Não inventar personas que não existam na Feature
- Não misturar personas distintas em uma Story única
- Não criar Stories sem benefício claro ("So that")
- AC's devem ser testáveis (não vagos)
- Dependências devem ser explícitas (nenhuma surpresa)
- Rastreabilidade PRD→Feature→Story completa
- Cada Story carrega origem no description (obrigatório)
- Labels incluem tags transversais de rastreabilidade

---

## Interações esperadas entre skills

| Trigger | Skill/Hook chamado |
|---------|--------------------|
| Features publicadas no Jira | `features-to-user-stories` pode ser invocado em seguida |
| Edição de `features-to-user-stories-orchestrator.md` | Hook sincroniza este SKILL.md |
| Novo arquivo em `output-files/` | Hook `update-readme-on-file-created` atualiza README |
| Stories publicadas no Jira | Dev recebe para usar com `/speckit.specify` |
