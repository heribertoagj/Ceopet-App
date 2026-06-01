# RN-009 — Representantes, comissão e metas

| Campo | Valor |
|-------|-------|
| **ID** | RN-009 |
| **Status** | Validado legado (demo) — CRUD + todos os atalhos lista (V0002) |
| **Última validação** | 2026-06-01 — [Sessão RN-009](../legado/registro-sessoes/SESSAO-2026-06-01-RN-009.md) |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_REPR`, `SCD_META`, `scd1rep.prg`, `scd3rep.prg` |
| **RT relacionado** | RT-009 (pendente) |

**Escopo desta validação:** base demo `C:\Hidra`. CRUD representante + **todos os atalhos** da lista (V0002). Pendente: Geral, positivação, mix produto.

## Objetivo

Gerenciar representantes comerciais, supervisores, comissões, metas, crédito operacional, geolocalização e desempenho de vendas.

## Contexto

Representantes são chave no B2B Ceopet: carteira de clientes (até 4 reps por cliente — RN-002), tabelas de preço A–J, comissão, metas e integração Venix. O fluxo separa **lista** (`Cadastro de Representantes`) de **manutenção** (`Informação do Representante`).

**Escopo MVP B2B-1:** cadastro CRUD incluído na fase de cadastros (decisão 2026-06-01), junto com RN-003 e RN-015.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Representante, supervisor, diretoria comercial |
| **Canal** | Desktop; Venix (senha) |
| **Objeto de negócio** | Representante, meta, comissão, crédito operacional |
| **Regra comercial** | Comissão/desconto máx. por rep; tabelas A–J habilitadas; meta em R$ |
| **Estoque** | Não impacta |

## Fluxo de telas

```text
Cadastros → Representantes
  ├─ Cadastro de Representantes (lista + detalhe + atalhos)
  │     • Pesquisa Representante
  │     • Geral | Incluir | Alterar | Excluir | Consulta
  │     • Grid: representante, CNPJ/CPF, código, telefone, HIST, situação
  │     • Painéis: Informação, Supervisor, Crédito (sync), Geolocalização
  │     • Atalhos: Pedidos ✅, Titulos ✅, Historico ✅, Comissão ✅, Referencia ✅, Finalizar
  └─ Informação do Representante (Incluir / Alterar / Consulta)
        ├─ Dados do Representante (cabeçalho fixo)
        └─ Abas: Dados Cadastrais | Operação de crédito | Informações Diversas
        • Gravar / Cancelar (incluir/alterar)

  Atalho Pedidos (lista):
  └─ Pedidos do Representante — cabeçalho rep. + grid pedidos
        • Finalizar | Detalhe → Consulta Pedidos de Venda | Resumo *(sem efeito demo)*

  Atalho Titulos (lista):
  └─ Titulos do Representante — cabeçalho rep. + grid titulos
        • Extrato ✅ | Resumo *(inerte)* | Baixa *(inerte)* | Juros ✅ | Finalizar

  Atalho Historico (lista):
  └─ Historico do Representante — cabeçalho rep. + área texto
        • Retornar | Gravar Historico

  Atalho Comissão (lista):
  └─ Extrato de Comissão — filtros + grid Historico + CRUD linhas
        • Incluir | Alterar | Excluir | Confirmar | Cancelar

  Atalho Referencia (lista):
  └─ Referencias para o Representante (período vendas)
        └─ Referencia do Representante — grid FABR/PROD + totais
              • Incluir | Alterar | Excluir | Processar | Imprimir | Finalizar

Atalho (lista clientes — RN-002):
  └─ Substituir Representantes → Transferencia de Representantes ✅
```

## Requisitos funcionais (RNG)

### Lista e painel *(validado — demo V0002)*

- **RNG-009-01:** Pesquisar representantes (**Pesquisa Representante**); grid: **REPRESENTANTES**, **CNPJ/CPF**, **CODIGO**, **TELEFONE**, **HIST**, **SITUACAO**.
- **RNG-009-01a:** CRUD **Geral**, **Incluir**, **Alterar**, **Excluir**, **Consulta** na barra superior.
- **RNG-009-01b:** Painel **Informação do Representante**: nome, endereço, cidade, UF, CEP, bairro, IE, e-mail.
- **RNG-009-01c:** Painel **Supervisor do Representante** — código do supervisor (lookup).
- **RNG-009-01d:** Painel **Informações de Crédito** — **Gerado**, **Utilizado**, **Restante**; legenda *Valores apurados na Ultima Sincronização*.
- **RNG-009-01e:** Painel **Geolocalização** — latitude, longitude, flag **M**, botão auxiliar (`.....`).
- **RNG-009-01f:** Atalhos inferiores: **Pedidos**, **Titulos**, **Historico**, **Comissão**, **Referencia**, **Finalizar**.

### Atalho Pedidos *(validado — demo V0002)*

- **RNG-009-15:** Lista → **Pedidos** — tela **Pedidos do Representante** *(título janela: Informação do Representante)*: cabeçalho **Representante**; grid **Pedidos**: **Data**, **Numero**, **Emissao**, **Nota Fiscal**, **Valor do Pedido**, **Situação**, **Data**, **Hora**, **Repres**; **Finalizar**, **Detalhe**, **Resumo**.
- **RNG-009-15a:** Grid → **Detalhe** abre **Consulta Pedidos de Venda** (RN-004 RNG-004-13) — consulta somente leitura; **Retornar** volta ao grid.
- **RNG-009-15b:** Botão **Resumo** na tela de pedidos do representante — **não abre tela** na demo *(clique sem efeito visível)*.

### Atalho Titulos *(validado — demo V0002)*

- **RNG-009-16:** Lista → **Titulos** — tela **Titulos do Representante** *(título: Informação do Representante)*: cabeçalho **Representante**; grid **Titulos**: **Documento**, **Emissao**, **Vencimento**, **Pagamento**, **Valor da Parcela**, **Portador**, **Banco**, **Boleto**, **Cliente**; **Extrato**, **Resumo**, **Baixa**, **Juros**, **Finalizar** *(mesma família RN-002 RNG-002-14 / RN-008 — filtro por representante)*.
- **RNG-009-16a:** Parcelas vinculadas ao pedido **9000975** do representante — cliente **C0082**; soma parcelas ≈ valor do pedido (855,57).
- **RNG-009-16b:** **Extrato** → modal **Extrato do Representante** — **Formato do Extrato** (ex.: Analítico), **Seleção do Extrato** (ex.: Aberto), **Selecionar Portador** (ex.: Todos), **Saída de Impressão** (ex.: Impressora), **Data Inicial/Final**, **Nome Arquivo (.txt)**; **Confirmar** gera/imprime relatório · **Cancelar** *(RN-008 RNG-008-09b)*.
- **RNG-009-16c:** Botões **Resumo** e **Baixa** na tela de títulos do representante — **sem ação** na demo *(clique sem efeito visível)* — distinto do atalho **Titulos** do **cliente** (RN-002: Baixa validada).
- **RNG-009-16d:** Botão **Juros** — **atualiza coluna Valor da Parcela** na grid com principal + juros recalculados *(mesma regra RN-002 RNG-002-14b / RN-008 RNG-008-09)*.

### Atalho Historico *(validado — demo V0002)*

- **RNG-009-17:** Lista → **Historico** — tela **Historico do Representante** *(título: Informação do Representante)*: cabeçalho **Representante**; área **Historico** (texto livre / memo); **Retornar**, **Gravar Historico** *(espelha RN-002 RNG-002-16 — sem painel financeiro nem bloqueio de cliente)*.

### Atalho Comissão *(validado — demo V0002)*

- **RNG-009-18:** Lista → **Comissão** — **Extrato de Comissão para** *(representante)*: **Formato do Extrato** (ex.: Recebimento), **Data de Comissão** (ex.: Data de Aviso), **Imprimir com Recibo**, **Imprimir 1/12 Avos**, **Imprimir em Aberto**; **Valor para Pagamento** (%), **Data Inicial/Final**, **Data Base**, **Data do Recibo**; grid **Historico** (Data, Historico, Valor, D/C) com **Incluir**, **Alterar**, **Excluir**; **Confirmar** / **Cancelar** *(RNG-009-07)*.
- **RNG-009-18a:** Demo V0002 — grid **Historico** vazio; defaults período **21/10/2016**–**01/06/2026**, pagamento **100,00%**, recibo **01/06/2026**.

### Atalho Referencia *(validado — demo V0002)*

- **RNG-009-19:** Lista → **Referencia** — modal **Referencias para o Representante**: **Periodo Inicial de Vendas**, **Periodo Final de Vendas**; **Referencia** / **Cancelar**.
- **RNG-009-19a:** Após **Referencia** — tela **Referencia do Representante** *(Informação do Representante)*: cabeçalho **Representante**; grid **Referencia**: **FABR**, **PROD**, **DESCRICAO**, **REFERENCIA R$**, **VENDA R$**, **(R%)**, **PREMIO**, **S**; totais **TOTAIS DAS REFERENCIAS**; **Incluir**, **Alterar**, **Excluir**, **Processar**, **Imprimir**, **Finalizar** *(relacionado positivação/prêmio — RNG-009-08)*.

### Formulário Incluir/Alterar *(validado — estrutura)*

- **RNG-009-02:** Cabeçalho **Dados do Representante**: **Código** (**Automático** ou manual), **Tipo** (Física/Jurídica + CNPJ/CPF), **Nome**, **Endereço**, **Cidade**, **Estado**, **CEP**, **Bairro**, **I.Estad.**, **E-mail**; **Gravar** / **Cancelar**.
- **RNG-009-02a:** Aba **Dados Cadastrais** — **Abreviado**, **Telefone**, **Cadastro**, **Situação** (ex.: Ativo), **Tabela**, **Preço** (ex.: Tabela Cliente), **Acréscimo** (ex.: Preço de Tabela); **Supervisor**, **Celular/Outros**, **Divisão/Produtos**; **Comissão**, **Comissão/Boleto**, **Desconto Máximo**, **Acréscimo Máximo** (%); seção **Informar Tabelas para preço Fixo e Variavel** — checkboxes **Tabela A–J**.
- **RNG-009-02b:** Aba **Operação de crédito** — **Operação de Crédito** (ex.: Não processar), **Bloquear Bonificação**, **Utilizar por Venda** (%); modo **Cálculo Automático de Crédito** ou **Informação Manual de Crédito**; **Valor/Data/Hora do Crédito**; totais **Valor Gerado** / **Valor Utilizado** (*última sincronização*).
- **RNG-009-02c:** Aba **Informações Diversas** — **Banco Padrão**, **Senha Venix**, **Calcular Meta** (ex.: Calculado em R$), **Informar Meta**, **Comissão (ST)** (%).

### Vínculos e operações

- **RNG-009-03:** Associar representantes a clientes (até 4 — RN-002 RNG-002-02) e mix produto × representante *(pendente)*.
- **RNG-009-04:** Substituir representante em carteira via **Transferencia de Representantes** (RN-002 RNG-002-18): origem/destino, **Transfere os Pedidos?**, **Período de Cadastro**.
- **RNG-009-05:** Informar e calcular **metas** por representante/período (aba Diversos — campo meta; relatórios pendentes).
- **RNG-009-06:** Calcular **comissão** sobre vendas conforme cadastro rep + tabela produto/faixa (RN-003).
- **RNG-009-07:** Gerar **Extrato de Comissão** por representante — ver RNG-009-18 *(relatórios produtos × vendas pendentes)*.
- **RNG-009-08:** **Positivação** / **Referencia** — vendas por produto/fabricante no período; prêmio e % referência *(atalho Referencia — RNG-009-19)*.
- **RNG-009-09:** Exportar representantes e supervisores para sistemas externos.
- **RNG-009-10:** Relatórios de devedores por representante (mensal, anual, resumo).

### Pendentes de validação

- **RNG-009-13:** Botão **Geral** (pesquisa ampliada — padrão clientes).
- **RNG-009-14:** Relatórios positivação/exportação; **Processar**/**Imprimir** Referencia; **Confirmar** extrato comissão *(não testados)*.

## Critérios de aceite de negócio (CANG)

- **CANG-009-01:** Dado venda faturada, quando comissão calculada, então percentual segue cadastro do representante e produto/faixa vigente na data.
- **CANG-009-02:** Dado meta definida, quando relatório de metas é emitido, então realizado vs. meta é exibido por representante.
- **CANG-009-03:** Dado representante gravado, quando vinculado em cliente (RN-002), então código/nome aparecem no lookup de representantes.
- **CANG-009-04:** Dado tabelas A–J marcadas no representante, quando pedido usa tabela desabilitada, então sistema impede ou alerta conforme política.
- **CANG-009-05:** Dado alteração em **Informação do Representante**, quando operador confirma **Gravar**, então dados persistem; **Cancelar** descarta.
- **CANG-009-06:** Dado representante selecionado, quando operador aciona **Pedidos**, então grid exibe pedidos daquele representante com número, valor e situação.
- **CANG-009-07:** Dado representante selecionado, quando operador aciona **Titulos**, então grid exibe parcelas a receber da carteira daquele representante (documento, vencimento, portador, cliente).
- **CANG-009-08:** Dado filtros em **Extrato do Representante**, quando operador **Confirmar**, então sistema imprime ou exporta relatório conforme **Saída de Impressão** e período informados.
- **CANG-009-09:** Dado títulos vencidos na grid do representante, quando operador aciona **Juros**, então **Valor da Parcela** exibe valor atualizado com juros.
- **CANG-009-10:** Dado texto em **Historico** do representante, quando operador aciona **Gravar Historico**, então observações persistem para consulta futura.
- **CANG-009-11:** Dado parâmetros em **Extrato de Comissão**, quando operador **Confirmar**, então sistema gera extrato/recibo conforme formato e período informados.
- **CANG-009-12:** Dado período de vendas em **Referencias para o Representante**, quando operador **Referencia**, então grid exibe produtos com referência, venda, % e prêmio no intervalo informado.

## Validação legado — demo (2026-06-01)

### Lista (`Cadastro de Representantes`)

**Caminho:** Cadastros → **Representantes**.

Representante selecionado: **V0002 — REPRESENTANTE TESTE**.

| Grid | Demo |
|------|------|
| REPRESENTANTES | REPRESENTANTE TESTE |
| CNPJ/CPF | 000.000.000-10 |
| CODIGO | **V0002** |
| TELEFONE | (99) 9999-9999 |
| HIST | N |
| SITUACAO | **ATIVO** |

#### Painéis

| Painel | Demo |
|--------|------|
| Informação | RUA TUTOIA 587 · SAO PAULO/SP · CEP 04007-000 · PARAISO |
| Supervisor | **V0002** |
| Crédito (sync) | Gerado/Utilizado/Restante **0,00** |
| Geolocalização | **-23,5757742** / **-46,6505369** · flag **M** marcada |

Vínculo RN-002: cliente demo usa **Representante 1 = V0002 — TESTE**.

#### Atalho **Pedidos** *(validado — V0002)*

**Caminho:** Cadastro de Representantes → **Pedidos**.

Cabeçalho **Representante**: V0002 · REPRESENTANTE TESTE · TESTE · CPF 000.000.000-10 · RUA TUTOIA 587 · SAO PAULO/SP · PARAISO · 04007-000 · (99) 9999-9999.

| Data | Número | NF | Valor | Situação | Hora | Repres |
|------|--------|-----|-------|----------|------|--------|
| 23/09/2016 | **9000975** | 0 | **855,57** | Entrega | 11:00:45 | V0002 |
| 23/09/2016 | **9000976** | 0 | **10,59** | Entrega | — | V0002 |

Colunas adicionais: **Emissao** *(vazia no demo)* · segunda coluna **Data**.

**Finalizar** · **Detalhe** · **Resumo** *(demo: **Resumo** inerte — sem tela)*

#### Detalhe — **Consulta Pedidos de Venda** *(pedido **9000975**)*

**Caminho:** Pedidos do Representante → selecionar **9000975** → **Detalhe**.

| Cabeçalho | Demo |
|-----------|------|
| Número | **9000975** |
| Data emissão | 23/09/2016 |
| Horário entrada | 07:39:50 |

**Abas:** Informações do Pedido · Produtos do Pedido · Fechamento do pedido *(só Informações validada)*.

| Bloco | Demo |
|-------|------|
| **Destinatário** | **C0082** — DISTRIBUIDORA BUENO LTDA - EPP *(RN-002)* |
| **Representante** | **V0002** — REPRESENTANTE TESTE |
| **Acompanhamento** | Pedido 22/09 17:39 · Financeiro 23/09 07:39 · Faturamento *Aguardando…* · Entrega 23/09 11:00 · Concluído *Aguardando…* |
| Tipo | **Pedido** *(Troca/Bonificação/Entrega/Outros)* |
| Data entrega | 23/09/2016 |
| Tabela | **Preço (A)** |
| Frete | 0-Emitente |
| Origem | Outros |
| Estoque | Baixa Estoque |
| Observação | **[B]** |
| Aplicativo origem | **VENIX** |

**Retornar** — abas Produtos/Fechamento pendentes.

#### Atalho **Titulos** *(validado — V0002)*

**Caminho:** Cadastro de Representantes → **Titulos**.

Cabeçalho **Representante** — mesmo padrão do atalho Pedidos (V0002).

| Documento | Emissão | Vencimento | Valor | Portador | Banco / Boleto | Cliente |
|-----------|---------|------------|-------|----------|----------------|---------|
| **9000975/1** | 23/09/2016 | 21/10/2016 | **427,79** | 3-Cheque | — | **C0082** |
| **9000975/2** | 23/09/2016 | 28/10/2016 | **427,78** | 2-Boleto | 237 / 23700000002P | **C0082** |

**Pagamento** vazio (em aberto). Total **855,57** = pedido **9000975** (RN-004). Mesmas parcelas do atalho **Titulos** do cliente C0082 (RN-002 RNG-002-14).

**Extrato** · **Resumo** · **Baixa** · **Juros** · **Finalizar**

#### Extrato *(validado — V0002)*

**Caminho:** Titulos do Representante → **Extrato** → **Extrato do Representante REPRESENTANTE TESTE**.

| Campo | Demo |
|-------|------|
| Formato do Extrato | **Analitico** |
| Seleção do Extrato | **Aberto** |
| Selecionar Portador | **Todos** |
| Saída de Impressão | **Impressora** |
| Data Inicial / Final | **21/10/2016** / **01/06/2026** |
| Nome Arquivo (.txt) | vazio |

**Confirmar** — **imprime relatório** (demo). **Cancelar** descarta.

*(Cliente C0082 — Extrato usa filtros distintos; ver RN-002 RNG-002-14c.)*

| Botão | Comportamento demo |
|-------|-------------------|
| **Resumo** | **Sem ação** |
| **Baixa** | **Sem ação** *(no cliente RN-002: Baixa funciona)* |
| **Juros** | **Atualiza Valor da Parcela** na grid (principal + juros) |

#### Atalho **Historico** *(validado — V0002)*

**Caminho:** Cadastro de Representantes → **Historico**.

Cabeçalho **Representante** — V0002 · REPRESENTANTE TESTE · TESTE · endereço/telefone *(mesmo padrão Pedidos/Titulos)*.

| Elemento | Demo |
|----------|------|
| Área **Historico** | **vazia** |
| **Gravar Historico** | disponível *(gravação não testada)* |
| **Retornar** | fecha tela |

Sem painel financeiro nem **Bloquear Cliente** *(presentes no Historico do cliente — RN-002)*.

#### Atalho **Comissão** *(validado — V0002)*

**Caminho:** Cadastro de Representantes → **Comissão** → **Extrato de Comissão para REPRESENTANTE TESTE**.

| Parâmetro | Demo |
|-----------|------|
| Formato do Extrato | **Recebimento** |
| Data de Comissão | **Data de Aviso** |
| Imprimir com Recibo | **Sim** |
| Imprimir 1/12 Avos | **Sim** |
| Imprimir em Aberto | **Não** |
| Valor para Pagamento | **100,00%** |
| Data Inicial / Final | **21/10/2016** / **01/06/2026** |
| Data Base | vazia |
| Data do Recibo | **01/06/2026** |

Grid **Historico** (Data · Historico · Valor · D/C): **vazio** no demo.

**Incluir** · **Alterar** · **Excluir** · **Confirmar** · **Cancelar** — CRUD de linhas e geração do extrato *(Confirmar/impressão pendente teste)*.

#### Atalho **Referencia** *(validado — V0002)*

**Caminho:** Cadastro de Representantes → **Referencia**.

**1. Modal Referencias para o Representante**

| Campo | Demo |
|-------|------|
| Periodo Inicial de Vendas | **01/06/2026** |
| Periodo Final de Vendas | **30/06/2026** |

**Referencia** · **Cancelar**

**2. Tela Referencia do Representante** *(após Referencia)*

Cabeçalho **Representante** — V0002 · REPRESENTANTE TESTE *(padrão demais atalhos)*.

| Coluna grid | |
|-------------|--|
| FABR · PROD · DESCRICAO | fabricante / produto |
| REFERENCIA R$ · VENDA R$ · (R%) · PREMIO | valores |
| S | status |

Grid **vazio** no demo · **TOTAIS DAS REFERENCIAS:** 0,00 · 0,00 · 0,00%

**Incluir** · **Alterar** · **Excluir** · **Processar** · **Imprimir** · **Finalizar** — Processar/Imprimir não testados.

### Formulário (`Informação do Representante`)

**Caminho:** Cadastros → Representantes → **Incluir** *(estrutura; alterar V0002 equivalente)*.

#### Dados do Representante *(defaults inclusão)*

| Campo | Default |
|-------|---------|
| Código | Automático |
| Tipo | Física |
| Nome / endereço / etc. | vazios |

#### Aba Dados Cadastrais

| Campo | Default demo |
|-------|--------------|
| Cadastro | 01/06/2026 |
| Situação | Ativo |
| Preço | Tabela Cliente |
| Acréscimo | Preço de Tabela |
| Comissão / Boleto / Desc. máx. / Acr. máx. | 0,00% |
| Tabelas A–J | **A, B, C, D** marcadas · E–J desmarcadas |

#### Aba Operação de crédito

| Campo | Default |
|-------|---------|
| Operação de Crédito | Não processar |
| Bloquear Bonificação | Não |
| Utilizar por Venda | 0,00% |
| Modo | Cálculo Automático de Crédito |
| Valor / Data / Hora | 0,00 · 01/06/2026 · 00:00:00 |
| Gerado / Utilizado | 0,00 |

#### Aba Informações Diversas

| Campo | Default |
|-------|---------|
| Calcular Meta | Calculado em R$ |
| Informar Meta | 0,00 |
| Comissão (ST) | 0,00% |
| Banco Padrão / Senha Venix | vazios |

### Transferência *(validado via RN-002)*

Tela **Transferencia de Representantes** — Rep. Origem/Destino, Transfere os Pedidos? = Não, período cadastro. Confirmação não executada.

Ver [RN-002](RN-002-cadastro-de-clientes.md) RNG-002-18.

## Funcionalidades pendentes (checklist)

| # | Item | Status |
|---|------|--------|
| 1 | Lista + painéis (V0002) | ✅ Validado |
| 2 | **Incluir/Alterar** — 3 abas + cabeçalho | ✅ Estrutura |
| 3 | Atalho **Pedidos** + **Detalhe** / **Resumo** | ✅ Detalhe OK · Resumo inerte |
| 3a | Atalho **Titulos** (grid) | ✅ Validado (9000975/1–2) |
| 3b | Titulos — Extrato / Juros / Resumo / Baixa | ✅ Extrato + Juros OK · Resumo/Baixa inertes |
| 3d | Atalho **Historico** | ✅ Validado (vazio) |
| 3e | Atalho **Comissão** (Extrato) | ✅ Validado (grid vazio) |
| 3f | Atalho **Referencia** (período + grid) | ✅ Validado (vazio) |
| 4 | Geral · positivação · Processar/Imprimir | Pendente |
| 5 | Mix produto × representante | Pendente |

## Dúvidas em aberto

- [ ] Supervisor **V0002** no demo — auto-supervisão ou placeholder?
- [ ] Diferença **Comissão** vs **Comissão (ST)** vs comissão por tabela produto (RN-003).
- [ ] Periodicidade de fechamento de comissão e sincronização de crédito.
- [x] **Resumo** (Pedidos rep.) — demo: sem efeito.
- [x] **Resumo** / **Baixa** (Titulos rep.) — demo: sem efeito; **Baixa** funciona pelo atalho **Titulos** do **cliente** (RN-002).
- [x] **Juros** (Titulos rep.) — demo: atualiza **Valor da Parcela** na grid.
- [ ] Hierarquia supervisor × representante × distribuidor.

## Referências legado

- Tabelas: `SCD_REPR`, `SCD_META`, campos comissão em `SCD_PROD`, `SCD_PEDI`, `SCD_DEST`
- Programas: `scd1rep.prg`, `scd3rep.prg`
- Sessão: [SESSAO-2026-06-01-RN-009.md](../legado/registro-sessoes/SESSAO-2026-06-01-RN-009.md)
- RNs relacionados: [RN-002](RN-002-cadastro-de-clientes.md), [RN-003](RN-003-cadastro-de-produtos.md), [RN-004](RN-004-pedidos-de-venda.md), [RN-008](RN-008-financeiro-boletos-cheques.md), [RN-015](RN-015-cadastro-de-fornecedores.md)
