Template PRD — Modelo Unificado
Quando usar: Qualquer tipo de entrega de PRD.
Propósito: Modelo padronizado de PRD que concentra todas as secoes obrigatorias e opcionais do contrato do projeto em um unico documento-base. O nivel de profundidade de preenchimento depende do tipo de entrega definido no steering.
 
Metadados do Documento
Campo	Valor
Produto / Módulo	[Nome do produto ou módulo]
Funcionalidade	[Nome da feature]
Tipo	[Novo produto / Novo módulo / Nova jornada / Nova feature / Melhoria-Evolução / Correção]
Status	[Em Definição / Em Desenvolvimento / Em Homologação / Entregue]
Product Owner	[Nome]
Stakeholders	[Lista de envolvidos]
Data de criação	[DD/MM/AAAA]
Última atualização	[DD/MM/AAAA]
Versão	[v0.1]

 
1. Visão do Produto
Descrição de alto nível do produto/funcionalidade, respondendo:
•	O que é? — Definição em uma frase.
•	Para quem? — Público-alvo primário.
•	Qual o valor entregue? — Benefício central para o negócio e o usuário.
•	Resumo executivo — Parágrafo curto que contextualiza a importância estratégica.

 
2. Problema e Evidências
2.1 Estado Atual (As-Is)
Descrição de como o processo funciona hoje, sem a funcionalidade proposta. Incluir workarounds relevantes.

2.2 Evidências de Dor
Fonte da Evidência	Descrição
Feedback de clientes	[Exemplo]
Dados internos	[Exemplo]
Time de Vendas / CS	[Exemplo]
Suporte / Tickets	[Exemplo]

2.3 Benchmark (Concorrência)
Concorrente	Funcionalidade Observada
[Concorrente A]	[Como resolve o problema]
[Concorrente B]	[Como resolve o problema]

 
3. Objetivos de Negócio
Lista dos objetivos mensuráveis que a funcionalidade busca atingir.
#	Objetivo	Métrica Alvo
O1	[Exemplo]	[Exemplo]
O2	[Exemplo]	[Exemplo]
O3	[Exemplo]	[Exemplo]

 
4. Pricing (Opcional)
Preencher quando houver implicação de precificação, embalagem comercial ou impacto de custo direto.

Campo	Valor
Modelo sugerido	[Exemplo]
Drivers de custo	[Exemplo]
Estimativa inicial	[Faixa de custo ou N/A]

Se não aplicável nesta fase:
- Registrar: N/A nesta versão
- Justificativa: [1-2 frases objetivas]

 
5. Personas
Para cada persona, descrever: perfil, necessidade principal, como a funcionalidade a atende.
Persona	Perfil	Necessidade Principal	Valor Recebido
[Persona 1]	[Descrição]	[Descrição]	[Descrição]
[Persona 2]	[Descrição]	[Descrição]	[Descrição]

 
6. Estrutura do Produto (Módulos / Blocos)
Visão macro de como a funcionalidade se organiza em módulos ou blocos lógicos.
Módulo [N] — [Nome] ([Propósito])
Funcionalidades:
•	[Funcionalidade 1]
•	[Funcionalidade 2]

 
7. Jornada do Usuário
7.1 Fluxo Principal (Happy Path)
1.	[Passo 1]
2.	[Passo 2]
3.	[Passo 3]

7.2 Fluxos Alternativos
•	[Fluxo alternativo 1]
•	[Fluxo alternativo 2]

 
8. Requisitos Funcionais
RF[XX] — [Nome do Requisito]
Campo	Descrição
Descrição	[O que o sistema deve fazer]
Regras de Negócio	[Condições, validações, limites]
Dados de Entrada	[Campos, parâmetros, seleções do usuário]
Dados de Saída	[O que o sistema retorna/exibe]
Comportamento de Erro	[Mensagens, bloqueios, fallback]
Dependências	[Outros RFs, APIs, módulos]
Prioridade	[Must / Should / Could / Won't]

 
9. Requisitos Não Funcionais
ID	Categoria	Requisito
RNF01	Performance	[Exemplo]
RNF02	Responsividade	[Exemplo]
RNF03	Segurança	[Exemplo]
RNF04	Auditoria	[Exemplo]
RNF05	Escalabilidade	[Exemplo]
RNF06	Acessibilidade	[Exemplo]

 
10. Compliance, LGPD e Requisitos Legais
10.1 Dados Sensíveis Envolvidos
[Descrição]

10.2 Requisitos de Conformidade
Requisito	Descrição
Trilha de auditoria (LGPD)	[Exemplo]
Matriz de segurança	[Exemplo]
Fidelidade legal	[Exemplo]
Marca d'água / Disclaimers	[Exemplo]
Integrações regulatórias	[Exemplo]

 
11. Entregáveis
Componente	Descrição da Entrega
Interface (UI)	[Exemplo]
Backend / API	[Exemplo]
Relatórios / PDFs	[Exemplo]
Integrações	[Exemplo]
Documentação	[Exemplo]

 
12. Cenários de Teste (QA)
12.1 Cenários Positivos (Happy Path)
#	Cenário	Descrição / Condições	Resultado Esperado
C1	[Exemplo]	[Exemplo]	[Exemplo]

12.2 Cenários Negativos (Edge Cases)
#	Cenário	Descrição / Condições	Resultado Esperado
C2	[Exemplo]	[Exemplo]	[Exemplo]

 
13. Dependências e Integrações
13.1 Dependências Internas
Dependência	Tipo	Descrição
[Dependência interna]	[Tipo]	[Descrição]

13.2 Integrações Externas
Sistema / API	Tipo	Descrição
[Integração externa]	[Tipo]	[Descrição]

 
14. Riscos e Mitigações
#	Risco	Impacto	Probabilidade	Mitigação
R1	[Exemplo]	[Exemplo]	[Exemplo]	[Exemplo]

 
15. Métricas de Sucesso
Métrica	Baseline (Atual)	Meta	Prazo de Medição
[Exemplo]	[Exemplo]	[Exemplo]	[Exemplo]

 
16. Plano de Execução (Opcional)
Preencher quando houver maturidade de planejamento suficiente.

Se não aplicável nesta fase:
- Registrar: N/A nesta versão
- Justificativa: [1-2 frases objetivas]

Fase 1 — [Nome]
•	[Entregável]
•	Duração estimada: [X sprints]

 
17. Glossário e Referências
Termo / Sigla	Definição
[Termo]	[Definição]

Referências:
•	[Link ou documento de referência]

 
18. Histórico de Versões
Versão	Data	Autor	Alterações
v0.1	DD/MM/AAAA	[Nome]	Criação do documento

 
Nota para o agente de IA: Ao preencher este template, garanta que:
- Não invente dados.
- Ajuste a profundidade de preenchimento conforme o tipo de entrega e as regras ativas do steering.
- Mantenha as secoes obrigatorias em qualquer cenario.
- Trate Pricing e Plano de Execucao como opcionais quando nao se aplicarem.