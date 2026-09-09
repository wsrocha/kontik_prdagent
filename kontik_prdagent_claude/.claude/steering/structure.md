---
inclusion: always
---

## Estrutura de Arquivos

### Camadas e responsabilidades
- `steering/`: regras, contratos e governança da geração.
- `steering/` pode ser dividido em: base genérica (cross-produto) e domínio específico (ex.: domínio de busca de passagens, domínio de pagamentos).
- `skills/`: ações executáveis e fluxo operacional.
- `templates/`: padrão de saída e estrutura dos documentos.
- `references/`: exemplos e checklists auxiliares consumidos pelas skills.

### Contrato de geração de PRD
- Seções obrigatórias nunca podem ser removidas.
- Seções opcionais sem dados devem permanecer com `N/A nesta versão` e justificativa curta.
- Lacunas críticas devem ser registradas como `Pendência` com próximo passo e responsável sugerido.

### Output de documentos gerados
- Todos os arquivos `.md` gerados pelo agente (PRDs, specs, documentações) devem ser salvos em `/output-files/`.
- Exemplo: `output-files/prd-alerta-preco-passagens.md`.
- Nunca salvar arquivos gerados na raiz do projeto.

### Estrutura compartilhada para escala
- Ativos compartilhados devem ser centralizados em `.shared/_assets/`.
- Pacotes específicos (ex.: `zupper/dev-analyst-kiro`) consomem esses ativos com fallback compatível.
- Conhecimentos de domínio devem seguir organização por `knowledge-packs/<dominio>/v<versao>/`.
