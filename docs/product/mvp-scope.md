# MVP Scope

## Objetivo do MVP

Validar se desenvolvedores e times que criam agentes de IA conseguem integrar pagamentos de forma mais simples usando uma interface MCP-first, com seguranca, auditoria e roteamento basico para providers regulados.

O MVP deve provar valor primeiro como software de controle financeiro para agentes. A captura de ganho transacional, como spread ou fee sobre pagamentos reais, deve ser tratada como expansao apos validacao de uso, volume e risco operacional.

O MVP deve provar tres hipoteses:

1. Um agente consegue executar pagamentos por tools MCP com pouco atrito.
2. Um operador humano consegue configurar limites e auditar o comportamento financeiro do agente.
3. O gateway consegue orquestrar providers sem assumir papel de liquidante.

## Foco Comercial

O SkillPay deve ser vendido inicialmente como camada de confianca para agentes que executam operacoes financeiras:

- integra pagamentos em agentes por MCP;
- aplica limites e permissoes por agente;
- bloqueia payloads invalidos ou fora de politica antes do provider;
- registra audit log por tool call;
- permite sandbox financeiro antes de producao.

A mensagem principal do MVP deve ser:

> Permita que seus agentes cobrem, consultem e estornem pagamentos com limites, auditoria e seguranca, sem integrar cada provider manualmente.

O modelo de receita inicial deve favorecer assinatura, uso de API/MCP, logs, sandbox e setup. Fee por transacao real pode existir, mas nao deve ser a unica fonte de receita no primeiro ciclo.

Detalhes de precificacao, margem e evolucao para gateway estao em [Business Model](business-model.md).

## Publico Inicial

O publico recomendado para o primeiro beta e composto por:

- desenvolvedores criando agentes internos;
- startups construindo automacoes agenticas;
- times tecnicos que precisam de sandbox financeiro auditavel;
- builders que ja usam Claude Desktop, Cursor, LangChain, CrewAI ou SDKs compatíveis com MCP.

Fintechs reguladas e enterprise devem ser tratadas como aprendizado, nao como cliente principal do MVP.

## Must Have

- MCP server com `process_payment`, `check_status` e `refund`.
- API backend Python para workspace, agentes, transacoes e providers.
- Policy engine deterministico antes de qualquer chamada externa.
- Sandbox interno para simular pagamentos.
- Adapter inicial para Pix ou provider local.
- Adapter opcional para Stripe se o escopo global for priorizado.
- Dashboard minimo com onboarding, agentes, transacoes, sandbox e logs.
- Audit log por tool call.
- API keys por workspace.
- Documentacao de integracao MCP.

## Should Have

- Webhooks configuraveis para mudanca de status.
- Retentativa de webhook com historico de entrega.
- Ambiente separado por sandbox/producao.
- Exportacao simples de logs.
- Exemplos de cliente MCP.
- Status page interna dos providers.

## Won't Have no MVP

- Licenca de subadquirencia propria.
- Custodia ou saldo interno.
- Marketplace.
- Split de pagamento.
- Checkout para humanos.
- Cartao com PCI direto.
- Roteamento por machine learning.
- Antifraude comportamental avancado.

## Criterios de Sucesso

- Um novo workspace consegue gerar credenciais e executar uma transacao sandbox em menos de 10 minutos.
- Toda transacao possui trilha de auditoria consultavel.
- Uma tentativa fora dos limites configurados e bloqueada antes do provider.
- O frontend permite entender por que uma transacao foi aprovada, bloqueada ou falhou.
- O contrato MCP e estavel o suficiente para criar exemplos e testes automatizados.

## Criterios de Nao Sucesso

- Pagamentos chegam ao provider sem decisao registrada do policy engine.
- O frontend exige configuracao manual demais antes da primeira simulacao.
- Logs nao permitem reconstruir o caminho de uma tool call.
- O gateway depende de um unico provider sem camada de adapter.
- A documentacao fica atrasada em relacao ao comportamento real da API.
