---
name: validar-entrega
description: >-
  Valida entrega contra CAT (RT) e CANG (RN) do Ceopet — TransformaTech (TTech). Use após implementação
  ou antes de release para RT-XXX / RN-XXX.
disable-model-invocation: true
---

# Validar entrega — Ceopet — TransformaTech (TTech)

## Fluxo

1. Ler `RN-XXX` (CANG) e `RT-XXX` (CAT).
2. Para cada CAT: Atendido | Parcial | Não atendido | Não verificado.
3. Confirmar que comportamento atende CANG do RN.
4. Registrar seção **Validação** no **RT** e, se regra de negócio falhou, nota no **RN**.

## Checklist

- [ ] Todos CAT críticos do MVP
- [ ] RN sem divergência não documentada
- [ ] Perfis (RTNF-003)
- [ ] Estoque/pagamento conforme RNG/RNNG
