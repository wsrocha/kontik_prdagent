# Template — Relatório de Análise de Impacto

---

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RELATÓRIO DE ANÁLISE DE IMPACTO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Produto / Módulo:      [ex: Zupper — Busca de Passagens]
Feature analisada:     [ex: Alertas de Preço com Monitoramento em Tempo Real]
PRD de origem:         [nome do arquivo ou "não fornecido"]
Skill de domínio:      [ex: zupper-busca-passagens]
Analista (agente):     impact-analyzer v1.0.0
Data:                  [DD/MM/AAAA]

SCORE DE RISCO GERAL:  [ ALTO ⛔ | MÉDIO ⚠️ | BAIXO ✅ ]
Justificativa do score: [1 frase explicando o principal fator de risco]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. PERSONAS AFETADAS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Persona | Tipo de Impacto | Descrição do impacto |
|---------|----------------|----------------------|
| [Persona 1] | Direto | [O que muda na experiência/fluxo dela] |
| [Persona 2] | Indireto | [Como é afetada por dependência] |
| [Persona 3] | Nenhum | [Confirmação de que não é afetada] |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
2. MÓDULOS E FUNCIONALIDADES RELACIONADOS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Módulo / Funcionalidade | Tipo de dependência | Nível de risco | Descrição |
|------------------------|-------------------|---------------|-----------|
| [Módulo A] | Fluxo | Alto | [Por que depende e o que pode ser afetado] |
| [Módulo B] | Dados | Médio | [Compartilha qual dado e o risco de inconsistência] |
| [Módulo C] | Integração | Alto | [Qual API ou sistema externo e o que muda] |

Tipos de dependência:
- Fluxo: uma etapa do processo depende de outra (ex: repasse depende do status da vistoria)
- Dados: módulos leem/escrevem o mesmo dado (ex: status do envelope usado em relatórios)
- Integração: chamada a API externa ou sistema de terceiro

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
3. RISCOS DE REGRESSÃO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| # | Risco | Severidade | Probabilidade | Área afetada |
|---|-------|-----------|--------------|-------------|
| R1 | [Descrição do risco — o que pode quebrar] | Crítico | Alta | [Módulo/fluxo] |
| R2 | [Descrição do risco] | Alto | Média | [Módulo/fluxo] |
| R3 | [Descrição do risco] | Médio | Baixa | [Módulo/fluxo] |

Critério de priorização: Severidade × Probabilidade
Ver tabela completa em references/risk-classification.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
4. DEPENDÊNCIAS TÉCNICAS E INTEGRAÇÕES EXTERNAS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Sistema / API | Tipo | O que muda | Risco de breaking change |
|--------------|------|-----------|--------------------------|
| [Sistema externo A] | Webhook recebido | [Novo evento ou campo no payload] | [Sim / Não / ⚠️ Verificar] |
| [API externa B] | Chamada de saída | [Novo endpoint ou parâmetro] | [Sim / Não / ⚠️ Verificar] |
| [Integração C] | Dados compartilhados | [Campo que muda de formato ou semântica] | [Sim / Não] |

Se não houver dependências externas: registrar "Nenhuma dependência técnica externa identificada."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
5. RECOMENDAÇÕES DE QA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

| Área | Tipo de teste recomendado | Prioridade | Observação |
|------|--------------------------|-----------|-----------|
| [Módulo/fluxo A] | Regressão manual | Crítica | [Por que é prioritário] |
| [Módulo/fluxo B] | Regressão automatizada | Alta | [Cobertura existente ou nova] |
| [Feature nova C] | Teste funcional completo | Alta | [Cobrir happy path + edge cases do PRD] |
| [Integração D] | Teste de contrato (contract test) | Alta | [Validar payload com sistema externo] |
| [Volume / carga] | Teste de performance | Média | [Se aplicável — descrever cenário] |

Cenários de borda que surgem com esta mudança:
- [Cenário 1: descrever condição de borda nova criada pela feature]
- [Cenário 2: ...]
- [Cenário 3: ...]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
6. SINALIZAÇÕES ABERTAS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Pontos que precisam ser validados antes de iniciar o desenvolvimento:

| # | Sinalização | Com quem validar | Impacto se não resolvido |
|---|------------|-----------------|--------------------------|
| S1 | [Dúvida ou incerteza identificada] | [Engenharia / PO / QA / Jurídico] | [O que pode acontecer] |
| S2 | [⚠️ Ponto incerto no PRD ou na skill de domínio] | [...] | [...] |

Se não houver sinalizações: registrar "Nenhuma sinalização aberta identificada."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FIM DO RELATÓRIO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
