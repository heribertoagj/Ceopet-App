# Documentação — Modernização Ceopet (legado Hidra)

Repositório de requisitos para atualização tecnológica do sistema **Hidra** (xHarbour/DBF) do **cliente Ceopet**, conduzido pela **TransformaTech (TTech)**.

## Partes do projeto

| Papel | Organização |
|-------|-------------|
| **Cliente** | Ceopet |
| **Consultoria e documentação** | TransformaTech (TTech) |

## Estrutura

| Pasta / arquivo | Conteúdo |
|-----------------|----------|
| [levantamento.md](levantamento.md) | Como o mapeamento foi feito e lacunas |
| [decisoes.md](decisoes.md) | ADRs do projeto |
| [legado/](legado/) | Engenharia reversa, roteiro e sessões |
| [legado/telas/](legado/telas/) | Catálogo visual — PNGs por menu + RN |
| [requisitos-negocio/](requisitos-negocio/) | RNs — organizados **por menu Hidra** |
| [requisitos-tecnicos/](requisitos-tecnicos/) | RTs — como implementar (a criar) |
| [_tools/](_tools/) | Scripts de inventário (JSON, charset) |
| `../apps/legado/Hidra/` | Binários e bases DBF do legado |
| `../apps/legado/_tools/` | Scripts operacionais do legado (PDF, setup) |

## Fluxo de trabalho

```text
Legado Hidra → Levantamento → RN (negócio) → RT (técnico) → Implementação
```

Skills Cursor em `.cursor/skills/` automatizam cada etapa.

## Entregas para stakeholders

| Arquivo | Descrição |
|---------|-----------|
| [Ceopet-Requisitos-Negocio-20260601.pdf](entregas/Ceopet-Requisitos-Negocio-20260601.pdf) | PDF consolidado RN-000 a RN-014 |

Para regenerar após alterações nos RNs:

```bash
python ceopet/apps/legado/_tools/gerar_pdf_requisitos.py
```

## Índice — Requisitos de negócio (RN)

| ID | Título | Status | Fase |
|----|--------|--------|------|
| [RN-000](requisitos-negocio/RN-000-visao-e-caracteristicas-negocio.md) | Visão e características de negócio | Rascunho | ambos |
| [RN-001](requisitos-negocio/RN-001-emitente-usuarios-e-parametros.md) | Emitente, usuários e parâmetros | Validado legado (demo) | B2B-1 |
| [RN-002](requisitos-negocio/RN-002-cadastro-de-clientes.md) | Cadastro de clientes | Validado legado (demo) — parcial | B2B-1 |
| [RN-003](requisitos-negocio/RN-003-cadastro-de-produtos.md) | Produtos e precificação | Validado legado (demo) | B2B-1 |
| [RN-004](requisitos-negocio/RN-004-pedidos-de-venda.md) | Pedidos de venda | Validado legado (demo) — parcial | B2B-1 |
| [RN-005](requisitos-negocio/RN-005-compras-e-entradas.md) | Compras e entradas | Validado legado (demo) — parcial | B2B-1 |
| [RN-006](requisitos-negocio/RN-006-estoque-e-lotes.md) | Estoque e lotes | Rascunho | B2B-1 |
| [RN-007](requisitos-negocio/RN-007-fiscal-nfe.md) | Fiscal / NF-e | Rascunho | B2B-1 |
| [RN-008](requisitos-negocio/RN-008-financeiro-boletos-cheques.md) | Financeiro | Rascunho | B2B-1 |
| [RN-009](requisitos-negocio/RN-009-representantes-comissao-metas.md) | Representantes e metas | Validado legado (demo) | B2B-1 |
| [RN-015](requisitos-negocio/RN-015-cadastro-de-fornecedores.md) | Fornecedores | Validado legado (demo) | B2B-1 |
| [RN-CAD-001](requisitos-negocio/RN-CAD-001-cadastros-mvp-consolidado.md) | **Cadastros MVP — visão consolidada** | Agregação 001/002/003/009/015 | B2B-1 |
| [RN-010](requisitos-negocio/RN-010-expedicao-romaneio-separacao.md) | Expedição e romaneio | Rascunho | B2B-1 |
| [RN-011](requisitos-negocio/RN-011-visitas-e-crm-comercial.md) | Visitas e CRM | Rascunho | B2B-1 |
| [RN-012](requisitos-negocio/RN-012-pda-pedidos-de-campo.md) | PDA / pedidos de campo | Rascunho | B2B-1 |
| [RN-013](requisitos-negocio/RN-013-relatorios-e-consultas.md) | Relatórios | Rascunho | B2B-1 |
| [RN-014](requisitos-negocio/RN-014-integracoes-externas.md) | Integrações externas | Rascunho | B2B-1 |

Template: [_TEMPLATE-RN.md](requisitos-negocio/_TEMPLATE-RN.md)

## Índice — Requisitos técnicos (RT)

| ID | RN | Status |
|----|-----|--------|
| RT-001 a RT-014 | A derivar dos RNs | Pendente |

## Legado

- [Mapa de módulos Hidra](legado/mapa-modulos-hidra.md)
- [Catálogo visual de telas (PNG)](legado/telas/README.md)
- [Mapeamento de tabelas DBF](legado/mapeamento-tabelas.md)
- Inventário machine-readable: [_tools/hidra_inventory.json](_tools/hidra_inventory.json)

## Próximos passos

1. Validar RNs com usuários Ceopet (marcar **Validado negócio**).
2. Extrair fontes de `ceopet/apps/legado/Hidra/Hidra.rar` quando possível.
3. Derivar RTs prioritários (MVP cadastros: RN-001, 002, **003, 009, 015**; operação: 004, 006, 007).
4. Definir escopo B2C (fase 2) se aplicável.
