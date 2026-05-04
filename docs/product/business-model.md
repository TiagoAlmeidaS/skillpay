# Business Model

## Posicionamento

O SkillPay deve ser posicionado primeiro como um gateway MCP-first para agentes de IA: uma camada de controle, orquestracao, auditoria e seguranca entre agentes e providers regulados.

O produto nao deve nascer tentando substituir Stripe, Pagar.me, adquirentes ou instituicoes reguladas. No MVP, o SkillPay deve usar esses providers por baixo e capturar valor na camada onde agentes precisam de algo que gateways tradicionais ainda nao entregam bem:

- tools MCP nativas para pagamentos;
- policy engine deterministico antes de qualquer provider;
- limites e permissoes por agente;
- auditoria por workspace, agente, sessao e tool call;
- roteamento entre providers;
- sandbox financeiro para testes de agentes;
- explicabilidade sobre por que uma transacao foi aprovada, bloqueada ou falhou.

## Tese Comercial

A promessa comercial inicial e:

> Permita que seus agentes cobrem, consultem e estornem pagamentos com limites, auditoria e seguranca, sem integrar cada provider manualmente.

O cliente nao compra apenas processamento de pagamento. Ele compra confianca operacional para permitir que agentes executem acoes financeiras.

## Estrategia de Gateway

O SkillPay pode evoluir para capturar receita de gateway de pagamento, mas essa evolucao deve acontecer em etapas.

### Etapa 1 - Orquestrador MCP-first

Objetivo: validar que times construindo agentes pagariam por uma interface segura, auditavel e simples para pagamentos.

Receitas recomendadas:

- assinatura mensal por workspace;
- cobranca por uso de API ou tool calls MCP;
- cobranca por retencao/exportacao de logs;
- planos pagos para sandbox, simulacoes e ambientes separados;
- setup fee para integracoes assistidas.

Nesta etapa, o SkillPay nao precisa ter margem sobre MDR. O foco e provar valor de software.

### Etapa 2 - Gateway com Providers Regulados

Objetivo: capturar parte da receita transacional sem assumir custodia ou papel de liquidante.

Receitas possiveis:

- fee percentual por transacao orquestrada;
- fee fixo por transacao;
- markup sobre taxas negociadas com providers;
- planos com volume incluido;
- cobranca por webhooks, reprocessamento, conciliacao e relatórios operacionais.

Exemplo simplificado:

```text
Valor transacionado:     R$100,00
Preco cobrado ao cliente: 1,0% + R$0,10 = R$1,10
Custo do provider:        0,8% + R$0,05 = R$0,85
Margem bruta SkillPay:                  R$0,25
```

Esse modelo exige volume. Com margem de 0,5%:

```text
R$100.000/mês processados  -> R$500/mês de margem bruta
R$1.000.000/mês processados -> R$5.000/mês de margem bruta
R$10.000.000/mês processados -> R$50.000/mês de margem bruta
```

Por isso, a receita de software deve ser prioritaria no inicio.

### Etapa 3 - Capacidades Financeiras Avancadas

Objetivo: avaliar se faz sentido assumir mais responsabilidade financeira.

Possibilidades futuras:

- contratos diretos com adquirentes;
- melhores taxas por volume agregado;
- antecipacao;
- split de pagamento;
- conciliacao avancada;
- planos enterprise com SLA financeiro;
- licencas ou parcerias reguladas, caso o volume justifique.

Essa etapa nao faz parte do MVP. Custodia, saldo interno e liquidacao propria aumentam risco regulatorio, capital necessario, fraude, chargebacks e carga operacional.

## Cliente Inicial Ideal

O publico inicial deve ser composto por:

- startups criando agentes que executam cobrancas;
- desenvolvedores usando MCP, Claude Desktop, Cursor, LangChain, CrewAI ou frameworks agenticos;
- times internos que querem permitir pagamentos por agentes com limites claros;
- empresas que precisam de sandbox financeiro auditavel antes de producao.

Fintechs reguladas e grandes empresas podem ser boas fontes de aprendizado, mas nao devem ser o cliente principal do MVP, pois tendem a exigir compliance, procurement, seguranca e SLAs antes da validacao do produto.

## Modelo de Precificacao Inicial

Uma precificacao inicial conservadora pode combinar:

- plano gratuito ou trial apenas para sandbox limitado;
- plano developer com limite de tool calls e logs;
- plano startup com mais workspaces, agentes e retencao de auditoria;
- plano growth com ambiente de producao e fee por transacao;
- plano enterprise com suporte, SLA, retencao customizada e integracoes privadas.

Exemplo de estrutura:

```text
Sandbox Free:
- simulacoes limitadas
- sem producao
- logs curtos

Developer:
- mensalidade baixa
- mais tool calls
- exemplos MCP
- sandbox completo

Startup:
- producao com provider configurado
- audit logs
- limites por agente
- fee por transacao orquestrada

Enterprise:
- contrato anual
- SLA
- retencao estendida
- integrações privadas
- suporte e onboarding
```

## Principio de Margem

O SkillPay deve proteger margem de duas formas:

1. Margem de software: assinatura, uso de API, logs, compliance, suporte e integrações.
2. Margem transacional: spread entre preco cobrado do cliente e custo pago ao provider.

No inicio, a margem de software e mais importante porque nao depende de alto volume transacional. A margem transacional deve ser adicionada quando houver volume suficiente para negociar taxas melhores e absorver custos operacionais.

## Riscos

- Spread transacional baixo pode nao pagar suporte, fraude, chargebacks, infraestrutura e impostos.
- Virar liquidante cedo demais aumenta risco regulatorio.
- Concorrer diretamente com adquirentes pode diluir o diferencial MCP-first.
- Cobrar apenas por transacao pode deixar o negocio dependente de volume antes de validar valor.
- Sem policy engine forte, o produto vira apenas um wrapper de provider.

## Decisao Para o MVP

O MVP deve manter o foco em:

- orquestracao MCP-first;
- policy engine;
- sandbox;
- audit logs;
- adapter para pelo menos um provider;
- dashboard de controle;
- modelo pago baseado primeiro em software.

O ganho transacional deve ser tratado como expansao do modelo, nao como dependencia inicial.
