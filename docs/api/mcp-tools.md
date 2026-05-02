# MCP Tools

## Objetivo

Este documento define os contratos iniciais das tools MCP do SkillPay. Os contratos devem ser tratados como interface publica do gateway e precisam permanecer estaveis para clientes MCP, exemplos e testes.

## Convencoes

- Valores monetarios devem ser enviados em unidade menor da moeda, por exemplo centavos para `BRL`.
- Toda chamada mutavel deve aceitar `idempotency_key`.
- Todo resultado deve retornar `correlation_id` para auditoria.
- Payloads devem usar `snake_case`.
- Campos desconhecidos devem ser rejeitados no MVP.
- Dados sensiveis devem ser mascarados em logs e respostas do frontend.

## Status Padronizados

| Status | Significado |
| --- | --- |
| `requires_action` | Pagamento criado, mas exige acao externa, como QR Code Pix. |
| `processing` | Provider aceitou a solicitacao e ainda processa. |
| `succeeded` | Pagamento confirmado. |
| `failed` | Pagamento falhou no provider ou no gateway. |
| `blocked` | Policy engine bloqueou antes do provider. |
| `expired` | Pagamento expirou. |
| `refunded` | Pagamento foi estornado totalmente. |
| `partially_refunded` | Pagamento foi estornado parcialmente. |

## Tool: `process_payment`

Cria uma intencao de pagamento e, se permitido pelo policy engine, roteia para o provider adequado.

### Input

```json
{
  "idempotency_key": "order_123_attempt_1",
  "agent_id": "agent_sales",
  "amount": 4990,
  "currency": "BRL",
  "payment_method": "pix",
  "description": "Order 123",
  "customer": {
    "id": "cus_123",
    "name": "Cliente Teste",
    "email": "cliente@example.com",
    "document": "00000000000"
  },
  "metadata": {
    "order_id": "123",
    "source": "agent_checkout"
  },
  "expires_in_seconds": 900
}
```

### Campos Obrigatorios

- `idempotency_key`
- `agent_id`
- `amount`
- `currency`
- `payment_method`

### Regras

- `amount` deve ser inteiro positivo.
- `currency` deve usar ISO 4217.
- `payment_method` inicialmente aceita `pix`, `card` ou `sandbox`.
- `agent_id` deve existir, estar ativo e pertencer ao workspace autenticado.
- `metadata` nao deve conter dados sensiveis brutos.
- Se `payment_method` for `pix`, a resposta pode conter QR Code ou codigo copia-e-cola.

### Output de Sucesso

```json
{
  "correlation_id": "corr_01",
  "transaction_id": "pay_123",
  "status": "requires_action",
  "amount": 4990,
  "currency": "BRL",
  "payment_method": "pix",
  "provider": "pix_provider",
  "provider_reference": "provider_abc",
  "action": {
    "type": "pix_qr_code",
    "qr_code": "000201...",
    "expires_at": "2026-05-02T15:00:00Z"
  },
  "policy_decision": {
    "result": "allowed",
    "rules_evaluated": [
      "agent_active",
      "amount_limit",
      "method_allowed"
    ]
  }
}
```

### Output Bloqueado

```json
{
  "correlation_id": "corr_02",
  "transaction_id": null,
  "status": "blocked",
  "error": {
    "code": "policy_denied",
    "message": "Payment request denied by policy engine.",
    "reason": "amount_exceeds_agent_limit"
  },
  "policy_decision": {
    "result": "denied",
    "rules_evaluated": [
      "agent_active",
      "amount_limit"
    ]
  }
}
```

## Tool: `check_status`

Consulta o status normalizado de uma transacao.

### Input

```json
{
  "transaction_id": "pay_123"
}
```

### Campos Obrigatorios

- `transaction_id`

### Output

```json
{
  "correlation_id": "corr_03",
  "transaction_id": "pay_123",
  "status": "succeeded",
  "amount": 4990,
  "currency": "BRL",
  "payment_method": "pix",
  "provider": "pix_provider",
  "provider_reference": "provider_abc",
  "created_at": "2026-05-02T14:45:00Z",
  "updated_at": "2026-05-02T14:46:10Z"
}
```

### Regras

- A transacao deve pertencer ao workspace autenticado.
- Se o provider estiver indisponivel, retornar o ultimo status conhecido e indicar `stale: true`.
- Consultas tambem devem gerar audit log, mas nao precisam passar por regras de limite financeiro.

## Tool: `refund`

Solicita estorno total ou parcial de uma transacao.

### Input

```json
{
  "idempotency_key": "refund_order_123_attempt_1",
  "agent_id": "agent_support",
  "transaction_id": "pay_123",
  "amount": 4990,
  "reason": "customer_request",
  "metadata": {
    "ticket_id": "SUP-123"
  }
}
```

### Campos Obrigatorios

- `idempotency_key`
- `agent_id`
- `transaction_id`
- `reason`

### Regras

- Se `amount` for omitido, o refund e total.
- Refund parcial nao pode exceder o valor capturado restante.
- O agente deve ter permissao explicita de refund.
- Refund deve passar pelo policy engine.
- Transacoes `blocked`, `failed` ou `expired` nao podem ser estornadas.

### Output

```json
{
  "correlation_id": "corr_04",
  "refund_id": "ref_123",
  "transaction_id": "pay_123",
  "status": "refunded",
  "amount": 4990,
  "currency": "BRL",
  "provider": "pix_provider",
  "provider_reference": "refund_provider_abc",
  "policy_decision": {
    "result": "allowed",
    "rules_evaluated": [
      "agent_active",
      "refund_allowed",
      "refund_amount_limit"
    ]
  }
}
```

## Erros Padronizados

| Codigo | Quando usar |
| --- | --- |
| `invalid_payload` | Schema invalido ou campos desconhecidos. |
| `unauthorized` | Credencial ausente ou invalida. |
| `agent_not_found` | Agente inexistente no workspace. |
| `agent_inactive` | Agente pausado ou desativado. |
| `policy_denied` | Policy engine bloqueou a acao. |
| `transaction_not_found` | Transacao inexistente ou fora do workspace. |
| `provider_unavailable` | Provider nao respondeu ou esta fora. |
| `provider_error` | Provider recusou ou falhou com erro normalizado. |
| `idempotency_conflict` | Mesma chave de idempotencia usada com payload diferente. |

## Idempotencia

Para `process_payment` e `refund`, o par `workspace_id + idempotency_key` deve identificar uma unica operacao mutavel.

Se a mesma chave for repetida com o mesmo payload, o gateway retorna o resultado anterior. Se a mesma chave for repetida com payload diferente, o gateway retorna `idempotency_conflict`.

## Auditoria

Toda tool call deve registrar:

- `correlation_id`;
- workspace;
- agente;
- nome da tool;
- payload mascarado;
- decisao do policy engine;
- provider selecionado;
- status final conhecido;
- timestamps.
