# RN-008 — Financeiro: boletos, cheques e contas

| Campo | Valor |
|-------|-------|
| **ID** | RN-008 |
| **Status** | Em validação — atalho Titulos via cliente (demo) |
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
- **RNG-008-08:** Gerenciar contas a pagar (fornecedores/despesas).
- **RNG-008-09:** Calcular juros sobre títulos em atraso; botão **Juros** na consulta por cliente **atualiza coluna Valor da Parcela** (atalho Cadastros → Clientes → Titulos).
- **RNG-008-09a:** Baixa manual de título (RECEBER): informar data pagamento, valor pago, variação; sistema calcula **valor calculado** com juros/multa conforme dias de atraso e parâmetros do emitente.
- **RNG-008-09b:** Extrato financeiro analítico por vencimento — filtros de formato, ordem, portador, período e representante.
- **RNG-008-10:** Enviar e-mail informativo de débitos por período.
- **RNG-008-11:** Registrar movimentos de caixa.

## Critérios de aceite de negócio (CANG)

- **CANG-008-01:** Dado retorno bancário de liquidação, quando importado, então título é baixado com data e valor corretos.
- **CANG-008-02:** Dado boleto estornado, quando consultado, então status impede nova remessa sem reemissão.
- **CANG-008-03:** Dado cheque sem seleção, quando operação exige seleção, então sistema alerta (mensagem legado).
- **CANG-008-04:** Dado títulos vencidos listados, quando operador aciona **Juros**, então valores na coluna **Valor da Parcela** refletem principal + juros recalculados.
- **CANG-008-05:** Dado baixa confirmada, quando data pagamento posterior ao vencimento, então **valor calculado** inclui juros (ex.: demo 427,79 → 5.432,93).

## Validação legado — demo (2026-06-01)

Via **Cadastros → Clientes → Titulos** (cliente C0082):

| Item | Demo |
|------|------|
| Parcelas | 9000975/1 Cheque 427,79 · 9000975/2 Boleto 427,78 (banco 237) |
| Juros | Atualiza **Valor da Parcela** na grid |
| Extrato | Analítico por vencimento — relatório com valor / valor pago / portador |
| Baixa | 9000975/1 — 28 dias, valor calculado 5.432,93 em pagamento 01/06/2026 |

Ver também [RN-002](../requisitos-negocio/RN-002-cadastro-de-clientes.md) (RNG-002-14).

## Dúvidas em aberto

- [ ] Bancos/carteiras ativos além do 341.
- [ ] Integração completa contas a pagar × compras.

## Referências legado

- Tabelas: `SCD_CHEQ`, `SCD_CAIX`, `SCD_MCAI`, `SCD_BANC`, `SCD_COND`
- Programas: `scd2bol.prg`, `scd2fin.prg`, `scd1che.prg`, `scd1ban.prg`
- Pastas: `Remessa/`, `Retorno/`
