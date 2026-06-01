# RN-007 — Fiscal e NF-e

| Campo | Valor |
|-------|-------|
| **ID** | RN-007 |
| **Status** | Rascunho |
| **Fase** | B2B-1 |
| **Origem legado** | `NFe_NOTA`, `NFe_ITEM`, `SCD_CFOP`, `scd2nfe.prg`, `scd2xml.prg` |
| **RT relacionado** | RT-007 (pendente) |

## Objetivo

Emitir e gerenciar documentos fiscais eletrônicos (NF-e), importar XML, calcular impostos e manter conformidade com CFOP/NCM.

## Contexto

Faturamento converte pedidos em NF-e. Há NF-e complementar, arquivo de notas, reprocessamento de impostos a partir de XML e envio por e-mail.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Fiscal, faturamento |
| **Canal** | Desktop; arquivos XML |
| **Objeto de negócio** | NF-e, item fiscal |
| **Regra comercial** | Tributação por produto/cliente/operacao |
| **Estoque** | Impacta — baixa fiscal/física |

## Requisitos funcionais (RNG)

- **RNG-007-01:** Gerar NF-e a partir de pedido(s) faturáveis.
- **RNG-007-02:** Emitir NF-e complementar.
- **RNG-007-03:** Informar/selecionar CFOP por operação.
- **RNG-007-04:** Calcular ICMS, IPI, PIS, COFINS, ST e FCP conforme cadastros.
- **RNG-007-05:** Numerar NF-e por série/emitente com controle de sequência.
- **RNG-007-06:** Importar arquivos XML de saída/entrada.
- **RNG-007-07:** Reprocessar resumo de impostos de XML.
- **RNG-007-08:** Consultar notas fiscais e resumo processado.
- **RNG-007-09:** Enviar NF-e por e-mail ao cliente.
- **RNG-007-10:** Manter cadastro CFOP e tabelas NCM (geral e por emitente).

## Critérios de aceite de negócio (CANG)

- **CANG-007-01:** Dado pedido não liberado, quando faturamento é acionado, então emissão é impedida.
- **CANG-007-02:** Dado XML importado válido, quando reprocessado, então impostos consolidados batem com totais do documento.
- **CANG-007-03:** Dado NF-e autorizada, quando estoque processado, então saldo reflete quantidades faturadas.

## Dúvidas em aberto

- [ ] Provedor/atual versão layout NF-e no legado (`E_VERS`).
- [ ] Tratamento de cancelamento, carta de correção e inutilização.

## Referências legado

- Tabelas: `NFe_NOTA`, `NFe_ITEM`, `SCD_CFOP`, `SCD_TNCM`
- Programas: `scd2nfe.prg`, `scd2xml.prg`, `scd1imp.prg`, `scd1nat.prg`
