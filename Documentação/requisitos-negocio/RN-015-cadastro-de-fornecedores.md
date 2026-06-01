# RN-015 — Cadastro de fornecedores

| Campo | Valor |
|-------|-------|
| **ID** | RN-015 |
| **Status** | Em validação legado (demo) — rascunho |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_FORN`, `scd1for.prg` |
| **RT relacionado** | RT-015 (pendente) |

## Objetivo

Cadastrar e manter fornecedores de mercadorias e serviços, com dados cadastrais, fiscais e comerciais para pedidos de compra, entradas e contas a pagar.

## Contexto

Fornecedor é entidade de **abastecimento** — distinta de **fabricante** (`SCD_FABR`, classificação de produto). Alimenta **RN-005** (pedido de compra, entrada) e **RN-008** (contas a pagar). Permissões CRUD em **RN-001** (I/A/E/C em Fornecedores).

**Escopo MVP B2B-1:** incluído junto com cadastros de clientes, produtos e representantes (decisão 2026-06-01). Validação legado **pendente** — após RN-003 ou em paralelo.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Compras, fiscal, financeiro |
| **Canal** | Desktop |
| **Objeto de negócio** | Fornecedor |
| **Regra comercial** | Condições de compra; vínculo com pedido e NF entrada |
| **Estoque** | Indireto — via entrada de mercadoria |

## Fluxo de telas *(inferido — validar no Hidra)*

```text
Cadastros ? Fornecedores
  ?? Cadastro de Fornecedores (lista + detalhe)
  ?     • Pesquisa
  ?     • Incluir | Alterar | Excluir | Consulta
  ?? Informação do Fornecedor
        • Abas: *(a confirmar via prints)*
```

## Requisitos funcionais (RNG)

- **RNG-015-01:** Pesquisar fornecedores e exibir lista com código, razão social, documento e indicadores operacionais.
- **RNG-015-02:** Incluir, alterar, excluir e consultar fornecedor; código automático ou informado.
- **RNG-015-03:** Manter dados cadastrais: tipo pessoa, CNPJ/CPF, razão social, fantasia, endereço, contato, e-mail, IE.
- **RNG-015-04:** Manter dados fiscais/comerciais relevantes à compra (condição pagamento, observações — *campos a confirmar*).
- **RNG-015-05:** Disponibilizar fornecedor em lookup de **pedido de compra** (RN-005) e **entrada de mercadoria**.

## Critérios de aceite de negócio (CANG)

- **CANG-015-01:** Dado fornecedor cadastrado, quando operador cria pedido de compra, então fornecedor é selecionável e dados cadastrais são recuperados.
- **CANG-015-02:** Dado alteração em **Informação do Fornecedor**, quando operador confirma **Gravar**, então dados persistem; **Cancelar** descarta.

## Dúvidas em aberto

- [ ] Estrutura de abas do formulário (paridade com cliente?).
- [ ] Fornecedor × fabricante — quando Ceopet usa cada um.
- [ ] Bloqueio/inativação de fornecedor.

## Referências legado

- Tabelas: `SCD_FORN`, `SCD_PCOM`, `SCD_ICOM`
- Programas: `scd1for.prg`
- RNs relacionados: [RN-005 — Compras](RN-005-compras-e-entradas.md), [RN-008 — Financeiro](RN-008-financeiro-boletos-cheques.md), [RN-001 — Permissões](RN-001-emitente-usuarios-e-parametros.md)
