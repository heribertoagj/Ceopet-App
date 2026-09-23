# Representantes

| Campo | Valor |
|-------|-------|
| **Menu** | Cadastros → Representantes |
| **RN** | [RN-009](../../requisitos-negocio/RN-009-representantes-comissao-metas.md) |
| **Programa legado** | `scd1rep.prg`, `scd3rep.prg` |
| **Tabelas DBF** | `SCD_REPR`, `SCD_META` |
| **Registro demo** | V0002 |
| **Última captura** | 2026-06-19 |
| **Sessão** | [SESSAO-2026-06-01-RN-009](../registro-sessoes/SESSAO-2026-06-01-RN-009.md) |

## Fluxo de telas

```text
Cadastros → Representantes
  ├─ Cadastro de Representantes (lista + atalhos)
  └─ Informação do Representante (Incluir/Alterar)
        ├─ Cadastro
        ├─ Comercial
        └─ Metas
```

## Checklist de capturas

| Arquivo | RNG | Descrição | Status |
|---------|-----|-----------|--------|
| `RN-009-01-lista.png` | RNG-009-01…01f | Grid + painéis V0002 | ✅ |
| `RN-009-02-incluir-cadastro.png` | RNG-009-02, 02a | Aba Dados Cadastrais + cabeçalho | ✅ |
| `RN-009-03-incluir-operacao-credito.png` | RNG-009-02b | Aba Operação de crédito | ✅ |
| `RN-009-04-incluir-diversas.png` | RNG-009-02c | Aba Informações Diversas (meta) | ✅ |
| `RN-009-05-alterar-v0002.png` | RNG-009-02 | Alterar V0002 — Dados Cadastrais | ✅ |
| `RN-009-06-atalho-pedidos.png` | RNG-009-15 | Pedidos 9000975, 9000976 | ✅ |
| `RN-009-07-atalho-comissao.png` | RNG-009-18 | Extrato de Comissão | ✅ |
| `RN-009-08-geolocalizacao.png` | RNG-009-01e | Painel geoloc na lista (ver #1) | ✅ |

## Atalhos adicionais

| Arquivo | RNG | Descrição | Status |
|---------|-----|-----------|--------|
| `RN-009-09-atalho-titulos.png` | RNG-009-16 | Títulos 9000975/1–2 · C0082 | ✅ |
| `RN-009-10-atalho-historico.png` | RNG-009-17 | Histórico do representante | ✅ |
| `RN-009-11-atalho-referencia-periodo.png` | RNG-009-19 | Modal período Referência | ✅ |
| `RN-009-12-atalho-referencia-grid.png` | RNG-009-19a | Grid Referência (demo vazio) | ✅ |

Operações de metas/comissão fora do cadastro: [07-comercial-crm/03-metas-projecoes](../../07-comercial-crm/03-metas-projecoes/).
