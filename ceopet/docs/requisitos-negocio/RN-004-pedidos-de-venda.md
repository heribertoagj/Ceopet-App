# RN-004 — Pedidos de venda

| Campo | Valor |
|-------|-------|
| **ID** | RN-004 |
| **Status** | Validado legado (demo) — consulta parcial (9000975); CRUD pendente |
| **Última validação** | 2026-06-01 — via [RN-009](../requisitos-negocio/RN-009-representantes-comissao-metas.md) |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_PEDI`, `SCD_ITEM`, `scd2ven.prg`, `scd2ped.prg` |
| **RT relacionado** | RT-004 (pendente) |

**Escopo desta validação:** consulta **Consulta Pedidos de Venda** (pedido **9000975**), aba Informações. Pendente: digitação, liberação, abas Produtos/Fechamento.

## Objetivo

Registrar e gerenciar o ciclo de vida de pedidos de venda B2B, desde a digitação até fechamento, liberação e preparação para faturamento/expedição.

## Contexto

Pedidos conectam cliente, representante, produtos, condições comerciais e estoque. Suportam reserva, transferência entre distribuidores, cancelamento e relacionamento entre pedidos.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Comercial, crédito, expedição |
| **Canal** | Desktop; origem PDA (ver RN-012) |
| **Objeto de negócio** | Pedido de venda, item de pedido |
| **Regra comercial** | Sujeito a crédito; descontos; frete; texto legal no rodapé |
| **Estoque** | Impacta — reserva/baixa/pendente |

## Requisitos funcionais (RNG)

- **RNG-004-01:** Criar pedido de venda vinculado a emitente, cliente, representante e condição de pagamento.
- **RNG-004-02:** Incluir, alterar e excluir itens com quantidade, preço, desconto, CFOP e lote quando aplicável.
- **RNG-004-03:** Calcular totais: mercadoria, descontos, frete, impostos, peso e volume.
- **RNG-004-04:** Gravar rascunho e fechar pedido (commit comercial).
- **RNG-004-05:** Bloquear e liberar pedido (crédito/operação).
- **RNG-004-06:** Reservar estoque para pedidos conforme status.
- **RNG-004-07:** Transferir pedido entre distribuidores/representantes.
- **RNG-004-08:** Cancelar pedido e manter histórico.
- **RNG-004-09:** Relacionar pedidos entre si.
- **RNG-004-10:** Consultar pedidos pendentes, cancelados, transferidos, reservados.
- **RNG-004-13:** **Consulta Pedidos de Venda** — cabeçalho: **Número**, **Data de Emissão**, **Horário de Entrada**; abas **Informações do Pedido**, **Produtos do Pedido**, **Fechamento do pedido**; blocos **Destinatário**, **Acompanhamento do Pedido** (Pedido, Financeiro, Faturamento, Entrega, Concluído), **Representante**; tipo (Pedido/Troca/Bonificação/Entrega/Outros), **Data de Entrega**, **Tabela**, **Frete**, **Origem**, **Estoque**, **Informação**, **Observação**; **Aplicativo de Origem** (ex.: **VENIX**); **Retornar**.
- **RNG-004-11:** Imprimir pedido e exportar mix de pedido.
- **RNG-004-12:** Registrar datas de liberação, saída e entrega programada.

## Critérios de aceite de negócio (CANG)

- **CANG-004-01:** Dado pedido fechado, quando item é alterado sem permissão, então operação é bloqueada.
- **CANG-004-02:** Dado cliente bloqueado, quando pedido é fechado, então sistema exige liberação ou impede.
- **CANG-004-03:** Dado pedido liberado, quando separação é iniciada, então itens e quantidades ficam disponíveis para expedição (RN-010).
- **CANG-004-04:** Dado pedido consultado, quando operador visualiza **Acompanhamento**, então datas/horas de Pedido, Financeiro, Faturamento, Entrega e Concluído refletem o status real.

## Validação legado — demo (2026-06-01)

**Caminho:** Cadastros → Representantes → Pedidos → **9000975** → **Detalhe** (ou equivalente em consulta de pedidos).

### Consulta Pedidos de Venda — **9000975**

| Campo | Demo |
|-------|------|
| Número / Emissão / Entrada | 9000975 · 23/09/2016 · 07:39:50 |
| Cliente | C0082 — DISTRIBUIDORA BUENO LTDA - EPP |
| Representante | V0002 — REPRESENTANTE TESTE |
| Acompanhamento | Pedido 22/09 17:39 · Fin. 23/09 07:39 · Fat. *Aguardando* · Entrega 23/09 11:00 · Conc. *Aguardando* |
| Tipo / Tabela / Frete | Pedido · Preço (A) · 0-Emitente |
| Origem / Estoque | Outros · Baixa Estoque |
| Observação | [B] |
| App origem | **VENIX** |

Abas **Produtos** e **Fechamento** — pendente.

## Regras e restrições conhecidas

- `PED_SIST` identifica subsistema/tipo (`C` observado em amostra = comercial).
- Status em `PED_STAT`; flags de separação/confirmação (`PED_SEPA`, `PED_CONF`).
- Mensagem padrão emitente: pedido sujeito a aprovação de crédito e estoque na entrega.

## Dúvidas em aberto

- [ ] Estados completos de `PED_STAT` e transições permitidas.
- [ ] Diferença entre "Pedidos Recebidos Opção A" e "Opção B".
- [ ] Regra exata de transferência entre distribuidores.

## Referências legado

- Tabelas: `SCD_PEDI`, `SCD_ITEM`, `SCD_COND`
- Programas: `scd2ven.prg`, `scd2ped.prg`, `scd2sep.prg`
- RNs relacionados: [RN-002](RN-002-cadastro-de-clientes.md), [RN-009](RN-009-representantes-comissao-metas.md), [RN-010](RN-010-separacao-e-expedicao.md), [RN-012](RN-012-pda-pedidos-de-campo.md)
