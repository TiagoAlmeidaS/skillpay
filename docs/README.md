# SkillPay Docs

Esta pasta concentra as decisoes de produto, arquitetura e seguranca do SkillPay. A documentacao deve ser tratada como fonte de verdade antes de qualquer implementacao relevante no gateway.

## Como Navegar

- [Architecture Overview](architecture/overview.md): visao tecnica do gateway MCP, API, policy engine, roteamento e providers.
- [ADR 0001 - Python-first Stack](architecture/adr-0001-python-first-stack.md): decisao inicial de stack e trade-offs.
- [MVP Scope](product/mvp-scope.md): escopo do MVP, criterios de entrada e itens fora do primeiro ciclo.
- [Frontend Wireframes](product/frontend-wireframes.md): jornadas, telas e wireframes textuais do control plane.
- [MCP Tools](api/mcp-tools.md): contratos iniciais das tools expostas pelo servidor MCP.
- [Policy Engine](security/policy-engine.md): regras deterministicas, limites, auditoria e postura de seguranca.

## Principios

1. O SkillPay deve ser um orquestrador financeiro MCP-first, nao um liquidante.
2. Toda transacao iniciada por agente deve passar por validacao deterministica antes de chegar a um provider.
3. O frontend e um control plane para configurar, simular e auditar o gateway.
4. A documentacao deve ser atualizada junto com qualquer mudanca funcional, API, fluxo negocial ou decisao arquitetural.

## Status Atual

O projeto esta em fase de desenho do MVP. Ainda nao ha implementacao produtiva do gateway, portanto estes documentos devem orientar a primeira fase de construcao.
