# Agente de PRD — Kontik/Zupper (Claude Code)

> Porte do agente de PRD originalmente construído para o Kiro (repositório `kontik_prdagent`) para rodar no Claude Code. O conteúdo normativo (regras, templates, fluxos) é o mesmo, byte a byte — só a "cola" de ativação foi adaptada para o mecanismo do Claude Code. Detalhes completos de cada adaptação: [PORTING-NOTES.md](PORTING-NOTES.md). Documentos originais do Kiro, intactos: [original-kiro-docs/](original-kiro-docs/).

## O que este agente faz

Ajuda times de produto do Zupper (e demais empresas do Grupo Kontik) a:

1. **Criar PRDs completos** (18 seções, template único, profundidade por tipo de entrega) — skill `prd-writing`.
2. **Quebrar um PRD aprovado em Épicos e Features** para o Jira, com gate de revisão humana — skill `prd-to-epics-features`.
3. **Quebrar Features em User Stories** com análise de persona/jornada e rastreabilidade PRD→Feature→Story — skill `features-to-user-stories`.
4. **Gerar User Stories rápidas** (formato `jiratxt`) direto de um PRD, RF ou briefing, sem o pipeline formal de épicos — skill `user-story-writer`.
5. **Analisar impacto** de uma feature/PRD (personas afetadas, módulos, riscos de regressão, QA) — skill `impact-analyzer`.
6. Gerar entregáveis em `.docx`, `.pptx`, `.xlsx`, apresentações Slidev ou artifacts web a partir de qualquer um dos documentos acima — skills genéricas `docx`, `pptx`, `xlsx`, `slidev-presentation`, `web-artifacts-builder`.

## Como usar

Abra este projeto no Claude Code (a raiz precisa ser este diretório, para o `CLAUDE.md` carregar). Duas formas de acionar:

**a) Linguagem natural** — basta pedir, ex.: *"cria um PRD para um sistema de alertas de preço de passagens aéreas"*. O `CLAUDE.md` mapeia a frase para a skill certa (mesmos gatilhos de texto que o agente Kiro original usava).

**b) Slash commands explícitos** (`.claude/commands/`):

| Comando | Equivale a |
|---|---|
| `/prd-novo <briefing>` | Skill `prd-writing` |
| `/prd-epicos-features <prd + project key Jira>` | Skill `prd-to-epics-features` |
| `/prd-user-stories <epics-*.md + project key Jira>` | Skill `features-to-user-stories` |
| `/prd-jiratxt <PRD, RF ou briefing>` | Skill `user-story-writer` |
| `/prd-impacto <feature ou PRD>` | Skill `impact-analyzer` |
| `/sync-steering` | Substituto manual dos hooks de sincronização do Kiro |

Todo `.md` gerado é salvo em [output-files/](output-files/) (pasta local, não sobe ao Git — ver `.gitignore`).

## Estrutura do repositório

```
/
├── CLAUDE.md                          ← equivalente aos steering "always-on" do Kiro + tabela de gatilhos de skill
├── README.md                          ← você está aqui
├── PORTING-NOTES.md                   ← auditoria completa do porte Kiro → Claude Code
├── original-kiro-docs/                ← README.md e prd-guide.md originais do Kiro, intocados
├── output-files/                      ← PRDs, épicos, stories e relatórios gerados (local, fora do Git)
│
└── .claude/
    ├── steering/                      ← as 13 regras normativas, copiadas verbatim do .kiro/steering/
    ├── templates/prd/prd-template.md  ← template oficial único do PRD, copiado verbatim
    ├── commands/                      ← slash commands (tabela acima)
    ├── hooks-original-kiro/           ← os 5 .kiro.hook originais, guardados como referência (não executam aqui)
    ├── settings-original-kiro/        ← mcp.json original (vazio)
    └── skills/
        ├── prd-writing/               ← discovery → benchmark → PRD → validação → saída
        ├── prd-to-epics-features/     ← PRD aprovado → Épicos/Features Jira (gate humano)
        ├── features-to-user-stories/  ← Features → User Stories (gate Analista+PO)
        ├── user-story-writer/         ← quebra rápida em jiratxt (sem pipeline de épicos)
        ├── impact-analyzer/           ← análise de impacto (bloqueia sem skill de domínio)
        ├── docx/ pptx/ xlsx/          ← geração de documentos Office (skills genéricas)
        ├── slidev-presentation/       ← apresentações Slidev (skill genérica)
        └── web-artifacts-builder/     ← artifacts web (skill genérica)
```

## Os 13 arquivos de steering (regras sempre ativas)

| Arquivo | Responsabilidade |
|---|---|
| `prd-orchestrator.md` | Contrato normativo do orquestrador de PRD: tipo de entrega, profundidade, entradas mínimas |
| `prd-default-workflow.md` | Regra obrigatória de ativação da skill `prd-writing` e gatilhos de texto |
| `prd-writing-rules.md` | As 18 seções obrigatórias do PRD |
| `prd-validation-rules.md` | Critérios de revisão e matriz de severidade |
| `prd-guardrails.md` | 16 guardrails anti-alucinação, LGPD, controle de escopo |
| `market-competitors.md` | Como identificar e comparar concorrentes |
| `lgpd-and-compliance.md` | Avaliação de compliance e dados pessoais |
| `product-tree.md` | Árvore de produto Zupper: domínios, verticais, POs |
| `product.md` | Postura de PO sênior — discovery e definição de problema |
| `structure.md` | Convenção de output (`output-files/`) e camadas do projeto |
| `epics-features-orchestrator.md` | Contrato PRD → Épicos/Features Jira |
| `features-to-user-stories-orchestrator.md` | Contrato Features → User Stories |
| `multi-stage-traceability.md` | Rastreabilidade PRD → Feature → Story → Task → Código |

## Pendências conhecidas (ver PORTING-NOTES.md para detalhes)

- **MCP do Jira** (`tasks-toolkit`): precisa ser configurado no Claude Code para as skills `prd-to-epics-features` e `features-to-user-stories` publicarem issues de verdade. Sem isso, elas geram o artefato intermediário e o gate de aprovação normalmente, só a publicação automática fica bloqueada.
- **Hooks automáticos do Kiro** não têm equivalente 1:1 no Claude Code (hooks aqui são determinísticos, não pedem julgamento de agente). Use `/sync-steering` manualmente após editar um steering file.

## Perguntas frequentes

**Isso muda alguma regra de negócio do agente original?**
Não. Todo o conteúdo normativo (steering, template, regras de escrita/validação/guardrails, exemplos de referência) foi copiado byte a byte — conferido com `diff`. As únicas mudanças são mecânicas: nomes de tools no frontmatter das skills, e a forma como a ativação acontece (steering "always-on" do Kiro → `CLAUDE.md` + slash commands no Claude Code).

**Onde vejo exatamente o que foi adaptado?**
Em [PORTING-NOTES.md](PORTING-NOTES.md) — lista completa, arquivo por arquivo.

**Tenho um PRD gerado pelo agente original no Kiro. Serve para validar o porte?**
Sim — é a forma mais direta de confirmar que o comportamento ficou idêntico. Se você tiver um exemplo, compartilhe para gerarmos um PRD equivalente aqui e comparar seção a seção.
