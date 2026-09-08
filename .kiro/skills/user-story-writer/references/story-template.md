# Template de Story — Referência Rápida

Use este arquivo como referência ao escrever ou revisar stories.
O template completo com instruções está no SKILL.md.

---

## Template Completo

```
TIPO: [Story / Task / Bug / Sub-task / Epic]
PRIORIDADE: [Must / Should / Could / Won't]
MÓDULO: [Nome do produto ou módulo]
RF ORIGEM: [RF01 / RF02 / ... — ou "N/A"]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TÍTULO: [verbo de ação + objeto + contexto]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

HISTÓRIA DO USUÁRIO:
*Como* [persona],
*quero* [ação ou funcionalidade],
*para* que [benefício / valor gerado].

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CONTEXTO:
[Problema ou oportunidade — máximo 4 linhas]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ESCOPO:

✅ Inclui:
- [comportamento esperado]

❌ Não inclui:
- [out of scope explícito]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITÉRIOS DE ACEITE:
- [ ] [happy path 1]
- [ ] [happy path 2]
- [ ] [happy path 3]
- [ ] [caso de borda / erro]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CRITÉRIOS DE PRONTO (DoR):
- [ ] História revisada pelo PO
- [ ] Mockup aprovado (se UI)
- [ ] Regras de negócio documentadas
- [ ] Dependências mapeadas
- [ ] Casos de borda nos critérios de aceite

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

LINKS E REFERÊNCIAS:
- PRD: [arquivo ou link]
- Protótipo: [Figma ou N/A]
- Docs técnicos: [link ou N/A]
```

---

## Template Reduzido (Task Técnica)

Para tasks sem valor direto ao usuário (setup, refactor, infra):

```
TIPO: Task
PRIORIDADE: [Must / Should / Could]
MÓDULO: [módulo]

TÍTULO: [verbo técnico + objeto]
ex: "Configurar job de monitoramento periódico de preços para alertas ativos"

DESCRIÇÃO:
[O que precisa ser feito e por quê — contexto técnico suficiente para execução]

CRITÉRIOS DE PRONTO:
- [ ] [entregável técnico verificável]
- [ ] [testes/validações necessários]
- [ ] [documentação se aplicável]

DEPENDÊNCIAS:
- [outras tasks ou stories que devem ser concluídas antes]
```

---

## Template Bug

```
TIPO: Bug
MÓDULO: [módulo]
SEVERIDADE: [Crítico / Alto / Médio / Baixo]

TÍTULO: [comportamento incorreto + contexto]
ex: "Valor total na tela de revisão não inclui bagagem despachada selecionada"

COMPORTAMENTO ATUAL:
[O que acontece hoje — passo a passo para reproduzir]

COMPORTAMENTO ESPERADO:
[O que deveria acontecer]

PASSOS PARA REPRODUZIR:
1. [passo 1]
2. [passo 2]
3. [resultado incorreto observado]

AMBIENTE:
- Versão: [versão do sistema]
- Navegador/dispositivo: [se aplicável]

EVIDÊNCIAS:
- [screenshot, log, vídeo — se disponível]
```
