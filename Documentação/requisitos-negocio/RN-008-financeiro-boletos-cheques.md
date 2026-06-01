# RN-008 — Financeiro: boletos, cheques e contas

| Campo | Valor |
|-------|-------|
| **ID** | RN-008 |
| **Status** | Em validação — Titulos cliente/rep + Contas a Pagar fornecedor (demo) |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_CHEQ`, `SCD_CAIX`, `SCD_MCAI`, `scd2bol.prg`, `scd2fin.prg`, `Boleto/bplugin` |
| **RT relacionado** | RT-008 (pendente) |

## Objetivo

Gerenciar recebíveis via boletos bancários, cheques de clientes, contas a pagar, juros e comunicação de débitos.

## Contexto

Após faturamento, títulos são gerados e enviados ao banco via remessa; retornos atualizam situação. Plugin dedicado gera boletos.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Financeiro, cobrança |
| **Canal** | Desktop; CNAB; e-mail |
| **Objeto de negócio** | Boleto, cheque, título, caixa |
| **Regra comercial** | Juros/multa configuráveis por emitente |
| **Estoque** | Não impacta |

## Requisitos funcionais (RNG)

- **RNG-008-01:** Gerar boletos a partir de NF/pedidos/títulos em aberto.
- **RNG-008-02:** Imprimir boleto e segunda via.
- **RNG-008-03:** Gerar arquivo remessa bancária (layout 341 observado).
- **RNG-008-04:** Importar arquivo retorno e baixar/estornar títulos.
- **RNG-008-05:** Estornar boleto com rastreabilidade.
- **RNG-008-06:** Consultar resumo de boletos processados.
- **RNG-008-07:** Controlar cheques recebidos de clientes.
- **RNG-008-08:** Gerenciar contas a pagar (fornecedores/despesas); atalho **Cadastros → Fornecedores → Contas a Pagar** — grid parcelas **PAGAR**, **Juros**, **Baixa** *(RN-015 RNG-015-08)*.
- **RNG-008-09:** Calcular juros sobre títulos em atraso; botão **Juros** atualiza coluna **Valor da Parcela** na grid — **cliente** (RN-002), **representante** (RN-009) e **fornecedor PAGAR** (RN-015).
- **RNG-008-09a:** Baixa manual de título: informar data pagamento, valor pago, variação; sistema calcula **Valor Calculado** com juros/multa conforme dias de atraso e parâmetros do emitente — tipo **RECEBER** (cliente) ou **PAGAR** (fornecedor).
- **RNG-008-09b:** **Extrato financeiro** — por **cliente** (RN-002: analítico, ordem vencimento, portador, período) ou por **representante** (RN-009: analítico, seleção Aberto, portador, impressora/arquivo .txt, período); **Confirmar** imprime ou exporta relatório.
- **RNG-008-10:** Enviar e-mail informativo de débitos por período.
- **RNG-008-11:** Registrar movimentos de caixa.

## Critérios de aceite de negócio (CANG)

- **CANG-008-01:** Dado retorno bancário de liquidação, quando importado, então título é baixado com data e valor corretos.
- **CANG-008-02:** Dado boleto estornado, quando consultado, então status impede nova remessa sem reemissão.
- **CANG-008-03:** Dado cheque sem seleção, quando operação exige seleção, então sistema alerta (mensagem legado).
- **CANG-008-04:** Dado títulos vencidos listados, quando operador aciona **Juros**, então valores na coluna **Valor da Parcela** refletem principal + juros recalculados.
- **CANG-008-05:** Dado baixa confirmada, quando data pagamento posterior ao vencimento, então **Valor Calculado** inclui juros (ex.: RECEBER 427,79 → 5.432,93; PAGAR 1.664,71 → 21.263,90).

## Validação legado — demo (2026-06-01)

Via **Cadastros → Clientes → Titulos** (cliente C0082):

| Item | Demo |
|------|------|
| Parcelas | 9000975/1 Cheque 427,79 · 9000975/2 Boleto 427,78 (banco 237) |
| Juros | Atualiza **Valor da Parcela** na grid |
| Extrato (cliente) | Analítico por vencimento — relatório com valor / valor pago / portador |
| Extrato (representante V0002) | Analítico · Aberto · Todos · Impressora · 21/10/2016–01/06/2026 → **impressão** ao Confirmar |
| Juros (cliente / representante) | Atualiza **Valor da Parcela** na grid |
| Baixa (cliente C0082) | 9000975/1 RECEBER — valor calculado com juros |
| Baixa (representante) | **Sem ação** na demo |
| Contas a Pagar (fornecedor F0024) | 5× 19528/1–5 · boleto 001 · status N |
| Juros (fornecedor F0024) | Atualiza **Valor da Parcela** *(paridade cliente/rep)* |
| Baixa PAGAR (F0024) | 19528/1 — 1.664,71 → **Valor Calculado 21.263,90** *(Confirmar não testado)* |

Ver também [RN-002](../requisitos-negocio/RN-002-cadastro-de-clientes.md) (RNG-002-14), [RN-009](../requisitos-negocio/RN-009-representantes-comissao-metas.md) (RNG-009-16b) e [RN-015](../requisitos-negocio/RN-015-cadastro-de-fornecedores.md) (RNG-015-08).

## Dúvidas em aberto

- [ ] Bancos/carteiras ativos além do 341.
- [ ] Integração completa contas a pagar × compras.

## Referências legado

- Tabelas: `SCD_CHEQ`, `SCD_CAIX`, `SCD_MCAI`, `SCD_BANC`, `SCD_COND`
- Programas: `scd2bol.prg`, `scd2fin.prg`, `scd1che.prg`, `scd1ban.prg`
- Pastas: `Remessa/`, `Retorno/`
