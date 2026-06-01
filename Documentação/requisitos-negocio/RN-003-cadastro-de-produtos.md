# RN-003 — Cadastro de produtos e precificação

| Campo | Valor |
|-------|-------|
| **ID** | RN-003 |
| **Status** | Validado legado (demo) — CRUD produto + auxiliares + NCM geral/emitente |
| **Última validação** | 2026-06-01 — [Sessão RN-003](../legado/registro-sessoes/SESSAO-2026-06-01-RN-003.md) |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_PROD`, `SCD_FAMI`, `SCD_FABR`, `SCD_CATE`, `SCD_LOTE`, `SCD_TNCM`, `scd1pro.prg`, `scd1ncm.prg`, `scd3pro.prg` |
| **RT relacionado** | RT-003 (pendente) |

| **Escopo desta validação:** base demo `C:\Hidra`. CRUD produto + auxiliares + **Tabela Ncm Geral** e **Tabela Ncm Emitente**; pendente: filtros pesquisa produto.

## Fluxo de telas

```text
Cadastros → Produtos
  ├─ Cadastro de Produtos (lista + detalhe + atalhos)
  │     • Pesquisa: Geral | Lotes | Fab | Fam
  │     • Incluir | Alterar | Excluir | Consulta
  │     • Grid: descrição, código, unid, flags, custo, preço, markup, saldo
  │     • Painel Informação do Produto + Venix/MixPedido + validade
  │     • Atalhos: Tabelas, Estoque, Ajustes, Pedido, Lotes, Liberar *(sem efeito demo)*,
  │                Inventario *(→ Estoques do Produto)*, Movimentação, Transferencia, Finalizar
  ├─ Tabelas do Produto (faixas A–J)
  ├─ Estoques do Produto / Movimentação
  │     └─ Contagem → Inventario (contagem física por produto)
  ├─ Ajuste do Produto
  ├─ Pedido & Resumo (consulta vendas)
  ├─ Lotes do Produto
  └─ Informação do Produto (Incluir / Alterar / Consulta)
        ├─ Dados do Produto (cabeçalho fixo)
        └─ Abas: Fiscal (NFe) | Tabela de Desconto | Configuração
                 | Apresentação | Produtos Similares
        • Gravar / Cancelar (incluir/alterar)
  ├─ Fabricantes de Produtos
  ├─ Familias de Produtos
  └─ Categorias de Produtos

Cadastros → Tributos por Ncm *(menu auxiliar — fora de Produtos)*
  ├─ Tabela Ncm Geral → Relação de Tributos por NCM *(CRUD completo)*
  └─ Tabela Ncm Emitente → Relação de Tributos por NCM *(Alterar + Finalizar)*
```

## Objetivo

Cadastrar e manter produtos comercializados, sua tributação, precificação por faixa e vínculos com fabricante, família e categorias.

## Contexto

Produtos alimentam pedidos, NF-e e estoque. Há múltiplas faixas de preço (A–J), comissão por tabela, integração Venix/MixPedido e controle de lotes/validade.

**Escopo MVP B2B-1 — cadastros relacionados (2026-06-01):** [RN-009](RN-009-representantes-comissao-metas.md), [RN-015](RN-015-cadastro-de-fornecedores.md) na mesma fase, após concluir CRUD produto.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Compras, comercial, fiscal, expedição |
| **Canal** | Desktop; Venix/MixPedido por produto |
| **Objeto de negócio** | Produto, tabela de preço, lote, movimento estoque |
| **Regra comercial** | Preço por tabela A–J; comissão por faixa; markup na lista |
| **Estoque** | Saldo, custo, compra/venda; vínculo fornecedor na entrada |

## Requisitos funcionais (RNG)

### Lista e painel *(validado)*

- **RNG-003-01:** Pesquisar produtos (**Pesquisa de Produtos**) com abas **Geral**, **Lotes**, **Fab**, **Fam**; exibir grid: descrição, código (`P_CODI`), unidade, flags BON/TAB/OBS, **custo**, **preço**, **markup**, **saldo**, indicador visual (col. V).
- **RNG-003-01a:** CRUD **Incluir**, **Alterar**, **Excluir**, **Consulta** na barra superior.
- **RNG-003-01b:** Painel **Informação do Produto**: fabricante, categoria, família, embalagem, código de barras (`P_EAN`), NCM, CEST, controlado, situação tributária, PIS/COFINS, listagens, divisão, apresentação.
- **RNG-003-01c:** Integrações **Venix** e **MixPedido** por produto (ex.: **ENVIAR PRODUTO**).
- **RNG-003-01d:** **Validade** — data, aviso (ex.: 30 dias), **Informar**, **Atualizar**, **Listagem**.

### Formulário Incluir/Alterar *(validado)*

- **RNG-003-17:** Manter **Dados do Produto** (cabeçalho): código (**Automático** ou manual), código de barra, descrição, adicional, unidade, **Controlado**, **Multiplo**, **Negrito**, **Inteiro**, peso líquido/bruto, **Embalagem**, fabricante/categoria/família (lookup), estoque mínimo/máximo, localização.
- **RNG-003-17a:** Cinco abas + **Gravar** / **Cancelar** (incluir/alterar); **Retornar** (consulta).
- **RNG-003-02:** Aba **Fiscal (NFe)** — classificação, origem, tributação ICMS, base/alíquota ICMS e IPI, IVA-ST, redução ST, NCM/Item, CEST/Devolução, **Cod/Fornecedor**, incide PIS/COFINS.
- **RNG-003-18:** Aba **Tabela de Desconto** — faixas **A a F** com **Apartir de**, **Bonificação**, **Desconto**, **Comissão** por faixa de quantidade *(distinto das Tabelas A–J de preço de venda — RNG-003-04)*.
- **RNG-003-19:** Aba **Configuração** — imagem, **Enviar MixPedido**, **Enviar Venix**, **Calculo Mix**, baixa estoque, mensagem, **Divisão**, **Bonificação** (par), **Listagem?**
- **RNG-003-20:** Aba **Apresentação** — texto **Apresentação do Produto** (área livre; memo `SCD_PROD.fpt`).
- **RNG-003-06:** Aba **Produtos Similares** — grid/lista de substitutos *(demo 00427 vazio)*.

### Cadastro e classificação *(lista)*

- **RNG-003-03:** Vincular **fabricante**, **família** e **categoria** (lookup; demo 00427: Fab/Fam **001 — DIVERSOS**).

### Precificação *(validado — Tabelas)*

- **RNG-003-04:** Tela **Tabelas do Produto**: faixas **Tabela A a J** + **Valor PMC**; colunas **Preço Venda**, **Margem**, **Promoção**, **Comissão**, **Desconto**; **Salvar tabela**, **Preços Básicos**, **Retornar**.
- **RNG-003-04a:** Rodapé: **Preço de Custo**, **Custo Médio**, **Margem Mínima**, **Preço Mínimo** (`P_CUST`, custo médio).
- **RNG-003-05:** Comissão configurável **por tabela** (ex.: A e D = 4%; B = 0%).

### Reajuste em lote *(validado — tela)*

- **RNG-003-11:** **Transferencia e Reajustes de Preço** — origem/destino de tabela, índice de reajuste, atualizar promoção/comissão/desconto, filtros divisão/fabricante/família; **CONFIRMAR** / **RETORNAR**.

### Estoque e movimentação *(validado — demo 00427)*

- **RNG-003-12:** **Estoques do Produto** — saldo inicial (data/hora/qtd/valor), grid de movimentos (data, hora, tipo, número, entrada, saída, saldo, compra, venda, custo), totais, **Cliente / Fornecedor / Usuario** da linha selecionada.
- **RNG-003-12a:** Movimento **COMPRA** exibe **FORNECEDOR** (ex.: FORNECEDOR SAUDE ANIMAL LTDA) — integração [RN-015](RN-015-cadastro-de-fornecedores.md).
- **RNG-003-12b:** Indicador **VENDA ASSOCIADA**; botões **Contagem**, **Pesquisa**, **Retornar** *(demo; **Ajustes** em outras sessões)*.
- **RNG-003-12c:** Atalho **Inventario** na lista abre **Estoques do Produto** (igual **Estoque**); contagem física via botão **Contagem** → modal **Inventario** (RNG-003-21).
- **RNG-003-21:** **Inventario** (contagem) — **Produto**, **Data**, **Hora**, **Descrição**, **Quantidade**, **Unitário**, **Total do Item**; aviso **confirmar data e hora** para efeito cronológico dos lançamentos; **Gravar** / **Finalizar**.

### Atalhos sem tela própria *(validado — demo)*

- **RNG-003-07:** Botão **Liberar** na lista — **não abre tela** na demo *(00427)*; liberação comercial aparenta ocorrer via aba **Configuração** (MixPedido/Venix) e painel **ENVIAR PRODUTO** — confirmar em produção ou produto bloqueado.
- **RNG-003-13:** **Movimentação do Produto** — visão analítica de entradas/saídas com margem (MB%); **Pesquisar**, **Excluir**, **Retornar**.
- **RNG-003-14:** **Ajuste do Produto** — quantidade, unitário, total, **Tipo de Operação** (ex.: ENTRADA), **Justificativa**; **Gravar** / **Finalizar**.

### Lotes *(validado — demo vazio)*

- **RNG-003-15:** **Lotes do Produto** — grid: status S, validade, lote, fabricação, emissão, NF, entrada, saída, saldo; **Incluir**, **Alterar**, **Excluir**, **Suspensão**, **Rastrear**, **Finalizar**; totais entradas/saídas/saldo.

### Consultas comerciais

- **RNG-003-16:** **Pedido & Resumo** — preferências: período de vendas, pesquisa geral, fabricante, família, opção de estoque, empresa; **PEDIDO**, **RESUMO**, **RETORNAR** (integração RN-004).

### Cadastros auxiliares — Fabricantes *(validado — demo)*

- **RNG-003-22:** **Relação de Fabricantes** — pesquisa **Pesquisa do Fabricante**; grid: status **S**, **Descrição do Fabricante**, **Cod**, **CNPJ**; **Incluir**, **Alterar**, **Excluir**, **Finalizar**.
- **RNG-003-22a:** **Dados do Fabricante** (inclusão/alteração): **Código**, **Descrição**, **CNPJ**, **Telefone**, **Razão Social**; bloco comercial: **Prêmio R$**, **Desconto**, **Data Inicial**, **Data Final**, **MixPedido** (ex.: *Fabricante Liberado para Envio*); **Gravar** / **Cancelar**.
- **RNG-003-22b:** Fabricante alimenta lookup em produto (RNG-003-03), filtros em **Pedido & Resumo** e **Transferencia e Reajustes**; integração MixPedido por fabricante. Distinto de **fornecedor** ([RN-015](RN-015-cadastro-de-fornecedores.md)).

### Cadastros auxiliares — Categorias *(validado — demo)*

- **RNG-003-23:** **Relação de Categorias** — pesquisa **Pesquisa Categoria**; grid: **Descrição da Categoria**, **Codigo**; **Incluir**, **Alterar**, **Excluir**, **Finalizar** *(demo: lista vazia)*.
- **RNG-003-23a:** **Informação da Categoria** (inclusão/alteração): **Código**, **Descrição**; **Gravar** / **Cancelar**.
- **RNG-003-23b:** Categoria alimenta lookup opcional no cabeçalho do produto (RNG-003-03); produto **00427** sem categoria no demo.

### Cadastros auxiliares — Famílias *(validado — demo)*

- **RNG-003-24:** **Relação de Famílias** — pesquisa **Pesquisa da Família**; grid: status **S**, **Descrição da Família**, **Código**; **Incluir**, **Alterar**, **Excluir**, **Finalizar**.
- **RNG-003-24a:** **Informação da Família** (inclusão/alteração): **Código**, **Descrição**; **Gravar** / **Cancelar**.
- **RNG-003-24b:** Família alimenta lookup em produto (RNG-003-03), filtros em **Pedido & Resumo** e **Transferencia e Reajustes**; demo **001 — DIVERSOS** (produto **00427**).

### Tributos por NCM — Tabela Ncm Geral *(validado — demo)*

- **RNG-003-25:** **Tabela Ncm Geral** — **Relação de Tributos por NCM**; pesquisa **Pesquisa do NCM**; grid: **NCM**, **ITEM**, **CEST**, **Nacional**, **Importado**; **Incluir**, **Alterar**, **Excluir**, **Finalizar**; rodapé *(ex.: Total=13 / 12 / 18)*.
- **RNG-003-25a:** **Informação do NCM** (inclusão/alteração): **Código**, **Item**, **Cest**, **Nacional**, **Importado** (%); **Gravar** / **Cancelar**. Mesmo NCM pode ter **vários itens** (ex.: `10011100` item `1.00` CEST `1111` e item `2.00` CEST `2222`).

### Tributos por NCM — Tabela Ncm Emitente *(validado — demo)*

- **RNG-003-26:** **Tabela Ncm Emitente** — **mesmo título** **Relação de Tributos por NCM** e **mesmo grid** que a tabela geral; pesquisa **Pesquisa do NCM**; ações **Alterar** e **Finalizar** apenas *(sem Incluir/Excluir no demo)* — percentuais **por emitente** (empresa logada).
- **RNG-003-26a:** **Informação do NCM - Alteração** — mesmos campos da geral (RNG-003-25a); demo **10011100** item **1.00**, CEST **1111**, Nacional **15,73**, Importado **4,80**.
- **RNG-003-25b:** Alimenta aba **Fiscal (NFe)** do produto — **NCM/Item** e **CEST/Devolução** (RNG-003-02); produto **00427** usa NCM **30041019**. Detalhe fiscal/NF-e: [RN-007](RN-007-fiscal-nfe.md) (RNG-007-10).

### Pendentes de validação

- **RNG-003-08:** Mix produto × representante (além de comissão por faixa).
- **RNG-003-09:** Etiquetas de produto.

## Critérios de aceite de negócio (CANG)

- **CANG-003-01:** Dado produto inativo, quando incluído em pedido, então sistema alerta ou impede conforme política.
- **CANG-003-02:** Dado quantidade abaixo da embalagem mínima, quando item é incluído, então sistema valida conforme regra comercial.
- **CANG-003-03:** Dado NCM inválido para operação fiscal, quando NF-e é gerada, então emissão é impedida ou corrigida.
- **CANG-003-04:** Dado tabela **Preço (A)** alterada em **Tabelas do Produto**, quando operador **Salvar tabela**, então preço/comissão persistem para pedidos daquela tabela (RN-002/RN-004).
- **CANG-003-05:** Dado movimento **COMPRA** selecionado, quando operador consulta estoque, então exibe **fornecedor** vinculado à entrada.
- **CANG-003-06:** Dado reajuste confirmado, quando índice aplicado, então tabela destino reflete novo preço/comissão/desconto conforme flags.
- **CANG-003-07:** Dado alteração em **Informação do Produto**, quando operador confirma **Gravar**, então dados persistem; **Cancelar** descarta.
- **CANG-003-08:** Dado contagem em **Inventario**, quando operador informa **Data** e **Hora**, então lançamento entra na ordem cronológica do estoque (movimentos em **Estoques do Produto**).
- **CANG-003-09:** Dado fabricante gravado, quando operador vincula em **Informação do Produto**, então código/descrição aparecem no lookup e painel da lista.
- **CANG-003-10:** Dado categoria gravada, quando operador vincula em **Informação do Produto**, então código/descrição aparecem no lookup e painel da lista.
- **CANG-003-11:** Dado família gravada, quando operador vincula em **Informação do Produto**, então código/descrição aparecem no lookup e painel da lista.
- **CANG-003-12:** Dado NCM/item/CEST na **Tabela Ncm Geral** ou ajustados na **Tabela Ncm Emitente**, quando produto referencia **NCM/Item** na aba Fiscal, então percentuais **Nacional**/**Importado** do emitente vigente aplicam-se na transparência fiscal (RN-007).

## Validação legado — demo (2026-06-01)

### Formulário — Inclusão (`Informação do Produto`)

**Caminho:** Cadastros → Produtos → **Incluir**.

#### Dados do Produto *(defaults inclusão)*

| Campo | Default |
|-------|---------|
| Código | Automático |
| Controlado / Multiplo / Negrito | Não |
| Inteiro | Sim |
| Peso líquido / bruto | 0,000 Kgs |
| Embalagem | 1 |
| Est. mínimo / máximo | 0 |
| Fabricante / Categoria / Família | lookup (vazio) |

#### Aba Fiscal (NFe)

| Campo | Default |
|-------|---------|
| Classificação | Mercadoria adquirida ou recebida de terceiros |
| Origem | 0-Nacional, exceto códigos 3, 4, 5 e 8 |
| Tributação | 00-Tributada Integralmente |
| Base ICMS / Alíq. ICMS / IPI / IVA-ST / Redução ST | 0,00% |
| NCM / Item · CEST / Devolução | vazios · devolução 0,00% |
| Cod / Fornecedor | vazio |
| Incide Pis/Cofins | Não |

**Abas pendentes na inclusão:** nenhuma — estrutura idêntica ao alterar (defaults vs. preenchido).

### Formulário — Alterar (produto **00427**)

**Caminho:** Cadastros → Produtos → **Alterar**.

#### Dados do Produto

| Campo | Valor demo |
|-------|------------|
| Código | 00427 |
| Código de barra | 7898586870055 |
| Descrição | AGROTHAL 5.350 UI+DICLOF - 15 ML |
| Unidade | FR |
| Peso líq. / bruto | 0,025 / 0,030 Kgs |
| Embalagem | **6** *(lista exibe "Com 6 FR")* |
| Est. mínimo / máximo | **48** / 0 |
| Controlado / Multiplo / Negrito | Não |
| Inteiro | Sim |
| Fabricante / Família | 001 — DIVERSOS |
| Categoria | vazia |

#### Aba Fiscal (NFe)

NCM **30041019** · origem 0-Nacional · trib. 00-Tributada Integralmente · PIS/COFins Não · Cod/Fornecedor vazio · demais alíquotas 0%.

#### Aba Tabela de Desconto

Faixas **A–F** — Apartir de, Bonificação, Desconto, Comissão: **0** / **0,00%** no demo (sem faixas configuradas).

#### Aba Configuração

| Campo | Demo |
|-------|------|
| Enviar MixPedido | Produto Liberado para ser Enviado ao MixPedido |
| Enviar Venix | Produto Liberado para ser Enviado ao Venix |
| Calculo Mix | Outros produtos não classificados |
| Divisão | **2** |
| Bonificação | 0 / 0 |
| Listagem? | **Sim** |

Coerente com painel lista (ENVIAR PRODUTO, divisão 2, listagens Sim).

#### Aba Apresentação

Área **Apresentação do Produto** vazia no demo.

#### Aba Produtos Similares

Grid vazio — sem similares cadastrados.

### Lista (`Cadastro de Produtos`)

Produto selecionado: **00427 — AGROTHAL 5.350 UI+DICLOF - 15 ML** (FR).

| Painel | Demo |
|--------|------|
| Fabricante / Família | 001 — DIVERSOS |
| Embalagem | Com 6 FR |
| EAN | 7898586870055 |
| NCM | 30041019 |
| Controlado | Não |
| Venix / MixPedido | ENVIAR PRODUTO |
| Validade | 30/06/2017 — avisar 30 dias |

Grid lista múltiplos produtos vet (EKTOPLUS, IVERMECTINA, etc.) com custo, preço, markup e saldo.

### Tabelas do Produto (00427)

| Tabela | Preço | Margem | Comissão |
|--------|-------|--------|----------|
| A | 9,87 | 13,84% | 4,00% |
| B | 9,87 | 13,84% | 0,00% |
| D | 11,35 | 30,91% | 4,00% |
| E–J | 0 | — | — |

Custo **8,67** · Custo médio **8,16**.

### Estoque / movimentação (00427)

| Campo | Demo |
|-------|------|
| Saldo inicial | 16/11/2015 — 272 un · R$ 2.179,54 |
| COMPRA 05/09/2016 | +120 → saldo **392** · R$ 1.039,87 |
| Fornecedor | FORNECEDOR SAUDE ANIMAL LTDA |
| Venda associada | 0 |

### Lotes (00427)

Grid vazio — sem lotes cadastrados; **Incluir** / **Finalizar** disponíveis.

### Atalhos Liberar e Inventario *(validado — demo 00427)*

| Atalho | Comportamento demo |
|--------|-------------------|
| **Liberar** | **Nenhuma tela** — clique sem efeito visível |
| **Inventario** | Abre **Estoques do Produto** (idêntico ao atalho **Estoque**) |

**Inventario → Estoques do Produto (00427):** cabeçalho 00427 · AGROTHAL · FR; grid movimentos; totais; rodapé **FORNECEDOR SAUDE ANIMAL LTDA** na linha COMPRA; botões **Contagem**, **Pesquisa**, **Retornar**; indicador **VENDA ASSOCIADA** = 0.

### Contagem → Inventario *(validado — demo 00427)*

**Caminho:** Estoques do Produto → **Contagem**.

| Campo | Demo |
|-------|------|
| Produto | 00427 |
| Descrição | AGROTHAL 5.350 UI+DICLOF - 15 ML |
| Data | vazia |
| Hora | 00:00:00 |
| Quantidade | 0 |
| Unitário | 0,0000 |
| Total do Item | 0,00 |

Aviso em destaque: *Favor confirmar a data e a hora da contagem para efeito cronológico dos lançamentos.*

**Gravar** / **Finalizar** — gravação de contagem física não testada no demo.

### Fabricantes de Produtos *(validado — demo)*

**Caminho:** Cadastros → Produtos → **Fabricantes de Produtos** → **Relação de Fabricantes**.

#### Lista

| Coluna | Demo |
|--------|------|
| S | ícone verde (ativo) |
| Descrição | **DIVERSOS** |
| Cod | **001** |
| CNPJ | vazio |

**Incluir** · **Alterar** · **Excluir** · **Finalizar** · pesquisa **Pesquisa do Fabricante**.

#### Inclusão (`Dados do Fabricante - Inclusão`)

| Campo | Default |
|-------|---------|
| Código / Descrição / CNPJ / Telefone / Razão Social | vazios |
| Prêmio R$ / Desconto | 0,00 / 0,00% |
| Data Inicial / Final | vazias |
| MixPedido | **Fabricante Liberado para Envio** |

**Gravar** / **Cancelar**.

#### Alteração (`Dados do Fabricante - Alteração` — **001**)

| Campo | Demo |
|-------|------|
| Código | 001 |
| Descrição | **DIVERSOS** |
| CNPJ / Telefone / Razão Social | vazios |
| Prêmio R$ / Desconto | 0,00 / 0,00% |
| Data Inicial / Final | vazias |
| MixPedido | **Fabricante Liberado para Envio** |

Mesmo fabricante vinculado ao produto **00427** (RNG-003-03).

### Categorias de Produtos *(validado — demo)*

**Caminho:** Cadastros → Produtos → **Categorias de Produtos** → **Relação de Categorias**.

#### Lista

| Coluna | Demo |
|--------|------|
| Descrição da Categoria | *(vazio — nenhum registro)* |
| Codigo | — |

**Incluir** · **Alterar** · **Excluir** · **Finalizar** · pesquisa **Pesquisa Categoria**.

#### Inclusão (`Informação da Categoria - Inclusão`)

Grupo **Dados da Categoria**:

| Campo | Default |
|-------|---------|
| Código | vazio |
| Descrição | vazio |

**Gravar** / **Cancelar**. Tela **Alterar** não testada *(lista vazia)*.

Coerente com produto **00427** — campo **Categoria** vazio no cabeçalho (RNG-003-03).

### Famílias de Produtos *(validado — demo)*

**Caminho:** Cadastros → Produtos → **Famílias de Produtos** → **Relação de Famílias**.

#### Lista

| Coluna | Demo |
|--------|------|
| S | ícone verde (ativo) |
| Descrição da Família | **DIVERSOS** |
| Código | **001** |

**Incluir** · **Alterar** · **Excluir** · **Finalizar** · pesquisa **Pesquisa da Família**.

#### Inclusão (`Informação da Família - Inclusão`)

Grupo **Dados da Família**:

| Campo | Default |
|-------|---------|
| Código | vazio |
| Descrição | vazio |

**Gravar** / **Cancelar**.

#### Alteração (`Informação da Família - Alteração` — **001**)

| Campo | Demo |
|-------|------|
| Código | 001 |
| Descrição | **DIVERSOS** |

Mesma família vinculada ao produto **00427** e ao fabricante **001 DIVERSOS** (RNG-003-03).

### Tributos por NCM — Tabela Ncm Geral *(validado — demo)*

**Caminho:** Cadastros → **Tributos por Ncm** → **Tabela Ncm Geral** → **Relação de Tributos por NCM**.

#### Lista

| Coluna | Exemplo demo |
|--------|--------------|
| NCM | 10011100, 10011200, … |
| ITEM | 1.00 / 2.00 *(ou vazio)* |
| CEST | 1111 / 2222 *(ou vazio)* |
| Nacional | 15,73% |
| Importado | 4,80% |

**Incluir** · **Alterar** · **Excluir** · **Finalizar** · **Pesquisa do NCM**. Vários registros por NCM quando **Item** difere.

#### Inclusão (`Informação do NCM - Inclusão`)

| Campo | Default |
|-------|---------|
| Código (NCM) | vazio |
| Item | vazio |
| Cest | vazio |
| Nacional / Importado | 0,00 / 0,00 |

**Gravar** / **Cancelar**.

#### Alteração (`Informação do NCM - Alteração` — **10011100** item **1.00**)

| Campo | Demo |
|-------|------|
| Código | 10011100 |
| Item | 1.00 |
| Cest | 1111 |
| Nacional | 15,73 |
| Importado | 4,80 |

### Tributos por NCM — Tabela Ncm Emitente *(validado — demo)*

**Caminho:** Cadastros → **Tributos por Ncm** → **Tabela Ncm Emitente** → **Relação de Tributos por NCM**.

Mesma UI da tabela geral; distingue-se pelo **menu de origem** e pelas ações disponíveis.

#### Lista

| Coluna | Exemplo demo |
|--------|--------------|
| NCM | 10011100, 23099090, 25030090, 38089192, … |
| ITEM | 1.00 *(demais vazios)* |
| CEST | 1111 *(demais vazios)* |
| Nacional | 15,73 · 32,09 · 48,47 … |
| Importado | 4,80 · 40,69 · 67,07 … |

**Alterar** · **Finalizar** · **Pesquisa do NCM** · Total=**13 / 12 / 18**. **Incluir** / **Excluir** ausentes — tabela por emitente, edição de percentuais existentes.

#### Alteração

Idêntica à geral — **10011100** item **1.00**, CEST **1111**, Nacional **15,73**, Importado **4,80**; **Gravar** / **Cancelar**.

Produto **00427** — NCM **30041019** na aba Fiscal *(pode constar na tabela emitente; não visível na amostra do print)*.

## Regras e restrições conhecidas

- Dez **tabelas de preço** (A–J) no atalho **Tabelas**; aba **Tabela de Desconto** usa faixas **A–F** (quantidade/bonificação) — conceitos distintos.
- Atalhos **Estoque** / **Inventario** → **Estoques do Produto**; **Contagem** abre modal **Inventario** (contagem física); **Liberar** inerte na demo — ver RNG-003-07/21.
- Coluna **V** na lista — indicador visual (produto com restrição/validade; confirmar significado).
- **Fabricante** (`SCD_FABR`) classifica produto comercialmente; **fornecedor** (`SCD_FORN`) abastece estoque — entidades distintas (RN-015).
- **Tabela Ncm Geral** (cadastro mestre, CRUD) vs **Tabela Ncm Emitente** (percentuais por empresa, só **Alterar**) — **mesmo título** de tela; diferenciar pelo submenu **Tributos por Ncm**.
- Memo fields (`SCD_PROD.fpt`) para observações longas.

## Funcionalidades pendentes (checklist)

| # | Item | Status |
|---|------|--------|
| 1 | **Incluir/Alterar** — 5 abas + cabeçalho | ✅ Validado (00427) |
| 2 | **Geral/Lotes/Fab/Fam** — filtros pesquisa | Pendente |
| 3 | **Liberar** · **Inventario** (= Estoque) · **Contagem** | ✅ Validado demo |
| 4 | **Fabricantes** CRUD | ✅ Validado (001 DIVERSOS) |
| 5 | **Categorias** CRUD | ✅ Validado (lista vazia) |
| 6 | **Famílias** CRUD | ✅ Validado (001 DIVERSOS) |
| 7 | **Tabela Ncm Geral** | ✅ Validado |
| 7a | **Tabela Ncm Emitente** | ✅ Validado |
| 8 | Produto com similares preenchidos | Pendente |

## Dúvidas em aberto

- [x] Faixas A–J existem na UI; demo usa A, B, D com preços distintos.
- [ ] Significado coluna **V** (ícone vermelho) na lista.
- [x] **Liberar** na lista — demo: sem tela; liberação MixPedido/Venix na aba Configuração.
- [x] **Inventario** (atalho lista) → **Estoques do Produto**; **Contagem** → modal **Inventario** (data/hora cronológica).
- [ ] Percentuais **Nacional** / **Importado** — uso exato na NF-e (IBPT vs. cálculo imposto).
- [ ] Regra de arredondamento preço/desconto cascata.

## Referências legado

- Tabelas: `SCD_PROD`, `SCD_FAMI`, `SCD_FABR`, `SCD_CATE`, `SCD_LOTE`, `SCD_TNCM`
- Programas: `scd1pro.prg`, `scd1fab.prg`, `scd1fam.prg`, `scd1cat.prg`, `scd1ncm.prg`, `scd3pro.prg`
- Sessão: [SESSAO-2026-06-01-RN-003.md](../legado/registro-sessoes/SESSAO-2026-06-01-RN-003.md)
- RNs relacionados: [RN-002](RN-002-cadastro-de-clientes.md), [RN-004](RN-004-pedidos-de-venda.md), [RN-006](RN-006-estoque-e-lotes.md), [RN-007](RN-007-fiscal-nfe.md), [RN-009](RN-009-representantes-comissao-metas.md), [RN-015](RN-015-cadastro-de-fornecedores.md)
