# Template — Épicos e Features (Artefato Intermediário)

> Este template é a estrutura oficial do artefato gerado pela skill `prd-to-epics-features`.
> O artefato é salvo em `output-files/epics-<nome-base-do-prd>.md` e serve como contrato de revisão humana antes da publicação no Jira.

---

## Cabeçalho de Origem

```markdown
# Breakdown: Épicos e Features

| Campo | Valor |
|-------|-------|
| **PRD de origem** | `output-files/<nome-do-prd>.md` |
| **Produto / Módulo** | _[conforme metadados do PRD]_ |
| **Tipo de entrega** | _[ex: Nova jornada / Nova feature / Melhoria]_ |
| **Versão do PRD** | _[ex: v0.1]_ |
| **PO responsável** | _[nome e username Jira]_ |
| **Projeto Jira** | _[ex: ZUPPER]_ |
| **ID de Entrega Jira** | _[ex: 59567 — deixar em branco se não aplicável]_ |
| **Data de geração** | _[AAAA-MM-DD]_ |
| **Status do artefato** | `Aguardando revisão` |

---

## Sumário de Épicos

| # | Épico | Nº de Features | Must Have | Should Have | Could Have | Won't Have | Jira Epic Key |
|---|-------|---------------|-----------|-------------|------------|------------|---------------|
| 1 | _Nome do Épico_ | 0 | 0 | 0 | 0 | 0 | _Após publicação ou pré-existente_ |

---

## Épicos e Features

### Épico 1 — [Nome do Épico]

> **Origem no PRD:** _Seção X — [nome da seção]_
> **Contexto:** _Resumo do objetivo deste épico, extraído do PRD (1–2 frases)._
> **Jira Epic Key:** `_a preencher automaticamente na publicação_`

#### Features

| # | Título da Feature | RF Origem | MoSCoW | Prioridade Jira | Critérios de Aceite (resumo) | Labels | Issue Key Jira |
|---|-------------------|-----------|--------|-----------------|------------------------------|--------|----------------|
| 1.1 | _Título da Feature_ | RF01 | Must Have | Alta | _Condições mensuráveis derivadas do RF_ | _ex: backend,api_ | _Após publicação_ |
| 1.2 | _Título da Feature_ | RF02 | Should Have | Média | _Condições mensuráveis_ | _ | _Após publicação_ |

---

### Épico 2 — [Nome do Épico]

> **Origem no PRD:** _Seção X — [nome da seção]_
> **Contexto:** _Resumo do objetivo deste épico._
> **Jira Epic Key:** `_a preencher automaticamente na publicação_`

#### Features

| # | Título da Feature | RF Origem | MoSCoW | Prioridade Jira | Critérios de Aceite (resumo) | Labels | Issue Key Jira |
|---|-------------------|-----------|--------|-----------------|------------------------------|--------|----------------|
| 2.1 | _Título da Feature_ | RF05 | Must Have | Alta | _Condições mensuráveis_ | _ | _Após publicação_ |

---

## Won't Have (não publicar no Jira)

> Features com prioridade `Won't Have` ficam documentadas aqui para rastreabilidade.
> **Não devem ser criadas no Jira nesta versão.**

| # | Título | RF Origem | Justificativa |
|---|--------|-----------|---------------|
| W1 | _Nome da feature descartada_ | RF09 | _Motivo do descarte (ex: fora do MVP, depende de roadmap futuro)_ |

---

## Gate de Revisão Humana

> **ANTES DE PUBLICAR:** O PO deve:
> 1. Revisar todos os Épicos e Features acima.
> 2. Confirmar se os Épicos serão criados automaticamente ou se deve usar `epic_keys_map` pré-existente.
> 3. Confirmar ou ajustar títulos, critérios de aceite e prioridades.
> 4. Registrar a aprovação abaixo.

| Campo | Valor |
|-------|-------|
| **Decisão** | `Aguardando` / `Aprovado` / `Ajustar` / `Cancelado` |
| **Aprovado por** | _[nome do PO]_ |
| **Data de aprovação** | _[AAAA-MM-DD]_ |
| **Observações** | _[ajustes solicitados, se houver]_ |

---

## Resultado da Publicação

> _Preenchido automaticamente pela skill após execução da Fase 5._

| Épico | Feature | Issue Key | URL | Status |
|-------|---------|-----------|-----|--------|
| _Épico 1_ | _Feature 1.1_ | _ZUPPER-XXX_ | _https://jira.zupper.com.br/browse/ZUPPER-XXX_ | _Criado_ |

**Total criado:** 0 issues  
**Erros:** Nenhum

---

## Histórico do Artefato

| Data | Versão | Ação | Autor |
|------|--------|------|-------|
| _AAAA-MM-DD_ | v1 | Gerado pela skill prd-to-epics-features | _[PO]_ |
```
