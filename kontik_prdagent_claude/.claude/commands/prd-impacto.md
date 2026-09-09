---
description: Analisa impacto de uma feature/PRD (personas afetadas, módulos, riscos de regressão, dependências, QA)
---

Ative a skill `impact-analyzer` seguindo exatamente `.claude/skills/impact-analyzer/SKILL.md`.

Lembre-se da regra de bloqueio: esta skill NÃO deve executar a análise sem uma skill de domínio de produto ativa. Como hoje nenhuma skill de domínio Zupper está cadastrada neste projeto, você deve exibir a mensagem de bloqueio definida na própria skill e pedir ao usuário para descrever manualmente o contexto do produto (módulos, personas, integrações) ou criar a skill de domínio antes de prosseguir.

Feature/PRD para analisar:

$ARGUMENTS
