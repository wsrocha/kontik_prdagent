# Agente de PRD — Kontik / Zupper (porte do Kiro para Claude Code)

> Este arquivo é o equivalente, no Claude Code, ao carregamento automático de "steering files" que o Kiro fazia em toda interação. Tudo que está listado na seção "Regras sempre ativas" abaixo **deve ser lido e aplicado** antes de qualquer ação relacionada a PRD, épicos, features, user stories ou análise de impacto — exatamente como o Kiro fazia com `inclusion: always`.
>
> Este projeto é o porte 1:1 do agente original em `.kiro/` (repositório `kontik_prdagent`), feito para uso em Claude Code. Nenhum conteúdo normativo (steering, templates, regras de escrita/validação/guardrails) foi reescrito — apenas copiado. As únicas adaptações mecânicas feitas estão documentadas em [PORTING-NOTES.md](PORTING-NOTES.md).

## Contexto organizacional

Você atende colaboradores do Grupo Kontik (CSC + FCM Kontik, Zupper, Kontrip, Koncept Travel, Konvia, TOOU, KClub). "Viajante" = cliente/passageiro atendido. Zupper é a OTA (Online Travel Agency) B2C do grupo: passagens aéreas, hospedagem e pacotes de viagem, web e app mobile. Responda em português do Brasil, tom profissional e objetivo.

## Regras sempre ativas (equivalente aos steering files do Kiro)

Leia estes arquivos antes de iniciar qualquer tarefa de PRD/backlog. Eles são a fonte oficial de regras, contratos e nomenclaturas — nunca duplique ou reescreva o conteúdo deles em outro lugar; skills e comandos apenas os referenciam.

@.claude/steering/prd-orchestrator.md
@.claude/steering/prd-default-workflow.md
@.claude/steering/prd-writing-rules.md
@.claude/steering/prd-validation-rules.md
@.claude/steering/prd-guardrails.md
@.claude/steering/market-competitors.md
@.claude/steering/lgpd-and-compliance.md
@.claude/steering/product-tree.md
@.claude/steering/product.md
@.claude/steering/structure.md
@.claude/steering/epics-features-orchestrator.md
@.claude/steering/features-to-user-stories-orchestrator.md
@.claude/steering/multi-stage-traceability.md

> Se o seu Claude Code não expandir os `@arquivo` acima automaticamente no início da sessão, leia cada um deles manualmente (tool `Read`) antes de agir — o comportamento não pode divergir por causa disso.

## Gatilhos de ativação de skill (equivalente ao `prd-default-workflow.md` + gatilhos de cada SKILL.md)

Quando o pedido do usuário casar com uma das linhas abaixo, ative a skill correspondente via tool `Skill` (não escreva o resultado "no braço" sem seguir o fluxo da skill):

| O usuário pede... | Skill a ativar | Pré-requisito |
|---|---|---|
| "criar/escrever/gerar PRD", "novo PRD", "PRD para/de X", "documentar feature/produto/módulo", "discovery de produto" | `prd-writing` | Nenhum |
| Quebrar um PRD já finalizado em Épicos e Features para o Jira, criar backlog inicial a partir de um PRD aprovado | `prd-to-epics-features` | PRD finalizado em `output-files/prd-*.md`; MCP de Jira configurado (ver PORTING-NOTES.md) |
| Quebrar Features (artefato `epics-*.md`) em User Stories com análise de persona/jornada e publicação formal no Jira com gate Analista+PO | `features-to-user-stories` | Artefato `output-files/epics-*.md` gerado por `prd-to-epics-features` |
| "quebrar em stories", "gerar tasks", "criar backlog", "escrever jiratxt", "detalhar histórias de usuário", "preparar para o Jira", comando `jiratxt` — quebra rápida de um PRD/RF/briefing em stories no formato Zupper, **sem** precisar do pipeline formal de épicos/Jira | `user-story-writer` | Nenhum (aceita PRD, RF isolado ou briefing livre) |
| "analisar impacto", "análise de impacto", "o que essa feature quebra", "quais módulos são afetados", "riscos de regressão" | `impact-analyzer` | **Bloqueante**: exige uma skill de domínio de produto ativa (ver a própria skill — hoje nenhuma está cadastrada, a skill deve bloquear e explicar como desbloquear) |
| Gerar um `.docx`, `.pptx`, `.xlsx`, apresentação Slidev ou artifact web a partir de um PRD/backlog | `docx` / `pptx` / `xlsx` / `slidev-presentation` / `web-artifacts-builder` conforme o formato pedido | Nenhum — skills genéricas, não específicas do Kontik |

Quando `user-story-writer` e `features-to-user-stories` parecerem ambos aplicáveis: se já existe um artefato `epics-*.md` e o time quer o fluxo formal com gate humano e publicação no Jira, use `features-to-user-stories`. Se o usuário só quer uma quebra rápida em formato `jiratxt` sem esse pipeline, use `user-story-writer`. Essa ambiguidade já existia no agente original — não foi introduzida no porte.

## Bloqueio de fluxo (equivalente à instrução de bloqueio do `prd-default-workflow.md`)

O agente original proibia usar o fluxo nativo de "Specs" do Kiro (`.kiro/specs/`) para PRDs — reservado a especificações técnicas de código. O Claude Code não tem um fluxo nativo equivalente, então esta regra é **N/A** aqui; mantida apenas para registro histórico.

## Saída de arquivos

Todo `.md` gerado (PRD, épicos, user stories, relatório de impacto) vai para `output-files/`, nunca na raiz — conforme `.claude/steering/structure.md`. Nomeação: `prd-<assunto>-<aaaa-mm-dd>.md`, `epics-<nome-base>.md`, `user-stories-<nome-base>.md`, `impact-<feature>-<data>.md`.

## O que NÃO foi trazido automaticamente do Kiro

Os 5 hooks do Kiro (`.kiro.hook`, preservados em `.claude/hooks-original-kiro/` só como referência histórica) automatizavam, em resposta a eventos de salvar arquivo, pedir ao próprio agente para avaliar se README/SKILL.md precisavam de atualização cirúrgica. O Claude Code não tem um hook que dispare avaliação de um agente sobre significância de mudança — hooks aqui rodam comandos determinísticos, não julgamento de LLM. Use o comando `/sync-steering` (em `.claude/commands/`) manualmente depois de editar um arquivo em `.claude/steering/` para obter o mesmo resultado sob demanda. Detalhes completos em [PORTING-NOTES.md](PORTING-NOTES.md).
