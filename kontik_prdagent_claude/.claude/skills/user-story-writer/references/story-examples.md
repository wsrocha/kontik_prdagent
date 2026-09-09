# Exemplos de Stories — Referência

Exemplos práticos para uso no contexto Zupper.
Use como referência de qualidade e formato ao gerar novas stories.

---

## Exemplo 1 — Story completa (origem: RF03 do PRD Alertas de Preço)

```
TIPO: Story
PRIORIDADE: Must
MÓDULO: Zupper — Busca de Passagens
RF ORIGEM: RF03

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TÍTULO: Receber notificação de queda de preço para rota monitorada

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

HISTÓRIA DO USUÁRIO:
*Como* viajante recorrente,
*quero* receber uma notificação quando o preço de uma passagem que estou monitorando cair
abaixo do valor que defini,
*para* que eu possa aproveitar a oferta sem precisar acessar o app diariamente.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CONTEXTO:
Hoje, o viajante precisa verificar manualmente se o preço da rota desejada caiu,
acessando o app ou site repetidamente. Isso gera frustração e perda de oportunidades.
O sistema de alertas de preço resolve essa dor ao monitorar automaticamente e notificar
o usuário no momento certo.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ESCOPO:

✅ Inclui:
- Cadastro do alerta com rota, data de viagem e preço-alvo
- Monitoramento periódico dos preços via integração com fornecedores (GDS)
- Notificação push no app quando o preço cair abaixo do threshold
- Fallback por e-mail se push não estiver habilitado
- Botão de ação direto na notificação: "Ver voo"

❌ Não inclui:
- Alerta para hotéis (escopo de story separada — Should)
- Histórico de variação de preços em gráfico (escopo futuro)
- Alertas de alta de preço (não solicitado nesta versão)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITÉRIOS DE ACEITE:
- [ ] O viajante consegue criar um alerta informando: rota (origem/destino), datas e preço-alvo
- [ ] Quando o preço cai abaixo do threshold, push notification é enviado em até 5 minutos
- [ ] A notificação exibe: rota, preço anterior, novo preço, percentual de desconto e botão "Ver voo"
- [ ] Ao tocar em "Ver voo", o app abre a busca com os parâmetros do alerta pré-preenchidos
- [ ] Se o usuário não tiver push habilitado, um e-mail é enviado como fallback
- [ ] Se a rota monitorada não tiver mais disponibilidade, o alerta é automaticamente desativado e o usuário é notificado

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITÉRIOS DE PRONTO (DoR):
- [ ] História revisada pelo PO
- [ ] Mockup da tela de criação de alerta aprovado pelo Design
- [ ] Regras de threshold (mínimo de queda em %) documentadas
- [ ] Integração com GDS para consulta de preços definida
- [ ] Casos de borda documentados nos critérios de aceite

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

LINKS E REFERÊNCIAS:
- PRD: output-files/prd-alerta-preco-passagens.md
- Protótipo: ⚠️ Necessário validar com Design
- Docs GDS/fornecedor: [a definir]
```

---

## Exemplo 2 — Task Técnica

```
TIPO: Task
PRIORIDADE: Must
MÓDULO: Zupper — Busca de Passagens

TÍTULO: Criar job de monitoramento periódico de preços para alertas ativos

DESCRIÇÃO:
Criar um job assíncrono (cron/worker) que consulta periodicamente os preços das rotas
com alertas ativos no sistema. O job deve comparar o preço atual com o threshold
de cada alerta, disparar a notificação quando a condição for atendida e registrar
o log de cada verificação com timestamp, rota, preço retornado e ação tomada.

CRITÉRIOS DE PRONTO:
- [ ] Job criado e agendado (frequência a definir com engenharia — sugestão: a cada 15 min)
- [ ] Consulta aos fornecedores implementada com tratamento de timeout e retry
- [ ] Lógica de comparação preço atual × threshold correta
- [ ] Disparo de notificação (push + e-mail fallback) funcionando
- [ ] Logs de execução com timestamp, rota, preço e ação registrados
- [ ] Alertas com rota sem disponibilidade desativados automaticamente

DEPENDÊNCIAS:
- Story: "Receber notificação de queda de preço" deve estar em andamento
- Integração com GDS/fornecedor de preços definida e disponível em sandbox
```

---

## Exemplo 3 — Story com estimativa

```
TIPO: Story
PRIORIDADE: Must
MÓDULO: Zupper — Checkout
RF ORIGEM: RF01
ESTIMATIVA: 5 pontos

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TÍTULO: Exibir resumo completo da reserva antes da confirmação de pagamento

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

HISTÓRIA DO USUÁRIO:
Como viajante,
quero ver um resumo detalhado da minha reserva antes de confirmar o pagamento,
para que eu possa revisar todos os dados e evitar erros antes de finalizar a compra.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CONTEXTO:
Atualmente, ~18% dos contatos de suporte são de viajantes que identificaram erros
após a finalização da compra (datas, nomes de passageiros, voo errado). Exibir um
resumo claro antes da confirmação reduz esse índice e melhora a experiência de compra.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ESCOPO:

✅ Inclui:
- Tela de revisão com: voo(s), passageiros, bagagem, dados de contato e valor total
- Breakdown de valores (tarifa base, taxas, bagagem, seguro se contratado)
- Botão "Confirmar e Pagar" e botão "Editar"
- Indicador de prazo de validade da tarifa selecionada

❌ Não inclui:
- Edição inline dos dados na tela de revisão (leva de volta ao passo anterior)
- Suporte a múltiplas formas de pagamento na mesma tela (escopo do módulo de pagamentos)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITÉRIOS DE ACEITE:
- [ ] A tela de revisão é exibida antes da tela de pagamento em 100% dos fluxos de checkout
- [ ] Todos os dados do voo selecionado são exibidos: cias aéreas, horários, escalas e duração
- [ ] O nome completo de cada passageiro e o CPF (mascarado) são exibidos para revisão
- [ ] O valor total com breakdown é exibido de forma clara
- [ ] Ao clicar em "Editar", o usuário retorna ao passo correto sem perder os dados preenchidos
- [ ] Se a tarifa expirar durante a revisão, o sistema exibe aviso e redireciona para nova busca

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITÉRIOS DE PRONTO (DoR):
- [ ] História revisada pelo PO
- [ ] Mockup aprovado pelo Design
- [ ] Regras de exibição de dados mascarados validadas com Segurança/LGPD
- [ ] Timeout de validade de tarifa definido com Produto e Fornecedor

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

LINKS E REFERÊNCIAS:
- PRD: output-files/prd-checkout-passagens.md
- Protótipo: ⚠️ Necessário validar com Design
```

---

## Exemplo 4 — Bug

```
TIPO: Bug
MÓDULO: Zupper — Checkout
SEVERIDADE: Alto

TÍTULO: Valor de bagagem despachada não é incluído no total exibido na tela de revisão

COMPORTAMENTO ATUAL:
Quando o viajante adiciona bagagem despachada ao pedido, o valor da bagagem não é
somado ao total exibido na tela de revisão. O total correto só aparece na tela de
confirmação de pagamento, causando surpresa negativa no momento da compra.

COMPORTAMENTO ESPERADO:
O valor total na tela de revisão deve incluir todos os itens selecionados:
tarifa base + taxas + bagagem + seguro (se contratado).

PASSOS PARA REPRODUZIR:
1. Buscar um voo e selecionar um resultado
2. Na seleção de passageiros, adicionar bagagem despachada (23kg)
3. Avançar para a tela de revisão
4. Observar que o valor exibido não inclui a bagagem

AMBIENTE:
- Plataforma: Web (Chrome 126) e App Android (v3.2.1)
- Reproduzível em: todos os voos testados

EVIDÊNCIAS:
- Screenshot da tela de revisão sem bagagem no total: [anexar]
- Screenshot da confirmação com valor correto: [anexar]
```
