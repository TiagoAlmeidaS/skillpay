# Frontend Wireframes

## Papel do Frontend

O frontend do SkillPay e o control plane do gateway. Ele existe para que operadores humanos configurem, simulem e auditem pagamentos feitos por agentes. Ele nao deve substituir o MCP server nem concentrar regra financeira sensivel.

## Principios de UX

- A primeira simulacao deve acontecer rapidamente.
- Toda tela financeira deve explicar o estado atual e o proximo passo.
- Acoes destrutivas ou de producao devem mostrar ambiente, agente e limite afetado.
- Logs devem ser legiveis por humanos, mas preservar dados sensiveis mascarados.
- Sandbox e producao devem ser visualmente distinguiveis.

## Navegacao Principal

- Dashboard
- Onboarding
- Agents
- Transactions
- Sandbox
- Providers
- Audit Logs
- Settings

## Jornada 1 - Primeiro Pagamento Sandbox

Objetivo: permitir que um desenvolvedor valide o fluxo MCP sem dinheiro real.

Passos:

1. Criar workspace.
2. Gerar API key sandbox.
3. Criar ou selecionar um agente.
4. Copiar configuracao MCP.
5. Executar `process_payment` no cliente MCP.
6. Ver a transacao aparecer em Transactions.
7. Abrir o detalhe e conferir policy decision, payload e status.

Wireframe textual:

```text
+------------------------------------------------------+
| SkillPay                         Sandbox   Workspace |
+------------------------------------------------------+
| Step 1  Create API Key                              |
| Step 2  Configure MCP Client                        |
| Step 3  Run Test Payment                            |
| Step 4  Inspect Result                              |
+------------------------------------------------------+
| MCP Config                                           |
| server: skillpay                                    |
| api_key: sk_test_xxx                                |
| tools: process_payment, check_status, refund        |
+------------------------------------------------------+
| [Copy Config] [Run Sandbox Simulation]              |
+------------------------------------------------------+
```

## Tela - Dashboard

Objetivo: mostrar saude operacional do gateway.

Conteudo:

- total de transacoes no periodo;
- volume simulado ou processado;
- taxa de sucesso;
- transacoes bloqueadas pelo policy engine;
- status dos providers;
- ultimas tool calls;
- alertas de configuracao.

Wireframe textual:

```text
+------------------------------------------------------+
| Dashboard                         Sandbox/Production |
+------------------------------------------------------+
| Volume          Success Rate     Blocked by Policy   |
| R$ 12.450       94.1%            18                  |
+------------------------------------------------------+
| Provider Status                                      |
| Pix Provider    Healthy                              |
| Stripe          Not configured                       |
+------------------------------------------------------+
| Recent Transactions                                  |
| id        agent        amount       status           |
| pay_123   agent_sales  R$ 49.90     succeeded        |
| pay_124   agent_ops    R$ 999.00    blocked          |
+------------------------------------------------------+
```

## Tela - Agents

Objetivo: controlar quais agentes podem executar operacoes financeiras.

Conteudo:

- lista de agentes;
- ambiente permitido;
- metodos permitidos;
- limite por transacao;
- limite diario;
- status ativo/inativo;
- ultima atividade.

Wireframe textual:

```text
+------------------------------------------------------+
| Agents                                  [New Agent]  |
+------------------------------------------------------+
| Name          Environment  Methods   Limit   Status  |
| agent_sales   sandbox      pix       R$100   active  |
| agent_ops     production   pix       R$500   paused  |
+------------------------------------------------------+
| Agent Detail                                         |
| Name: agent_sales                                    |
| Allowed methods: Pix                                 |
| Max per transaction: R$ 100                          |
| Daily limit: R$ 1.000                                |
| Require metadata: customer_id, order_id              |
| [Save Changes] [Rotate Key] [Pause Agent]            |
+------------------------------------------------------+
```

## Tela - Transactions

Objetivo: investigar pagamentos e entender o ciclo de vida de cada transacao.

Conteudo:

- filtros por status, agente, provider, metodo, ambiente e periodo;
- lista de transacoes;
- detalhe da transacao;
- timeline;
- payload MCP mascarado;
- decisao do policy engine;
- resposta normalizada do provider.

Wireframe textual:

```text
+------------------------------------------------------+
| Transactions                                         |
+------------------------------------------------------+
| Search id/order/customer                             |
| Status [All] Agent [All] Provider [All] Environment  |
+------------------------------------------------------+
| pay_123  succeeded  agent_sales  pix  R$49.90        |
| pay_124  blocked    agent_ops    pix  R$999.00       |
+------------------------------------------------------+
| Detail: pay_124                                      |
| Status: blocked                                      |
| Reason: amount_exceeds_agent_limit                   |
| Agent: agent_ops                                     |
| Policy evaluated: max_transaction_amount             |
| Provider called: no                                  |
+------------------------------------------------------+
```

## Tela - Sandbox

Objetivo: permitir testes guiados das tools MCP.

Conteudo:

- seletor de tool;
- formulario baseado no contrato;
- cenarios simulados;
- resultado da tool;
- link para transacao gerada.

Cenarios iniciais:

- pagamento aprovado;
- pagamento rejeitado pelo policy engine;
- provider indisponivel;
- pagamento expirado;
- refund aprovado;
- refund negado.

## Tela - Providers

Objetivo: configurar contas externas e acompanhar disponibilidade.

Conteudo:

- provider;
- ambiente;
- status;
- credenciais mascaradas;
- metodos suportados;
- ultima verificacao;
- botao para testar conexao.

No MVP, credenciais sensiveis devem ser enviadas ao backend e nunca persistidas no frontend.

## Tela - Audit Logs

Objetivo: reconstruir o comportamento financeiro de agentes.

Conteudo:

- timestamp;
- workspace;
- agent id;
- tool;
- decision;
- transaction id;
- correlation id;
- resumo do payload;
- link para detalhe.

Wireframe textual:

```text
+------------------------------------------------------+
| Audit Logs                                           |
+------------------------------------------------------+
| Time          Agent        Tool             Decision |
| 10:01:22      agent_sales  process_payment  allowed  |
| 10:02:10      agent_ops    process_payment  denied   |
+------------------------------------------------------+
| Selected Event                                       |
| Correlation ID: corr_abc                             |
| Policy: max_transaction_amount                       |
| Decision: denied                                     |
| Reason: Requested R$999.00, limit R$500.00           |
+------------------------------------------------------+
```

## Estados Vazios

- Sem providers: direcionar para Providers e explicar sandbox.
- Sem agentes: direcionar para criar primeiro agente.
- Sem transacoes: oferecer simulacao sandbox.
- Sem logs: explicar que logs aparecem apos tool calls.

## Proximos Passos de Design

1. Escolher identidade visual basica.
2. Transformar estes wireframes em prototipo navegavel.
3. Definir componentes reutilizaveis: status badge, environment switcher, policy decision panel, transaction timeline.
4. Validar a jornada de primeiro pagamento com 3 a 5 desenvolvedores.
