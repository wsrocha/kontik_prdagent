---
inclusion: always
---

# Regra de Workflow Padrão para PRDs

## Instrução obrigatória

Quando o usuário solicitar criação, refinamento ou validação de PRD (Product Requirements Document), o agente DEVE:

1. Ativar a skill `prd-writing` via `discloseContext`.
2. Seguir o fluxo de 5 fases definido na skill (Discovery → Análise de Mercado → Estruturação → Validação → Saída).
3. Aplicar obrigatoriamente as seguintes steerings do projeto durante todo o fluxo:
   - `prd-orchestrator.md` — contrato de orquestração (tipo de entrega, profundidade, entradas mínimas)
   - `prd-writing-rules.md` — regras de escrita e estrutura das 18 seções
   - `prd-validation-rules.md` — critérios de revisão e matriz de severidade
   - `prd-guardrails.md` — guardrails anti-alucinação, LGPD, controle de escopo
   - `market-competitors.md` — análise de concorrência obrigatória
   - `lgpd-and-compliance.md` — avaliação de compliance e dados pessoais
   - `product-tree.md` — referência de produtos, linhas e POs designados
   - `product.md` — postura de PO sênior (discovery e definição de problema)
   - `structure.md` — regras de output e organização de arquivos
4. Quando houver steering de domínio ativo (ex.: `prd-domain-busca-passagens.md`), aplicar em complemento ao fluxo base.
5. Salvar o output em `output-files/` conforme steering `structure.md`.
6. Usar o template oficial: `.kiro/skills/prd-writing/references/prd-template.md`.

## Instrução de bloqueio

O agente NÃO DEVE usar o fluxo nativo de Specs do Kiro (`.kiro/specs/`) para criação de PRDs.
O fluxo de Specs (requirements.md, design.md, tasks.md) é reservado exclusivamente para especificações técnicas de implementação de código.

## Gatilhos de ativação

Ativar a skill `prd-writing` sempre que o usuário usar qualquer uma dessas expressões:
- "criar PRD", "escrever PRD", "gerar PRD"
- "novo PRD", "PRD para", "PRD de"
- "documentar feature", "documentar produto", "documentar módulo"
- "discovery de produto"
- Qualquer menção a "PRD" combinada com intenção de criação ou refinamento

## Resumo

| Ação do usuário | Workflow correto |
|-----------------|-----------------|
| Criar/refinar PRD | Skill `prd-writing` → output em `output-files/` |
| Spec técnica de código | Fluxo nativo Specs do Kiro → `.kiro/specs/` |
