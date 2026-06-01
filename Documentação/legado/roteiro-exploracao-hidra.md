# Roteiro de exploração guiada — Hidra

Documento operacional da **TransformaTech (TTech)** para validar requisitos do **cliente Ceopet** usando o sistema legado em execução.

| Campo | Valor |
|-------|-------|
| **Sistema** | Hidra (`scd.exe`, xHarbour) |
| **Ambiente local** | `C:\Hidra` (ver `Documentacao/_tools/setup_hidra.ps1`) |
| **Saída esperada** | Atualização dos RNs + registros de sessão |

---

## Antes de começar

### Subir o sistema

```powershell
powershell -ExecutionPolicy Bypass -File Documentacao\_tools\setup_hidra.ps1
cd C:\Hidra
.\scd.exe
```

### Login demo (validar com Ceopet)

| Usuário | Senha (legado) | Perfil |
|---------|----------------|--------|
| `ROBINSON` | `111` | Administrativo |
| `SEP` | `111` | Separação / expedição |

### Checklist inicial (toda sessão)

- [ ] Emitente selecionado/cadastrado (obrigatório — erro `emit` se faltar índice ou pasta errada)
- [ ] Anotar data, participantes Ceopet e TTech
- [ ] Gravar caminho de menu percorrido (Cadastros → …)
- [ ] Tirar print da tela (nome sugerido: `RN-XXX-01-descricao.png`)
- [ ] Anotar mensagens de erro/validação exatas
- [ ] Conferir qual DBF muda após a operação (ver tabela por RN abaixo)

---

## O que registrar em cada tela

Use este padrão (copie para o registro de sessão):

```markdown
### [RN-XXX] Nome da função
- **Menu:** …
- **Ação testada:** …
- **Campos obrigatórios:** …
- **Regras observadas:** …
- **Mensagens do sistema:** …
- **Tabelas DBF afetadas:** …
- **Dúvida para Ceopet:** …
- **Atualizar RN:** RNG-… / CANG-… / seção Dúvidas
```

Template de sessão: [registro-sessoes/_TEMPLATE-SESSAO.md](registro-sessoes/_TEMPLATE-SESSAO.md)

---

## Ordem sugerida (prioridade MVP)

| Ordem | Sessão | RN | Duração est. |
|-------|--------|-----|--------------|
| 1 | Login, emitente, usuários | RN-001 | 1 h |
| 2 | Clientes | RN-002 | 2 h |
| 3 | **Produtos e preços** | **RN-003** | 2 h |
| 3a | **Representantes (CRUD)** | **RN-009** | 1 h |
| 3b | **Fornecedores (CRUD)** | **RN-015** | 1 h |
| 4 | Pedido de venda (fluxo completo) | RN-004 | 3 h |
| 5 | Estoque e lotes | RN-006 | 1,5 h |
| 6 | NF-e | RN-007 | 2 h |
| 7 | Expedição / romaneio | RN-010 | 1,5 h |
| 8 | Financeiro / boletos | RN-008 | 2 h |
| 9 | Comissão, metas, positivação | RN-009 (operações) | 1 h |
| 10 | Compras e entradas | RN-005 | 1,5 h |
| 11 | Visitas / CRM | RN-011 | 1 h |
| 12 | Relatórios críticos | RN-013 | 1 h |
| 13 | PDA e integrações | RN-012, RN-014 | 1 h |

**Nota (2026-06-01):** RN-009 e RN-015 entraram no **escopo MVP de cadastros**. Explorar **RN-003 primeiro**; em seguida Representantes e Fornecedores (mesmo padrão de prints: lista + abas).

---

## Sessão 1 — Emitente, usuários e parâmetros (RN-001)

**Programas legado:** `scd1emi.prg`, `scd1sen.prg`  
**Tabelas:** `SCD_EMIT`, `SCD_SENH`

### Percorrer no menu

- [ ] Selecionar / cadastrar emitente
- [ ] Alterar dados do emitente (fiscal, endereço, SMTP)
- [ ] Parametrização NFe (série, numeração, pastas)
- [ ] Pastas remessa / retorno / lixeira / saidatxt
- [ ] Cadastro de usuários e permissões

### Validar com Ceopet

- [ ] Quantos emitentes usam na operação real?
- [ ] Matriz de permissões (`S_ACES`) — o que cada perfil pode?
- [ ] Paths `D:\sistema\...` ainda usados→ Migrar para `C:\Hidra\...`?

### Evidências DBF

| Campo / tabela | O que observar |
|----------------|----------------|
| `SCD_EMIT.E_*` | Sequências NFe, pedido, textos e-mail |
| `SCD_SENH.S_ACES` | String de flags de acesso |

---

## Sessão 2 — Clientes (RN-002)

**Programas:** `scd1cli.prg`, `scd2cli.prg`, `scd3cli.prg`, `scd3map.prg`  
**Tabelas:** `SCD_DEST`, `SCD_CONT`, `SCD_ATIV`

### Percorrer no menu

- [ ] Cadastro de clientes (incluir, alterar, consultar)
- [ ] Bloquear / liberar cliente
- [ ] Representantes vinculados, tabela de preço, limite
- [ ] Dados veterinários (CRMV, veterinário responsável)
- [ ] Rotas, visitas, geolocalização / mapa
- [ ] Clientes novos, liberados, internet (filtros do menu)

### Validar com Ceopet

- [ ] Regra de bloqueio automático vs. manual?
- [ ] Campos `D_BLO1`–`D_BLO4` — significado?
- [ ] Crédito: onde é definido limite e liberação?

---

## Sessão 3 — Produtos e precificação (RN-003)

**Programas:** `scd1pro.prg`, `scd1fab.prg`, `scd1fam.prg`, `scd1cat.prg`, `scd1ncm.prg`  
**Tabelas:** `SCD_PROD`, `SCD_FAMI`, `SCD_FABR`, `SCD_CATE`, `SCD_TNCM`

### Percorrer no menu

- [ ] Cadastro de produto (NCM, EAN, embalagem)
- [ ] Faixas de preço e desconto
- [ ] Produtos similares, inativos
- [ ] Etiquetas, mix por representante
- [ ] Famílias, fabricantes, categorias, NCM

### Validar com Ceopet

- [ ] Quantas faixas de preço (A–J) são usadas?
- [ ] Embalagem mínima — validação na digitação do pedido?

---

## Sessão 4 — Pedidos de venda (RN-004) → crítico

**Programas:** `scd2ven.prg`, `scd2ped.prg`  
**Tabelas:** `SCD_PEDI`, `SCD_ITEM`, `SCD_COND`

### Fluxo mínimo a demonstrar

```
Novo pedido → incluir itens → gravar → fechar
→ bloquear/liberar → reservar → (separar) → faturar
```

### Percorrer no menu

- [ ] Gerar / gravar pedido
- [ ] Bloquear e liberar pedido
- [ ] Pedidos pendentes, cancelados, transferidos, reservados
- [ ] Relacionar pedidos, imprimir, exportar mix
- [ ] Fechamento do pedido
- [ ] Texto legal rodapé (crédito / estoque)

### Validar com Ceopet

- [ ] Significado de cada valor de `PED_STAT`
- [ ] Opção A vs. B em “Pedidos recebidos”
- [ ] Transferência entre distribuidores — quando ocorre?

### Evidências DBF

| Campo | Observar |
|-------|----------|
| `PED_STAT` | Transições de status |
| `PED_CLIE`, `PED_REPR` | Vínculos |
| `PED_ROMA`, `PED_SEPA` | Ligação expedição |

---

## Sessão 5 — Estoque e lotes (RN-006)

**Tabelas:** `SCD_PROD` (saldos), `SCD_LOTE`, `SCD_MLOT`

### Percorrer no menu

- [ ] Consulta estoque (físico, pendente, consolidado)
- [ ] Ajuste / movimentação / inventário
- [ ] Processar estoque
- [ ] Lotes e validade
- [ ] Visualizar lotes na separação

### Validar com Ceopet

- [ ] Fórmula: físico × pendente × efetivo (`P_ESIS`, `P_EPEN`, `P_EEFU`)
- [ ] FEFO/FIFO em produtos veterinários?

---

## Sessão 6 — Fiscal / NF-e (RN-007)

**Programas:** `scd2nfe.prg`, `scd2xml.prg`  
**Tabelas:** `NFe_NOTA`, `NFe_ITEM`, `SCD_CFOP`

### Percorrer no menu

- [ ] Faturamento NFe a partir de pedido
- [ ] NF-e complementar
- [ ] Importar XML, reprocessar impostos
- [ ] Enviar e-mail NF-e

### Validar com Ceopet

- [ ] Certificado e ambiente (homologação/produção) — fora do demo
- [ ] Cancelamento, carta de correção, inutilização

---

## Sessão 7 — Expedição (RN-010)

**Programas:** `scd2sep.prg`  
**Tabelas:** `SCD_ROMA`, campos `PED_ROMA`, `PED_SEPA`

### Percorrer no menu

- [ ] Separação de pedidos (Dig vs. Ref)
- [ ] Romaneio e romaneio consolidado
- [ ] Imprimir romaneio

---

## Sessão 8 — Financeiro (RN-008)

**Programas:** `scd2bol.prg`, `scd2fin.prg`, `Boleto/bplugin/bplugin.exe`  
**Tabelas:** `SCD_CHEQ`, `SCD_CAIX`, `SCD_MCAI`, `SCD_BANC`

### Percorrer no menu

- [ ] Boletos, remessa, retorno, estorno
- [ ] Cheques de clientes
- [ ] Contas a pagar, comissão, juros
- [ ] E-mail de débitos

---

## Sessão 9 — Representantes e metas (RN-009)

**Programas:** `scd1rep.prg`, `scd3rep.prg`  
**Tabelas:** `SCD_REPR`, `SCD_META`

### Percorrer no menu

- [ ] Metas e projeções
- [ ] Comissão, positivação
- [ ] Relatórios representante × produtos × vendas

---

## Sessões complementares

### RN-005 Compras — `scd2com.prg`, `scd2ent.prg`, `SCD_PCOM`, `SCD_ICOM`

### RN-011 CRM — `scd1vis.prg`, `scd2ate.prg`, `SCD_HVIS`

### RN-012 PDA — `PDA_*`, menu Pedidos recebidos A/B

### RN-013 Relatórios — `scd3rel.prg` (listar os 5 mais usados no dia a dia)

### RN-014 Integrações — remessa CNAB, Venix, Accera, Google Maps

---

## Após cada sessão

1. Preencher [registro de sessão](registro-sessoes/_TEMPLATE-SESSAO.md) (copiar para `SESSAO-YYYY-MM-DD-RN-XXX.md`)
2. Atualizar seção **Dúvidas em aberto** do RN correspondente
3. Marcar RNG/CANG confirmados; status RN → **Validado negócio** quando Ceopet assinar
4. Registrar no [levantamento.md](../levantamento.md) (histórico)

---

## Ferramentas auxiliares (TTech)

| Ferramenta | Uso |
|------------|-----|
| `EMAGDBU.EXE` | Inspecionar DBF antes/depois da operação |
| `Documentacao/_tools/analyze_hidra.py` | Inventário JSON |
| [mapeamento-tabelas.md](mapeamento-tabelas.md) | Referência de campos |
| [mapa-modulos-hidra.md](mapa-modulos-hidra.md) | Visão de módulos |

---

## Referência rápida — menus extraídos do executável

Cadastros · Clientes · Produtos · Fornecedores · Representantes · Pedidos de Venda · Pedidos de Compra · Estoque · Notas Fiscais · Boletos · Romaneio · Separação · Visitas · Metas · Relatórios · Integrações (Venix, Accera, e-mail, maps)

Lista completa: `Documentacao/_tools/hidra_inventory.json` → `menu_items`
