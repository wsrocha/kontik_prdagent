# Padrão de Requisitos Funcionais

Guia para estruturar requisitos funcionais de forma clara e executável.

> Este arquivo e auxiliar. A fonte oficial do contrato do PRD continua nos steering files do workspace.
> Em caso de conflito, prevalecem `prd-writing-rules.md`, `prd-validation-rules.md` e `prd-guardrails.md`.

## Estrutura Padrão de RF

Cada requisito funcional deve seguir este padrão:

```
RF[XX] — [Nome do Requisito]

| Campo | Descrição |
|-------|-----------|
| Descrição | [O que o sistema deve fazer] |
| Regras de Negócio | [Condições, validações, limites] |
| Dados de Entrada | [Campos, parâmetros, seleções do usuário] |
| Dados de Saída | [O que o sistema retorna/exibe] |
| Comportamento de Erro | [Mensagens, bloqueios, fallback] |
| Dependências | [Outros RFs, APIs, módulos] |
| Prioridade | Must / Should / Could / Won't |
```

---

## Exemplo 1: Sistema de RH - Validação de Saldo de Férias

### RF01 — Validar Saldo de Férias Automaticamente

| Campo | Descrição |
|-------|-----------|
| Descrição | O sistema deve validar automaticamente se o colaborador possui saldo suficiente de férias para a solicitação, considerando dias úteis, feriados e períodos já aprovados. |
| Regras de Negócio | - Saldo = dias adquiridos - dias já utilizados - dias em solicitação pendente<br>- Não contar feriados e fins de semana<br>- Considerar períodos de férias já aprovadas<br>- Validar contra tabela de feriados nacionais e estaduais<br>- Mínimo 1 dia de férias por solicitação<br>- Máximo 30 dias por solicitação (conforme CLT) |
| Dados de Entrada | - ID do colaborador<br>- Data de início das férias<br>- Data de fim das férias<br>- Tipo de férias (gozo, abono, etc.) |
| Dados de Saída | - Saldo disponível (número de dias)<br>- Saldo após solicitação (número de dias)<br>- Status de validação (válido/inválido)<br>- Mensagem de erro (se inválido) |
| Comportamento de Erro | - Se saldo insuficiente: "Saldo insuficiente. Disponível: X dias. Solicitado: Y dias."<br>- Se período inválido: "Período inválido. Verifique as datas."<br>- Se colaborador não encontrado: "Colaborador não encontrado no sistema." |
| Dependências | - RF02: Calcular dias úteis<br>- Tabela de feriados (integração externa)<br>- Cadastro de colaborador |
| Prioridade | Must |

---

### RF02 — Calcular Dias Úteis

| Campo | Descrição |
|-------|-----------|
| Descrição | O sistema deve calcular o número de dias úteis entre duas datas, excluindo fins de semana e feriados. |
| Regras de Negócio | - Dias úteis = segunda a sexta<br>- Excluir feriados nacionais e estaduais<br>- Considerar feriados municipais (se aplicável)<br>- Incluir data de início e fim |
| Dados de Entrada | - Data de início<br>- Data de fim<br>- Estado (para feriados estaduais)<br>- Município (opcional, para feriados municipais) |
| Dados de Saída | - Número de dias úteis |
| Comportamento de Erro | - Se data de fim < data de início: Retornar 0<br>- Se datas inválidas: Retornar erro |
| Dependências | - Tabela de feriados (integração externa) |
| Prioridade | Must |

---

## Exemplo 2: E-commerce - Rastreamento de Pedidos

### RF03 — Exibir Localização do Pedido em Mapa

| Campo | Descrição |
|-------|-----------|
| Descrição | O sistema deve exibir a localização atual do pedido em um mapa interativo, atualizado em tempo real. |
| Regras de Negócio | - Atualizar localização a cada 5 minutos<br>- Mostrar apenas para pedidos em trânsito<br>- Respeitar privacidade do entregador (não mostrar nome/foto)<br>- Mostrar rota estimada até o destino<br>- Mostrar histórico de localizações (últimas 24 horas) |
| Dados de Entrada | - ID do pedido<br>- ID do cliente |
| Dados de Saída | - Mapa com localização atual<br>- Coordenadas (latitude, longitude)<br>- Endereço aproximado<br>- Rota estimada<br>- Histórico de localizações |
| Comportamento de Erro | - Se pedido não encontrado: "Pedido não encontrado."<br>- Se sem permissão: "Você não tem permissão para rastrear este pedido."<br>- Se sem localização: "Localização não disponível no momento." |
| Dependências | - RF04: Integração com API de GPS<br>- RF05: Integração com API de mapas (Google Maps, etc.)<br>- Autenticação de cliente |
| Prioridade | Must |

---

### RF04 — Integrar com API de GPS

| Campo | Descrição |
|-------|-----------|
| Descrição | O sistema deve receber e armazenar dados de localização GPS dos dispositivos dos entregadores em tempo real. |
| Regras de Negócio | - Receber dados a cada 5 minutos<br>- Validar coordenadas (latitude -90 a 90, longitude -180 a 180)<br>- Armazenar histórico de localizações por 30 dias<br>- Criptografar dados de localização em trânsito<br>- Respeitar LGPD (consentimento do entregador) |
| Dados de Entrada | - ID do entregador<br>- Latitude<br>- Longitude<br>- Timestamp<br>- Precisão (em metros) |
| Dados de Saída | - Confirmação de recebimento<br>- ID da localização armazenada |
| Comportamento de Erro | - Se coordenadas inválidas: Rejeitar e registrar erro<br>- Se entregador não encontrado: Rejeitar<br>- Se sem consentimento LGPD: Rejeitar |
| Dependências | - Integração com dispositivo GPS<br>- Banco de dados de localizações<br>- Consentimento LGPD |
| Prioridade | Must |

---

## Exemplo 3: SaaS de Gestão de Projetos

### RF05 — Criar Tarefa com Descrição e Aceitação

| Campo | Descrição |
|-------|-----------|
| Descrição | O sistema deve permitir que o project manager crie uma tarefa com descrição clara, critérios de aceitação e atribua a um membro do time. |
| Regras de Negócio | - Título obrigatório (mínimo 10 caracteres)<br>- Descrição obrigatória (mínimo 50 caracteres)<br>- Critérios de aceitação obrigatórios (mínimo 3)<br>- Atribuição obrigatória<br>- Data de vencimento obrigatória<br>- Prioridade obrigatória (Alta, Média, Baixa)<br>- Máximo 5 tags por tarefa |
| Dados de Entrada | - Título<br>- Descrição<br>- Critérios de aceitação (lista)<br>- Atribuído para (ID do usuário)<br>- Data de vencimento<br>- Prioridade<br>- Tags<br>- Projeto (ID) |
| Dados de Saída | - ID da tarefa<br>- Tarefa criada com status "A Fazer"<br>- Notificação enviada ao atribuído<br>- Tarefa visível no dashboard |
| Comportamento de Erro | - Se título vazio: "Título é obrigatório."<br>- Se descrição < 50 caracteres: "Descrição deve ter no mínimo 50 caracteres."<br>- Se critérios de aceitação < 3: "Defina no mínimo 3 critérios de aceitação."<br>- Se usuário não encontrado: "Usuário não encontrado." |
| Dependências | - RF06: Notificar usuário<br>- RF07: Integrar com Git (opcional)<br>- Autenticação de usuário |
| Prioridade | Must |

---

### RF06 — Notificar Usuário de Tarefa Atribuída

| Campo | Descrição |
|-------|-----------|
| Descrição | O sistema deve notificar o usuário quando uma tarefa é atribuída a ele, via email e notificação in-app. |
| Regras de Negócio | - Enviar notificação imediatamente após atribuição<br>- Incluir título, descrição resumida e link para tarefa<br>- Respeitar preferências de notificação do usuário<br>- Não enviar duplicatas<br>- Registrar notificação no histórico |
| Dados de Entrada | - ID do usuário<br>- ID da tarefa<br>- Tipo de notificação (email, in-app, ambos) |
| Dados de Saída | - Notificação enviada<br>- Confirmação de entrega |
| Comportamento de Erro | - Se usuário desativou notificações: Não enviar<br>- Se email inválido: Registrar erro e enviar apenas in-app<br>- Se usuário não encontrado: Registrar erro |
| Dependências | - Serviço de email<br>- Preferências de notificação do usuário |
| Prioridade | Should |

---

## Checklist para Requisitos Funcionais

Para cada RF, valide:

- [ ] **Descrição clara**: Qualquer desenvolvedor consegue entender o que fazer
- [ ] **Regras de negócio explícitas**: Todas as condições, validações e limites documentados
- [ ] **Dados de entrada definidos**: Todos os campos, parâmetros e seleções
- [ ] **Dados de saída especificados**: O que o sistema retorna/exibe
- [ ] **Comportamento de erro documentado**: Mensagens, bloqueios, fallback
- [ ] **Dependências mapeadas**: Outros RFs, APIs, módulos
- [ ] **Prioridade atribuída**: Must, Should, Could, Won't
- [ ] **Vinculado a persona(s)**: Qual persona usa este RF
- [ ] **Vinculado a objetivo(s)**: Qual objetivo de negócio este RF atende
- [ ] **Testável**: Possível criar cenários de teste
- [ ] **Sem ambiguidades**: Não há interpretações diferentes

## Dicas para Escrever RFs Efetivos

1. **Use linguagem ativa**: "O sistema deve validar..." em vez de "Validação de..."
2. **Seja específico**: "Máximo 30 dias" em vez de "Período razoável"
3. **Inclua exemplos**: "Ex: 20 dias gozo + 10 abono"
4. **Documente exceções**: "Não contar feriados e fins de semana"
5. **Vincule a personas**: Cada RF deve servir a alguém
6. **Considere LGPD**: Dados sensíveis precisam de tratamento especial
7. **Pense em erros**: Qual é o comportamento quando algo dá errado?
8. **Priorize com MoSCoW**: Must (obrigatório), Should (importante), Could (legal ter), Won't (fora do escopo)

## Priorização MoSCoW

- **Must**: Obrigatório para MVP. Sem isso, a feature não funciona.
- **Should**: Importante, mas pode ser feito em fase 2 se necessário.
- **Could**: Legal ter, mas não é crítico. Pode ser adiado.
- **Won't**: Fora do escopo desta versão. Considerar para futuro.

## Exemplo de RF Bem Estruturado vs Mal Estruturado

### ❌ Mal Estruturado
```
RF01 — Validar Férias
O sistema deve validar as férias do colaborador.
```

### ✅ Bem Estruturado
```
RF01 — Validar Saldo de Férias Automaticamente

| Campo | Descrição |
|-------|-----------|
| Descrição | O sistema deve validar automaticamente se o colaborador possui saldo suficiente de férias para a solicitação, considerando dias úteis, feriados e períodos já aprovados. |
| Regras de Negócio | - Saldo = dias adquiridos - dias já utilizados - dias em solicitação pendente<br>- Não contar feriados e fins de semana<br>- Considerar períodos de férias já aprovadas<br>- Validar contra tabela de feriados nacionais e estaduais<br>- Mínimo 1 dia de férias por solicitação<br>- Máximo 30 dias por solicitação (conforme CLT) |
| Dados de Entrada | - ID do colaborador<br>- Data de início das férias<br>- Data de fim das férias<br>- Tipo de férias (gozo, abono, etc.) |
| Dados de Saída | - Saldo disponível (número de dias)<br>- Saldo após solicitação (número de dias)<br>- Status de validação (válido/inválido)<br>- Mensagem de erro (se inválido) |
| Comportamento de Erro | - Se saldo insuficiente: "Saldo insuficiente. Disponível: X dias. Solicitado: Y dias."<br>- Se período inválido: "Período inválido. Verifique as datas."<br>- Se colaborador não encontrado: "Colaborador não encontrado no sistema." |
| Dependências | - RF02: Calcular dias úteis<br>- Tabela de feriados (integração externa)<br>- Cadastro de colaborador |
| Prioridade | Must |
```

A diferença é clara: o segundo é executável, o primeiro é vago.
