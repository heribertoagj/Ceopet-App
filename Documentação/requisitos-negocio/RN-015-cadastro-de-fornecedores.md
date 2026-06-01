# RN-015 — Cadastro de fornecedores

| Campo | Valor |
|-------|-------|
| **ID** | RN-015 |
| **Status** | Validado legado (demo) — CRUD + lista + atalhos (F0024) |
| **Última validação** | 2026-06-01 — [Sessão RN-015](../legado/registro-sessoes/SESSAO-2026-06-01-RN-015.md) |
| **Visão consolidada** | [RN-CAD-001](RN-CAD-001-cadastros-mvp-consolidado.md) — G3.4, G6, fluxo compra |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_FORN`, `SCD_PCOM`, `scd1for.prg` |
| **RT relacionado** | RT-015 (pendente) |

**Escopo desta validação:** base demo `C:\Hidra`. CRUD **Incluir/Alterar** + lista + atalhos (F0024). Pendente: Baixa **Confirmar**, abas Produtos/Fechamento entrada (RN-005).

## Objetivo

Cadastrar e manter fornecedores de mercadorias e serviços, com dados cadastrais, fiscais e comerciais para pedidos de compra, entradas, notas fiscais de entrada e contas a pagar.

## Contexto

Fornecedor é entidade de **abastecimento** — distinta de **fabricante** (`SCD_FABR`). Alimenta **RN-005**, **RN-007** e **RN-008**. Permissões CRUD em **RN-001** (I/A/E/C em Fornecedores).

**Escopo MVP B2B-1:** cadastros (decisão 2026-06-01), junto com RN-003 e RN-009.

## Fluxo de telas

```text
Cadastros → Fornecedores
  ├─ Cadastro de Fornecedores (lista + atalhos)
  │     • Incluir | Alterar | Excluir | Consulta
  │     • Notas Fiscais | Pedidos de Compra | Contas a Pagar | Retornar
  └─ Informação do Fornecedor (modal Incluir/Alterar)
        ├─ Cadastro — código, tipo, empresa, endereço, fiscal
        ├─ Dados do Fornecedor — fantasia, contato, frete, prazo
        └─ SALVAR | CANCELAR
```

## Requisitos funcionais (RNG)

### Lista *(validado — F0024)*

- **RNG-015-01:** Grid **FORNECEDORES**, **CNPJ/CPF**, **TELEFONE**, **COD**; painel endereço, IE, Licença, Validade, Email.

### Consulta / atalhos *(validado — F0024)*

- **RNG-015-02:** **Consulta** — Fornecedor + Financeiro + Compras.
- **RNG-015-06:** **Notas Fiscais** → entrada **19528**.
- **RNG-015-07:** **Pedidos de Compra** → alerta sem pedido aberto.
- **RNG-015-08:** **Contas a Pagar** → 5 parcelas **19528**; **Juros** / **Baixa PAGAR**.

### Formulário Incluir/Alterar *(validado — demo)*

- **RNG-015-03:** **Incluir** / **Alterar** abrem modal **Informação do Fornecedor** — duas seções **Cadastro** e **Dados do Fornecedor** *(tela única, sem abas)*; **SALVAR** / **CANCELAR** *(rótulos em vermelho = obrigatórios)*.
- **RNG-015-03a — Seção Cadastro:** **Codigo** (combo **Automatico** + campo manual), **Tipo** (combo **Juridica** + **CNPJ/CPF** mascarado), **Empresa**, **Endereço**, **Cidade** · **Estado** · **Cep**, **Bairro** · **I.Estadual**, **Licença** · **Validade**, **Email**.
- **RNG-015-03b — Seção Dados do Fornecedor:** **Fantasia**, **Contato** · **Telefone**, **Celular** · **Frete** (combo), **Transporte**, **Prazo**, **Obs**, **Cadastro** (data).
- **RNG-015-03c — Defaults Incluir (demo):** Codigo **Automatico** · Tipo **Juridica** · Frete **Fornecedor (CIF)** · **Cadastro 01/06/2026** · demais vazios.
- **RNG-015-03d:** **Alterar F0024** — mesma estrutura; **Empresa** exibida como **Nome** na Consulta/lista.

## Validação legado (demo)

### Formulário **Incluir**

**Caminho:** Cadastros → Fornecedores → **Incluir**.

#### Seção **Cadastro**

| Campo | Default inclusão |
|-------|------------------|
| Codigo | **Automatico** (+ campo código) |
| Tipo | **Juridica** + CNPJ/CPF mascarado |
| Empresa | vazio |
| Endereço | vazio |
| Cidade / Estado / Cep | vazios |
| Bairro / I.Estadual | vazios |
| Licença / Validade | vazios |
| Email | vazio |

#### Seção **Dados do Fornecedor**

| Campo | Default inclusão |
|-------|------------------|
| Fantasia | vazio |
| Contato / Telefone | vazios *(máscara telefone)* |
| Celular / Frete | vazio · **Fornecedor (CIF)** |
| Transporte | vazio |
| Prazo | vazio |
| Obs | vazio |
| Cadastro | **01/06/2026** |

**SALVAR** · **CANCELAR**

### Contas a Pagar · Baixa *(19528/1)*

**Baixa PAGAR:** valor **1.664,71** → **Valor Calculado 21.263,90** *(Confirmar pendente)*.

## Checklist MVP cadastros

| # | Item | Status |
|---|------|--------|
| 1 | Lista + Consulta + atalhos | ✅ |
| 2 | **Incluir/Alterar** (2 seções + defaults) | ✅ |
| 3 | Baixa Confirmar | Pendente |

## Dúvidas em aberto

- [ ] Coluna **Nota Fiscal = 0** no grid Compras.
- [ ] Opções completas combo **Frete** além de *Fornecedor (CIF)*.
- [x] Formulário = **1 modal**, seções Cadastro + Dados do Fornecedor (RNG-015-03).

## Referências

- Tabelas: `SCD_FORN`, `SCD_PCOM`, `SCD_ICOM`
- RNs: [RN-CAD-001](RN-CAD-001-cadastros-mvp-consolidado.md), [RN-003](RN-003-cadastro-de-produtos.md), [RN-005](RN-005-compras-e-entradas.md), [RN-008](RN-008-financeiro-boletos-cheques.md)
