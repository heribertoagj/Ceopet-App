# RN-000 — Visão e características de negócio

| Campo | Valor |
|-------|-------|
| **ID** | RN-000 |
| **Status** | Rascunho |
| **Fase** | ambos |
| **Origem legado** | Hidra — visão geral |
| **RT relacionado** | — (documento guarda-chuva) |

## Objetivo

Estabelecer a visão de negócio do ecossistema atual (Hidra) e as características que orientam a modernização tecnológica do **cliente Ceopet**, em projeto liderado pela **TransformaTech (TTech)**.

## Partes do projeto

| Papel | Organização |
|-------|-------------|
| **Cliente** | Ceopet |
| **Consultoria** | TransformaTech (TTech) |

## Visão

Sistema de gestão comercial e operacional para **distribuição B2B de produtos veterinários e correlatos**, cobrindo cadastros, pedidos, estoque, fiscal (NF-e), financeiro (boletos/cheques), representação comercial e integrações de campo.

## Stakeholders e atores

| Ator | Papel |
|------|-------|
| **Administrador / back-office** | Cadastros, parametrização, fiscal, financeiro, relatórios |
| **Operador comercial** | Pedidos, liberação, bloqueio, atendimento |
| **Representante / supervisor** | Metas, visitas, positivação, carteira de clientes |
| **Expedição / separação** | Romaneio, separação, lotes |
| **Cliente B2B** | Distribuidor, clínica, pet shop — recebe pedidos/NF/boletos (indireto) |
| **Fornecedor** | Abastecimento — pedidos de compra e entradas |
| **Sistemas externos** | Bancos (CNAB), SEFAZ (NFe/XML), e-mail, mapas, Venix/Accera, PDA |

## Características do negócio

### Modelo comercial

- Venda **B2B** com tabela de preços, descontos por faixa, condição de pagamento e limite de crédito.
- Operação por **representante** com comissão, metas e rotas de visita.
- Pedidos podem ser **bloqueados/liberados** (crédito, estoque, política comercial).
- Texto padrão de pedido indica sujeição a **aprovação de crédito** e estoque na entrega.

### Produto e estoque

- Produtos com NCM, EAN, embalagem, fabricante, família e categorias.
- Controle de **estoque físico, pendente e efetivo**; **lotes** e validade.
- Produtos similares e mix por representante.

### Fiscal

- Emissão e gestão de **NF-e**, CFOP, impostos (ICMS, IPI, PIS/COFINS).
- Importação e reprocessamento de **XML** fiscal.

### Financeiro

- **Boletos** com remessa/retorno bancário, estorno e e-mails de cobrança.
- **Cheques** de clientes e contas a pagar.
- Comissões e extratos por representante.

### Multi-empresa

- Suporte a **múltiplos emitentes** na mesma instalação (seleção de emitente obrigatória nas operações).

### Domínio veterinário (diferencial Ceopet)

- Cadastro de clientes com **veterinários responsáveis**, CRMV e dados de visita por dia/horário.
- Campos de alvará, observações comerciais e metas por cliente.

## Canais

| Canal | Uso |
|-------|-----|
| Desktop (Hidra) | Operação principal |
| PDA / campo | Captura e recebimento de pedidos (`PDA_*`) |
| E-mail | NF-e, débitos, comunicação em massa |
| Arquivos | CNAB, XML NFe, export MixPedido / representantes |

## Objetos de negócio principais

Emitente → Cliente → **Representante** → Produto → Pedido → Item → Romaneio → NF-e → Título/Boleto → Estoque  
Fornecedor → Pedido de compra → Entrada → Estoque

## Escopo MVP B2B-1 — cadastros *(2026-06-01)*

| RN | Cadastro | Status validação |
|----|----------|------------------|
| RN-001 | Emitente, usuários | ✅ Demo |
| RN-002 | Clientes | ✅ Demo |
| **RN-003** | **Produtos** | ✅ Demo (CRUD 5 abas) |
| **RN-009** | **Representantes** | ✅ Demo (CRUD 3 abas) |
| **RN-015** | **Fornecedores** | ⏳ Pendente |

Demais RNs (004 pedidos, 005 compras, 006 estoque, 007 NF-e, 008 financeiro) seguem após cadastros base.

## Mapa de requisitos derivados

| RN | Módulo |
|----|--------|
| [RN-001](requisitos-negocio/RN-001-emitente-usuarios-e-parametros.md) | Emitente, usuários e parâmetros |
| [RN-002](requisitos-negocio/RN-002-cadastro-de-clientes.md) | Clientes |
| [RN-003](requisitos-negocio/RN-003-cadastro-de-produtos.md) | Produtos e precificação |
| [RN-004](requisitos-negocio/RN-004-pedidos-de-venda.md) | Pedidos de venda |
| [RN-005](requisitos-negocio/RN-005-compras-e-entradas.md) | Compras e entradas |
| [RN-006](requisitos-negocio/RN-006-estoque-e-lotes.md) | Estoque e lotes |
| [RN-007](requisitos-negocio/RN-007-fiscal-nfe.md) | Fiscal / NF-e |
| [RN-008](requisitos-negocio/RN-008-financeiro-boletos-cheques.md) | Financeiro |
| [RN-009](requisitos-negocio/RN-009-representantes-comissao-metas.md) | Representantes |
| [RN-015](requisitos-negocio/RN-015-cadastro-de-fornecedores.md) | Fornecedores |
| [RN-010](requisitos-negocio/RN-010-expedicao-romaneio-separacao.md) | Expedição |
| [RN-011](requisitos-negocio/RN-011-visitas-e-crm-comercial.md) | Visitas / CRM |
| [RN-012](requisitos-negocio/RN-012-pda-pedidos-de-campo.md) | PDA |
| [RN-013](requisitos-negocio/RN-013-relatorios-e-consultas.md) | Relatórios |
| [RN-014](requisitos-negocio/RN-014-integracoes-externas.md) | Integrações |

## Referências

- [Mapa de módulos Hidra](legado/mapa-modulos-hidra.md)
- [Mapeamento de tabelas](legado/mapeamento-tabelas.md)
- [Roteiro de exploração guiada](legado/roteiro-exploracao-hidra.md)
- [Levantamento](levantamento.md)

## Dúvidas em aberto

- [x] Escopo fase 1 cadastros MVP: RN-001, 002, **003, 009, 015** (2026-06-01).
- [ ] Escopo operacional além de cadastros (004, 006, 007, …) — confirmar com Ceopet.
- [ ] Relação comercial Ceopet × clientes finais (consultoria vs. operação própria).
- [ ] Módulos Venix/Accera ainda utilizados?
