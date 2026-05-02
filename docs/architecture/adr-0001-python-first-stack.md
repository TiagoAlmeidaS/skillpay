# ADR 0001 - Python-first Stack

## Status

Aceita para o MVP.

## Contexto

O SkillPay nasce como um gateway de pagamentos MCP-first para agentes de IA. O repositorio foi inicializado como projeto Python 3.11 e ainda nao possui implementacao produtiva. O MVP precisa validar rapidamente:

- exposicao de tools MCP;
- validacao deterministica de payloads gerados por agentes;
- roteamento simples entre providers;
- auditoria por agente, sessao e tool call;
- um frontend de controle para configuracao e simulacao.

O SDK oficial Python de MCP e uma opcao viavel para construcao de servidores MCP e permite validar o core do produto sem migrar o projeto para outro ecossistema no primeiro ciclo.

## Decisao

O MVP sera construido com uma abordagem Python-first:

- MCP server em Python usando o SDK oficial `mcp`;
- API backend em Python, preferencialmente FastAPI;
- validacao de contratos com Pydantic;
- workers assíncronos em Python para webhooks e tarefas de background;
- PostgreSQL como banco transacional;
- Redis para filas e controle de limites;
- frontend separado em Next.js apenas como control plane.

O frontend nao deve conter regra financeira sensivel. Ele consome a API do backend para configurar, simular e auditar o gateway.

## Stack Inicial

| Camada | Tecnologia Recomendada | Motivo |
| --- | --- | --- |
| MCP Server | Python + `mcp` | Mantem o repo coerente e valida rapidamente tools MCP. |
| API Backend | FastAPI | Boa integracao com Pydantic, OpenAPI e async IO. |
| Contratos | Pydantic | Tipagem e validacao deterministica dos payloads. |
| Banco | PostgreSQL | Consistencia para transacoes, auditoria e configuracoes. |
| Fila | Redis + RQ/Celery/Arq | Execucao assíncrona de webhooks e retentativas. |
| Frontend | Next.js + shadcn/ui | Rapidez para construir dashboard e prototipos navegaveis. |
| Observabilidade | Sentry + logs estruturados | Rastreabilidade de falhas em pagamentos e tool calls. |

## Consequencias Positivas

- Menor atrito com o estado atual do repositorio.
- Python facilita validacao, schemas e experimentacao com agentes.
- FastAPI gera documentacao OpenAPI util para o frontend e para clientes externos.
- Pydantic ajuda a bloquear payloads incompletos, malformados ou alucinados antes do provider.
- A stack permite separar claramente gateway, policy engine, adapters e control plane.

## Trade-offs

- O ecossistema frontend continua em TypeScript, entao havera dois runtimes.
- Type sharing entre backend e frontend exigira OpenAPI/codegen ou contratos manuais.
- Para altissimo throughput, Node/Go podem ser avaliados no futuro.
- Se o produto depender muito de SDKs JavaScript de agentes, um MCP server TypeScript pode voltar a ser considerado.

## Alternativas Consideradas

### TypeScript Full-stack

Vantagens: um unico ecossistema para MCP, API e frontend; tipos compartilhados; boa aderencia com Next.js.

Motivo para nao escolher agora: exigiria reinicializar a direcao do repo antes de validar a tese principal. O ganho ainda nao compensa a troca no MVP.

### Python Backend com MCP TypeScript Separado

Vantagens: usa TypeScript onde o MCP e frontend podem compartilhar tipos.

Motivo para nao escolher agora: adiciona complexidade operacional cedo demais. A fronteira entre MCP server e API ficaria mais cara de manter no primeiro ciclo.

## Criterios de Revisao

Esta decisao deve ser reavaliada se:

- o MCP server Python nao atender requisitos de transporte, streaming ou performance;
- houver necessidade forte de compartilhar tipos em tempo real com o frontend;
- a latencia p95 da tool call ficar consistentemente acima da meta do MVP;
- o time passar a operar majoritariamente em TypeScript.
