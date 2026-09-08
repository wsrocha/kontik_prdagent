# Orquestrador de Épicos e Features (Steering Normativo)

> **Responsabilidade deste arquivo:** Definir o contrato, hierarquia canônica, entradas mínimas e critérios de qualidade para decompor um PRD em Épicos e Features Jira.
> O fluxo operacional e os passos de execução pertencem à skill `prd-to-epics-features`.

---

## Escopo do orquestrador

- Garantir hierarquia canônica **Épico ▶ Feature** ao decompor o PRD.
- Garantir rastreabilidade bidirecional: todo artefato Jira deve ter origem mapeada ao PRD.
- Garantir gate de revisão humana **obrigatório** antes de qualquer publicação no Jira.
- Garantir aplicação correta do mapeamento PRD → campos Jira.
- Garantir publicação automática em duas etapas: criação de Épicos e, em seguida, criação de Features vinculadas aos Épicos.

---

## Entradas mínimas obrigatórias

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `prd_path` | string | Caminho do PRD de entrada (ex: `output-files/prd-*.md`) |
| `jira_project_key` | string | Chave do projeto Jira (ex: `ZUPPER`, `ZUPPER-CORE`) |

### Entradas opcionais

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `assignee` | string | Username Jira para atribuição dos issues (ex: `thiago.soares`). Usar quando PO quer atribuir a si mesmo. |
| `epic_keys_map` | map(string→string) | Mapeamento Épico PRD → chave Jira Epic existente (sobrescreve criação automática quando informado) |
| `entrega_id` | string | ID da Entrega no Jira (campo `customfield_10323`), quando obrigatório no projeto |
| `labels_base` | string | Labels adicionais separadas por vírgula aplicadas a todos os issues |

### Nota sobre Autenticação

Autenticação no MCP (`X-Zupper-Username` header) é **implícita** e gerenciada automaticamente pelo agente Kiro.  
O campo `assignee` (opcional) é apenas para indicar quem deve ser atribuído os issues, não para autenticação.

---

## Hierarquia canônica

```
PRD
└── Épico  (representa Módulo ou Fase do produto)
    └── Feature  (representa Requisito Funcional refinado)
```

### Regras de hierarquia

1. **Épico** = agrupador de contexto. Deriva de Seção 16 (fases do plano de execução) como fonte primária. Fallback: Seção 6 (módulos da estrutura do produto). Fallback final: agrupar RFs da Seção 8 por tema funcional.
2. **Feature** = entrega executável. Deriva de cada RF da Seção 8 com prioridade MoSCoW aplicada.
3. Toda Feature deve ter **exatamente um** Épico pai.
4. Um Épico sem Features não deve ser publicado.
5. Stories são **fora de escopo** nesta versão — não criar sub-níveis abaixo de Feature.

---

## Fonte de dados por seção do PRD

| Seção PRD | O que extrai | Nível gerado |
|-----------|-------------|--------------|
| Seção 16 — Plano de Execução | Fases com entregáveis | Épicos (fonte primária) |
| Seção 6 — Estrutura do Produto | Módulos com funcionalidades | Épicos (fallback) |
| Seção 8 — Requisitos Funcionais | RFs com MoSCoW e critérios | Features |
| Seção 3 — Objetivos de Negócio | Contexto e valor | Descrição dos Épicos |
| Seção 1 — Metadados | Produto, versão, PO | Metadados dos issues |

---

## Regras de filtragem por MoSCoW

| Prioridade MoSCoW | Ação no breakdown | Prioridade Jira |
|-------------------|-------------------|-----------------|
| Must Have | Incluir | Alta |
| Should Have | Incluir | Média |
| Could Have | Incluir (marcado como opcional) | Baixa |
| Won't Have | Documentar no artefato, **NÃO publicar** no Jira | — |

---

## Protocolo de publicação automática

O tool `jira_create_issue` deve suportar criação de `Epic` e `Feature`.

**Ordem obrigatória:**
1. Criar todos os Épicos identificados no breakdown.
2. Registrar `issue_key` e URL de cada Épico no artefato intermediário.
3. Criar as Features vinculadas via `epic_key`.
4. Atualizar o artefato com `issue_key` e URL de cada Feature.

**Fallback seguro (resiliência):**
Se a criação automática de Epic falhar por limitação temporária do ambiente, interromper a publicação e solicitar ao PO as chaves de Épicos criados manualmente para retomar a criação das Features.

---

## Critérios de qualidade do breakdown

O breakdown é considerado válido para publicação se:

- [ ] Cada Feature tem título claro e acionável (≤ 255 caracteres).
- [ ] Cada Feature tem descrição com critérios de aceite derivados do PRD.
- [ ] Cada Feature tem RF de origem mapeado.
- [ ] Cada Feature tem prioridade MoSCoW mapeada para prioridade Jira.
- [ ] Cada Feature tem Épico pai definido.
- [ ] Cada Épico tem `issue_key` Jira após publicação (ou mapeamento pré-existente válido).
- [ ] Nenhuma Feature de prioridade "Won't Have" está marcada para publicação.
- [ ] O gate humano foi executado e a aprovação foi registrada no artefato.

Se qualquer critério falhar: sinalizar e bloquear publicação até correção.

---

## Canal de publicação

- **MCP server**: `tasks-toolkit`
- **Tool de criação**: `jira_create_issue`
- **Guia de referência**: consultar documentação interna do Zupper sobre integração Jira
- **Mapeamento de campos**: `.kiro/skills/prd-to-epics-features/references/jira-field-mapping.md`

---

## Artefato intermediário

- **Localização**: `output-files/epics-<nome-base-do-prd>.md`
- **Template**: `.kiro/skills/prd-to-epics-features/references/epics-features-template.md`
- **Status do artefato**: `Aguardando revisão` → `Aprovado` → `Publicado`
- **Atualização pós-publicação**: o artefato deve conter todos os `issue_key` e URLs retornados pelo MCP.

---

## Referências normativas deste orquestrador

| Arquivo | Responsabilidade |
|---------|-----------------|
| `.kiro/steering/multi-stage-traceability.md` | Cadeia de custódia PRD → Code (Origin fields, labels) |
| `.kiro/steering/prd-writing-rules.md` | Estrutura do PRD de entrada (18 seções) |
| `.kiro/steering/prd-guardrails.md` | Guardrails anti-alucinação (aplicar na leitura do PRD) |
| `.kiro/skills/prd-to-epics-features/SKILL.md` | Fluxo operacional completo |
| `.kiro/skills/prd-to-epics-features/references/jira-field-mapping.md` | Campos Jira e mapeamento |
| `.kiro/skills/prd-to-epics-features/references/epics-features-template.md` | Template do artefato |
