---
name: prd-to-epics-features
description: >
  Skill para decompor um PRD finalizado em Épicos e Features do Jira.
  Parte da saída do prd-writing e gera um artefato intermediário revisável,
  com gate de aprovação humana obrigatório antes de publicar via MCP tasks-toolkit.
  Use quando precisar criar o backlog inicial de Épicos e Features a partir de um PRD aprovado.
license: MIT
compatibility: Requer MCP tasks-toolkit ativo e acessível. Ideal após execução do skill prd-writing.
metadata:
  category: product-management
  version: 1.1.0
  language: pt-BR
  use-cases: "backlog-inicial, epicos-jira, features-jira, prd-para-jira"
allowed-tools: readFile readMultipleFiles fsWrite strReplace mcp_mcp-toolkit_tool_create_jira_issue mcp_mcp-toolkit_tool_search_jira_issues
---

# PRD to Epics & Features Skill

Skill especializado em transformar um PRD aprovado em Épicos e Features estruturados para o Jira,
seguindo as regras ativas do workspace e o contrato do orquestrador normativo.

---

## Quando Usar Este Skill

- PO acabou de finalizar um PRD com o skill `prd-writing` e quer criar o backlog Jira
- Time precisa decompor requisitos do PRD em issues rastreáveis antes do sprint planning
- Necessidade de sincronizar a estrutura do produto (PRD) com o board Jira

## Pré-requisitos

- PRD finalizado em `output-files/prd-*.md` (gerado pelo skill `prd-writing`)
- MCP `tasks-toolkit` ativo e autenticado
- Permissão no Jira para criar `Epic` e `Feature` no projeto informado

---

## Referências Operacionais

| Arquivo | Uso |
|---------|-----|
| `.kiro/steering/epics-features-orchestrator.md` | Contrato normativo: hierarquia, entradas, critérios de qualidade |
| `.kiro/skills/prd-to-epics-features/references/epics-features-template.md` | Template do artefato intermediário |
| `.kiro/skills/prd-to-epics-features/references/jira-field-mapping.md` | Mapeamento PRD → campos Jira e estratégia de publicação |
| `.kiro/steering/prd-guardrails.md` | Guardrails anti-alucinação (aplicar na leitura do PRD) |

## Steering Files ativos

| Steering | Responsabilidade |
|----------|-----------------|
| `epics-features-orchestrator.md` | Hierarquia canônica, entradas mínimas, critérios de qualidade, protocolo de épicos |
| `prd-writing-rules.md` | Estrutura do PRD de entrada (18 seções) |
| `prd-guardrails.md` | Anti-alucinação e proibição de invenção na leitura do PRD |
| `structure.md` | Convenção de output-files/ e desacoplamento de camadas |

---

## Fluxo de Trabalho

### Fase 1 — Discovery e Configuração

**Objetivo:** Coletar entradas mínimas e carregar o PRD.

1. Extrair do prompt do PO tudo que já estiver explícito (caminho do PRD, projeto Jira, etc.).
2. Para cada campo abaixo ainda ausente, perguntar em **um único bloco** (não perguntar um por um):

   ```
   Por favor, confirme as seguintes informações:

   1. Qual o PRD de entrada? (ex: output-files/prd-nome.md)
      → Se houver mais de um PRD em output-files/, listar as opções.
   2. Qual o Project Key do Jira? (ex: ZUPPER, ZUPPER-CORE)
   3. Deseja atribuir os issues a alguém? (opcional, ex: thiago.soares)
      → Autenticação no MCP é gerenciada automaticamente pelo agente.
   4. Há um ID de Entrega Jira (entrega_id)? Se sim, qual? (opcional)
   5. Há labels padrão que todos os issues devem ter? (ex: sprint-23,zupper-core) (opcional)
   6. Deseja usar `epic_keys_map` pré-existente para algum épico? (opcional)
   ```

3. Ler o PRD de entrada com foco nas seções críticas:
   - Seção 1 (Metadados): produto, versão, tipo de entrega
   - Seção 3 (Objetivos): contexto e valor para descrição dos Épicos
   - Seção 6 (Estrutura): módulos (fallback para Épicos)
   - Seção 8 (RFs): requisitos com MoSCoW e critérios (fonte das Features)
   - Seção 16 (Plano de Execução): fases (fonte primária dos Épicos, se existir)

4. Confirmar ao PO: "PRD lido com sucesso. Produto: [X], [N] RFs encontrados, [M] módulos/fases identificados."

---

### Fase 2 — Extração da Hierarquia

**Objetivo:** Derivar Épicos e Features do conteúdo do PRD.

#### Extração de Épicos

Aplicar em ordem de prioridade:

1. **Fonte primária — Seção 16 (Plano de Execução):** cada fase com entregáveis → 1 Épico.
   - Título: nome da fase (ex: "Fase 1 — Motor de Regras")
   - Contexto: entregáveis + duração mencionados no PRD
2. **Fallback 1 — Seção 6 (Estrutura do Produto):** cada módulo → 1 Épico.
   - Título: nome do módulo
   - Contexto: propósito do módulo conforme descrito no PRD
3. **Fallback 2 — Agrupamento temático dos RFs da Seção 8:** agrupar RFs por domínio funcional → 1 Épico por grupo.
   - Aplicar apenas se Seções 16 e 6 estiverem ausentes ou incompletas.

> **Guardrail:** Não inventar Épicos. Se o PRD não tiver Seção 16 nem Seção 6, informar ao PO e aplicar o fallback 2 com agrupamento explícito de RFs.

#### Extração de Features

Para cada RF da Seção 8:

1. Título: derivado do campo "Descrição" do RF (adaptar para ≤ 255 caracteres).
2. Épico pai: identificar a qual Épico o RF pertence (por fase/módulo/tema).
3. MoSCoW: copiar diretamente do RF (`Prioridade MoSCoW`).
4. Critérios de aceite: derivados dos campos "Regras de Negócio" e critérios explícitos do RF.
5. RF de origem: registrar o ID do RF (ex: RF03).
6. Won't Have: marcar como "não publicar" — manter no artefato apenas para rastreabilidade.

---

### Fase 3 — Geração do Artefato Intermediário

**Objetivo:** Salvar o breakdown revisável antes de qualquer ação no Jira.

1. Usar o template em `.kiro/skills/prd-to-epics-features/references/epics-features-template.md`.
2. Preencher:
   - Cabeçalho de origem (produto, versão, projeto Jira, data)
   - Sumário de Épicos com contagem de Features por prioridade
   - Seção de cada Épico com tabela de Features
   - Seção "Won't Have" com features descartadas
   - Status: `Aguardando revisão`
   - Coluna "Jira Epic Key" vazia (a ser preenchida automaticamente na publicação)
3. Salvar em: `output-files/epics-<nome-base-do-prd>.md`
   - Nome base: remover `prd-` do início e `.md` do final do arquivo PRD de entrada.
   - Exemplo: `output-files/prd-sugestao-inteligente.md` → `output-files/epics-sugestao-inteligente.md`

---

### Fase 4 — Gate de Revisão Humana (OBRIGATÓRIO)

**Objetivo:** Garantir aprovação explícita antes de qualquer publicação no Jira.

1. Apresentar sumário ao PO:
   ```
   📋 BREAKDOWN GERADO — Aguardando revisão

   Épicos identificados: [N]
   Features para publicar: [M] (Must Have: X | Should Have: Y | Could Have: Z)
   Features Won't Have (não publicar): [W]

   Artefato salvo em: output-files/epics-[nome].md

   ⚠️  PRÓXIMO PASSO OBRIGATÓRIO (antes de publicar):
   Confirmar se devo:
   1. Criar automaticamente todos os [N] Épicos no Jira, ou
   2. Reutilizar chaves já existentes via epic_keys_map para parte deles

   Após confirmar a estratégia, escolha uma ação:
   ✅ APROVAR e publicar Épicos + Features no Jira
   ✏️  AJUSTAR o artefato antes de publicar
   🚫 CANCELAR (manter artefato mas não publicar)
   ```

2. **Aguardar resposta explícita do PO.** Não prosseguir sem uma das três respostas.

3. Se **AJUSTAR**: incorporar os ajustes no artefato e voltar ao início do gate.
4. Se **CANCELAR**: registrar status `Cancelado` no artefato e encerrar. Não chamar MCP.
5. Se **APROVAR**: atualizar status para `Aprovado` e prosseguir para Fase 5.

---

### Fase 5 — Publicação no Jira via MCP

**Objetivo:** Criar Épicos e Features no Jira usando o MCP `tasks-toolkit`.

#### 5.1 — Preparação

1. Ler o mapeamento de campos: `.kiro/skills/prd-to-epics-features/references/jira-field-mapping.md`
2. Preparar payload de Épicos e Features validando:
   - `summary` ≤ 255 caracteres (truncar com `…` se necessário, avisar)
   - `priority` = um dos valores: `"Baixa"`, `"Média"`, `"Alta"`, `"Crítica"`
   - `labels` sem espaços (substituir espaços por hífens)
   - `issue_type` = `Epic` para épicos e `Feature` para features
   - `epic_key` formato `PROJETO-NNN` para cada Feature
   - Features `Won't Have` → NÃO incluir
3. Ordenar criação: primeiro Épicos, depois Features por Épico (Must→Should→Could).

#### 5.2 — Criação sequencial

Para cada Épico identificado:

```
Criar Epic: [titulo]
   project_key: [ZUPPER]
   issue_type: Epic
   summary: [titulo <=255]
   description: [contexto do epico + origem no PRD]
   priority: [Média por default, ou conforme consolidacao]
   assignee: [username do PO - se fornecido]
   labels: [labels_base + labels especificas do epico]
   entrega_id: [entrega_id - se fornecido]
```

- Se `epic_keys_map` trouxer chave para o épico atual, reutilizar e pular criação.
- Registrar o `issue_key`/URL de cada Épico no artefato antes de iniciar as Features.

Para cada Feature (ordenado por Épico, depois por MoSCoW: Must→Should→Could):

```
Criar Feature: [título]
  project_key: [ZUPPER]
  issue_type: Feature
   summary: [titulo <=255]
  description: [template de descrição com contexto + critérios de aceite + RF origem]
  priority: [Alta|Média|Baixa]
   epic_key: [chave do Epic criado ou reutilizado]
   assignee: [username do PO - se fornecido]
  labels: [labels_base + labels específicas da feature]
   entrega_id: [entrega_id - se fornecido]
```

- Registrar o `issue_key` retornado e a URL no artefato após cada criação bem-sucedida.
- Se ocorrer erro: registrar o erro no artefato e **continuar com as próximas Features** (não parar o fluxo inteiro). Reportar erros no sumário final.

#### 5.3 — Atualização do artefato

1. Preencher a coluna "Issue Key Jira" e URL para cada Feature criada.
2. Preencher a coluna "Jira Epic Key" e URL dos Épicos.
3. Preencher a tabela "Resultado da Publicação".
4. Atualizar status do artefato para `Publicado`.
5. Adicionar entrada no "Histórico do Artefato".
6. Salvar o artefato atualizado.

#### 5.4 — Sumário final

Apresentar ao PO:

```
✅ PUBLICAÇÃO CONCLUÍDA

Épicos criados/reutilizados: [E]
Features criadas: [F]
Total criado com sucesso: [N] issues
Erros: [M] issues (ver seção "Resultado da Publicação" no artefato)

Links rápidos:
- [ZUPPER-YYY] Epic: [título] → https://jira.zupper.com.br/browse/ZUPPER-YYY
- [ZUPPER-XXX] Feature: [título] → https://jira.zupper.com.br/browse/ZUPPER-XXX
- ...

Artefato atualizado: output-files/epics-[nome].md

Próximos passos sugeridos:
1. Adicionar os Épicos e Features criados aos Sprints planejados.
2. Criar subtasks de Análise (Análise de sistemas) e Programação para cada Feature.
3. Revisar ordenação de execução por MoSCoW no board.
```

---

## Regras de Qualidade

Aplicar antes de gerar o artefato (Fase 3) e antes de publicar (Fase 5):

- Não inventar RFs, módulos ou fases que não existam no PRD.
- Não alterar o texto de critérios de aceite do PRD — extrair fielmente.
- Não publicar Features com MoSCoW `Won't Have`.
- Não chamar o MCP sem aprovação explícita do PO no gate (Fase 4).
- Criar Épicos antes de Features, sempre.
- Se o PRD estiver incompleto (seções ausentes), sinalizar como `Pendência` e perguntar ao PO antes de prosseguir.
- Títulos de Features devem ser autoexplicativos fora do contexto do PRD.

---

## Interações esperadas entre skills

| Trigger | Skill/Hook chamado |
|---------|--------------------|
| Novo PRD gerado em `output-files/` | `prd-to-epics-features` pode ser invocado diretamente em seguida |
| Edição de `epics-features-orchestrator.md` | `.kiro/hooks/sync-epics-skill-on-steering-change.kiro.hook` atualiza este SKILL.md |
| Novo arquivo em `output-files/` | `.kiro/hooks/update-readme-on-file-created.kiro.hook` atualiza o README |
