# Catálogo visual de telas — Hidra (legado)

Evidências em PNG do sistema **Hidra** (`scd.exe`), organizadas pelo **caminho de menu** e nomeadas por **RN** para rastreabilidade com requisitos e sessões de exploração.

| Campo | Valor |
|-------|-------|
| **Ambiente** | `C:\Hidra` (ver `ceopet/apps/legado/_tools/setup_hidra.ps1`) |
| **Roteiro** | [roteiro-exploracao-hidra.md](../roteiro-exploracao-hidra.md) |
| **Sessões** | [registro-sessoes/](../registro-sessoes/) |
| **Mapa funcional** | [mapa-modulos-hidra.md](../mapa-modulos-hidra.md) |

---

## Convenções

### Pastas

- **Prefixo numérico** (`01-`, `02-`…) — ordem de navegação no menu, não prioridade de projeto.
- **Slug em minúsculas**, sem acentos, hífen como separador (`fornecedores`, `pedidos-venda`).
- **Espelham o menu Hidra** — não o ID do RN. Um RN pode ter telas em pastas diferentes.
- **`_fora-do-menu/`** — telas acessadas por botão, atalho ou modal (ex.: Bloquear via Histórico).
- **`_dialogos/`** — alertas, validações e mensagens de erro isoladas.

### Arquivos PNG

```
RN-{NNN}-{seq}-{slug}.png
```

| Parte | Exemplo | Regra |
|-------|---------|-------|
| RN | `RN-015` | Requisito de negócio principal da tela |
| seq | `01`, `02`… | Ordem lógica dentro da pasta (lista → incluir → abas) |
| slug | `lista`, `incluir-cadastro` | kebab-case, descritivo |

**Exemplo completo:**

```text
01-cadastros/06-fornecedores/RN-015-01-lista.png
01-cadastros/06-fornecedores/RN-015-02-incluir-cadastro.png
```

### Metadados

Cada pasta de funcionalidade deve ter um **`README.md`** (copiar de [`_TEMPLATE-TELA.md`](_TEMPLATE-TELA.md)) com:

- Caminho de menu
- RN(s) relacionados
- Registro demo (C0082, F0024…)
- Tabela arquivo → RNG → descrição
- Status (✅ / ⏳ / ⏸)

### Captura

| Item | Padrão |
|------|--------|
| Resolução | 1920×1080 (fixa entre sessões) |
| Emitente | CEOPET (ou anotar outro) |
| Usuário | ROBINSON (admin) — anotar se SEP ou outro |
| Dado demo | Sempre que existir — selecionar registro antes do print |
| Data | Registrar no README da pasta, não no nome do arquivo |

### Fluxo de trabalho

```text
Explorar no Hidra → Salvar PNG na pasta → Atualizar README da pasta
  → Registrar sessão → Atualizar RN → (opcional) indice-por-rn.md
```

---

## Árvore de pastas

```text
telas/
├── README.md                          ← este arquivo
├── _TEMPLATE-TELA.md
├── indice-por-rn.md
│
├── 01-cadastros/                      ← RN-001, 002, 003, 009, 015 (+ auxiliares)
│   ├── 01-emitentes/
│   ├── 02-usuarios-acessos/
│   ├── 03-clientes/
│   ├── 04-produtos/
│   ├── 05-produtos-auxiliares/
│   │   ├── 01-fabricantes/
│   │   ├── 02-familias/
│   │   ├── 03-categorias/
│   │   ├── 04-ncm-geral/
│   │   └── 05-ncm-emitente/
│   ├── 06-fornecedores/
│   ├── 07-representantes/
│   ├── 08-transportadoras/
│   ├── 09-bancos/
│   ├── 10-cfop/
│   └── 11-condicoes-pagamento/
│
├── 02-vendas/                         ← RN-004, RN-010
│   ├── 01-pedidos-venda/
│   ├── 02-fechamento-pedido/
│   ├── 03-romaneio/
│   ├── 04-separacao/
│   └── 05-devolucao-clientes/
│
├── 03-compras/                        ← RN-005
│   ├── 01-pedidos-compra/
│   ├── 02-entrada-mercadorias/
│   └── 03-fechamento-devolucao/
│
├── 04-estoque/                        ← RN-006
│   ├── 01-consulta-movimentacao/
│   ├── 02-lotes-validade/
│   └── 03-processar-estoque/
│
├── 05-fiscal/                         ← RN-007
│   ├── 01-faturamento-nfe/
│   ├── 02-arquivo-nfe/
│   └── 03-importacao-xml/
│
├── 06-financeiro/                     ← RN-008
│   ├── 01-boletos/
│   ├── 02-cheques/
│   ├── 03-contas-pagar/
│   └── 04-comissao-juros/
│
├── 07-comercial-crm/                  ← RN-009 (op.), RN-011
│   ├── 01-visitas/
│   ├── 02-atendimento/
│   ├── 03-metas-projecoes/
│   └── 04-positivacao/
│
├── 08-pda-campo/                      ← RN-012
│   └── 01-pedidos-recebidos/
│
├── 09-relatorios/                     ← RN-013
│   ├── 01-vendas/
│   ├── 02-produtos/
│   ├── 03-representantes/
│   └── 04-especificos/                ← Clientes Chrome, Internet…
│
├── 10-integracoes/                    ← RN-014
│   ├── 01-email/
│   ├── 02-google-maps/
│   ├── 03-venix-accera/
│   └── 04-arquivos-bancarios/
│
├── _fora-do-menu/                     ← acesso indireto (botões, histórico)
└── _dialogos/                         ← alertas e validações
```

---

## Prioridade de captura (MVP B2B-1)

Ordem alinhada ao [roteiro de exploração](../roteiro-exploracao-hidra.md):

| Ordem | Pasta | RN | Demo |
|-------|-------|-----|------|
| 1 | `01-cadastros/01-emitentes/` + `02-usuarios-acessos/` | RN-001 | CEOPET |
| 2 | `01-cadastros/03-clientes/` | RN-002 | C0082 |
| 3 | `01-cadastros/04-produtos/` + `05-produtos-auxiliares/` | RN-003 | 00427 |
| 4 | `01-cadastros/07-representantes/` | RN-009 | V0002 |
| 5 | `01-cadastros/06-fornecedores/` | RN-015 | F0024 |
| 6+ | `02-vendas/` … | RN-004+ | — |

Índice cruzado RN → pasta: [indice-por-rn.md](indice-por-rn.md).

---

## Git e volume

- PNGs ficam **neste repositório** (evidência de validação).
- Preferir PNG comprimido; evitar 4K desnecessário.
- Se o volume passar de ~200 MB, considerar **Git LFS** para `*.png` em `telas/`.

---

## Referências

- [RN-CAD-001 — Cadastros MVP](../../requisitos-negocio/RN-CAD-001-cadastros-mvp-consolidado.md) — padrão G2 (lista + CRUD + modal)
- Inventário de menus: [`hidra_inventory.json`](../../_tools/hidra_inventory.json) → `menu_items`
