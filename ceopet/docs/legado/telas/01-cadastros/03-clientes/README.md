# Clientes

| Campo | Valor |
|-------|-------|
| **Menu** | Cadastros → Clientes |
| **RN** | [RN-002](../../requisitos-negocio/RN-002-cadastro-de-clientes.md) |
| **Programa legado** | `scd1cli.prg`, `scd2cli.prg`, `scd3cli.prg` |
| **Tabelas DBF** | `SCD_DEST`, `SCD_CONT`, `SCD_ATIV` |
| **Registro demo** | C0082 |
| **Última captura** | 2026-06-20 |
| **Sessão** | [SESSAO-2026-06-01-RN-002](../registro-sessoes/SESSAO-2026-06-01-RN-002.md) |

## Fluxo de telas

```text
Cadastros → Clientes
  ├─ Cadastro de Clientes (lista + atalhos)
  │     • Incluir | Alterar | Excluir | Consulta | Geral
  └─ Informação do Cliente (Incluir/Alterar)
        ├─ Dados Cadastrais
        ├─ Complemento
        ├─ Responsaveis
        ├─ Referencias
        ├─ Financeiro
        └─ Diversos
```

## Checklist — lista e CRUD

| Arquivo | RNG | Descrição | Status |
|---------|-----|-----------|--------|
| `RN-002-01-lista.png` | RNG-002-01…05, 08, 12 | Grid C0082 + painéis | ✅ |
| `RN-002-02-pesquisa-geral.png` | RNG-002-08 | Botão Geral — pesquisa ampliada | ⏳ |
| `RN-002-03-incluir-dados-cadastrais.png` | RNG-002-06 | Aba Dados Cadastrais (Incluir) | ⏳ |
| `RN-002-04-incluir-complemento.png` | RNG-002-07a | Aba Complemento | ⏳ |
| `RN-002-05-incluir-responsaveis.png` | RNG-002-07b | Aba Responsaveis | ⏳ |
| `RN-002-06-incluir-referencias.png` | RNG-002-07c | Aba Referencias | ⏳ |
| `RN-002-07-incluir-financeiro.png` | RNG-002-04 | Aba Financeiro | ⏳ |
| `RN-002-08-incluir-diversos.png` | RNG-002-06c | Aba Diversos (veterinários) | ⏳ |
| `RN-002-09-alterar-c0082.png` | RNG-002-06 | Alterar demo C0082 | ⏳ |
| `RN-002-10-consulta.png` | RNG-002-03 | Tela Consulta | ⏳ |

## Checklist — atalhos da lista

| Arquivo | RNG | Descrição | Status |
|---------|-----|-----------|--------|
| `RN-002-11-atalho-pedidos.png` | RNG-002-13 | Pedidos 9000975, 9000976 | ✅ |
| `RN-002-12-atalho-titulos.png` | RNG-002-14 | Títulos 9000975/1–2 | ✅ |
| `RN-002-13-atalho-devolvidos.png` | RNG-002-15a | Alerta cheques devolvidos C0082 | ✅ |
| `RN-002-14-historico-bloqueio.png` | RNG-002-16…16f | Histórico + ações rodapé | ✅ |
| `RN-002-17-atalho-visitas.png` | RNG-002-17, 17a | Consulta de Visita | ✅ |
| `RN-002-18-substituir-representantes.png` | RNG-002-18…18d | Transferência representantes | ✅ |
| `RN-002-19-mixpedido-alterar.png` | RNG-002-05b | Alterar Informações MixPedido | ⏳ |
| `RN-002-20-geolocalizacao-lote.png` | RNG-002-08a | Geral → geocodificação lote | ⏳ |

## Caminhos alternativos

| Função | Como acessar | Onde salvar |
|--------|--------------|-------------|
| Clientes Novos / Liberados | Cadastros → Integração | `_fora-do-menu/clientes-integracao/` |
| Clientes Chrome (mapa) | Relatórios → Específicos | `09-relatorios/04-especificos/` |
| Bloquear / Liberar | Lista → Histórico → rodapé | `_fora-do-menu/clientes-historico/` |

Ver checklist completo no [RN-002](../../requisitos-negocio/RN-002-cadastro-de-clientes.md#funcionalidades-pendentes-checklist-de-exploração).
