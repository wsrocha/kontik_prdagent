# Rastreabilidade Multi-Estágio (Steering Normativo)

> **Responsabilidade deste arquivo:** Definir contrato obrigatório de rastreabilidade transversal PRD → Features → Stories → Tasks → Código.
> Garante que toda decisão de produto pode ser auditada até sua origem no PRD e implementação no código.

---

## Escopo do orquestrador

- Garantir **cadeia de custódia ininterrupta** entre PRD e código entregue.
- Garantir que toda Feature, Story e Task carrega referência de origem (origem no PRD).
- Garantir labels obrigatórias no Jira para rastreamento automático.
- Garantir campo `Origin` em cada Jira Issue com rastreabilidade completa.
- Facilitar auditória regressiva: dado um Bug, encontrar RF/Feature/Story que o gerou.

---

## Hierarquia de Rastreabilidade

```
PRD (Seção, Versão)
  └── Épico (E0XX, gerado de Seção 16 ou 6)
      └── Feature (F0XX, gerado de RF[X])
          └── User Story (S0XX, gerado de Feature + persona)
              └── Task (T0XX, gerado de Story)
                  └── Commit/PR (#commit-hash, linked no Task)
```

**Cada nível deve conter referência bidirecional:**
- Feature carrega `[RF#]` no título
- Story carrega `[F#]` no título
- Task carrega `[S#]` no título
- Commit carrega `[T#]` no message

---

## Campo `Origin` Obrigatório

Cada Jira Issue DEVE ter descrição com seção `## Origin` estruturada:

### Para Feature (gerado por prd-to-epics-features)

```markdown
## Origin

**PRD:** output-files/prd-alerta-preco-passagens.md  
**PRD Section:** 8 — Requisitos Funcionais  
**RF Code:** RF03  
**RF Title:** Alerta automático de queda de preço  
**Epic:** E001 (Fase 1 — Motor de Alertas)  
**Critérios de Aceite (PRD):** [link ou resumo]
```

### Para Story (gerado por features-to-user-stories)

```markdown
## Origin

**Feature:** F012 (Alerta automático de queda de preço)  
**Feature Jira Key:** ZUPPER-450  
**RF:** RF03  
**PRD:** prd-alerta-preco-passagens.md  
**Persona:** Viajante recorrente  
**Jornada:** Configuração e recebimento de alerta de preço  
**Epic:** E001
```

### Para Task (gerado por spec-kit.tasks)

```markdown
## Origin

**Story:** S045 (Como viajante, quero receber alerta quando o preço cair...)  
**Story Jira Key:** ZUPPER-456  
**Feature:** F012  
**RF:** RF03  
**PRD:** prd-alerta-preco-passagens.md  
**Implementation Link:** /speckit/stories/S045/tasks
```

### Para Commit (Dev escreve no PR description)

```
Closes ZUPPER-789 (Task T045)
Story: ZUPPER-456
Feature: ZUPPER-450 (RF03)
PRD: prd-alerta-preco-passagens.md

## Changes
- Implementar serviço de monitoramento de preços
- Testes unitários com 100% cobertura
```

---

## Labels Obrigatórios por Issue Type

### Feature Labels
```
Labels obrigatórios:
  - prd:{nome-base-prd}        (ex: prd:alerta-preco-passagens)
  - rf:{RF-code}               (ex: rf:RF03)
  - priority:moscow            (ex: priority:must-have)
  - epic:{epic-name}           (ex: epic:motor-alertas)

Labels sugeridos:
  - domain:{domínio}           (ex: domain:busca-passagens)
  - vertical:{linha}           (ex: vertical:experiencia-viajante)
```

### Story Labels
```
Labels obrigatórios:
  - prd:{nome-base-prd}        (ex: prd:alerta-preco-passagens)
  - rf:{RF-code}               (ex: rf:RF03)
  - feature:{F-code}           (ex: feature:F012)
  - persona:{persona-name}     (ex: persona:viajante-recorrente)
  - priority:moscow            (ex: priority:must-have)

Labels sugeridos:
  - milestone:{sprint}         (ex: milestone:sprint-23)
```

### Task Labels
```
Labels obrigatórios:
  - prd:{nome-base-prd}
  - rf:{RF-code}
  - story:{S-code}             (ex: story:S045)
  - type:{tipo-task}           (ex: type:backend, type:test, type:doc)

Labels sugeridos:
  - effort:{estimate}          (ex: effort:3-days)
```

---

## Matriz de Rastreabilidade (Traceability Matrix)

### Formato do artefato `output-files/traceability-matrix-*.md`

```markdown
# Traceability Matrix — [PRD Name]

| PRD Section | RF Code | RF Title | Epic | Feature | Story(ies) | Task(s) | Status | Deployed |
|---|---|---|---|---|---|---|---|---|
| 8 | RF03 | Alerta automático de queda de preço | E001 | F012 | S045, S046 | T089-T095 | In Progress | ✗ |
| 8 | RF04 | Notificação de disponibilidade de assento | E001 | F013 | S047 | T096-T102 | Blocked (depends on T095) | ✗ |
```

**Mantém-se vivo durante o ciclo de desenvolvimento:**
- Atualizado após cada Story publicada no Jira
- Atualizado após cada Task concluída
- Consultado em code reviews para validar cobertura

---

## Fluxo de Rastreabilidade Obrigatório

### 1. PRD → Feature (prd-to-epics-features)
✅ Feature description DEVE conter seção `## Origin` com RF code  
✅ Feature title DEVE prefixar `[RF#]`  
✅ Feature labels DEVEM incluir `rf:{RF-code}`  

**Validação:** Skill bloqueia publicação se faltarem campos

### 2. Feature → Story (features-to-user-stories)
✅ Story description DEVE conter seção `## Origin` com Feature reference  
✅ Story title DEVE prefixar `[F#]`  
✅ Story labels DEVEM incluir `rf:{RF-code}` + `feature:{F-code}`  
✅ Story DEVE estar linkada ao Épico correto

**Validação:** Skill bloqueia publicação se faltarem campos

### 3. Story → Task (spec-kit.tasks)
A cargo do spec-kit validar, mas deve manter rastreabilidade no description  
✅ Cada Task DEVE ter description com `## Origin` linkando à Story  
✅ Task labels DEVEM incluir todos os níveis (prd, rf, story)

### 4. Task → Code (Developer + Git)
✅ Commit message DEVE referenciar Task Jira key (ex: `ZUPPER-789`)  
✅ PR description DEVE listar todas as Stories cobertas  
✅ GitHub PR DEVE estar linkado ao Jira Task (via integração)

---

## Validações Automáticas (Guardrails)

### Na publicação de Feature (prd-to-epics)
- [ ] Título contém `[RF#]`
- [ ] Origin section preenchida
- [ ] RF Code referenciado existe no PRD
- [ ] Labels incluem `prd:*` + `rf:*`

### Na publicação de Story (features-to-user-stories)
- [ ] Título contém `[F#]`
- [ ] Origin section preenchida
- [ ] Feature referenciada existe no Jira
- [ ] Labels incluem `prd:*` + `rf:*` + `feature:*`

### Na publicação de Task (spec-kit)
- [ ] Origin section preenchida
- [ ] Story referenciada existe no Jira
- [ ] Labels herdam do Story (não perdem rastreabilidade)

### No merge de PR (GitHub + Jira)
- [ ] Commit message referencia Task key
- [ ] Task key existe no projeto
- [ ] Task está linkado à Story
- [ ] PR description lista Stories cobertas

---

## Auditoria Regressiva (Use Case)

**Problema:** Bug encontrado em produção → Preço incorreto exibido ao viajante  
**Solução:** Rastrear até origem

```
Production Bug Report
  → Encontra commit hash c4a3f9
  → Git message: "Closes ZUPPER-789"
  → Jira Task ZUPPER-789: "Validar integração de preços com GDS"
  → Task description / Origin: Story ZUPPER-456
  → Jira Story ZUPPER-456: "[F012] Como viajante, quero ver o preço correto"
  → Story description / Origin: Feature ZUPPER-450 (RF03)
  → Jira Feature ZUPPER-450: "[RF03] Alerta automático de queda de preço"
  → Feature description / Origin: PRD prd-alerta-preco-passagens.md, Seção 8, RF03
  → PRD RF03 Critérios de Aceite: [verificar se bug é gap de especificação]
  
Conclusão: Bug é gap em critério de aceite ou implementação divergiu de spec
Ação: Abrir story de correção linkada ao RF original
```

---

## Referências Normativas deste Orquestrador

| Arquivo | Responsabilidade |
|---------|-----------------|
| `.kiro/steering/epics-features-orchestrator.md` | Contrato PRD → Features |
| `.kiro/steering/features-to-user-stories-orchestrator.md` | Contrato Features → Stories |
| `.kiro/skills/prd-to-epics-features/SKILL.md` | Implementação feature generation |
| `.kiro/skills/features-to-user-stories/SKILL.md` | Implementação story generation |
| `.kiro/steering/prd-writing-rules.md` | Estrutura PRD (origem de RFs) |
