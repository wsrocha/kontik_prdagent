---
description: Quebra um artefato de Features (epics-*.md) em User Stories com análise de persona/jornada e gate Analista+PO
---

Ative a skill `features-to-user-stories` seguindo exatamente `.claude/skills/features-to-user-stories/SKILL.md` e o contrato normativo em `.claude/steering/features-to-user-stories-orchestrator.md` e `.claude/steering/multi-stage-traceability.md`.

Esta skill espera um artefato `output-files/epics-*.md` já publicado (gerado por `prd-to-epics-features`). Se o usuário só quer uma quebra rápida em stories sem esse pipeline formal, sugira `/prd-jiratxt` em vez disso.

Entrada fornecida pelo usuário:

$ARGUMENTS
