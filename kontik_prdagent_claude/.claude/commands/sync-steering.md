---
description: Substituto manual dos hooks Kiro de sincronização — avalia se um steering editado exige atualizar SKILL.md ou o README
---

Isto reproduz manualmente o que os hooks `.claude/hooks-original-kiro/*.hook` faziam automaticamente no Kiro ao salvar um arquivo (o Claude Code não dispara avaliação de agente em resposta a eventos de arquivo).

Passos:

1. Rode `git diff -- .claude/steering/` (ou peça ao usuário qual arquivo de steering foi editado) e leia o diff.
2. Avalie se a mudança é significativa (nova regra, novo escopo, arquivo antes vazio agora preenchido) — mudanças cosméticas não exigem ação.
3. Se significativa:
   - Para qualquer steering em `.claude/steering/*.md`: verifique se `.claude/skills/prd-writing/SKILL.md` precisa refletir a mudança (tabela de steering/referências).
   - Se o arquivo editado for `epics-features-orchestrator.md`: verifique também `.claude/skills/prd-to-epics-features/SKILL.md`.
   - Se o arquivo editado for `features-to-user-stories-orchestrator.md`: verifique também `.claude/skills/features-to-user-stories/SKILL.md`.
   - Verifique se o conteúdo editado agora sobrepõe algum arquivo em `references/` de alguma skill — se sim, converta a referência em ponteiro apontando para o steering.
4. Avalie se o `README.md` da raiz precisa de uma atualização cirúrgica (nunca reescreva o documento inteiro).
5. Aplique apenas as alterações cirúrgicas identificadas e informe ao usuário exatamente o que mudou e por quê.
