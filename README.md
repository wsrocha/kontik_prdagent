# Agente de PRD — Zupper

> Guia completo para Product Owners e times de produto entenderem, usarem e evoluírem este agente de criação de PRDs.

---

## O que é isso aqui?

Este repositório é a "caixa de ferramentas" de um agente de IA configurado dentro do Kiro para ajudar times de produto do Zupper a criar PRDs (Product Requirements Documents) de forma estruturada, consistente e rápida.

Em vez de cada PO começar um PRD do zero, com formatos diferentes e seções faltando, o agente garante que todo PRD siga o mesmo padrão, faça as perguntas certas antes de escrever, e entregue um documento pronto para ser consumido por times de design, engenharia e negócio.

O agente cobre **todos os produtos e domínios do Zupper**. Pode ser usado para qualquer área: Busca e Comparação, Reservas, Pagamentos, Experiência do Viajante, Suporte, Backoffice, etc.

---

## Como o agente funciona (visão geral)

O agente segue um fluxo em três etapas:

```
1. PERGUNTAS INICIAIS  →  2. ANÁLISE E CONTEXTO  →  3. GERAÇÃO DO PRD
```

**Etapa 1 — Perguntas iniciais**
Antes de escrever qualquer coisa, o agente busca no briefing e nos materiais anexados as respostas para as perguntas obrigatórias. Só faz as perguntas cujas respostas não foram identificadas no prompt inicial. As perguntas são:
- É inovação ou melhoria/evolução de produto?
- Para qual produto/domínio do Zupper será o PRD?
- Para qual funcionalidade ou módulo específico?
- Terá IA embarcada e/ou construção de tela no sistema?
- É para mobile e/ou desktop?

**Etapa 2 — Análise e contexto**
Com as respostas em mãos, o agente:
- Analisa o briefing e identifica o problema central
- Pesquisa concorrentes (mínimo 3: Decolar, 123milhas, Viajanet, CVC e outros relevantes)
- Monta uma análise comparativa de funcionalidades, pricing e diferenciais
- Identifica oportunidades de mercado não exploradas

**Etapa 3 — Geração do PRD**
O agente escreve o PRD completo usando o template oficial do tipo de entrega e o contrato definido pelos steering files, revisa criticamente o próprio documento e entrega um score de qualidade.

---

## Estrutura do repositório

```
/
├── README.md                          ← você está aqui
├── .gitignore                         ← impede que output-files/ suba ao GitHub
├── prd-guide.md                       ← guia rápido de uso
├── output-files/                      ← pasta local para salvar PRDs gerados (não vai ao GitHub)
│
├── .kiro/
│   ├── steering/                      ← regras e instruções do agente
│   │   ├── prd-orchestrator.md        ← contrato normativo do orquestrador de PRD (classificação, profundidade e referências)
│   │   ├── prd-validation-rules.md    ← regras de revisão e score de qualidade
│   │   ├── prd-general-guidelines.md  ← diretrizes gerais de execução e convenções globais
│   │   ├── product.md                 ← regras de discovery e definição de problema
│   │   ├── prd-writing-rules.md       ← regras de escrita do PRD completo
│   │   ├── market-competitors.md      ← regras de análise de concorrentes
│   │   ├── product-tree.md            ← árvore de produto: domínios, produtos e POs
│   │   ├── prd-guardrails.md          ← 16 guardrails de qualidade e segurança para PRD
│   │   ├── lgpd-and-compliance.md     ← regras de compliance e LGPD
│   │   └── structure.md               ← estrutura de camadas, contratos e output
│   │
│   ├── templates/prd/                 ← template oficial de saída do PRD
│   │   └── prd-template.md            ← template oficial único do PRD
│   │
│   └── skills/prd-writing/            ← skill de escrita de PRD
│       ├── SKILL.md                   ← instruções e fluxo completo do skill
│       └── references/                ← arquivos de referência usados pelo skill
│           ├── prd-template.md
│           ├── validation-checklist.md
│           ├── personas-examples.md
│           ├── functional-requirements.md
│           ├── competitor-analysis.md
│           └── compliance-lgpd.md
│
└── aidlc-docs/                        ← documentação interna do ciclo de vida do agente
    ├── aidlc-state.md                 ← estado atual do projeto
    ├── audit.md                       ← log de auditoria das execuções
    └── inception/
        ├── requirements/requirements.md
        └── plans/execution-plan.md
```

---

## O que são os Steering Files?

Steering files são arquivos `.md` dentro de `.kiro/steering/` que funcionam como **instruções permanentes para o agente**. Pense neles como o "manual de comportamento" do agente — toda vez que ele executa uma tarefa, ele lê esses arquivos e segue as regras definidas neles.

Cada arquivo tem uma responsabilidade específica:

| Arquivo | Responsabilidade |
|---------|-----------------|
| `prd-orchestrator.md` | Define contrato normativo de orquestração: entradas mínimas, regra de profundidade e referências obrigatórias |
| `prd-validation-rules.md` | Como o agente revisa e pontua a qualidade do PRD gerado |
| `prd-general-guidelines.md` | Diretrizes gerais de execução, idioma e convenções globais |
| `product.md` | Como o agente interpreta briefings e define o problema central |
| `prd-writing-rules.md` | As 18 seções obrigatórias do PRD e o que cada uma deve conter |
| `market-competitors.md` | Como identificar e comparar concorrentes (Decolar, 123milhas, Viajanet, CVC...) |
| `product-tree.md` | Árvore de produto completa: domínios, produtos e PO designado por produto |
| `prd-guardrails.md` | 16 guardrails obrigatórios de PRD: foco em problema antes de solução, anti-alucinação, LGPD, controle de escopo, rastreabilidade, consistência entre seções e qualidade mínima para aprovação |
| `lgpd-and-compliance.md` | Regras de LGPD e compliance |
| `structure.md` | Padrão de estrutura por camadas, contrato de seções e convenções de output |

### Separação de responsabilidades

- `steering/` é a fonte oficial de regras, contratos, nomenclaturas e critérios.
- `templates/` contém apenas a estrutura oficial de saída do PRD.
- `skills/` executam o workflow e não devem duplicar governança.
- `references/` são materiais auxiliares, checklists e exemplos.

### Template oficial

- `.kiro/templates/prd/prd-template.md` é o template oficial único do PRD.
- A diferença entre novo produto/módulo/jornada e feature/melhoria/correção fica na profundidade de preenchimento definida pelo steering.

### Como o Kiro usa os steering files

Por padrão, **todos os steering files são carregados automaticamente** em toda interação com o agente. Isso significa que as regras definidas neles estão sempre ativas — você não precisa lembrar de "ativar" nada.

É possível configurar um steering file para ser carregado apenas em situações específicas (por exemplo, só quando um arquivo `.java` é aberto), mas por ora todos estão configurados como "sempre ativos".

---

## As 18 seções do PRD

Todo PRD gerado por este agente deve conter estas seções, nesta ordem:

| # | Seção | Para que serve |
|---|-------|---------------|
| 1 | Metadados do Documento | Rastreabilidade e governança (produto, tipo, PO, versão, status) |
| 2 | Visão do Produto | Alinhamento estratégico — o quê, para quem e por quê |
| 3 | Problema e Evidências | Justificar a iniciativa com dados reais (as-is, dores, benchmark) |
| 4 | Objetivos de Negócio | Metas mensuráveis com métricas e prazos |
| 5 | Pricing | Custos identificados e modelos de precificação possíveis |
| 6 | Personas | Perfis de usuário, necessidades e valor recebido |
| 7 | Estrutura do Produto | Módulos/blocos lógicos — mapa macro da feature |
| 8 | Fluxo do Usuário | Happy path e fluxos alternativos passo a passo |
| 9 | Requisitos Funcionais | Cada RF com regras, entradas, saídas e erros |
| 10 | Requisitos Não Funcionais | Performance, segurança, acessibilidade, escalabilidade |
| 11 | Compliance, LGPD e Requisitos Legais | Dados sensíveis e obrigações regulatórias |
| 12 | Entregáveis | O que será entregue: UI, API, relatórios, docs |
| 13 | Cenários de Teste (QA) | Cenários positivos e negativos com resultado esperado |
| 14 | Dependências e Integrações | Dependências internas e integrações externas |
| 15 | Riscos e Mitigações | Riscos com impacto, probabilidade e plano de mitigação |
| 16 | Métricas de Sucesso | Baseline, meta e prazo de medição pós-release |
| 17 | Glossário e Referências | Termos padronizados e fontes |
| 18 | Histórico de Versões | Rastreabilidade do próprio documento |

---

## Como usar o agente (passo a passo)

### 1. Abra o Kiro e inicie uma conversa

Abra o chat do Kiro neste workspace. O agente já estará carregado com todas as regras dos steering files.

### 2. Forneça o briefing

Descreva a funcionalidade ou produto que você quer documentar. Pode ser um texto livre, um rascunho, uma apresentação colada no chat, ou até um arquivo anexado. Quanto mais contexto, melhor o resultado.

Exemplo:
```
Quero criar um PRD para um sistema de alertas de preço de passagens aéreas
no aplicativo Zupper.
```

### 3. Responda as perguntas iniciais

O agente vai fazer as perguntas antes de começar a escrever, executando a skill de PRD e respeitando o contrato do `prd-orchestrator.md`. Responda com clareza — essas respostas definem o tom e o escopo de todo o PRD.

### 4. Aguarde a análise de concorrentes

O agente vai pesquisar automaticamente como Decolar, 123milhas, Viajanet, CVC e outros resolvem o mesmo problema. Isso alimenta a seção de benchmark do PRD.

### 5. Receba e revise o PRD

O agente entrega o PRD completo e já faz uma auto-revisão com score de qualidade (0 a 10) e lista de pontos de melhoria. Você pode pedir ajustes diretamente no chat.

### 6. Salve o arquivo

O agente vai perguntar onde você quer salvar o PRD antes de gerá-lo. As opções são:

- **Opção A — recomendada**: pasta `output-files/` dentro deste workspace. Os arquivos salvos aqui ficam apenas no seu computador e **não são enviados ao GitHub**.
- **Opção B**: informe um caminho personalizado, como `C:\Users\seu-nome\Desktop\`.

Se não souber o que escolher, confirme a Opção A. O agente vai salvar automaticamente.

O nome do arquivo segue o padrão:
```
prd-[nome-da-feature]-[data].md
```
Exemplos:
- `prd-alerta-preco-passagens-2026-09-08.md`
- `prd-checkout-hotel-mobile-2026-09-08.md`

---

## Como criar ou editar um Steering File

Steering files são arquivos de texto simples em Markdown. Para criar ou editar um:

### Criando um novo steering file

1. Crie um arquivo `.md` dentro de `.kiro/steering/`
2. Escreva as instruções em linguagem natural, como se estivesse explicando para uma pessoa o que ela deve fazer
3. Seja específico: diga o que o agente deve fazer, como deve fazer, e qual o output esperado
4. Salve o arquivo — o Kiro carrega automaticamente na próxima interação

### Estrutura recomendada para um steering file

```markdown
Sua responsabilidade é:
- [O que o agente deve fazer neste contexto]

Regras:
- [Restrição ou comportamento obrigatório]
- [Outra regra]

Output esperado:
- [O que deve ser entregue]
- [Formato esperado]
```

### Editando um steering file existente

Abra o arquivo em `.kiro/steering/`, edite diretamente e salve. As mudanças entram em vigor na próxima conversa com o agente.

### Boas práticas para steering files

- **Seja direto e imperativo**: "Identifique concorrentes" é melhor que "Você pode identificar concorrentes"
- **Evite ambiguidade**: Se uma regra pode ser interpretada de duas formas, reescreva
- **Um arquivo por responsabilidade**: Não misture regras de personas com regras de compliance no mesmo arquivo
- **Documente o propósito**: Adicione uma linha no topo explicando para que serve o arquivo
- **Teste após mudanças**: Faça uma pergunta de teste ao agente para verificar se o comportamento mudou como esperado

---

## Estrutura compartilhada para escala

Foi criada uma base compartilhada em `.shared/_assets/` para organizar ativos reutilizáveis por pacote:
- `steering/` para governança e contratos comuns.
- `templates/` para padronização de saída.
- `scripts/` para utilitários comuns.
- `knowledge-packs/` para organização de conhecimento por domínio e versão.

### Organização de knowledge packs (planejado)

Os domínios serão organizados por demanda, sem conteúdo inicial neste ciclo:
- `knowledge-packs/busca-passagens/v1/`
- `knowledge-packs/reservas-hotel/v1/`
- `knowledge-packs/checkout-pagamentos/v1/`

Cada pack deverá conter metadados de origem, data da última atualização e responsável funcional.

---

## Direcionadores estratégicos

Ao preencher os metadados do PRD, use sempre um dos quatro direcionadores estratégicos:

| Direcionador | Quando usar |
|-------------|-------------|
| **Densidade** | Funcionalidade que aprofunda o valor de um produto existente para clientes atuais |
| **Cobertura** | Funcionalidade que expande o produto para novos segmentos ou mercados |
| **Sustentação** | Correções, melhorias de performance, débito técnico, compliance |
| **Inovação** | Funcionalidade nova que não existe no mercado ou usa tecnologia disruptiva (ex: IA) |

Um PRD pode ter mais de um direcionador, mas deve ter pelo menos um.

---

## Tipos de entrega

No campo "Tipo" dos metadados, use sempre uma destas categorias:

- **Novo produto** — produto que não existe no Zupper
- **Novo módulo** — módulo novo dentro de um produto existente
- **Nova jornada** — fluxo novo dentro de um módulo existente
- **Nova feature** — funcionalidade nova dentro de uma jornada existente
- **Melhoria/Evolução** — aprimoramento de algo que já existe
- **Correção** — bug fix ou ajuste de comportamento incorreto

---

## Perguntas frequentes

**O agente substitui o trabalho do PO?**
Não. O agente acelera a documentação e garante consistência, mas o PO ainda precisa fornecer o contexto, validar o conteúdo gerado e tomar as decisões de produto. O agente é um copiloto, não um piloto automático.

**O que faço se o PRD gerado estiver incompleto?**
Peça ao agente para completar seções específicas no chat. Você pode dizer, por exemplo: "Complete a seção de Requisitos Funcionais com mais detalhes sobre validações de entrada" e o agente vai reescrever apenas aquela parte.

**Como adiciono um novo concorrente para o agente monitorar?**
Edite o arquivo `.kiro/steering/market-competitors.md` e adicione o nome e URL do concorrente na lista de exemplos. O agente vai incluí-lo nas próximas análises.

**Posso ter PRDs em subpastas por domínio de produto?**
Sim. Basta criar as pastas e atualizar o `structure.md` com a convenção adotada. O agente vai seguir a estrutura definida lá.

---

## Manutenção automática deste documento

Este README é atualizado automaticamente pelo agente sempre que mudanças com impacto no seu conteúdo forem detectadas no workspace. Dois gatilhos estão configurados:

- **Edição de steering file** — quando qualquer arquivo em `.kiro/steering/` for salvo, o agente avalia se a mudança é significativa (nova regra, novo escopo, arquivo antes vazio que foi preenchido) e atualiza as seções impactadas do README de forma cirúrgica.
- **Criação de novo arquivo** — quando um novo `.md` for criado no workspace (novo PRD, novo steering file, novo guia), o agente avalia se o arquivo deve ser refletido no README (ex: estrutura do repositório, tabela de steering files) e faz a atualização necessária.

Mudanças pequenas ou cosméticas (ajustes de texto sem impacto estrutural) não disparam atualização. O agente só age quando há impacto real no conteúdo documentado aqui.

---

## Histórico deste documento

| Versão | Data | Autor | Alterações |
|--------|------|-------|-----------|
| v1.0 | 09/04/2026 | Kiro (gerado) | Criação do documento |
| v1.1 | 09/04/2026 | Kiro (gerado) | Adicionada seção de manutenção automática |
| v1.2 | 09/04/2026 | Kiro (gerado) | guardrails.md preenchido — atualizada tabela de steering files e removido da lista de vazios |
| v1.3 | 13/04/2026 | Kiro (gerado) | Ajustada a lógica de decisão do agente para definição do tipo de PRD |
| v1.4 | 14/04/2026 | Kiro (gerado) | product-tree.md criado — árvore de produto com domínios, produtos e POs |
| v1.5 | 14/04/2026 | Kiro (gerado) | Renomeado guardrails.md para prd-guardrails.md para explicitar especialização em PRD |
| v2.0 | 08/09/2026 | Kiro (gerado) | Migração do agente para o contexto Zupper/Kontikx |
| v2.1 | 08/09/2026 | Kiro (gerado) | product-tree.md atualizado — árvore migrada para Zupper (domínios: Busca e Comparação, Reservas e Checkout, Pagamentos, Experiência do Viajante, Atendimento, Plataforma, Dados e Analytics) |
