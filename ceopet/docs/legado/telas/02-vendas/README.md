# Vendas

Menu **Vendas** / **Pedidos de Venda**. RN-004, RN-010.

| Pasta | Menu Hidra | RN |
|-------|------------|-----|
| [01-pedidos-venda](01-pedidos-venda/) | Pedidos de Venda | RN-004 |
| [02-fechamento-pedido](02-fechamento-pedido/) | Fechamento do pedido | RN-004 |
| [03-romaneio](03-romaneio/) | Romaneio / Romaneio Consolidado | RN-010 |
| [04-separacao](04-separacao/) | Separação de Pedidos | RN-010 |
| [05-devolucao-clientes](05-devolucao-clientes/) | Devolução de clientes | RN-004 / RN-008 |

## Fluxo crítico (RN-004)

```text
Novo pedido → incluir itens → gravar → fechar
  → bloquear/liberar → reservar → separar → faturar
```

Capturar cada **estado** do pedido (`PED_STAT`) em PNG separado quando possível.
