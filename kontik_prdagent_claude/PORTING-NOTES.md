# Notas de Porte — Kiro → Claude Code

Este documento existe para auditoria: lista **exatamente** o que foi copiado sem alteração e o que precisou de adaptação mecânica para funcionar no Claude Code, e por quê. Nenhuma regra de negócio, texto normativo, template ou fluxo operacional foi reescrito ou resumido.

Fonte: repositório `kontik_prdagent` (Kiro), commit `f52dafb`. Data do porte: 2026-09-09.

## 1. Copiado 100% verbatim (verificado por `cmp`/`diff` byte a byte)

- `.kiro/steering/*.md` (13 arquivos) → `.claude/steering/*.md`
- `.kiro/templates/prd/prd-template.md` → `.claude/templates/prd/prd-template.md`
- `.kiro/skills/{prd-writing,prd-to-epics-features,features-to-user-stories,user-story-writer,impact-analyzer}/references/*` → mesmos caminhos em `.claude/skills/`
- `.kiro/skills/{docx,pptx,xlsx,slidev-presentation,web-artifacts-builder}/` (skills genéricas, não específicas do Kontik) → `.claude/skills/` — copiadas inteiras, incluindo scripts Python/shell e schemas
- `.kiro/hooks/*.hook` → `.claude/hooks-original-kiro/` (mantidos como referência histórica; não são executados pelo Claude Code — ver seção 4)
- `.kiro/settings/mcp.json` (vazio no repo original) → `.claude/settings-original-kiro/mcp.json`
- `README.md` e `prd-guide.md` originais → `original-kiro-docs/` (preservados intactos; o `README.md` da raiz deste projeto é um documento novo, não uma edição do original)

## 2. Adaptação mecânica: `allowed-tools` das 5 SKILL.md de domínio

Kiro e Claude Code usam nomes de tools diferentes para as mesmas operações. Isso não muda **o que** a skill faz, só **como o runtime nomeia** a operação. Todas as outras linhas de cada SKILL.md (descrição, fluxo, fases, regras, exemplos, referências) permanecem idênticas ao original — só a linha `allowed-tools` do frontmatter foi traduzida, e um comentário HTML foi adicionado logo abaixo dela citando o valor original, para rastreabilidade.

| Skill | `allowed-tools` original (Kiro) | `allowed-tools` no porte (Claude Code) |
|---|---|---|
| `prd-writing` | `readFile readMultipleFiles fsWrite strReplace remote_web_search webFetch` | `Read Write Edit WebSearch WebFetch` |
| `prd-to-epics-features` | `readFile readMultipleFiles fsWrite strReplace mcp_mcp-toolkit_tool_create_jira_issue mcp_mcp-toolkit_tool_search_jira_issues` | `Read Write Edit mcp__tasks-toolkit__jira_create_issue mcp__tasks-toolkit__jira_search_issues` |
| `features-to-user-stories` | idem acima | idem acima |
| `user-story-writer` | `readFile readMultipleFiles fsWrite strReplace` | `Read Write Edit` |
| `impact-analyzer` | `readFile readMultipleFiles fsWrite discloseContext` | `Read Write` (ver nota `discloseContext` abaixo) |

Mapeamento geral usado:

| Conceito Kiro | Equivalente Claude Code |
|---|---|
| `readFile` / `readMultipleFiles` | `Read` |
| `fsWrite` | `Write` |
| `strReplace` | `Edit` |
| `remote_web_search` | `WebSearch` |
| `webFetch` | `WebFetch` |
| `discloseContext` (injeta outro steering/skill no contexto ativo) | Sem tool equivalente — o efeito é obtido lendo diretamente o arquivo relevante com `Read`, que é o que a skill já faz na prática |
| `mcp_mcp-toolkit_tool_create_jira_issue` / `..._search_jira_issues` | `mcp__tasks-toolkit__jira_create_issue` / `mcp__tasks-toolkit__jira_search_issues` — **placeholder**: o nome real depende de como o MCP de Jira for registrado no Claude Code (ver seção 3) |

## 3. Pendência: MCP do Jira (`tasks-toolkit`)

O `.kiro/settings/mcp.json` original estava **vazio** — o MCP `tasks-toolkit` usado pelas skills `prd-to-epics-features` e `features-to-user-stories` era configurado em outro lugar do ambiente Kiro (não neste repositório), então não havia nada para portar tecnicamente.

Para essas duas skills publicarem de fato no Jira a partir do Claude Code, alguém precisa:
1. Registrar um MCP server equivalente (`claude mcp add ...` ou `.mcp.json` do projeto) que exponha as tools de criação/busca de issue do Jira do Zupper.
2. Ajustar o nome exato das tools nas linhas `allowed-tools` acima (hoje são placeholders `mcp__tasks-toolkit__*`) para bater com o nome real que o MCP expuser.

Até isso ser feito, as duas skills funcionam normalmente até a Fase de geração do artefato intermediário (`epics-*.md` / `user-stories-*.md`) e do gate de aprovação humana — só a publicação automática no Jira ficará bloqueada.

## 4. Hooks do Kiro — sem equivalente automático no Claude Code

Os 5 hooks (`.kiro.hook`, preservados em `.claude/hooks-original-kiro/`) disparavam em eventos de `fileEdited`/`fileCreated` e pediam ao próprio agente Kiro para **julgar** se a mudança era significativa e então editar README/SKILL.md cirurgicamente:

- `update-readme-on-steering-change.kiro.hook`
- `update-readme-on-file-created.kiro.hook`
- `sync-skill-on-steering-change.kiro.hook`
- `sync-skill-on-steering-edit.kiro.hook`
- `sync-epics-skill-on-steering-change.kiro.hook`

Hooks do Claude Code (configurados em `settings.json`, eventos como `PreToolUse`/`PostToolUse`) rodam **comandos determinísticos**, não conseguem pedir a um agente para avaliar significância semântica de uma mudança e decidir o que editar. Não existe, portanto, um port 1:1 automático.

Substituto adotado: o comando `/sync-steering` (`.claude/commands/sync-steering.md`) reproduz o mesmo texto de instrução que os hooks usavam, mas sob demanda — rode-o manualmente depois de editar algo em `.claude/steering/`.

## 5. Coisas que ficaram de fora deliberadamente

- `.vscode/settings.json` do repo original (`"kiroAgent.configureMCP": "Disabled"`) — é uma configuração da extensão do Kiro no VS Code, sem equivalente ou utilidade no Claude Code.
- `README.md`/`prd-guide.md` originais não foram editados — foram copiados para `original-kiro-docs/` e um novo `README.md` foi escrito do zero para explicar o uso no Claude Code, já que o README original menciona uma estrutura (ex.: `prd-general-guidelines.md`, `aidlc-docs/`) que não corresponde 1:1 aos arquivos que de fato existem no repositório hoje — isso é uma inconsistência pré-existente no agente original, não algo introduzido por este porte.

## 6. Verificação de integridade

Toda cópia verbatim (seção 1) foi validada com `cmp`/`diff -rq` contra a origem no momento do porte, sem nenhuma diferença. Se precisar reconferir depois de qualquer edição futura, rode a partir da raiz do repositório Kiro original:

```
diff -rq .kiro/steering kontik_prdagent_claude/.claude/steering
diff -rq .kiro/templates/prd kontik_prdagent_claude/.claude/templates/prd
diff -rq .kiro/skills/prd-writing/references kontik_prdagent_claude/.claude/skills/prd-writing/references
```
