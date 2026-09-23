# Fornecedores

| Campo | Valor |
|-------|-------|
| **Menu** | Cadastros → Fornecedores |
| **RN** | [RN-015](../../requisitos-negocio/RN-015-cadastro-de-fornecedores.md) |
| **Programa legado** | `scd1for.prg` |
| **Tabelas DBF** | `SCD_FORN`, `SCD_PCOM` |
| **Registro demo** | F0024 |
| **Última captura** | 2026-06-19 |
| **Sessão** | [SESSAO-2026-06-01-RN-015](../registro-sessoes/SESSAO-2026-06-01-RN-015.md) |

## Fluxo de telas

```text
Cadastros → Fornecedores
  ├─ Cadastro de Fornecedores (lista + atalhos)
  │     • Incluir | Alterar | Excluir | Consulta
  │     • Notas Fiscais | Pedidos de Compra | Contas a Pagar
  └─ Informação do Fornecedor (Incluir/Alterar)
        ├─ Cadastro — código, tipo, empresa, endereço, fiscal
        ├─ Dados do Fornecedor — fantasia, contato, frete, prazo
        └─ SALVAR | CANCELAR
```

## Checklist de capturas

| Arquivo | RNG | Descrição | Status |
|---------|-----|-----------|--------|
| `RN-015-01-lista.png` | RNG-015-01 | Grid FORNECEDORES + painel F0024 | ✅ |
| `RN-015-02-incluir-cadastro.png` | RNG-015-03a | Seção Cadastro (defaults Incluir) | ✅ |
| `RN-015-03-incluir-dados-fornecedor.png` | RNG-015-03b | Seção Dados do Fornecedor | ✅ |
| `RN-015-04-alterar-f0024.png` | RNG-015-03d | Alterar F0024 | ✅ |
| `RN-015-05-consulta.png` | RNG-015-02 | Consulta cadastro (RETORNAR) | ✅ |
| `RN-015-05b-consulta-financeiro-compras.png` | RNG-015-02 | Consulta — Financeiro + Compras | ✅ |
| `RN-015-06-atalho-notas-fiscais.png` | RNG-015-06 | Compras / entrada 19528 | ✅ |
| `RN-015-07-atalho-pedidos-compra.png` | RNG-015-07 | Alerta: pedidos não localizados F0024 | ✅ |
| `RN-015-08-atalho-contas-pagar.png` | RNG-015-08 | 5 parcelas 19528 + Juros + Baixa | ✅ |

## Pendente (RN-005)

| Arquivo | Descrição | Status |
|---------|-----------|--------|
| `RN-015-09-baixa-confirmar.png` | Baixa Confirmar em Contas a Pagar | ⏳ |
| `RN-015-10-produtos-entrada.png` | Abas Produtos / Fechamento entrada | ⏳ |

## Notas

- Defaults Incluir: Automatico · Juridica · CIF · data cadastro demo.
- Fornecedor abastece compra; fabricante classifica venda (RN-003).
