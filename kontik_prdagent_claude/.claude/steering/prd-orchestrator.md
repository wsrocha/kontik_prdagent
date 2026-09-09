Contrato do Orquestrador de PRD (Steering Normativo)

Objetivo:
- Definir o contrato de orquestração para geração de PRD sem descrever passo a passo operacional.
- A execução do fluxo pertence à skill `prd-writing`.

Escopo do orquestrador:
- Garantir classificação correta do tipo de entrega.
- Garantir seleção de profundidade adequada.
- Garantir aplicação dos steerings normativos e do template oficial.
- Garantir desacoplamento entre regras genéricas e regras de domínio.

Entradas mínimas obrigatórias:
- Tipo de entrega.
- Linha de solução.
- Produto.
- Contexto de IA embarcada e/ou construção de tela no sistema.
- Plataforma alvo: mobile e/ou desktop.

Taxonomia canônica de tipo de entrega:
- Novo produto
- Novo módulo
- Nova jornada
- Nova feature
- Melhoria-Evolução
- Correção

Regra de profundidade:
- Preenchimento aprofundado: Novo produto, Novo módulo, Nova jornada.
- Preenchimento enxuto: Nova feature, Melhoria-Evolução, Correção.
- Em caso de dúvida: usar preenchimento aprofundado apenas quando houver criação de nova jornada ponta a ponta; caso contrário usar preenchimento enxuto.

Referências normativas obrigatórias:
- Template oficial: `.kiro/templates/prd/prd-template.md`.
- Escrita: `prd-writing-rules.md`.
- Validação: `prd-validation-rules.md`.
- Guardrails: `prd-guardrails.md`.
- Benchmark: `market-competitors.md`.

Critérios de qualidade obrigatórios:
- Não inventar dados.
- Sinalizar lacunas críticas como "Pendência" com próxima ação recomendada.
- Manter objetividade comparativa e foco em impacto de produto e decisão estratégica.

Saída esperada do processo de orquestração:
- PRD estruturado conforme template oficial e profundidade correta por tipo de entrega.
- Benchmark de mercado incorporado ao PRD conforme `market-competitors.md`.

Regra de desacoplamento de domínio:
- Regras específicas de produto/vertical não devem ficar neste orquestrador genérico.
- Quando houver steering de domínio ativo (ex.: `prd-domain-busca-passagens.md`), aplicar esse steering em complemento ao fluxo base.
