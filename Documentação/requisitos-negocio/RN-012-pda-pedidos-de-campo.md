# RN-012 — PDA e pedidos de campo

| Campo | Valor |
|-------|-------|
| **ID** | RN-012 |
| **Status** | Em validação — MixPedido por cliente (demo) |
| **Fase** | B2B-1 |
| **Origem legado** | `PDA_PEDI`, `PDA_ITEM`, `PDA_CLIE`, menus "Pedidos Recebidos" |
| **RT relacionado** | RT-012 (pendente) |

## Objetivo

Receber e integrar pedidos capturados em dispositivos de campo (PDA) ao fluxo principal de vendas.

## Contexto

Representantes ou promotores coletam pedidos offline/campo; back-office importa/sincroniza para `SCD_PEDI`.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Representante (campo), back-office |
| **Canal** | PDA / dispositivo móvel legado |
| **Objeto de negócio** | Pedido PDA |
| **Regra comercial** | Mesmas validações de crédito/estoque após importação |
| **Estoque** | Indireto — após conversão em pedido oficial |

## Requisitos funcionais (RNG)

- **RNG-012-01:** Receber pedidos de campo nas modalidades suportadas (Opção A / Opção B).
- **RNG-012-02:** Validar cliente e produtos na importação.
- **RNG-012-03:** Converter pedido PDA em pedido de venda oficial.
- **RNG-012-04:** Manter rastreabilidade entre pedido PDA e pedido sistema.
- **RNG-012-05:** Exportar mix de pedido para dispositivos (`Exportar MixPedido`).
- **RNG-012-06:** Parametrizar **MixPedido por cliente** (Cadastros → Clientes → Alterar Informações): modo Enviar/receber, nova senha portal, crédito R$, bloqueio de portadores por cliente. Complementa parametrização global do emitente (RN-001).

## Validação legado — demo (2026-06-01)

Cliente C0082 — tela **MixPedido**: modo **Enviar**, Nova Senha **Não**, Crédito **0,00**, portadores não bloqueados. Cliente liberado para pedidos internet na lista.

Ver [RN-002](../requisitos-negocio/RN-002-cadastro-de-clientes.md) (RNG-002-05b).

## Critérios de aceite de negócio (CANG)

- **CANG-012-01:** Dado pedido PDA recebido, quando convertido, então número e itens aparecem no módulo de vendas.
- **CANG-012-02:** Dado cliente inexistente no PDA, quando recebido, então sistema alerta e impede conversão até cadastro.

## Dúvidas em aberto

- [ ] Diferença técnica entre Opção A e B de recebimento.
- [ ] PDA ainda em uso ou substituído por app/web.

## Referências legado

- Tabelas: `PDA_PEDI`, `PDA_ITEM`, `PDA_CLIE`
- Programas: referências em `scd2ped.prg` (inferido)
