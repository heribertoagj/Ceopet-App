# RN-005 — Compras, entradas e devoluções

| Campo | Valor |
|-------|-------|
| **ID** | RN-005 |
| **Status** | Rascunho |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_PCOM`, `SCD_ICOM`, `scd2com.prg`, `scd2ent.prg` |
| **RT relacionado** | RT-005 (pendente) |

## Objetivo

Gerenciar pedidos de compra a fornecedores, entradas de mercadorias no estoque e devoluções de clientes/fornecedores.

## Contexto

Abastecimento reversa o fluxo de venda: compra gera expectativa, entrada confirma físico e atualiza custo/estoque. Devoluções têm fechamento próprio.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Compras, recebimento, fiscal |
| **Canal** | Desktop |
| **Objeto de negócio** | Pedido de compra, entrada, devolução |
| **Regra comercial** | Confronto pedido × recebido |
| **Estoque** | Impacta — entrada aumenta saldo |

## Requisitos funcionais (RNG)

- **RNG-005-01:** Criar pedido de compra para fornecedor com itens, preços e prazos.
- **RNG-005-02:** Fechar compra e registrar condições de pagamento.
- **RNG-005-03:** Registrar entrada de mercadoria vinculada ou avulsa.
- **RNG-005-04:** Detalhar produtos da compra e imprimir documento de entrada.
- **RNG-005-05:** Processar devolução de clientes com itens e motivos.
- **RNG-005-06:** Fechar devolução e refletir impacto em estoque/financeiro.
- **RNG-005-07:** Consultar entradas por período e fornecedor.

## Critérios de aceite de negócio (CANG)

- **CANG-005-01:** Dado entrada confirmada, quando processada, então saldo de estoque do produto é incrementado.
- **CANG-005-02:** Dado devolução fechada, quando vinculada a NF/pedido, então rastreabilidade é mantida.

## Dúvidas em aberto

- [ ] Integração compra → contas a pagar automática.
- [ ] CFOP e tratamento fiscal na devolução.

## Referências legado

- Tabelas: `SCD_PCOM`, `SCD_ICOM`, `SCD_FORN`, `SCD_DESP`
- Programas: `scd2com.prg`, `scd2ent.prg`
