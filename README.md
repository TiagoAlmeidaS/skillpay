# SkillPay

⚠️ Análise predominantemente especulativa — recomenda-se validar com fontes externas antes de decidir. Todos os relatórios de entrada foram marcados como `[speculative]` (buscas web não retornaram dados verificados). Nenhum número de mercado, estimativa de custo ou projeção financeira neste documento possui fonte primária verificável.

---

# 📋 Blueprint de MVP

## Ideia Central

Um gateway de pagamentos construído nativamente sobre o Model Context Protocol (MCP), posicionado como a **camada de orquestração financeira da Economia dos Agentes** — permitindo que qualquer agente de IA execute, rotee e audite transações sem que um desenvolvedor precise escrever centenas de linhas de integração.

---

## Sumário Executivo

- **A janela estratégica é real, mas estreita** `[speculative]`: o MCP está emergindo como protocolo de facto para expor ferramentas a LLMs, e nenhum player estabelecido (Stripe, Adyen) nasceu com arquitetura agêntica nativa. A estimativa de 18–36 meses antes de reação competitiva séria é plausível, mas não garantida.
- **O posicionamento correto é camada de inteligência, não liquidante** `[speculative]`: atuar como orquestrador sobre subadquirentes reguladas (Stripe, Pagar.me) elimina o maior risco do negócio — regulatório e de capital — sem sacrificar o diferencial técnico central.
- **A hipótese central ainda não foi validada em produção** `[speculative]`: não há evidência concreta de que empresas migrarão fluxos de pagamento de produção (não apenas POCs) para um gateway MCP-nativo. Essa é a validação mais urgente antes de qualquer investimento significativo.
- **O mercado-alvo imediato é pequeno mas de alto crescimento** `[speculative]`: o nicho de "pagamentos para agentes de IA" está estimado em menos de US$ 500M hoje, com potencial de crescimento não-linear atrelado diretamente à adoção de agentic frameworks — um mercado em formação, não consolidado.
- **O risco técnico de alucinação e segurança é subestimado** `[speculative]`: agentes de IA gerando ordens de pagamento criam superfícies de ataque novas (fraude por agente comprometido, payloads malformados) que exigem uma camada de validação determinística obrigatória — não opcional — antes de qualquer chamada a adquirentes.

---

## Contexto de Mercado

### Oportunidade

O mercado global de gateways de pagamento está estimado em US$ 45–55 bilhões com CAGR de ~15% a.a. `[speculative]`. O segmento de Agentic AI/infraestrutura cresce em ~40–45% a.a. `[speculative]`. A convergência desses dois mercados cria um nicho nascente de **pagamentos para agentes de IA** atualmente estimado em menos de US$ 500M, ainda sem dados consolidados `[speculative]`.

O gap estrutural central: o mercado de pagamentos foi **inteiramente construído assumindo que o pagador é humano**, com fricção intencional (2FA, CAPTCHAs, revisão manual) como camada de segurança. Frameworks agênticos (LangChain, CrewAI, AutoGen, Claude Agents) criam uma ruptura: agentes precisam transacionar de forma autônoma, e a infraestrutura atual não foi projetada para isso `[speculative]`.

O Brasil representa um mercado de validação privilegiado: o ecossistema de Open Finance mais avançado do mundo, Pix como infraestrutura pública, e APIs padronizadas pelo BACEN que já existem como "trilhos públicos" `[speculative]`.

### Concorrência

| Player | Fraqueza Explorável |
|---|---|
| **Stripe** | Lançou Stripe MCP em 2025 como camada superficial sobre REST legado; sem roteamento dinâmico por adquirente nem risco comportamental de agente `[speculative]` |
| **Adyen** | Foco exclusivo em enterprise com ciclo de venda longo; zero produto para desenvolvedores de agentes `[speculative]` |
| **Braintree/PayPal** | Inovação lenta, sem posicionamento em IA `[speculative]` |
| **Pagar.me / Juno** | Subadquirentes brasileiras sem roadmap agêntico — candidatos a **parceria**, não apenas concorrência `[speculative]` |
| **Skyfire / PaymentsGPT** | Early-stage, foco exclusivo nos EUA, sem suporte a métodos locais, sem arquitetura MCP nativa `[speculative]` |
| **Plugins open-source MCP** | Sem compliance formal, sem SLA, protótipos não produtivos `[speculative]` |

### Timing

A janela estimada para se tornar o "Stripe do MCP" é de **18–36 meses** antes de grandes players reagirem com produtos dedicados `[speculative]`. Esse número deve ser monitorado mensalmente via adoção de repositórios MCP públicos versus alternativas (LangChain tools, OpenAI function calling nativo).

---

## Proposta de Valor Refinada

> **"O único gateway que fala a língua nativa dos agentes de IA — transformando qualquer capacidade de pagamento em uma skill MCP pronta para consumo, com roteamento inteligente entre adquirentes, suporte a métodos locais como Pix, e trilha de auditoria granular de cada instrução de agente."**

Os três pilares diferenciadores, em ordem de prioridade:

1. **Natividade MCP** `[speculative]`: o gateway *é* uma skill, não uma API adaptada. Qualquer agente compatível (Claude, GPT, modelos locais) executa transações complexas sem camadas de integração customizadas pelo desenvolvedor.

2. **Universalidade de métodos locais** `[speculative]`: Pix, boleto e métodos regionais expostos via MCP para agentes globais — diferencial imediato e concreto frente à Stripe, que não cobre esses fluxos nativamente.

3. **Auditabilidade de agente** `[speculative]`: cada transação carrega a identidade, contexto e limitações autorizadas do agente emissor — resolvendo o problema de cadeia de responsabilidade que impede empresas reguladas de dar autonomia financeira a bots.

**Nota estratégica:** A proposta de valor de "roteamento dinâmico via IA aumentando taxa de aprovação" é um diferencial de médio prazo, não de MVP. Requer volume de dados para se provar — e deve ser entregue primeiro como roteamento por regra simples, evoluindo para ML.

---

## MVP Recomendado

### Posicionamento Fundamental

**NÃO ser liquidante. SER o cérebro MCP.** `[speculative]`

O MVP atua como camada de inteligência e orquestração sobre subadquirentes reguladas (Stripe + Pagar.me). Essa decisão elimina: PCI-DSS Nível 1 (~US$ 50–100k/ano) `[speculative]`, capital regulatório para custódia, e 18–24 meses de processo de licenciamento. O diferencial técnico permanece intacto.

### Features (MoSCoW)

**Must Have — o produto em si:**
- MCP Server com tools `process_payment`, `check_status`, `refund` — suportando qualquer cliente MCP-compatível (Claude Desktop, SDKs Anthropic/OpenAI) `[speculative]`
- Roteamento para 2 subadquirentes: Stripe (global) + Pagar.me (BRL) — roteamento inicial por regra simples (moeda), não ML `[speculative]`
- Suporte a Pix via geração de QR Code por MCP — diferencial imediato e concreto frente a Stripe `[speculative]`
- **Camada de validação determinística obrigatória** antes de qualquer chamada a adquirentes (circuit breaker por anomalia de valor/frequência, validação de payload) — não negociável dado o risco de alucinação `[speculative]`
- Dashboard mínimo: API Keys, histórico de transações, logs por identidade de agente `[speculative]`

**Should Have (pós-lançamento):**
- Webhooks configuráveis para notificar agente sobre mudança de status
- SDKs Python e Node.js com exemplos
- Logs estruturados com rastreabilidade de agente/sessão
- Suporte a cartão via tokenização delegada ao subadquirente `[speculative]`

**Won't Have no MVP:**
- Licença de subadquirência própria
- Criptomoedas
- Marketplace / split de pagamentos
- White-label
- Roteamento dinâmico por ML (começa por regra, evolui depois) `[speculative]`

### Stack Técnica

| Camada | Tecnologia | Justificativa |
|---|---|---|
| MCP Server | TypeScript + SDK oficial `@anthropic-ai/mcp` | SDK mais maduro; tipagem garante contratos estáveis `[speculative]` |
| API Backend | Node.js + Fastify | Latência ultra-baixa; mesmo ecossistema `[speculative]` |
| Banco de dados | PostgreSQL via Supabase | ACID para transações financeiras; reduz ops `[speculative]` |
| Fila de eventos | BullMQ + Redis | Webhooks assíncronos sem bloquear tool call `[speculative]` |
| Frontend | Next.js 14 + shadcn/ui | Velocidade de desenvolvimento `[speculative]` |
| Infra | Railway + Vercel | Zero DevOps no MVP `[speculative]` |
| Observabilidade | Axiom + Sentry | Debug de transações em produção `[speculative]` |

> ⚠️ **Restrição crítica de performance** `[speculative]`: o MCP Server deve responder em < 200ms antes de despachar para o subadquirente. Use streaming MCP para dar feedback imediato enquanto a transação processa em background. A latência p95 end-to-end deve ser < 400ms — se ultrapassar consistentemente, o produto perde seu caso de uso em fluxos síncronos.

### Timeline

| Fase | Duração | Entregáveis-chave |
|---|---|---|
| Discovery & Design | 2 semanas | Contratos MCP tool calls; acordos com subadquirentes; wireframes |
| Desenvolvimento MVP | 5 semanas | MCP Server funcional; dashboard; sandbox |
| Beta Fechado (15–20 devs) | 3 semanas | Coleta de taxa de ativação, NPS, friction points |
| Ajustes pós-beta | 1 semana | Top 3 friction points corrigidos |
| Lançamento público | Semana 12 | Product Hunt; métricas ativas |

**Total: ~12 semanas (3 meses).** `[speculative]`

### Custo Estimado

| Cenário | Custo total (3 meses) |
|---|---|
| Founder como Tech Lead | R$ 23.000 – R$ 32.500 `[speculative]` |
| Equipe contratada (2 devs + designer PT + PM) | R$ 59.000 – R$ 78.500 `[speculative]` |

### KPIs de Sucesso do MVP

| KPI | Meta | Prazo | Sinal de falha |
|---|---|---|---|
| Taxa de ativação (conta → 1ª transação via MCP) | ≥ 40% | 30 dias pós-beta | < 20% após 2 ciclos de ajuste = mercado imaturo `[speculative]` |
| Developers ativos (≥ 1 tx/semana) | 25 | 60 dias pós-lançamento | `[speculative]` |
| GMV processado | R$ 50.000 | 60 dias | `[speculative]` |
| Latência p95 end-to-end | < 400ms | Ao lançar | `[speculative]` |
| NPS beta | ≥ 50 | Final do beta | `[speculative]` |

---

## Modelo de Negócio

### Modelo Recomendado: Híbrido Take Rate + SaaS `[speculative]`

```
CAMADA 1 — Take Rate por Transação (core)
0,15% – 0,35% sobre volume processado
(abaixo da Stripe ~2,9% porque não é acquirer) [speculative]

CAMADA 2 — SaaS por Workspace de Agentes
US$ 49–299/mês por workspace
(por seats ou agentes ativos) [speculative]

CAMADA 3 — Premium Intelligence (upsell)
Roteamento inteligente, fraud scoring, analytics
US$ 299–1.999/mês (enterprise add-on) [speculative]
```

**Justificativa:** Take rate puro depende de volume (arriscado no early stage). SaaS puro não captura o upside de crescimento do cliente. O híbrido cria receita previsível (SaaS) + crescimento orgânico (volume) — o mesmo modelo de Stripe e Twilio em suas camadas de plataforma `[speculative]`.

### Cen

---
_Auto-generated from Cortex idea_forum `01KQMGKAJA75EHEJWAV3MFF574` em 2026-05-02._
