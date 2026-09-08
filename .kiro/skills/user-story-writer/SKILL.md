---
name: user-story-writer
description: >
  Skill especializado em quebrar PRDs, RFs ou briefings em histórias de usuário (User Stories)
  prontas para o Jira, no formato padrão do Zupper. Use esta skill SEMPRE que o usuário
  pedir para "quebrar em stories", "gerar tasks", "criar backlog", "escrever jiratxt", "detalhar
  histórias de usuário", "preparar para o Jira" ou qualquer variação desses termos.
  Também ative ao receber o comando `jiratxt`.
metadata:
  category: product-management
  version: 1.0.0
  language: pt-BR
  use-cases: "user-stories, backlog, jira, tasks, jiratxt, sprint-planning"
allowed-tools: readFile readMultipleFiles fsWrite strReplace
---

# User Story Writer

Skill para transformar PRDs, requisitos funcionais ou briefings em histórias de usuário estruturadas,
prontas para importação ou cópia no Jira.

---

## Quando Usar Este Skill

- Usuário pede para "quebrar em stories" ou "gerar backlog"
- Usuário usa o comando `jiratxt`
- Usuário quer preparar um PRD para o sprint planning
- Usuário precisa detalhar um RF em tarefas executáveis
- Usuário quer gerar critérios de aceite para uma feature

---

## Fluxo de Trabalho

### Passo 1 — Entender a fonte

Antes de gerar qualquer story, identificar de onde vem o input:

- **PRD completo** → ler seções de Personas, RFs e Jornadas do Usuário
- **RF isolado** → pedir o contexto de persona e módulo se não fornecido
- **Briefing livre** → fazer as perguntas de contexto antes de gerar

Perguntas de contexto (fazer apenas as que faltarem):
- Qual produto / módulo esta story pertence?
- Qual persona é o foco principal?
- Qual RF ou funcionalidade será quebrada?
- Qual o tipo de entrega? (Story / Bug / Task / Sub-task)
- Precisa de estimativa de story points?

### Passo 2 — Definir o tipo de item

| Tipo | Quando usar |
|------|------------|
| **Epic** | Agrupa stories de **um produto inteiro, um módulo novo ou uma jornada completa de usuário** — representa meses de trabalho e múltiplas sprints |
| **Story** | Entrega de valor para uma persona — tem "Como... quero... para..." |
| **Task** | Trabalho técnico sem valor direto para o usuário (ex: migração de dados, setup de infra) |
| **Bug** | Comportamento incorreto de funcionalidade existente |
| **Sub-task** | Parte de uma Story que precisa ser dividida para execução paralela |

#### Regra crítica — Quantidade de Epics

- **Nova feature / Melhoria / RF isolado** → **1 único Epic** para todo o backlog gerado
- **Novo módulo** → 1 Epic por módulo (máximo 3–4 Epics para um módulo grande)
- **Novo produto** → 1 Epic por jornada principal do produto

**Os blocos da seção "Estrutura do Produto" do PRD NÃO são Epics.** Eles são agrupadores lógicos internos do documento. No Jira, cada bloco vira um grupo de Stories dentro de 1 Epic, não um Epic separado.

> Anti-padrão crítico: criar 1 Epic por RF ou por bloco da "Estrutura do Produto" → resulta em Epics granulares demais que perdem o sentido de agrupamento estratégico no Jira. Se o backlog tem mais Epics do que sprints estimadas para a entrega, os Epics estão granulares demais — consolidar.

### Passo 3 — Gerar as Stories

Aplicar o template padrão (ver seção abaixo) para cada item.
Gerar **uma story por RF** como regra base — quebrar em sub-tasks se o RF for complexo.

### Passo 4 — Validar antes de entregar

Checar cada story gerada contra o checklist:
- [ ] Título com verbo de ação claro
- [ ] História no formato "Como... quero... para..."
- [ ] Pelo menos 3 critérios de aceite verificáveis e objetivos
- [ ] Pelo menos 1 caso de borda / cenário negativo identificado
- [ ] Prioridade MoSCoW definida
- [ ] RF de origem referenciado (se vier de um PRD)
- [ ] DoR checado — story está pronta para entrar em sprint?

---

## Template Padrão — Story (jiratxt)

```
TIPO: [Story / Task / Bug / Sub-task / Epic]
PRIORIDADE: [Must / Should / Could / Won't]
MÓDULO: [Nome do produto ou módulo]
RF ORIGEM: [RF01 / RF02 / ... — ou "N/A" se não vier de PRD]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TÍTULO: [verbo de ação + objeto + contexto]
ex: "Exibir status do voo na tela de detalhes da reserva"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

HISTÓRIA DO USUÁRIO:
Como [persona],
quero [ação ou funcionalidade],
para que [benefício / valor gerado].

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CONTEXTO:
[Descrever o problema ou oportunidade que motiva esta story.
Referenciar o módulo, a persona afetada e o impacto no negócio.
Máximo 4 linhas — objetivo e direto.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ESCOPO:

✅ Inclui:
- [O que deve ser feito — comportamentos esperados, não soluções técnicas]
- [...]

❌ Não inclui (out of scope):
- [O que explicitamente NÃO faz parte desta story]
- [...]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITÉRIOS DE ACEITE:
- [ ] [Critério positivo — happy path — verificável e objetivo]
- [ ] [Critério positivo — ...]
- [ ] [Critério positivo — ...]
- [ ] [Caso de borda / cenário negativo — comportamento esperado em erro]
- [ ] [Caso de borda — ...]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITÉRIOS DE PRONTO (DoR — Definition of Ready):
- [ ] História de usuário escrita e revisada pelo PO
- [ ] Mockup ou protótipo aprovado (se houver componente de UI)
- [ ] Regras de negócio documentadas e sem ambiguidades
- [ ] Dependências mapeadas (outros RFs, APIs, módulos externos)
- [ ] Casos de borda identificados e documentados nos critérios de aceite
- [ ] Estimativa de esforço feita pelo time (se exigido pela equipe)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

LINKS E REFERÊNCIAS:
- PRD: [link ou nome do arquivo de origem]
- Protótipo: [link Figma ou equivalente — se disponível]
- Documentação técnica: [link — se aplicável]
```

---

## Regras de Escrita

### Título
- Sempre iniciar com verbo no infinitivo: *Exibir*, *Permitir*, *Validar*, *Bloquear*, *Notificar*
- Ser específico o suficiente para identificar a story sem contexto adicional
- Evitar títulos genéricos como "Melhorar tela de busca"

### História do Usuário
- Persona deve ser a do PRD — não inventar novas personas
- A ação deve ser o que o usuário faz, não o que o sistema faz
- O benefício deve conectar com um objetivo de negócio do PRD
- Exemplo correto: "Como viajante frequente, quero receber um alerta quando o preço da minha rota preferida cair, para que eu possa aproveitar a oferta sem verificar o app diariamente."
- Exemplo errado: "Como usuário, quero usar o sistema, para que funcione melhor."

### Critérios de Aceite
- Usar linguagem **verificável**: "O sistema exibe...", "O botão está desabilitado quando...", "A mensagem X aparece quando..."
- Evitar subjetivos: "deve ser rápido", "deve ser intuitivo", "deve funcionar bem"
- Mínimo: 3 critérios positivos + 1 caso de borda por story
- Cobrir sempre: estado inicial, ação do usuário, resultado esperado

### Escopo
- "Inclui" deve listar comportamentos, não implementações técnicas
- "Não inclui" é tão importante quanto o inclui — evita escopo inflado
- Dúvidas sobre escopo → marcar como `⚠️ Necessário validar com PO`

---

## Geração em Lote (a partir de PRD)

Quando o usuário fornecer um PRD completo e pedir para quebrar em backlog:

### Passo 1 — Definir quantos Epics criar (ANTES de gerar qualquer Story)

Usar a tabela de decisão abaixo:

| Tipo de entrega do PRD | Quantos Epics criar |
|------------------------|-------------------|
| Nova feature | **1 Epic** com o nome da feature |
| Melhoria / Evolução | **1 Epic** com o nome da melhoria |
| Correção | **0 Epics** — apenas Stories/Bugs |
| Nova jornada | **1 Epic por jornada** (normalmente 1–2) |
| Novo módulo | **1 Epic por módulo** (máximo 3–4) |
| Novo produto | **1 Epic por jornada principal** do produto |

> ⚠️ Os blocos da seção "Estrutura do Produto" do PRD são agrupadores lógicos do documento, **não são Epics no Jira**. Usar essa seção apenas como referência para entender o escopo, não para criar um Epic por bloco.

### Passo 2 — Gerar Stories e Tasks

1. Para cada Epic definido, ler os **RFs correspondentes** do PRD
2. Gerar uma Story por RF (ou sub-tasks se o RF for muito grande)
3. Tasks técnicas ficam soltas (sem Epic obrigatório) ou associadas ao Epic de maior relação
4. Ordenar as stories por dependência e prioridade (Must → Should → Could)
5. Entregar como lista com separador `---` entre cada item

### Passo 3 — Resumo do backlog

Ao final, gerar um **resumo do backlog** com:
- Total de Epics, Stories, Tasks e Sub-tasks gerados
- Distribuição por prioridade (Must / Should / Could)
- Dependências críticas identificadas

---

## Estimativa de Story Points (opcional)

Se o usuário pedir estimativa, usar a escala de Fibonacci simplificada:

| Pontos | Complexidade | Critério orientativo |
|--------|-------------|----------------------|
| 1 | Trivial | Alteração de texto, ajuste visual simples |
| 2 | Simples | CRUD básico sem regra de negócio complexa |
| 3 | Pequena | Feature com regra de negócio e 1-2 integrações simples |
| 5 | Média | Feature com múltiplas regras, validações ou integração externa |
| 8 | Grande | Jornada completa ou integração complexa com múltiplos sistemas |
| 13 | Muito grande | Candidata a quebrar em stories menores |

> Regra: story com estimativa > 8 deve ser questionada para quebra em sub-tasks.

---

## Anti-padrões a Evitar

| Anti-padrão | Por quê é ruim | Como corrigir |
|-------------|---------------|---------------|
| Critério vago: "deve funcionar corretamente" | Não é verificável em QA | Descrever o comportamento exato esperado |
| Historia sem persona real: "Como usuário..." | Perde o contexto de quem e por quê | Usar a persona do PRD |
| Escopo sem "não inclui" | Deixa margem para escopo inflado | Sempre explicitar o out of scope |
| Story técnica disfarçada: "Como sistema, quero..." | Story é de valor para o usuário | Transformar em Task técnica |
| Título genérico: "Ajustar tela X" | Não comunica o valor ou comportamento | Usar verbo de ação + objeto + contexto |
| Critério de aceite com "e": "Exibe X e Y e Z" | Deveria ser 3 critérios separados | Separar em itens individuais |
| **1 Epic por bloco da "Estrutura do Produto" do PRD** | Epics granulares demais — perdem o sentido estratégico no Jira | Consolidar em 1 Epic por feature/jornada; usar os blocos apenas como agrupadores de comentário |
| **1 Epic por RF** | RF é granularidade de Story, não de Epic | 1 Epic deve conter múltiplas Stories/RFs |

---

## Referências

| Referência | Arquivo |
|-----------|---------|
| Template da Story (jiratxt) | `.kiro/skills/user-story-writer/references/story-template.md` |
| Exemplos de Stories por módulo | `.kiro/skills/user-story-writer/references/story-examples.md` |
