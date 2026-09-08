---
name: impact-analyzer
description: >
  Skill especializado em análise de impacto de features e PRDs. Dado um PRD ou requisito
  funcional, mapeia personas afetadas, módulos relacionados, riscos de regressão, dependências
  técnicas e recomendações de QA. Use esta skill SEMPRE que o usuário pedir "analisar impacto",
  "análise de impacto", "o que essa feature quebra", "quais módulos são afetados",
  "riscos de regressão" ou qualquer variação desses termos.
  REQUISITO OBRIGATÓRIO: uma skill de domínio do produto deve estar disponível antes de executar.
metadata:
  category: product-management
  version: 1.0.0
  language: pt-BR
  use-cases: "impact-analysis, change-impact, regression-risk, dependency-mapping"
allowed-tools: readFile readMultipleFiles fsWrite discloseContext
---

# Impact Analyzer

Skill para análise de impacto de features, PRDs e requisitos funcionais.
Mapeia o que muda, quem é afetado, o que pode quebrar e o que o time de QA precisa cobrir.

---

## Regra de Bloqueio — Skill de Domínio Obrigatória

**Esta skill NÃO executa sem uma skill de domínio do produto ativa.**

Antes de qualquer análise, verificar obrigatoriamente:

### Passo 0 — Verificar skill de domínio disponível

1. Identificar o produto informado no PRD (campo "Produto / Módulo" dos metadados)
2. Consultar a tabela de skills de domínio disponíveis abaixo
3. Se encontrada: ativar a skill de domínio via `discloseContext` antes de prosseguir
4. Se NÃO encontrada: **bloquear e exibir a mensagem de bloqueio**

### Skills de domínio disponíveis

| Produto / Domínio | Skill de domínio | Status |
|-------------------|-----------------|--------|
| Busca de Passagens (Zupper) | `zupper-busca-passagens` | ❌ Não disponível |
| Checkout e Pagamentos (Zupper) | `zupper-checkout-pagamentos` | ❌ Não disponível |
| Experiência do Viajante (Zupper) | `zupper-experiencia-viajante` | ❌ Não disponível |
| Reservas e Gestão de Viagens (Zupper) | `zupper-reservas` | ❌ Não disponível |
| Atendimento e Suporte (Zupper) | `zupper-suporte` | ❌ Não disponível |
| Produto externo / desconhecido | — | ❌ Não disponível |

### Mensagem de bloqueio (exibir quando skill não disponível)

```
⛔ ANÁLISE DE IMPACTO BLOQUEADA

O produto "[nome do produto]" não possui uma skill de domínio cadastrada neste workspace.

A Análise de Impacto requer conhecimento profundo do produto para mapear
corretamente módulos relacionados, fluxos dependentes e riscos de regressão.
Sem esse contexto, o resultado seria genérico e potencialmente enganoso.

Para desbloquear, uma das opções abaixo é necessária:

1. Criar uma skill de domínio para "[produto]" em .kiro/skills/[nome-produto]/
   com a base de conhecimento do produto (módulos, personas, fluxos, integrações).

2. Informar manualmente o contexto do produto como parte do prompt —
   descrevendo os módulos existentes, personas e integrações relevantes.

Skills de domínio disponíveis atualmente: nenhuma cadastrada — crie a primeira para o seu domínio Zupper.
```

---

## Fluxo de Trabalho

### Fase 1 — Leitura e contexto

1. Identificar o produto e ativar a skill de domínio correspondente
2. Ler o PRD de origem (se fornecido) — foco nas seções:
   - Metadados (produto, módulo, tipo)
   - Personas
   - Estrutura do Produto (módulos)
   - Requisitos Funcionais
   - Dependências e Integrações
3. Se não houver PRD, pedir ao usuário: feature, módulo afetado e descrição do que muda

### Fase 2 — Análise nas 5 dimensões

Executar as 5 dimensões **em sequência**, usando o conhecimento da skill de domínio:

**Dimensão 1 — Personas afetadas**
- Quem usa diretamente a funcionalidade que muda?
- Quem usa funcionalidades que dependem dela?
- Quem é impactado indiretamente (ex: relatórios, notificações, integrações)?
- Classificar cada persona: Impacto Direto / Indireto / Nenhum

**Dimensão 2 — Módulos e funcionalidades relacionados**
- Quais módulos do produto tocam o que está sendo alterado?
- Tipo de dependência: Dados (lê/escreve o mesmo dado) / Fluxo (uma etapa depende da outra) / Integração (chama API ou serviço externo)
- Nível de risco da dependência: Alto / Médio / Baixo

**Dimensão 3 — Riscos de regressão**
- O que pode quebrar silenciosamente com essa mudança?
- Classificar por severidade: Crítico / Alto / Médio / Baixo
- Classificar por probabilidade: Alta / Média / Baixa
- Prioridade de atenção = Severidade × Probabilidade

**Dimensão 4 — Dependências técnicas e integrações externas**
- Quais APIs, webhooks ou sistemas externos são chamados ou recebem dados da feature?
- O que muda nessas chamadas (payload, frequência, novo endpoint)?
- Risco de breaking change para consumidores externos

**Dimensão 5 — Recomendações de QA**
- Quais áreas precisam de regressão manual?
- Quais cenários novos precisam de cobertura de testes?
- Há necessidade de teste de performance ou carga?
- Prioridade por área: Crítica / Alta / Média

### Fase 3 — Score e sinalizações

- Calcular score de risco geral: ALTO / MÉDIO / BAIXO
  - ALTO: qualquer risco Crítico identificado, ou 3+ riscos Altos
  - MÉDIO: 1-2 riscos Altos, ou 3+ riscos Médios
  - BAIXO: apenas riscos Médios e Baixos, sem dependências externas críticas
- Listar sinalizações abertas: pontos que precisam ser validados com engenharia ou PO antes do desenvolvimento

### Fase 4 — Entregar o relatório

Usar o template em `references/impact-report-template.md`.
Salvar em `output-files/impact-[nome-feature]-[data].md`.

---

## Regras

- Nunca inventar módulos ou fluxos que não estejam na skill de domínio ou no PRD
- Se um impacto for incerto, marcar como `⚠️ Necessário validar com engenharia`
- Manter linguagem direta — o relatório deve ser executável por QA e Engenharia
- Focar no que muda, não em descrever o que já existe
- Score de risco deve ser conservador: na dúvida, elevar uma categoria

---

## Referências

| Arquivo | Conteúdo |
|---------|---------|
| `references/impact-report-template.md` | Template completo do relatório de impacto |
| `references/risk-classification.md` | Critérios de severidade, probabilidade e score geral |
