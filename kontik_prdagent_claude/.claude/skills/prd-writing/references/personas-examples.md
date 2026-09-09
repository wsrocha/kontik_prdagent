# Exemplos de Personas

Referência com exemplos de personas bem estruturadas para diferentes contextos.

> Este arquivo e auxiliar. A fonte oficial do contrato do PRD continua nos steering files do workspace.
> Em caso de conflito, prevalecem `prd-writing-rules.md`, `prd-validation-rules.md` e `prd-guardrails.md`.

## Exemplo 1: Sistema de RH (Rastreamento de Férias)

### Persona 1: Analista de RH

**Perfil:**
- Nome: Maria Silva
- Idade: 32 anos
- Experiência: 5 anos em RH
- Formação: Administração
- Localização: São Paulo, SP

**Responsabilidades:**
- Gerenciar solicitações de férias
- Validar saldos
- Comunicar aprovações/rejeições
- Gerar relatórios mensais

**Necessidade Principal:**
Reduzir tempo gasto em validações manuais de saldos de férias e evitar erros de cálculo que geram retrabalho.

**Dor Atual:**
- Gasta 4 horas/dia validando saldos manualmente
- Erros de cálculo causam 15% de retrabalho
- Não consegue visualizar histórico de férias rapidamente

**Valor Recebido:**
- Validação automática de saldos (economia de 3 horas/dia)
- Histórico completo de férias por colaborador
- Relatórios automáticos para compliance

**Requisitos Relacionados:**
- RF01: Validar saldo de férias automaticamente
- RF02: Exibir histórico de férias
- RF03: Gerar relatório de férias por período

---

### Persona 2: Gestor de Departamento

**Perfil:**
- Nome: Carlos Santos
- Idade: 45 anos
- Experiência: 10 anos em gestão
- Formação: Engenharia
- Localização: Rio de Janeiro, RJ

**Responsabilidades:**
- Aprovar/rejeitar solicitações de férias
- Garantir cobertura do departamento
- Comunicar decisões aos colaboradores

**Necessidade Principal:**
Visualizar rapidamente quem está de férias, quando e por quanto tempo, para garantir cobertura adequada.

**Dor Atual:**
- Não tem visão consolidada de férias do departamento
- Recebe solicitações por email e WhatsApp
- Difícil comunicar decisões de forma centralizada

**Valor Recebido:**
- Calendário visual de férias do departamento
- Notificações automáticas de solicitações
- Histórico de decisões centralizado

**Requisitos Relacionados:**
- RF04: Exibir calendário de férias por departamento
- RF05: Notificar gestor de novas solicitações
- RF06: Registrar decisão de aprovação/rejeição

---

### Persona 3: Colaborador

**Perfil:**
- Nome: Ana Costa
- Idade: 28 anos
- Experiência: 3 anos na empresa
- Formação: Marketing
- Localização: Belo Horizonte, MG

**Responsabilidades:**
- Solicitar férias
- Acompanhar status da solicitação
- Planejar períodos de descanso

**Necessidade Principal:**
Solicitar férias de forma simples, visualizar saldo disponível e acompanhar status da solicitação em tempo real.

**Dor Atual:**
- Não sabe exatamente quanto saldo tem
- Processo de solicitação é confuso
- Demora para saber se foi aprovado

**Valor Recebido:**
- Visualização clara do saldo de férias
- Processo de solicitação intuitivo
- Notificação imediata de aprovação/rejeição

**Requisitos Relacionados:**
- RF07: Exibir saldo de férias do colaborador
- RF08: Permitir solicitação de férias
- RF09: Notificar colaborador de decisão

---

## Exemplo 2: E-commerce (Rastreamento de Pedidos em Tempo Real)

### Persona 1: Cliente Comprador

**Perfil:**
- Nome: João Oliveira
- Idade: 35 anos
- Experiência: Compra online há 5 anos
- Renda: Classe média
- Localização: São Paulo, SP

**Responsabilidades:**
- Fazer compras online
- Acompanhar entrega
- Receber produto

**Necessidade Principal:**
Saber exatamente onde está seu pedido em tempo real, para planejar sua disponibilidade para receber.

**Dor Atual:**
- Não consegue rastrear pedido em tempo real
- Recebe notificações genéricas
- Não sabe se vai chegar hoje ou amanhã

**Valor Recebido:**
- Rastreamento em tempo real com localização no mapa
- Notificações precisas de entrega
- Estimativa de horário de chegada

**Requisitos Relacionados:**
- RF01: Exibir localização do pedido em mapa
- RF02: Enviar notificação de mudança de status
- RF03: Estimar horário de entrega

---

### Persona 2: Gerente de Logística

**Perfil:**
- Nome: Roberto Silva
- Idade: 42 anos
- Experiência: 8 anos em logística
- Formação: Logística
- Localização: Guarulhos, SP

**Responsabilidades:**
- Gerenciar frota de entrega
- Otimizar rotas
- Resolver problemas de entrega

**Necessidade Principal:**
Visualizar status de todos os pedidos em tempo real para otimizar rotas e resolver problemas rapidamente.

**Dor Atual:**
- Não tem visão consolidada de todos os pedidos
- Difícil comunicar atrasos aos clientes
- Rotas não são otimizadas

**Valor Recebido:**
- Dashboard com status de todos os pedidos
- Sugestões de otimização de rota
- Alertas automáticos de atrasos

**Requisitos Relacionados:**
- RF04: Exibir dashboard de pedidos em tempo real
- RF05: Sugerir otimização de rota
- RF06: Alertar sobre atrasos

---

## Exemplo 3: SaaS de Gestão de Projetos

### Persona 1: Project Manager

**Perfil:**
- Nome: Fernanda Costa
- Idade: 38 anos
- Experiência: 7 anos em gestão de projetos
- Certificação: PMP
- Localização: Brasília, DF

**Responsabilidades:**
- Planejar projetos
- Acompanhar progresso
- Comunicar status aos stakeholders
- Gerenciar riscos

**Necessidade Principal:**
Ter visão consolidada do progresso do projeto, identificar gargalos e comunicar status de forma clara aos stakeholders.

**Dor Atual:**
- Gasta 2 horas/dia em reuniões de status
- Difícil identificar tarefas atrasadas
- Relatórios manuais consomem tempo

**Valor Recebido:**
- Dashboard com progresso do projeto
- Alertas automáticos de tarefas atrasadas
- Relatórios automáticos para stakeholders

**Requisitos Relacionados:**
- RF01: Exibir dashboard de progresso
- RF02: Alertar sobre tarefas atrasadas
- RF03: Gerar relatório de status

---

### Persona 2: Desenvolvedor

**Perfil:**
- Nome: Lucas Martins
- Idade: 26 anos
- Experiência: 4 anos como desenvolvedor
- Formação: Ciência da Computação
- Localização: Curitiba, PR

**Responsabilidades:**
- Executar tarefas técnicas
- Atualizar status de tarefas
- Colaborar com time

**Necessidade Principal:**
Ter clareza sobre o que fazer, quando fazer e colaborar facilmente com o time sem burocracia.

**Dor Atual:**
- Muitas reuniões de alinhamento
- Tarefas não são claras
- Difícil colaborar com design e QA

**Valor Recebido:**
- Tarefas claras com descrição e aceitação
- Integração com ferramentas de desenvolvimento
- Comunicação centralizada com time

**Requisitos Relacionados:**
- RF04: Exibir tarefas atribuídas
- RF05: Integrar com Git
- RF06: Permitir comentários em tarefas

---

## Estrutura Padrão para Persona

Use este template para criar suas próprias personas:

```markdown
### Persona: [Nome da Persona]

**Perfil:**
- Nome: [Nome fictício]
- Idade: [Faixa etária]
- Experiência: [Anos de experiência]
- Formação: [Educação/Certificações]
- Localização: [Cidade, Estado]

**Responsabilidades:**
- [Responsabilidade 1]
- [Responsabilidade 2]
- [Responsabilidade 3]

**Necessidade Principal:**
[Uma frase clara sobre o que essa persona precisa resolver]

**Dor Atual:**
- [Dor 1]
- [Dor 2]
- [Dor 3]

**Valor Recebido:**
- [Benefício 1]
- [Benefício 2]
- [Benefício 3]

**Requisitos Relacionados:**
- [RF01: Descrição]
- [RF02: Descrição]
- [RF03: Descrição]
```

## Dicas para Criar Personas Efetivas

1. **Base em dados reais**: Use feedback de clientes, entrevistas, analytics
2. **Seja específico**: Nomes, idades, localizações tornam personas mais reais
3. **Foque em necessidades**: Não descreva apenas características, mas o que a persona precisa resolver
4. **Vincule a requisitos**: Cada persona deve estar conectada a requisitos funcionais
5. **Mínimo 3 personas**: Garanta cobertura de diferentes tipos de usuários
6. **Máximo 5 personas**: Evite complexidade excessiva
7. **Atualize regularmente**: Personas mudam conforme o produto evolui

## Validação de Personas

- [ ] Cada persona tem perfil claro e realista
- [ ] Necessidade principal é específica e mensurável
- [ ] Dores são baseadas em evidências
- [ ] Valor recebido é tangível
- [ ] Cada persona está vinculada a requisitos funcionais
- [ ] Personas cobrem diferentes tipos de usuários
- [ ] Personas são diferenciadas (não redundantes)
