# Produtos

| Campo | Valor |
|-------|-------|
| **Menu** | Cadastros → Produtos |
| **RN** | [RN-003](../../requisitos-negocio/RN-003-cadastro-de-produtos.md) |
| **Programa legado** | `scd1pro.prg` |
| **Tabelas DBF** | `SCD_PROD`, `SCD_LOTE` |
| **Registro demo** | 00427 |
| **Última captura** | — |
| **Sessão** | [SESSAO-2026-06-01-RN-003](../registro-sessoes/SESSAO-2026-06-01-RN-003.md) |

## Fluxo de telas

```text
Cadastros → Produtos
  ├─ Cadastro de Produtos (lista + atalhos)
  └─ Informação do Produto (Incluir/Alterar)
        ├─ Cadastro
        ├─ Fiscal
        ├─ Precificação
        ├─ Estoque
        └─ Diversos
```

## Checklist — lista e formulário

| Arquivo | RNG | Descrição | Status |
|---------|-----|-----------|--------|
| `RN-003-01-lista.png` | RNG-003-17 | Grid + painel produto | ⏳ |
| `RN-003-02-incluir-cadastro.png` | RNG-003-18 | Aba Cadastro | ⏳ |
| `RN-003-03-incluir-fiscal.png` | RNG-003-02 | Aba Fiscal (NCM) | ⏳ |
| `RN-003-04-incluir-precificacao.png` | RNG-003-04 | Tabelas A–J, descontos | ⏳ |
| `RN-003-05-incluir-estoque.png` | RNG-003-12 | Aba Estoque | ⏳ |
| `RN-003-06-incluir-diversos.png` | RNG-003-19 | Venix, MixPedido | ⏳ |
| `RN-003-07-alterar-00427.png` | RNG-003-18 | Alterar demo 00427 | ⏳ |

## Checklist — atalhos da lista

| Arquivo | RNG | Descrição | Status |
|---------|-----|-----------|--------|
| `RN-003-08-atalho-tabelas.png` | RNG-003-04 | Tabelas de preço | ⏳ |
| `RN-003-09-atalho-estoque.png` | RNG-003-13 | Consulta estoque | ⏳ |
| `RN-003-10-atalho-lotes.png` | RNG-003-15 | Lotes | ⏳ |
| `RN-003-11-atalho-inventario.png` | RNG-003-14 | Inventário | ⏳ |

Cadastros auxiliares (fabricante, família, NCM…): pasta [05-produtos-auxiliares](05-produtos-auxiliares/).
