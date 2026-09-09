# Checklist de Validação de PRD

Use este checklist para validar a qualidade e completude do seu PRD antes de submeter para stakeholders.

> Este arquivo e auxiliar. A fonte oficial de contrato e severidade continua nos steering files do workspace.
> Em caso de conflito, prevalecem `prd-writing-rules.md`, `prd-validation-rules.md` e `prd-guardrails.md`.

## Metadados e Governança

- [ ] Domínio de produto identificado (ex: Busca e Comparação / Reservas e Checkout / Pagamentos / Experiência do Viajante / Atendimento / Plataforma / Dados e Analytics)
- [ ] Vertical identificada, quando aplicável (ex: Passagens Aéreas / Hospedagem / Pacotes)
- [ ] Produto identificado conforme árvore de produto em `.kiro/steering/product-tree.md`
- [ ] Product Owner designado conforme `.kiro/steering/product-tree.md` — consulte o arquivo para confirmar o PO correto do produto
- [ ] Funcionalidade nomeada de forma descritiva
- [ ] Tipo definido de forma consistente com o template selecionado
- [ ] Status atualizado
- [ ] Stakeholders listados
- [ ] Data de criação preenchida
- [ ] Versão iniciada (v0.1)

## Visão do Produto

- [ ] O que é? — Definição clara em uma frase
- [ ] Para quem? — Público-alvo identificado
- [ ] Qual o valor? — Benefício central explícito
- [ ] Resumo executivo — Contexto estratégico claro
- [ ] Alinhamento com objetivos de negócio

## Problema e Evidências

- [ ] Estado atual (As-Is) descrito
- [ ] Workarounds mencionados
- [ ] Mínimo 3 fontes de evidência de dor
- [ ] Dados quantificados (% de retrabalho, volume de tickets, etc.)
- [ ] Mínimo 3 concorrentes analisados
- [ ] Funcionalidades dos concorrentes documentadas
- [ ] Oportunidades de diferenciação identificadas

## Objetivos de Negócio

- [ ] Mínimo 3 objetivos definidos
- [ ] Cada objetivo vinculado a uma métrica mensurável
- [ ] Métricas com valores específicos (não genéricas)
- [ ] Prazos de medição definidos
- [ ] Alinhamento com estratégia da empresa

## Pricing

- [ ] Custos identificados, quando aplicável
- [ ] Modelos de precificação explorados, quando aplicável
- [ ] Impacto financeiro estimado, quando aplicável
- [ ] ROI calculado, se aplicável

## Personas

- [ ] Mínimo 3 personas definidas
- [ ] Cada persona com perfil claro
- [ ] Necessidade principal de cada persona
- [ ] Valor recebido pela feature
- [ ] Cada persona vinculada a requisitos funcionais

## Estrutura do Produto

- [ ] Módulos/blocos lógicos identificados
- [ ] Cada módulo com propósito claro
- [ ] Funcionalidades agrupadas coerentemente
- [ ] Visão macro do produto clara
- [ ] Navegação do PRD facilitada

## Jornada do Usuário

- [ ] Happy path descrito passo-a-passo
- [ ] Mínimo 5 passos no fluxo principal
- [ ] Fluxos alternativos identificados
- [ ] Cada fluxo com valor agregado
- [ ] Pontos de decisão claros
- [ ] Possíveis erros/exceções mencionados

## Requisitos Funcionais

- [ ] Mínimo 10 requisitos funcionais
- [ ] Cada RF com descrição clara
- [ ] Regras de negócio explícitas
- [ ] Dados de entrada definidos
- [ ] Dados de saída especificados
- [ ] Comportamento de erro documentado
- [ ] Dependências mapeadas
- [ ] Prioridade (MoSCoW) atribuída
- [ ] Cada RF vinculado a persona(s)
- [ ] Cada RF vinculado a objetivo(s) de negócio
- [ ] Requisitos suficientemente detalhados para quebra em tarefas

## Requisitos Não Funcionais

- [ ] Performance definida (tempo de resposta, throughput)
- [ ] Responsividade (desktop, mobile, tablet)
- [ ] Segurança (autenticação, autorização, criptografia)
- [ ] Auditoria (logs, rastreabilidade)
- [ ] Escalabilidade (volume de dados, usuários simultâneos)
- [ ] Acessibilidade (WCAG 2.1 nível AA)
- [ ] Compatibilidade (browsers, sistemas operacionais)
- [ ] Disponibilidade (uptime, SLA)

## Compliance, LGPD e Requisitos Legais

- [ ] Dados sensíveis mapeados
- [ ] Trilha de auditoria definida
- [ ] Matriz de segurança respeitada
- [ ] Fidelidade legal garantida
- [ ] Disclaimers e marcas d'água definidas
- [ ] Integrações regulatórias consideradas
- [ ] Consentimento do usuário documentado
- [ ] Direito ao esquecimento considerado
- [ ] Portabilidade de dados considerada

## Entregáveis

- [ ] Interface (UI) descrita
- [ ] Backend / API especificado
- [ ] Relatórios / PDFs definidos
- [ ] Integrações listadas
- [ ] Documentação planejada
- [ ] Cada entregável com descrição clara
- [ ] Escopo concreto de delivery

## Cenários de Teste (QA)

- [ ] Mínimo 5 cenários positivos (happy path)
- [ ] Mínimo 5 cenários negativos (edge cases)
- [ ] Cada cenário com descrição clara
- [ ] Condições de teste especificadas
- [ ] Resultado esperado documentado
- [ ] Cobertura de validações
- [ ] Cobertura de permissões
- [ ] Cobertura de limites
- [ ] Cobertura de erros
- [ ] Cenários de integração considerados

## Dependências e Integrações

- [ ] Dependências internas mapeadas
- [ ] Tipo de dependência identificado (dados, serviço, etc.)
- [ ] Integrações externas listadas
- [ ] Impacto de cada dependência avaliado
- [ ] Plano de desbloqueio definido
- [ ] Riscos de dependência considerados

## Riscos e Mitigações

- [ ] Mínimo 3 riscos identificados
- [ ] Cada risco com impacto avaliado
- [ ] Probabilidade estimada
- [ ] Plano de mitigação definido
- [ ] Responsável pela mitigação designado
- [ ] Riscos técnicos considerados
- [ ] Riscos de negócio considerados
- [ ] Riscos de compliance considerados

## Métricas de Sucesso

- [ ] Mínimo 3 métricas definidas
- [ ] Baseline (situação atual) documentado
- [ ] Meta específica e mensurável
- [ ] Prazo de medição definido
- [ ] Responsável pela medição designado
- [ ] Ferramenta de medição identificada
- [ ] Frequência de acompanhamento definida

## Plano de Execução

- [ ] Se aplicável, fases definidas com entregáveis claros
- [ ] Se aplicável, duração estimada em sprints
- [ ] Se aplicável, dependências entre fases mapeadas
- [ ] Se aplicável, gates de qualidade definidos
- [ ] Se aplicável, responsáveis por fase designados
- [ ] Se aplicável, riscos de timeline considerados

## Glossário e Referências

- [ ] Termos-chave definidos
- [ ] Siglas explicadas
- [ ] Referências documentadas
- [ ] Links funcionais
- [ ] Documentos anexados (se necessário)

## Histórico de Versões

- [ ] Versão inicial registrada
- [ ] Data de criação preenchida
- [ ] Autor identificado
- [ ] Alterações documentadas

## Qualidade Geral

- [ ] Linguagem clara e objetiva
- [ ] Sem ambiguidades
- [ ] Sem lacunas de informação (ou sinalizadas como TBD)
- [ ] Consistência de termos
- [ ] Formatação padronizada
- [ ] Fácil navegação
- [ ] Pronto para desenvolvimento
- [ ] Pronto para homologação com stakeholders

## Score de Qualidade

Conte o número de itens marcados:

- **90-100%**: Excelente — PRD pronto para desenvolvimento
- **80-89%**: Bom — Pequenos ajustes necessários
- **70-79%**: Aceitável — Revisão recomendada
- **60-69%**: Fraco — Revisão significativa necessária
- **< 60%**: Inadequado — Reescrever seções críticas

**Score Final: ___/100**

## Próximos Passos

1. Circule o PRD com stakeholders (PO, Design, Engenharia, Compliance)
2. Colete feedback
3. Atualize versão (v0.2, v0.3, etc.)
4. Registre alterações no histórico de versões
5. Obtenha aprovação final
6. Compartilhe com times de desenvolvimento
