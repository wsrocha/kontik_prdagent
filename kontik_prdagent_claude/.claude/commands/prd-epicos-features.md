---
description: Decompõe um PRD finalizado em Épicos e Features do Jira (gate humano obrigatório antes de publicar)
---

Ative a skill `prd-to-epics-features` seguindo exatamente `.claude/skills/prd-to-epics-features/SKILL.md` e o contrato normativo em `.claude/steering/epics-features-orchestrator.md`.

Lembre-se: NÃO publique nada no Jira sem o gate de revisão humana explícito (Fase 4). Se o MCP de tarefas/Jira não estiver configurado neste ambiente, gere o artefato intermediário normalmente e avise o usuário que a publicação ficará pendente até o MCP ser configurado (ver PORTING-NOTES.md).

Entrada fornecida pelo usuário (caminho do PRD, project key do Jira, etc.):

$ARGUMENTS
