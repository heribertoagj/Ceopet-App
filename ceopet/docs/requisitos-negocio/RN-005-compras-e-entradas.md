# RN-005 — Compras, entradas e devoluções

| Campo | Valor |
|-------|-------|
| **ID** | RN-005 |
| **Status** | Validado legado (demo) — consulta entrada parcial (19528 / F0024) |
| **Última validação** | 2026-06-01 — via [RN-015](RN-015-cadastro-de-fornecedores.md) atalho Notas Fiscais |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_PCOM`, `SCD_ICOM`, `scd2com.prg`, `scd2ent.prg` |
| **RT relacionado** | RT-005 (pendente) |

## Objetivo

Gerenciar pedidos de compra a fornecedores, entradas de mercadorias no estoque e devoluções de clientes/fornecedores.

## Contexto

Abastecimento reversa o fluxo de venda: compra gera expectativa, **entrada** confirma físico e atualiza custo/estoque. Devoluções têm fechamento próprio.

Acesso validado via **Cadastros → Fornecedores → Notas Fiscais → Detalhe** (fornecedor F0024, documento 19528).

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Compras, recebimento, fiscal |
| **Canal** | Desktop |
| **Objeto de negócio** | Pedido de compra, entrada de mercadoria, devolução |
| **Regra comercial** | Confronto pedido × recebido; rateio produtos/descontos |
| **Estoque** | Impacta — entrada aumenta saldo |

## Fluxo de telas *(parcial — consulta entrada)*

```text
Fornecedores → Notas Fiscais
  └─ Informação do Fornecedor — grid Compras
        └─ Detalhe
              └─ Consulta Entrada de Mercadoria - Compra
                    • DOCUMENTO · FORNECEDOR · DATA DA ENTRADA
                    • Abas: Informações da Compra ✅ | Produtos | Fechamento
                    • Retornar | Imprimir

Pedidos de Compra (fornecedor sem pedido aberto):
  └─ Alerta — Pedidos de Compras não Localizados
```

## Requisitos funcionais (RNG)

### Consulta entrada *(validado — demo 19528)*

- **RNG-005-07:** Consultar entrada de mercadoria por fornecedor — **Consulta Entrada de Mercadoria - Compra**; cabeçalho **DOCUMENTO**, **FORNECEDOR**, **DATA DA ENTRADA**; abas **Informações da Compra**, **Produtos da Compra**, **Fechamento da Compra**; **Retornar**, **Imprimir**.
- **RNG-005-07a:** Aba **Informações da Compra** — **Remetente** (CNPJ, nome, endereço, IE, e-mail); **Natureza**, **ChaveNFe**; **Tipo** (NF-e), **Regime**, **Frete**; **VALORES PARA RATEIO** (Produtos, Peso Bruto, Frete, Descontos); tipo operação (**Compra**, Devolução, Bonificação, Entrega, Outros); **Modelo**, **Serie**, **Emissão**, **Saida**; **Observação (120)**.
- **RNG-005-07b:** Demo **19528** / **F0024** — NF-e modelo **55**, chave **35160815741848000149550010000195281577540405**, natureza **VENDA DE MERCADORIA**, produtos **10.150,68**, descontos **1.827,12**, líquido **8.323,56** *(valor no grid Compras)*.

### Demais *(pendente)*

- **RNG-005-01:** Criar pedido de compra para fornecedor com itens, preços e prazos.
- **RNG-005-02:** Fechar compra e registrar condições de pagamento.
- **RNG-005-03:** Registrar entrada de mercadoria vinculada ou avulsa.
- **RNG-005-04:** Detalhar produtos da compra e imprimir documento de entrada.
- **RNG-005-05:** Processar devolução de clientes com itens e motivos.
- **RNG-005-06:** Fechar devolução e refletir impacto em estoque/financeiro.

## Critérios de aceite de negócio (CANG)

- **CANG-005-01:** Dado entrada confirmada, quando processada, então saldo de estoque do produto é incrementado.
- **CANG-005-02:** Dado devolução fechada, quando vinculada a NF/pedido, então rastreabilidade é mantida.
- **CANG-005-03:** Dado entrada **19528** consultada, quando operador visualiza **Informações da Compra**, então **ChaveNFe**, rateio e remetente correspondem ao documento fiscal registrado.

## Validação legado (demo)

**Documento:** **19528** · **F0024** · entrada **30/08/2016**

Ver detalhamento em [RN-015](RN-015-cadastro-de-fornecedores.md) — atalho **Notas Fiscais** → **Detalhe**.

## Funcionalidades pendentes (checklist)

| # | Item | Status |
|---|------|--------|
| 1 | Consulta entrada — aba Informações | ✅ Validado |
| 2 | Abas Produtos / Fechamento | Pendente |
| 3 | CRUD pedido de compra | Pendente |
| 4 | Digitação/liberação entrada | Pendente |
| 5 | Devoluções | Pendente |

## Dúvidas em aberto

- [ ] Integração compra → contas a pagar automática.
- [ ] CFOP e tratamento fiscal na devolução.
- [ ] Relação pedido de compra aberto × entrada 19528 (entrada avulsa ou PO já fechado?).

## Referências legado

- Tabelas: `SCD_PCOM`, `SCD_ICOM`, `SCD_FORN`, `SCD_DESP`
- Programas: `scd2com.prg`, `scd2ent.prg`
- RNs relacionados: [RN-015](RN-015-cadastro-de-fornecedores.md), [RN-006](RN-006-estoque-e-lotes.md), [RN-007](RN-007-fiscal-nfe.md), [RN-008](RN-008-financeiro-boletos-cheques.md)
