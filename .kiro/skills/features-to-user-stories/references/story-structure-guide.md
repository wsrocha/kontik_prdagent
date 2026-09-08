# Guia de Estruturação de User Stories

> Referência operacional para estruturar histórias de usuário no padrão **"As a...I want...So that"** com qualidade e rastreabilidade.

---

## Padrão Obrigatório: As a...I want...So that

```
As a [role/persona]
I want [action/capability]
So that [benefit/value]
```

### Exemplos Corretos

```
✅ As a contador
  I want to see the suggested transaction
  So that I can validate and apply it quickly

✅ As a gerente fiscal
  I want to monitor suggestion accuracy in a dashboard
  So that I can identify patterns and improve rules

✅ As a admin
  I want to register new fiscal classification rules
  So that the engine adapts to business changes
```

### Exemplos Incorretos

```
❌ "Implementar sugestão de transação"
   (não tem As a... foco em técnico, não em valor)

❌ "Contador vê transação?"
   (pergunta, não afirmação; falta benefício)

❌ "Como sistema, processar regra de transação"
   (role é sistema, não persona; isso é task, não story)

❌ "Como admin, eu quero que o contador veja sugestão"
   (mistura 2 personas; admin não vê sugestão, contador é quem precisa)
```

---

## Mapeamento: Feature AC's → Story AC's

### Padrão de Conversão

**Feature (abstrato/técnico):**
```
AC: "Sugestão exibida em < 500ms"
```

**Story (concreto/comportamental):**
```
AC: "Quando contador salva um PV e clica em [buscar sugestão],
     a modal com a sugestão aparece em < 500ms"
```

### Mais Exemplos

| Feature AC | Story AC |
|-----------|----------|
| "Indicador de confiança [80-100%]" | "Modal exibe % ao lado da sugestão (ex: 95%)" |
| "Compatível com LGPD" | "Nenhum dado sensível é exibido sem mascaramento" |
| "Auditável" | "Cada ação do usuário (aceitar/descartar) é registrada com timestamp" |
| "Performático" | "Lista de 100 sugestões carrega em < 2s" |

---

## Diferenciação: Quando Quebrar em Múltiplas Stories

### ✅ QUEBRA em múltiplas Stories

**Caso 1: Personas Distintas**
```
Feature: Dashboard de Sugestões

❌ Story Ruim:
"As a contador/gerente/admin
 I want to use the dashboard
 So that I manage suggestions"

✅ Stories Boas:
"As a contador
 I want to see my recent suggestions
 So that I validate them quickly"

"As a gerente  
 I want to monitor accuracy metrics
 So that I identify patterns"

"As a admin
 I want to configure business rules
 So that the engine adapts"
```

**Caso 2: Fluxos Independentes**
```
Feature: Notificação de Sugestão

❌ Story Ruim:
"As a contador
 I want to receive and act on suggestions
 So that I process them fast"

✅ Stories Boas:
"As a contador
 I want to receive a notification when suggestion is available
 So that I don't miss it"

"As a contador
 I want to accept or reject the suggestion
 So that I maintain control"
```

### ❌ NÃO QUEBRA (mantém 1 Story)

**Caso 1: Fluxo Linear Coeso**
```
Feature: Login via SSO

Story Suficiente:
"As a user
 I want to login via SSO
 So that I don't manage multiple passwords"

(Não quebra: fluxo é linear, 1 persona, 1 jornada)
```

**Caso 2: Variações, Não Personas**
```
Feature: Aplicar Suggested Transaction

❌ Quebra Errada:
"As a contador, I want to apply suggestion (Story 1)
 As a contador, I want to reject suggestion (Story 2)"

✅ Melhor (1 Story com AC's):
"As a contador
 I want to review suggestion and choose to apply or reject
 So that I maintain control"

AC's:
  - [ ] Can apply suggestion
  - [ ] Can reject suggestion with reason (optional)
```

---

## Estrutura Completa de AC (Acceptance Criteria)

### Componentes de um AC Bem Estruturado

```
Given [contexto/pré-condição]
When [ação do usuário]
Then [resultado esperado]

Example:
Given contador tem PV aberto com items
When contador clica em [sugerir transação]
Then sistema exibe modal com transação sugerida + confiança %
```

### Exemplo Completo

```markdown
**Story:** As a contador I want to see suggested transaction so that I can validate it quickly

**Critérios de Aceite:**

1. Given contador tem um PV aberto
   When contador clica em [Sugerir Transação]
   Then sistema exibe modal com:
        - Transação sugerida (ex: 5102)
        - Confiança % (ex: 95%)
        - Botões "Usar" e "Descartar"

2. Given modal está exibida
   When contador clica "Usar"
   Then transação é aplicada ao PV instantaneamente
      AND modal fecha
      AND PV headers updated

3. Given modal está exibida
   When contador clica "Descartar"
   Then Jira comment é criado com:
        - Timestamp
        - Username do contador
        - Campo "Motivo" (opcional)
      AND modal fecha

4. Given modal está exibida
   When 3 minutos passam sem ação
   Then modal auto-fechada
      AND timer visível durante countdown
```

---

## Identifying Personas vs Roles

### ✅ Personas (Use in Stories)

```
- Contador (financial specialist, validates transactions)
- Gerente Fiscal (manager, monitors compliance)
- Admin (power user, configures rules)
- CEO (executive, views KPIs)
```

### ❌ Roles (Don't use; too generic)

```
- User
- Admin
- System
- API Client
- Developer
```

**Why:** Roles are technical; personas focus on behavior + value.

---

## Dependency Documentation

### How to Declare Dependencies

```markdown
**Dependências:**

1. Internal (outra Story no mesmo backlog):
   - Depende de: S01 (histórico deve estar populated)
   - Bloqueada por: S02 (não pode prosseguir sem S02)

2. External (fora do scope):
   - Depende de: Dashboard Infra v2+ (DevOps)
   - Depende de: SEFAZ API (vendor)

3. Feature-level:
   - Depende de: F010 (Motor de Regras core)

4. Order:
   - S01 and S03 podem rodar em paralelo (independentes)
   - S02 deve esperar S01 finalizar
```

### Visualização de Grafo

```
S01 (Foundation)
├── S02 (depends on S01)
└── S03 (parallel with S02)
    └── S04 (depends on S01 AND S03)
```

---

## Red Flags (Signs of Bad User Stories)

| Red Flag | Problem | Fix |
|----------|---------|-----|
| "...so that the system works" | No benefit to user | Add actual value: "...so that I process faster" |
| "As a admin/developer" | Technical role, not persona | Replace with business persona |
| "I want the API to..." | Confusing user + system | This is a Task, not a Story |
| "I want A and B and C..." | Too large; multiple needs | Split: "I want A" + "I want B" |
| 50+ line AC's | Too detailed (belongs in Task) | Summarize; let Dev detail |
| No "So that" | Missing value/benefit | Add: "So that I [achieve business goal]" |
| Duplicate AC's across Stories | Copy-paste error | Consolidate or clarify difference |

---

## Checklist: Is This a Good User Story?

- [ ] Has clear persona (not generic role)
- [ ] Has action (I want to...)
- [ ] Has benefit (So that...)
- [ ] Can be completed in 1-3 days
- [ ] Can be tested independently
- [ ] Doesn't mix multiple personas in one story
- [ ] AC's are testable (no ambiguity)
- [ ] Rastreability linkage exists (Feature → Story)
- [ ] Dependencies documented (if any)
- [ ] Title is specific (not "Implement feature X")

---

## Examples by Domain

### Fiscal/Accounting Domain

✅ Good: "As a contador I want to receive suggestion for CFOP when creating PV so that I reduce manual errors"  
✅ Good: "As a gerente I want to export accuracy report monthly so that I validate process quality"  
❌ Bad: "As a user I want suggestion"

### CRM Domain

✅ Good: "As a salesperson I want to see customer contact history so that I personalize conversation"  
✅ Good: "As a manager I want to track deal pipeline by stage so that I forecast revenue"  
❌ Bad: "Implement CRM module"

### E-Commerce Domain

✅ Good: "As a customer I want to see recommended products based on my history so that I discover relevant items"  
✅ Good: "As a merchant I want to configure recommendation engine so that I tune algorithms"  
❌ Bad: "Add recommendations feature"
