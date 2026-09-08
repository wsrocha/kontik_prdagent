# Classificação de Risco — Referência

Critérios usados pelo `impact-analyzer` para classificar riscos de regressão
e calcular o score de risco geral do relatório.

---

## Severidade

| Nível | Critério | Exemplos |
|-------|---------|---------|
| **Crítico** | Quebra um fluxo principal que bloqueia o usuário ou gera perda de dados | Busca de voos não retorna resultados; checkout não finaliza; dados do viajante são perdidos |
| **Alto** | Degrada significativamente a experiência ou causa erro visível para o usuário | Status incorreto exibido; notificação não enviada; relatório com dados errados |
| **Médio** | Impacto funcional limitado, workaround possível | Botão desabilitado incorretamente; campo pré-preenchido errado; log de auditoria incompleto |
| **Baixo** | Impacto cosmético ou de UX sem consequência funcional | Label incorreto; ordenação de lista errada; tooltip desatualizado |

---

## Probabilidade

| Nível | Critério |
|-------|---------|
| **Alta** | A mudança altera código, dados ou fluxo diretamente compartilhado com o módulo afetado |
| **Média** | A mudança altera comportamento adjacente — módulo afetado consome indiretamente |
| **Baixa** | Dependência existe mas é isolada por camada de abstração (ex: interface estável, dados lidos em read-only) |

---

## Matriz de Prioridade (Severidade × Probabilidade)

|  | Probabilidade Alta | Probabilidade Média | Probabilidade Baixa |
|--|-------------------|--------------------|--------------------|
| **Crítico** | 🔴 P1 — Endereçar antes de qualquer desenvolvimento | 🔴 P1 | 🟠 P2 |
| **Alto** | 🔴 P1 | 🟠 P2 | 🟡 P3 |
| **Médio** | 🟠 P2 | 🟡 P3 | 🟢 P4 |
| **Baixo** | 🟡 P3 | 🟢 P4 | 🟢 P4 |

- **P1** — Bloqueante: deve ser endereçado antes de iniciar o desenvolvimento ou exige plano de mitigação aprovado
- **P2** — Alta atenção: incluir no escopo de testes de regressão prioritários
- **P3** — Monitorar: incluir nos testes de regressão, não bloqueia desenvolvimento
- **P4** — Registrar: documentar e monitorar pós-release

---

## Score de Risco Geral

| Score | Critério |
|-------|---------|
| **ALTO ⛔** | Qualquer risco P1 identificado, OU 3 ou mais riscos P2 |
| **MÉDIO ⚠️** | 1–2 riscos P2, OU 3 ou mais riscos P3, sem P1 |
| **BAIXO ✅** | Apenas riscos P3 e P4, sem dependências externas críticas |

**Regra conservadora:** na dúvida entre dois scores, elevar uma categoria.
A subestimação de risco é sempre mais custosa do que a superestimação.

---

## Tipos de Dependência

| Tipo | Descrição | Risco típico |
|------|---------|-------------|
| **Fluxo** | Uma etapa do processo depende de outra (sequência obrigatória) | Quebra de fluxo downstream se estado intermediário mudar |
| **Dados** | Módulos diferentes leem ou escrevem o mesmo dado ou entidade | Inconsistência de estado, cache desatualizado, relatório errado |
| **Integração** | Chamada a API externa, webhook ou sistema de terceiro | Breaking change, timeout, payload incompatível |
| **Permissão** | Perfis de acesso ou regras de autorização compartilhadas | Acesso indevido ou bloqueio incorreto após mudança |
| **Configuração** | Parâmetros de sistema ou tenant compartilhados | Mudança afeta todos os clientes ou apenas os com configuração específica |
