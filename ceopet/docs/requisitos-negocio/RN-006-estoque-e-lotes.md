# RN-006 — Estoque e lotes

| Campo | Valor |
|-------|-------|
| **ID** | RN-006 |
| **Status** | Rascunho |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_LOTE`, `SCD_MLOT`, campos estoque em `SCD_PROD`, menus estoque |
| **RT relacionado** | RT-006 (pendente) |

## Objetivo

Controlar quantidades em estoque (físico, pendente, efetivo), movimentações, ajustes, lotes e consolidação.

## Contexto

Estoque é atualizado por pedidos, entradas, NF-e e ajustes manuais. Lotes controlam validade — crítico para produtos veterinários/farmaceuticos.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Estoque, expedição, compras |
| **Canal** | Desktop |
| **Objeto de negócio** | Saldo, movimento, lote |
| **Regra comercial** | Disponível = f( físico, pendente, reservas) |
| **Estoque** | Impacta — núcleo do RN |

## Requisitos funcionais (RNG)

- **RNG-006-01:** Exibir saldo por produto: físico, pendente, efetivo, reservado.
- **RNG-006-02:** Registrar movimentação manual com data, hora, usuário e motivo.
- **RNG-006-03:** Permitir ajuste de estoque (inventário / correção).
- **RNG-006-04:** Inicializar estoque (carga inicial).
- **RNG-006-05:** Processar estoque em lote (reprocessamento integrado com pedidos/NFe).
- **RNG-006-06:** Consolidar estoque multi-depósito ou visão consolidada.
- **RNG-006-07:** Cadastrar e visualizar lotes com validade vinculados a produtos.
- **RNG-006-08:** Baixar estoque por lote na separação/venda quando exigido.
- **RNG-006-09:** Inventário cíclico (contagem normal vs. rápida — menus legado).

## Critérios de aceite de negócio (CANG)

- **CANG-006-01:** Dado pedido reservado, quando confirmado, então pendente/físico reflete reserva conforme regra.
- **CANG-006-02:** Dado lote vencido, quando separação tenta usar lote, então sistema alerta ou bloqueia.
- **CANG-006-03:** Dado ajuste aprovado, quando gravado, então histórico de movimentação registra usuário e delta.

## Dúvidas em aberto

- [ ] Fórmula exata físico × pendente × efetivo (`P_ESIS`, `P_EPEN`, `P_EEFU`, `P_SALD`).
- [ ] Política FEFO/FIFO para lotes.

## Referências legado

- Tabelas: `SCD_PROD`, `SCD_LOTE`, `SCD_MLOT`, `SCD_ITEM`
- Programas: menus "Processar Estoque", "Ajuste no Estoque"
