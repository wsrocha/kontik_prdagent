# Orquestrador de Features → User Stories (Steering Normativo)

> **Responsabilidade deste arquivo:** Definir análise de produto, quebra de Features em User Stories, e mapeamento de granularidade.
> O fluxo operacional e steps pertence à skill `features-to-user-stories`.

---

## Escopo do orquestrador

- Garantir que cada Feature seja analisada para identificar **personas distintas** e **jornadas de usuário**.
- Garantir que User Stories sejam estruturadas no formato **As a [role] I want [action] so that [benefit]**.
- Garantir rastreabilidade bidirecional: cada Story referencia sua Feature, cada Feature sabe quantas Stories gerou.
- Garantir que dependências entre Stories sejam explicitadas.
- Garantir gate de aprovação **humano obrigatório** (Analista + PO) antes de publicação.

---

## Entradas mínimas obrigatórias

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `epics_path` | string | Caminho do artefato epics-*.md (ex: `output-files/epics-sugestao-inteligente.md`) |
| `jira_project_key` | string | Chave do projeto Jira (ex: `ZUPPER`, `ZUPPER-CORE`) |
| `po_username` | string | Username Jira do PO para aprovação final (ex: `thiago.soares`) |

### Entradas opcionais

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `analyst_defaults` | object | Estrutura padrão de personas conhecidas no projeto (ex: `{viajante, gestor-viagem, atendente}`) |
| `labels_base` | string | Labels adicionais aplicados a todas as Stories |
| `filter_moscow` | string | Filtrar apenas Stories de prioridade MoSCoW específica (ex: `Must-Have,Should-Have`) |

---

## Hierarquia de Granularidade

```
Feature (nível RF do PRD)
  ├─ análise: "Quantas personas? Quantas jornadas?"
  │
  └── User Story 1 (persona A × jornada 1)
      User Story 2 (persona B × jornada 2)
      ... até N stories
```

### Regras de Granularidade

1. **1 Feature simples** (1-2 Critérios de Aceite, 1 actor) = **1 User Story** (pode ir direto ao Dev)
2. **1 Feature complexa** (5+ Critérios, múltiplos personas/jornadas) = **2-5 User Stories** (sempre quebra)
3. **Uma User Story jamais refere 2+ Features** (violaria rastreabilidade)
4. **Uma User Story jamais une 2+ personas em uma ação** (perde clareza de valor)

---

## Fonte de Dados por Seção do Artefato epics-*.md

| Seção do artefato | O que extrai | Usa para |
|------------------|------------|----------|
| Cabeçalho Feature | Título, descrição, RF origem | Título base da Story |
| Critérios de Aceite | Comportamentos mensuráveis | Quebra em jornadas |
| Regras de Negócio | Constrains técnicas/business | Notas de Story |
| Dependências | Features que bloqueiam | Dependências de Story |

---

## Conversão: Feature → Stories

### Análise Qualitativa

Para cada Feature:

1. **Identificar Personas:**
   - Quem usa isto? (contador, gerente, admin, API client?)
   - Cada persona tem necessidade diferente?

2. **Identificar Jornadas:**
   - Fluxos distintos da Feature?
   - Casos happy path vs error handling?
   - Operações de leitura vs escrita?

3. **Identificar Valor:**
   - Qual o benefício por persona?
   - Pode-se entregar valor sem outras personas?

4. **Identificar Dependências:**
   - Esta Story depende de outra Feature/Story?
   - Outras Stories a bloqueiam?

### Exemplos de Quebra

#### Exemplo 1: Feature Simples → 1 Story
```
Feature: "[RF01] Autenticação única (SSO)"
Critérios: 
  - Login redireciona para SSO
  - Token recebido e armazenado
  - Logout limpa token

Análise:
  - Personas: não distinguem comportamento
  - Jornadas: fluxo linear
  - Decisão: 1 Story suficiente

Story:
  "[S001] Como usuário, quero fazer login via SSO 
           para não precisar gerenciar múltiplas senhas"
```

#### Exemplo 2: Feature Complexa → 3 Stories
```
Feature: "[RF03] Alerta automático de queda de preço de passagem"
Critérios:
  - Motor de monitoramento verifica preços periodicamente
  - Alerta exibido ao viajante no app e por e-mail
  - Gestor de viagem corporativa visualiza alertas da equipe
  - Admin configura limites e frequência de verificação
  - Logs auditáveis de disparos

Análise:
  - Personas distintas: Viajante, Gestor de Viagem, Admin
  - Jornadas: receber alerta (viajante) vs monitorar equipe (gestor) vs configurar (admin)
  - Decisão: 3 Stories

Story S01:
  "[S001] Como viajante, quero receber um alerta quando o preço 
          da passagem cair para poder aproveitar a oferta"
  
Story S02:
  "[S002] Como gestor de viagem corporativa, quero visualizar 
          os alertas de preço ativos da minha equipe para 
          otimizar os custos de viagem"
  
Story S03:
  "[S003] Como admin, quero configurar os limites de queda 
          de preço e a frequência de verificação para 
          controlar o volume de notificações"
```

---

## Critérios de Aceite: Feature → Story

### Mapeamento de Critérios (AC's)

**Feature AC's são técnicos/abstratos:**
```
- Alerta disparado em < 5 minutos após detecção de queda
- Queda mínima configurável (ex: 10%)
- Compatível com LGPD
```

**Story AC's são comportamentais/concretos:**
```
Story S01 AC's:
  - [ ] Quando o preço cai abaixo do threshold configurado, push notification é enviado ao app
  - [ ] Notificação mostra: rota, preço anterior, novo preço, % de desconto e botão "Ver voo"
  - [ ] Ao tocar em "Ver voo", o app abre a busca com os parâmetros pré-preenchidos
  - [ ] Se o viajante não tiver push habilitado, o alerta é enviado por e-mail como fallback
```

---

## Dependências entre Stories

### Grafo de Dependências

Cada Story pode ter:
- **Internal dependencies:** S01 deve estar pronta antes de S02
- **External dependencies:** depende de Story em outro Épico/Feature
- **Technical blockers:** depende de infra/vendor/3rd-party

### Exemplo

```
Story S01: "Como viajante, quero receber alerta de queda de preço"
  └─ Depende de: Feature F001 (Motor de monitoramento de preços pronto)
  └─ Bloqueia: S02, S03 (que consomem dados de S01)

Story S02: "Como gestor de viagem, quero monitorar alertas da equipe"
  └─ Depende de: S01 (histórico de alertas disparados)
  └─ Depende de: Dashboard corporativo
  └─ Bloqueia: nenhuma

Story S03: "Como admin, quero configurar limites de alerta"
  └─ Depende de: S01 (motor base deve estar funcional)
  └─ Bloqueia: Performance tuning tasks
```

---

## Critérios de Qualidade da Quebra

Uma conversão Features → Stories é válida se:

- [ ] Cada Story tem uma **persona claramente definida**
- [ ] Cada Story responde aos 3 "por quês" (As a... I want... so that...)
- [ ] **Nenhuma Story une 2+ personas** em uma ação
- [ ] **Cada Story é entregável independentemente** (ou dependência é explícita)
- [ ] **Critérios de Aceite são testáveis** (não vagos)
- [ ] **Dependências são documentadas** (nenhuma surpresa no board)
- [ ] **Rastreabilidade PRD→Feature→Story completa**
- [ ] Gate humano foi executado (Analista + PO aprovaram)

Se qualquer critério falhar: sinalizar e bloquear publicação até correção.

---

## Canal de Publicação

- **MCP server**: `tasks-toolkit`
- **Tool de criação**: `jira_create_issue`
- **Tipo de issue**: `Story`
- **Mapeamento de campos**: `.kiro/skills/features-to-user-stories/references/story-field-mapping.md`

---

## Artefato Intermediário

- **Localização**: `output-files/user-stories-<nome-base>.md`
- **Template**: `.kiro/skills/features-to-user-stories/references/user-stories-template.md`
- **Status do artefato**: `Descoberta` → `Estruturado` → `Aguardando aprovação` → `Aprovado` → `Publicado`
- **Atualização pós-publicação**: artefato contém todos os Jira Story Keys criados

---

## Referências Normativas deste Orquestrador

| Arquivo | Responsabilidade |
|---------|-----------------|
| `.kiro/steering/multi-stage-traceability.md` | Cadeia de custódia PRD → Code |
| `.kiro/steering/epics-features-orchestrator.md` | Contrato de Feature extraction |
| `.kiro/skills/features-to-user-stories/SKILL.md` | Fluxo operacional de análise + estruturação |
| `.kiro/skills/features-to-user-stories/references/user-stories-template.md` | Template do artefato intermediário |
| `.kiro/skills/features-to-user-stories/references/story-structure-guide.md` | Exemplos e padrões de estruturação |
