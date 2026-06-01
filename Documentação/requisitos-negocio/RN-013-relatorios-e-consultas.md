# RN-013 — Relatórios e consultas gerenciais

| Campo | Valor |
|-------|-------|
| **ID** | RN-013 |
| **Status** | Rascunho |
| **Fase** | B2B-1 |
| **Origem legado** | `scd3rel.prg`, `scd3fin.prg`, menus Relatórios |
| **RT relacionado** | RT-013 (pendente) |

## Objetivo

Disponibilizar consultas e relatórios operacionais e gerenciais sobre vendas, estoque, financeiro e desempenho comercial.

## Contexto

Grande volume de relatórios no legado — extraídos de strings como "PRODUTOS X VENDAS", "REPRESENTANTES X VENDAS", devedores, estoque, comissão.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Gestão, comercial, financeiro, estoque |
| **Canal** | Desktop; export Excel |
| **Objeto de negócio** | Relatório, exportação |
| **Regra comercial** | Filtros por período, representante, produto |
| **Estoque** | Consulta |

## Requisitos funcionais (RNG)

- **RNG-013-01:** Consultar vendas por período, cliente, representante e produto.
- **RNG-013-02:** Relatório produtos × vendas e clientes × representantes.
- **RNG-013-03:** Resumo por produto e por representante.
- **RNG-013-04:** Relatórios de estoque físico/pendente.
- **RNG-013-05:** Relatórios financeiros: devedores, boletos, comissão, extrato.
- **RNG-013-06:** Relatório de metas por representante.
- **RNG-013-07:** Estatísticas gerais e comparativos.
- **RNG-013-08:** Exportar resultados (Excel e formatos legado).

## Critérios de aceite de negócio (CANG)

- **CANG-013-01:** Dado filtro de período, quando relatório de vendas é gerado, então totais batem com pedidos/NF do intervalo.
- **CANG-013-02:** Dado export Excel, quando aberto, então colunas correspondem ao layout exibido na tela.

## Dúvidas em aberto

- [ ] Lista completa priorizada de relatórios usados diariamente vs. obsoletos.

## Referências legado

- Programas: `scd3rel.prg`, `scd3rep.prg`, `scd3pro.prg`, `scd3fin.prg`, `scd3cli.prg`
