# RN-010 — Expedição: romaneio e separação

| Campo | Valor |
|-------|-------|
| **ID** | RN-010 |
| **Status** | Rascunho |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_ROMA`, `scd2sep.prg`, menus romaneio |
| **RT relacionado** | RT-010 (pendente) |

## Objetivo

Organizar a separação física de pedidos, consolidação em romaneios e impressão para expedição/logística.

## Contexto

Após liberação comercial, pedidos passam por separação (digitada ou por referência), agrupados em romaneios para entrega.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Separador, expedição, transportadora |
| **Canal** | Desktop |
| **Objeto de negócio** | Romaneio, ordem de separação |
| **Regra comercial** | Quantidade padrão de separação configurável |
| **Estoque** | Impacta — baixa na separação |

## Requisitos funcionais (RNG)

- **RNG-010-01:** Selecionar pedidos elegíveis para separação.
- **RNG-010-02:** Registrar separação por produto/quantidade (modos digitação e referência).
- **RNG-010-03:** Gerar romaneio individual e consolidado.
- **RNG-010-04:** Imprimir romaneio para conferência e entrega.
- **RNG-010-05:** Vincular romaneio a transportadora/placa quando aplicável.
- **RNG-010-06:** Pesquisar romaneio para rastreamento.
- **RNG-010-07:** Atualizar status do pedido após separação/conferência.

## Critérios de aceite de negócio (CANG)

- **CANG-010-01:** Dado pedido separado parcialmente, quando romaneio é fechado, então pendências ficam registradas.
- **CANG-010-02:** Dado romaneio consolidado, quando impresso, então lista todos os pedidos/produtos do agrupamento.

## Dúvidas em aberto

- [ ] Diferença expedição (Dig) vs. (Ref) no menu legado.
- [ ] Integração romaneio → NF-e (automática ou manual).

## Referências legado

- Tabelas: `SCD_ROMA`, campos `PED_ROMA`, `PED_SEPA`, `PED_CONF`
- Programas: `scd2sep.prg`
