# Template — User Stories (Artefato Intermediário)

> Este template é a estrutura oficial do artefato gerado pela skill `features-to-user-stories`.
> O artefato é salvo em `output-files/user-stories-<nome-base>.md` e serve como contrato de revisão humana antes da publicação no Jira.

---

## Cabeçalho de Origem

```markdown
# User Stories — Breakdown

| Campo | Valor |
|-------|-------|
| **Artefato de Features** | `output-files/epics-<nome>.md` |
| **Produto / Módulo** | _[conforme metadados do Features]_ |
| **Número de Features** | _[ex: 3]_ |
| **Número de Stories Geradas** | _[ex: 8]_ |
| **Projeto Jira** | _[ex: ZUPPER]_ |
| **Analista Responsável** | _[nome e username]_ |
| **PO para Aprovação** | _[nome e username]_ |
| **Data de Geração** | _[AAAA-MM-DD]_ |
| **Status do Artefato** | `Estruturado` |

---

## Matriz de User Stories

| # | Feature | F# | Nº Stories | Personas | Must Have | Should Have | Could Have | Jira Story Keys |
|---|---------|---|------------|----------|-----------|-------------|------------|-----------------|
| 1 | _Nome da Feature 1_ | F012 | 3 | viajante,gestor-viagem,admin | 2 | 1 | 0 | _Após publicação_ |
| 2 | _Nome da Feature 2_ | F013 | 2 | viajante,atendente | 2 | 0 | 0 | _Após publicação_ |

---

## User Stories Detalhadas

### Feature F012 — [RF03] Alerta automático de queda de preço de passagem

> **Origem no PRD:** Seção 8 — Requisitos Funcionais  
> **Contexto:** Motor de monitoramento verifica preços periodicamente e notifica o viajante quando o threshold é atingido  
> **Épico Jira:** E001 (Fase 1 — Motor de Alertas de Preço)

#### Story S01

**Título:** [S001] As a viajante I want to receive a price drop notification so that I can book the flight at the right moment

**Persona:** Viajante  
**Jornada:** Configurar alerta, receber notificação, acessar voo  
**MoSCoW:** Must Have  
**Prioridade Jira:** Alta

**Critérios de Aceite:**
- [ ] O viajante consegue criar um alerta informando rota, datas e preço-alvo
- [ ] Quando o preço cai abaixo do threshold, push notification é enviada em até 5 minutos
- [ ] Notificação exibe: rota, preço anterior, novo preço, % de desconto e botão "Ver voo"
- [ ] Ao tocar em "Ver voo", o app abre a busca com parâmetros pré-preenchidos
- [ ] Se push não estiver habilitado, e-mail é enviado como fallback
- [ ] Alerta sem disponibilidade de rota é desativado automaticamente com notificação

**Notas Técnicas:**
- Consulta via GDS/fornecedor com cache para otimizar custo de chamada
- Frequência de verificação a definir com engenharia (sugestão: 15 min)
- Log auditável de cada disparo (timestamp, rota, preço, ação)

**Dependências:**
- ✅ Depende de: Feature F010 (Motor de Monitoramento — deve estar pronto)
- ⚠️ Bloqueada por: nenhuma (pode começar)
- ℹ️ Bloqueia: S02, S03 (dependem de dados de S01)

**Origem Rastreável:**
```
Feature: F012 (ZUPPER-450)
RF: RF03 (Seção 8)
PRD: prd-alerta-preco-passagens.md
Épico: E001
MoSCoW: Must Have → Jira Priority: Alta
```

**Jira Story Key:** `_Após publicação_`

---

#### Story S02

**Título:** [S002] As a gestor de viagem corporativa I want to monitor my team's price alerts so that I can optimize travel costs

**Persona:** Gestor de Viagem Corporativa  
**Jornada:** Consultar alertas ativos e histórico de disparos da equipe  
**MoSCoW:** Must Have  
**Prioridade Jira:** Alta

**Critérios de Aceite:**
- [ ] Painel exibe todos os alertas ativos dos viajantes da empresa
- [ ] Filtros disponíveis: por viajante, rota, período, status (ativo/disparado/expirado)
- [ ] Coluna com economia estimada ao clicar no alerta disparado
- [ ] Tabela com últimos 20 alertas disparados (rota, viajante, queda %, timestamp)
- [ ] Botão "Exportar relatório" gera CSV com dados do período

**Notas Técnicas:**
- Dados derivados dos alertas criados e disparados em S01
- Permissão restrita à role "Gestor Corporativo"

**Dependências:**
- ✅ Depende de: S01 (histórico de alertas)
- ⚠️ Depende de: módulo corporativo ativo no plano do cliente
- ℹ️ Bloqueia: nenhuma

**Origem Rastreável:**
```
Feature: F012 (ZUPPER-450)
RF: RF03 (Seção 8)
PRD: prd-alerta-preco-passagens.md
Épico: E001
MoSCoW: Must Have → Jira Priority: Alta
```

**Jira Story Key:** `_Após publicação_`

---

#### Story S03

**Título:** [S003] As a admin I want to configure alert thresholds and verification frequency so that I can control notification volume

**Persona:** Admin  
**Jornada:** Configurar parâmetros globais de alerta  
**MoSCoW:** Should Have  
**Prioridade Jira:** Média

**Critérios de Aceite:**
- [ ] Painel admin exibe: queda mínima configurável (%), frequência de verificação, limite de alertas por usuário
- [ ] Validação: não permitir frequência menor que 5 minutos
- [ ] Histórico de alterações: quem configurou, quando, valor anterior vs. novo
- [ ] Ao salvar, novo parâmetro é aplicado ao motor sem reinicialização

**Notas Técnicas:**
- Mudanças aplicadas dinamicamente (sem downtime)
- Auditoria completa de todas as alterações de configuração

**Dependências:**
- ✅ Depende de: S01 (motor base deve estar funcional)
- ⚠️ Bloqueia: Performance tuning tasks (futuro)

**Origem Rastreável:**
```
Feature: F012 (ZUPPER-450)
RF: RF03 (Seção 8)
PRD: prd-alerta-preco-passagens.md
Épico: E001
MoSCoW: Should Have → Jira Priority: Média
```

**Jira Story Key:** `_Após publicação_`

---

## Dependências Globais

```
Story               Depende de      Tipo Dependência
────────────────────────────────────────────────────
S01                 F010 (Feature)  Bloqueia (motor de monitoramento deve estar pronto)
S02                 S01, Infra      Sequencial (histórico de alertas + módulo corporativo)
S03                 S01             Sequencial (motor base deve estar pronto)
S04                 S01, S02        Sequencial (depende de dados de alertas)

Resumo de Ordem:
  Ordem 1 (pre-req)  → F010 (motor de monitoramento de preços)
  Ordem 2 (paralelo) → S01 + S03 (implementação do motor e configuração)
  Ordem 3 (paralelo) → S02 + testes de S01
  Ordem 4 (final)    → S04 (features avançadas)
```

---

## Gate de Revisão Humana

> **ANTES DE PUBLICAR:** Analista + PO devem revisar:
> 1. Cada Story é independente e entregável (ou dependência é explícita)?
> 2. AC's são testáveis (não vagos)?
> 3. Personas são distintas (não misturadas em uma Story)?
> 4. Dependencies não têm surpresas?
> 5. Rastreabilidade está completa (PRD→Feature→Story)?

| Campo | Valor |
|-------|-------|
| **Decisão** | `Estruturado` / `Aprovado` / `Ajustar` / `Cancelado` |
| **Aprovado por** | _[nome do Analista]_ |
| **PO Confirmou** | _[nome do PO e data]_ |
| **Data de aprovação** | _[AAAA-MM-DD]_ |
| **Observações** | _[ajustes solicitados, se houver]_ |

---

## Resultado da Publicação

> _Preenchido automaticamente pela skill após execução da Fase 6._

| Feature | Story | Summary | Jira Key | URL | Status |
|---------|-------|---------|----------|-----|--------|
| F012 | S01 | As a viajante I want... | ZUPPER-461 | https://jira.zupper.com.br/browse/ZUPPER-461 | Criado |
| F012 | S02 | As a gestor-viagem... | ZUPPER-462 | https://jira.zupper.com.br/browse/ZUPPER-462 | Criado |
| F012 | S03 | As a admin... | ZUPPER-463 | https://jira.zupper.com.br/browse/ZUPPER-463 | Criado |

**Total criado:** 3 stories  
**Erros:** 0

---

## Histórico do Artefato

| Data | Versão | Ação | Autor |
|------|--------|------|-------|
| _AAAA-MM-DD_ | v1 | Gerado pela skill features-to-user-stories | _[Analista]_ |
```
