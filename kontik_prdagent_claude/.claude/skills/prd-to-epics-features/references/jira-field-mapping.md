# Mapeamento de Campos: PRD → Jira

> Referência operacional para a skill `prd-to-epics-features`.
> Baseado no guia oficial de integração Jira do Zupper (consultar documentação interna).

---

## Capacidade de criação via MCP

> O tool `jira_create_issue` do `tasks-toolkit` deve suportar `issue_type: Epic` e `issue_type: Feature` neste fluxo.
>
> O skill deve publicar em duas etapas: primeiro Epics, depois Features vinculadas por `epic_key`.
>
> Em caso de indisponibilidade temporária para Epic no ambiente, aplicar fallback com `epic_keys_map` fornecido pelo PO.

---

## Hierarquia de publicação

```
Jira Epic  ← criado pela skill via jira_create_issue (issue_type: Epic)
└── Feature  ← criado pela skill via jira_create_issue (issue_type: Feature + epic_key)
```

---

## Mapeamento: Épico PRD → Parâmetros `jira_create_issue`

| Campo do artefato (Épico) | Parâmetro Jira | Obrigatório | Notas |
|---------------------------|----------------|-------------|-------|
| Nome do Épico | `summary` | ✅ Sim | Máx 255 caracteres |
| Contexto / Objetivo do Épico | `description` | ❌ Não | Usar contexto das seções 3, 6 e 16 do PRD |
| `Epic` (fixo) | `issue_type` | ✅ Sim | Sempre `"Epic"` |
| Projeto Jira | `project_key` | ✅ Sim | Ex: `"ZUPPER"` |
| Prioridade consolidada do Épico | `priority` | ❌ Não | Recomenda-se `Média` por default |
| PO username | `assignee` | ❌ Não | Ex: `"thiago.soares"` |
| Labels | `labels` | ❌ Não | Ex: `"prd,epic,produto-x"` |
| ID de Entrega | `entrega_id` | ❌ Não | `customfield_10323` quando exigido |
| Jira Epic Key | retorno `issue_key` | — | Persistir no artefato para vincular Features |

---

## Mapeamento: Feature PRD → Parâmetros `jira_create_issue`

| Campo do artefato (Feature) | Parâmetro Jira | Obrigatório | Notas |
|-----------------------------|----------------|-------------|-------|
| Título da Feature | `summary` | ✅ Sim | Máx 255 caracteres. Prefixar com RF de origem se útil (ex: `[RF01] Título`) |
| Descrição / Critérios de Aceite | `description` | ❌ Não | Usar template: `## Contexto\n...\n## Critérios de Aceite\n- [ ] ...` |
| `Feature` (fixo) | `issue_type` | ✅ Sim | Sempre `"Feature"` para este nível |
| Projeto Jira (da entrada) | `project_key` | ✅ Sim | Ex: `"ZUPPER"`, `"ZUPPER-CORE"` |
| Jira Epic Key do Épico pai | `epic_key` | ✅ Sim | Deve vir do Epic criado (ou de `epic_keys_map` pré-existente) |
| Prioridade Jira (mapeada de MoSCoW) | `priority` | ❌ Não | Ver tabela de mapeamento MoSCoW → Jira abaixo |
| PO username | `assignee` | ❌ Não | Ex: `"thiago.soares"` |
| Labels (base + contexto) | `labels` | ❌ Não | Formato: `"label1,label2"` sem espaços |
| ID de Entrega | `entrega_id` | ❌ Não | `customfield_10323` — obrigatório em alguns projetos |
| RF Origem | — | — | Não é campo Jira; constar na descrição da Feature |

---

## Mapeamento: MoSCoW → Prioridade Jira

| MoSCoW | Prioridade Jira | Quando usar |
|--------|-----------------|-------------|
| Must Have | `Alta` | Essencial ao MVP |
| Should Have | `Média` | Importante mas contornável |
| Could Have | `Baixa` | Desejável se houver capacidade |
| Won't Have | — | **Não publicar no Jira** (documentar no artefato) |

---

## Template de descrição recomendado para Features

```markdown
## Contexto

[Problema ou necessidade que esta Feature resolve — derivado da Seção 3 e 8 do PRD]

## Requisito de Origem

RF: [código do RF, ex: RF03]  
Seção PRD: [ex: 8 — Requisitos Funcionais]  
PRD: [nome do arquivo PRD de origem]

## Critérios de Aceite

- [ ] [Critério 1 — mensurável e testável]
- [ ] [Critério 2]
- [ ] [Critério N]

## Regras de Negócio

[Regras extraídas do campo "Regras de Negócio" do RF no PRD]

## Dependências

[Dependências internas ou externas, conforme Seção 13 do PRD]
```

---

## Exemplo de chamada `jira_create_issue` para uma Feature

```json
{
  "project_key": "ZUPPER",
  "summary": "[RF03] Alerta automático de queda de preço de passagem",
  "issue_type": "Feature",
  "description": "## Contexto\nO sistema deve notificar o viajante quando o preço de uma rota monitorada cair abaixo do threshold configurado...\n\n## Critérios de Aceite\n- [ ] Alerta disparado em até 5 minutos após detectar queda\n- [ ] Notificação enviada por push e e-mail (fallback)\n\n## Requisito de Origem\nRF: RF03\nPRD: output-files/prd-alerta-preco-passagens.md",
  "priority": "Alta",
  "epic_key": "ZUPPER-50",
  "assignee": "thiago.soares",
  "labels": "prd-alerta-preco,busca-passagens,notificacoes",
  "entrega_id": "59567"
}
```

---

## Exemplo de chamada `jira_create_issue` para um Epic

```json
{
  "project_key": "ZUPPER",
  "summary": "Fase 1 - Motor de Alertas de Preço",
  "issue_type": "Epic",
  "description": "## Contexto\nCriar a base do motor de monitoramento de preços de passagens para disparo automático de alertas...\n\n## Origem PRD\nSeção 16 - Plano de Execução\nPRD: output-files/prd-alerta-preco-passagens.md",
  "priority": "Média",
  "assignee": "thiago.soares",
  "labels": "prd,epic,busca-passagens",
  "entrega_id": "59567"
}
```

---

## Validações que o skill deve aplicar antes de chamar o MCP

1. `summary` ≤ 255 caracteres — truncar com `...` e avisar se ultrapassar.
2. `project_key` não vazio — bloquear se ausente.
3. Para Épicos: `issue_type` deve ser `Epic`; para Features: `issue_type` deve ser `Feature`.
4. `epic_key` formato válido `PROJETO-NNN` — obrigatório para Features e validado por regex.
5. `entrega_id` — incluir apenas se fornecido pelo PO (não inventar).
6. `priority` — usar apenas: `"Baixa"`, `"Média"`, `"Alta"`, `"Crítica"`.
7. `labels` — sem espaços, separadas por vírgula. Converter espaços em hífens automaticamente.
8. Features com MoSCoW `Won't Have` — não incluir na chamada MCP, apenas registrar no artefato.

---

## Referências

- Guia oficial: consultar documentação interna do Zupper sobre integração Jira
- Steering deste skill: `.kiro/steering/epics-features-orchestrator.md`
- Tool MCP: `mcp_mcp-toolkit_tool_create_jira_issue`
