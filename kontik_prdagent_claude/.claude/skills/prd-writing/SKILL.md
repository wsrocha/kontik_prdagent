---
name: prd-writing
description: Skill completo para escrita de PRD (Product Requirements Document) estruturado, alinhado com melhores práticas de product management. Cobre discovery, análise de concorrentes, definição de personas, requisitos funcionais/não-funcionais, compliance LGPD, cenários de teste e métricas de sucesso. Use quando precisar criar, refinar ou validar um PRD completo.
license: MIT
compatibility: Requer acesso a web para análise de concorrentes. Contexto de domínio ativo: Zupper (OTA B2C — passagens aéreas, hospedagem e pacotes de viagem, web e mobile).
metadata:
  category: product-management
  version: 1.0.0
  language: pt-BR
  use-cases: "novo-produto, feature, modulo, jornada, melhoria"
allowed-tools: Read Write Edit WebSearch WebFetch
---
<!-- Porte Kiro -> Claude Code: allowed-tools original era "readFile readMultipleFiles fsWrite strReplace remote_web_search webFetch". Ver PORTING-NOTES.md para o mapeamento completo. Nenhum outro conteúdo desta skill foi alterado. -->

# PRD Writing Skill

Skill especializado na acao de criar, refinar e validar PRDs (Product Requirements Documents) usando as regras ativas do workspace e os templates oficiais do projeto.

## Quando Usar Este Skill

- Criar um novo PRD do zero
- Refinar ou validar um PRD existente
- Estruturar descobertas de produto em documento executável
- Documentar features, módulos ou jornadas de usuário
- Preparar PRD para homologação com stakeholders

## Fluxo de Trabalho Recomendado

### Fase 1: Discovery e Orquestração
1. Extrair do briefing e dos anexos tudo que ja estiver explicito.
2. Fazer apenas as perguntas faltantes para completar as entradas minimas.
3. Perguntar explicitamente o tipo de entrega: Novo produto, Novo modulo, Nova jornada, Nova feature, Melhoria-Evolucao, Correcao.
4. Se o usuario nao souber o tipo de entrega, oferecer exemplos curtos e pedir escolha.
5. Confirmar linha de solucao e produto.
6. Confirmar se ha IA embarcada e/ou construcao de tela no sistema.
7. Confirmar plataforma alvo: mobile e/ou desktop.
8. Se o usuario nao souber algum item, registrar como TBD com responsavel sugerido.
9. Classificar profundidade com base no tipo de entrega conforme steering normativo.
10. Confirmar o local de saida antes de gerar o documento.

### Fase 2: Análise de Mercado
1. Identificar concorrentes diretos e indiretos relevantes para o contexto.
2. Montar uma comparacao objetiva de funcionalidades, pricing e diferenciais.
3. Registrar oportunidades de diferenciacao e lacunas de mercado.

### Fase 3: Estruturação do PRD
1. Gerar o PRD a partir do template oficial unico.
2. Preencher as secoes com base no briefing, nas respostas coletadas e nas regras ativas do workspace.
3. Quando faltar dado obrigatorio, registrar lacuna e orientar validacao.
4. Quando uma secao opcional nao se aplicar, manter a secao conforme o contrato do projeto.

### Fase 4: Validação
1. Revisar o documento gerado contra as regras e `prd-guardrails.md`.
2. Gerar score de qualidade.
3. Listar problemas por severidade.
4. Sugerir melhorias acionaveis.

### Fase 5: Saída
1. Salvar o arquivo no local confirmado com o nome padrao do projeto.
2. Entregar o PRD junto com o resultado da revisao.
3. Destacar pendencias criticas, quando existirem.

## Responsabilidade da Skill

- Esta skill descreve a acao de gerar e revisar PRDs.
- As regras persistentes do projeto pertencem aos arquivos de `steering/`.
- O template oficial pertence a `.kiro/templates/`.
- Os arquivos em `references/` sao materiais auxiliares, exemplos e checklists operacionais.
- Esta skill executa o fluxo operacional que o steering `prd-orchestrator.md` apenas normatiza.

## Entradas Esperadas

- Briefing funcional ou problema de negocio.
- Respostas para as perguntas de discovery que estiverem faltando.
- Materiais anexados e evidencias disponiveis.

## Saídas Esperadas

- PRD gerado usando o template oficial unico do projeto.
- Revisao do PRD com score, problemas e melhorias.
- Registro explicito de lacunas e pendencias, quando houver.
- Registro explicito das respostas de discovery coletadas e dos itens em TBD, quando houver.

## Referências Operacionais

- Template PRD: `.kiro/skills/prd-writing/references/prd-template.md`
- Checklist de validacao: `.kiro/skills/prd-writing/references/validation-checklist.md`
- Exemplos de requisitos funcionais: `.kiro/skills/prd-writing/references/functional-requirements.md`
- Exemplos de personas: `.kiro/skills/prd-writing/references/personas-examples.md`
- Apoio para benchmark: `.kiro/skills/prd-writing/references/competitor-analysis.md`
- Apoio para compliance: `.kiro/skills/prd-writing/references/compliance-lgpd.md`

## Notas de Execução

- Nao duplicar contratos, nomenclaturas ou regras que pertencem ao steering.
- Nao transformar arquivos de referencia em fonte oficial de governanca.
- Ao encontrar conflito entre referencias auxiliares e steering, prevalece o steering do workspace.

## Dicas Práticas

1. **Comece pelo problema**: Não pule a fase de discovery. Um PRD bem fundamentado economiza semanas de desenvolvimento.

2. **Valide com stakeholders**: Antes de finalizar, circule o PRD com PO, design, engenharia e compliance.

3. **Use templates**: Reutilize estruturas de PRDs anteriores para manter consistência.

4. **Seja específico**: Evite "melhorar experiência do usuário". Diga "reduzir tempo de aprovação de 5 dias para 1 dia".

5. **Documente incertezas**: Se não souber algo, sinalize como "TBD" (To Be Determined) e defina quem vai resolver.

6. **Vincule a métricas**: Cada objetivo deve ter uma métrica mensurável e um prazo de medição.

7. **Considere LGPD desde o início**: Não deixe compliance para o final. Mapear dados sensíveis desde a discovery.

8. **Cenários de teste desde o início**: Escrever testes enquanto define requisitos melhora a qualidade.
