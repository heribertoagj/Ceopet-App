# RN-002 — Cadastro de clientes

| Campo | Valor |
|-------|-------|
| **ID** | RN-002 |
| **Status** | Validado legado (demo) — escopo MVP coberto; pendências não bloqueantes (Internet, e-mail, mapa por coords) |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_DEST`, `SCD_CONT`, `scd1cli.prg`, `scd2cli.prg`, `scd3cli.prg` |
| **RT relacionado** | RT-002 (pendente) |
| **Última validação** | 2026-06-01 — [Sessão RN-002](../legado/registro-sessoes/SESSAO-2026-06-01-RN-002.md) |

## Objetivo

Gerenciar clientes B2B (distribuidores, clínicas, pet shops) com dados comerciais, fiscais, logísticos, crédito, representação comercial, geolocalização e integração MixPedido.

## Contexto

No Hidra o cliente é **destinatário** (`SCD_DEST`), vinculado ao emitente (`D_EMIT`). O fluxo separa **lista/consulta** (`Cadastro de Clientes`) de **manutenção** (`Informação do Cliente`, 6 abas). A lista expõe atalhos para pedidos, títulos, histórico, visitas e MixPedido.

**Escopo desta validação:** base demo `C:\Hidra` — lista, Pesquisa Geral, geolocalização, CRUD 6 abas, atalhos, Integração (Novos/Liberados), **Clientes Chrome** (mapa). *Fora do demo:* Clientes Internet (não abriu), e-mail, Atendimento.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Comercial, crédito, representante, back-office |
| **Canal** | Desktop; geolocalização / mapa na lista |
| **Objeto de negócio** | Cliente / destinatário |
| **Regra comercial** | Tabela de preço por cliente; até 4 representantes; liberação MixPedido; limite de crédito |
| **Estoque** | Não impacta diretamente |

## Fluxo de telas

```text
Cadastros → Clientes
  ├─ Cadastro de Clientes (lista + detalhe + atalhos)
  │     • Pesquisa Destinatario
  │     • Geral → Pesquisa Geral (lista por nome + seleção)
  │     • Incluir | Alterar | Excluir | Consulta
  │     • Grid: nome, CNPJ, código, telefone, HIST, BLOQ, TAB
  │     • Geolocalização: coords + botão ao lado (por cliente) + Geral (lote)
  │     • Atalhos: Pedidos, Titulos, Devolvidos, Historico, Visitas,
  │                Substituir Representantes, Finalizar
  └─ Informação do Cliente
        ├─ Dados do Cliente (cabeçalho fixo)
        └─ Abas: Dados Cadastrais | Complemento | Responsaveis
                 | Referencias | Financeiro | Diversos
        • Incluir/Alterar: Gravar / Cancelar
        • Consulta: Retornar
```

## Requisitos funcionais (RNG)

### Lista e CRUD

- **RNG-002-01:** Pesquisar clientes por destinatário e exibir grid com razão social, CNPJ/CPF, código interno (`D_CODI`), telefone, indicadores **HIST**, **BLOQ** e **TAB** (tabela de preço).
- **RNG-002-01a:** Incluir, alterar, excluir e consultar cliente; código **automático** ou informado (`D_CODI`).
- **RNG-002-01b:** Manter cabeçalho do cliente: tipo pessoa (Jurídica/Física), CNPJ/CPF (`D_DEST`), razão social (`D_NOME`), endereço (`D_ENDE`), referência, cidade (`D_XMUN`), UF (`D_ESTA`), CEP (`D_CEPN`), bairro (`D_BAIR`), IE (`D_IEST`), e-mail (`D_EMAI`).
- **RNG-002-01c:** Botão **Geral** abre **Pesquisa Geral**: lista todos os clientes por **Nome do Cliente**, exibe painel **Informação do Cliente** (razão social, fantasia, endereço, cidade/UF/CEP, bairro/telefone, e-mail) do item selecionado, contador **Encontrados: N** e ações **Selecionar** (posiciona na lista principal) / **Retornar** (fecha sem alterar seleção).

### Aba Dados Cadastrais *(validado)*

- **RNG-002-02:** Associar até **quatro representantes** (`D_REP1`…`D_REP4`) com lookup e nome; transportadora preferencial (`D_TRAN` + lookup).
- **RNG-002-03:** Registrar fantasia (`D_FANT`), site, contato (`D_CON1`), telefone (`D_FONE`), celular, distrito, setor e roteiro.
- **RNG-002-04:** Definir **divisão** (`D_DIVI`, ex.: 1-Revendedor), **tabela de preço** (`D_TABE`, ex.: Preço C) e **frete** (`D_FRET`, ex.: 0-Emitente).

### Lista — painéis complementares *(validado)*

- **RNG-002-05:** Exibir status **MixPedido** na lista (ex.: “CLIENTE LIBERADO PARA EFETUAR PEDIDOS NA INTERNET”) e **limite de crédito** (`D_LIMI`); botão **Alterar Informações** para parametrização por cliente.
- **RNG-002-05b:** Tela **MixPedido — Parametros MixPedido** (via **Alterar Informações**): modo **MixPedido** (ex.: Enviar), **Nova Senha** (Sim/Não), **Credito R$** (`D_LIMI` / `D_ILIM`), **Bloquear Portadores** (Depósito, Boleto, Cheque, Dinheiro, Cartão); **Confirmar Informações** / **Cancelar**. Parametrização global do emitente na aba MixPedido (RN-001 RNG-001-11).
- **RNG-002-08:** Armazenar e exibir **geolocalização** (`D_LATI`, `D_LONG`) no painel da lista.
- **RNG-002-08a:** Processamento **geral de geolocalização** (em lote): geocodificar endereços dos clientes via serviço externo e exibir **Resultado da geolocalização** com contadores: total de clientes, processados, localizados, não localizados, problemas de conexão/localização, pesquisa sem latitude × longitude.
- **RNG-002-08b:** Se endereço não puder ser localizado, alertar *"Erro - Verificar Endereço para Localização.!"* com endereço completo formatado (logradouro, bairro, cidade, UF, CEP, Brasil) para correção cadastral — vale para lote e consulta individual.
- **RNG-002-08c:** Botão **ao lado das coordenadas** (painel Geolocalização, cliente selecionado): dispara geocodificação **individual** do endereço cadastrado; em falha exibe alerta RNG-002-08b; em sucesso atualiza `D_LATI`/`D_LONG` e pode abrir mapa (`scd3map.prg` — RN-014 RNG-014-04; *não validado com serviço ativo no demo*).
- **RNG-002-12:** A partir da lista, acessar **Pedidos**, **Títulos**, **Devolvidos**, **Histórico**, **Visitas** e **Substituir Representantes** do cliente selecionado.

### Atalho Pedidos *(validado)*

- **RNG-002-13:** Ao acionar **Pedidos** na lista, exibir painel consolidado com dados do cliente, **resumo financeiro** e **grid de pedidos** do destinatário.
- **RNG-002-13a:** Resumo financeiro: status de liberação (ex.: **PARCIALMENTE LIBERADO**), contadores e valores de **vencidos**, **a vencer**, **devolvidos**, **cheques pré**, **total devedor**, limite de crédito, vencidos com juros, consultas Serasa/SPC, data de cadastro, última e maior compra.
- **RNG-002-13b:** Grid de pedidos com colunas: data, número, emissão, nota fiscal, valor, **situação** (ex.: Entrega), data/hora entrega, representante, transportadora; ações **Detalhe** (abre pedido — ver RN-004) e **Finalizar**.

### Atalho Titulos *(validado)*

- **RNG-002-14:** Ao acionar **Titulos** na lista, exibir painel consolidado (cliente + resumo financeiro — igual Pedidos) e **grid de títulos a receber** do destinatário (`SCD_CONT`).
- **RNG-002-14a:** Grid de títulos: documento (pedido/parcela), emissão, vencimento, pagamento, **valor da parcela**, portador, banco, número boleto.
- **RNG-002-14b:** Botão **Juros** — recalcula juros sobre títulos em atraso e **atualiza o valor exibido na coluna Valor da Parcela** (principal + juros; parâmetros de juros do emitente — RN-001 `E_JURB` / RN-008).
- **RNG-002-14c:** Botão **Extrato** — abrir preferências (formato analítico/sintético, ordem por vencimento, portador, conteúdo, período, representante) e imprimir **Extrato Financeiro** do cliente.
- **RNG-002-14d:** Botão **Baixa** — registrar liquidação do título selecionado (tipo RECEBER, portador, dias em atraso, valor original, data pagamento, valor pago, variação, **valor calculado** com juros/multa, observação) — detalhe em RN-008.

### Atalho Devolvidos *(validado — demo sem dados)*

- **RNG-002-15:** Consultar **cheques devolvidos** do cliente (`SCD_CHEQ`).
- **RNG-002-15a:** Pela **lista** → **Devolvidos**: se não houver registros, alerta *"Cheques Devolvidos não Localizados.!!! Cliente (C0082)"* (não abre grid).
- **RNG-002-15b:** Pelo **Histórico** → **Visualizar Devolvidos**: abre tela **Cheques devolvidos do Cliente** com grid (mesmas colunas dos títulos + Repres, S); se vazio, grid em branco + **FINALIZAR CONSULTA DOS CHEQUES DEVOLVIDOS**.

### Atalho Historico *(validado)*

- **RNG-002-16:** Ao acionar **Historico** na lista, exibir painel consolidado (cliente + financeiro) e **área de texto** para histórico/observações do cliente (`SCD_HIST` / `D_OBS*`).
- **RNG-002-16a:** Botão **Gravar Historico** — persiste texto digitado na área de histórico.
- **RNG-002-16b:** Botão **Bloquear Pedido** — bloqueio operacional a partir do contexto do cliente (integração RN-004).
- **RNG-002-16c:** Botão **Bloquear Cliente** — bloqueio cadastral/comercial (`D_BLOQ`, `D_BLOC`; RN-002 RNG-002-05a).
- **RNG-002-16d:** Botão **Visualizar Titulos** — consulta somente leitura **Titulos do Cliente** (grid com Repres, status S; **FINALIZAR CONSULTA DOS TITULOS**); sem Extrato/Baixa/Juros.
- **RNG-002-16e:** Botão **Visualizar Devolvidos** — consulta somente leitura de cheques devolvidos (ver RNG-002-15b).
- **RNG-002-16f:** Botão **Retornar** — fecha tela de histórico.

### Atalho Visitas *(validado)*

- **RNG-002-17:** Ao acionar **Visitas** na lista, abrir **Consulta de Visita** com razão social do cliente, **representante** (Rep 1: código + nome), **dia da visita** (dropdown, ex.: Segunda), **semanas do mês** (1ª a 5ª — checkboxes), datas **Venix**, **Proxima**, **Livre**, campo **Hora** e botões **Gravar** / **Retornar**.
- **RNG-002-17a:** Exibir regra na tela: *"A data de visita enviada ao representante, será a mais próxima de [data base] das datas acima relacionadas."* — planejamento Venix / representante (RN-011, RN-014).
- **RNG-002-17b:** Campos prováveis em `SCD_DEST`: `D_REP1`, `D_VISI`, `D_SEM1`–`D_SEM4`, `D_HOR1`, datas Venix/próxima/livre.

### Atalho Substituir Representantes *(validado)*

- **RNG-002-18:** Ao acionar **Substituir Representantes** na lista (com cliente selecionado), abrir **Transferencia de Representantes**.
- **RNG-002-18a:** Informar **Representante — Origem** e **Representante — Destino** (lookup código + nome).
- **RNG-002-18b:** Opção **Transfere os Pedidos?** (Sim/Não) — define se pedidos em aberto migram com a carteira.
- **RNG-002-18c:** Filtrar por **Período de Cadastro** (data inicial e final).
- **RNG-002-18d:** Confirmar ou cancelar transferência (**CONFIRMAR TRANSFERENCIA** / **CANCELAR TRANSFERENCIA**). Detalhe operacional em RN-009 (RNG-009-03).

### Aba Complemento *(validado — inclusão)*

- **RNG-002-06:** Registrar endereço de **cobrança** e **entrega** distintos do principal, cada um com cidade, UF e CEP (`D_ENDC`, `D_XMUC`, `D_ESTC`, `D_CEPC`; `D_ENDN`, `D_XMUN`, `D_ESTN`, `D_CEPN` ou equivalentes).
- **RNG-002-06a:** Classificar cliente por **Classe 1**, **Classe 2** e **Atividade** (lookup `SCD_ATIV`; `D_CLAS`, `D_CLA2`, `D_ATIV`).
- **RNG-002-06b:** Informar **Cod.Ibge** do município (`D_CMUN`) e número **Suframa** (`D_SUFR`).
- **RNG-002-06c:** Controlar **Validade Anvisa**, **Validade Mapa** e respectivos **Números de Alvará** (campos regulatórios pet/vet).

### Aba Responsaveis *(validado — inclusão)*

- **RNG-002-07:** Cadastrar até **três proprietários/responsáveis** legais, cada um com nome, **Observação**, **Nascimento**, **CPF** e **RG** (`D_PROP*` / `D_RESP*`).

### Aba Referencias *(validado — inclusão)*

- **RNG-002-03a:** Registrar até **três referências comerciais** (Empresa + Telefone; `D_REC1`…`D_REC3`, `D_TEL1`…`D_TEL3`).
- **RNG-002-03b:** Registrar até **duas referências bancárias** (Banco, AG, CC, Telefone; `D_BAN1`, `D_BAN2`, agência/conta).

### Aba Financeiro *(validado — inclusão)*

- **RNG-002-04a:** Manter parâmetros financeiros: **Cadastro** (`D_CADT`), **LimiteC** (`D_LIMI`), **Desconto** (`D_DESC`), **Licença**, **Data**, **Comissão** (`D_COMI`), **Meta/Mes** (`D_META`).
- **RNG-002-04b:** Definir **Sistema** (ex.: Prospecto — `D_SITU`), política de **Juros** (ex.: Não Cobrar Juros), **Subs. Trib.** (ex.: Calculo Externo), **Motivo** e **Devolução** (%).
- **RNG-002-04c:** Registrar consulta **Serasa/Spc**, **Bloquear Fabricantes** (lookup), **Qual Motivo** e **Lembrete** (`D_BLOC`, `D_MOTI`, `D_LEMB`).
- **RNG-002-04d:** Integrações **Venix** e **MixPedido** (ex.: Enviar) e **Bloquear Portadores** na aba Financeiro: Depósito, Boleto, Cheque + Dinheiro, Cartão (subset dos 5 portadores; tela dedicada MixPedido em RNG-002-05b inclui Nova Senha e Crédito).

### Aba Diversos *(validado — inclusão)*

- **RNG-002-07a:** Cadastrar até **dois veterinários** vinculados ao cliente: nome, Nascimento, CPF, RG, **Crmv**, Telefone (`D_VET1`, `D_VET2`, `D_CRM1`, `D_CRM2`).
- **RNG-002-07b:** Informar **Crmv Emp** — registro CRMV da empresa/clínica (`D_CRME`).

- **RNG-002-05a:** Bloquear/liberar cliente com motivo (`D_BLOQ`, `D_LLIB`, `D_LIBE`, `D_BLOC`) — ação **Bloquear Cliente** na tela **Historico** (validado); **Liberar Cliente** no mesmo contexto *(não testado)*.

### Integração e segmentos de clientes

- **RNG-002-09:** Classificar cliente (atividade, divisão, setor, situação `D_SITU`).
- **RNG-002-10:** Consultar segmentos de clientes recebidos de integrações ou filtros comerciais: novos, liberados, internet, chrome.
- **RNG-002-10a:** **Clientes Novos** — **Cadastros → Integração → Clientes Novos**; mesma **tela de lista** de Clientes Liberados, filtrando clientes pendentes de liberação/cadastro (Venix/MixPedido/PDA); se vazio, alerta *"Clientes Novos não Localizados...Finalizar"* → **OK** (não abre grid).
- **RNG-002-10b:** **Clientes Liberados** — **Cadastros → Integração → Clientes Liberados**; **mesma lista/tela** que Clientes Novos, filtrando clientes já liberados; no demo, comportamento idêntico (fila vazia — alerta e encerra sem grid).
- **RNG-002-10c:** **Clientes Chrome** — **Relatórios → … → Específicos → Clientes Chrome**; abre **Google Maps** (navegador) plotando clientes do canal Chrome/MixPedido pelas coordenadas/endereços (`D_LATI`, `D_LONG`; `scd3map.prg` — RN-014 RNG-014-04).
- **RNG-002-10d:** **Clientes Internet** — mesmo submenu **Específicos**; consulta clientes do canal internet *(não validado no demo)*.
- **RNG-002-11:** Enviar/atualizar e-mail; comunicação em massa.

## Critérios de aceite de negócio (CANG)

- **CANG-002-01:** Dado cliente bloqueado (`D_BLOQ` / coluna BLOQ), quando operador tenta fechar pedido, então sistema impede ou exige liberação conforme política.
- **CANG-002-02:** Dado cliente sem e-mail, quando envio de NF-e por e-mail é solicitado, então sistema alerta operador.
- **CANG-002-03:** Dado código do cliente (`D_CODI`), quando pesquisado em pedido, então dados cadastrais são recuperados.
- **CANG-002-03a:** Dado **Pesquisa Geral** com cliente selecionado, quando operador aciona **Selecionar**, então lista principal posiciona no cliente escolhido; **Retornar** fecha sem mudar seleção.
- **CANG-002-03b:** Dado processamento geral de geolocalização concluído, quando operador confirma **Resultado da geolocalização**, então contadores refletem clientes processados, localizados e falhas; coordenadas válidas persistem em `D_LATI`/`D_LONG`.
- **CANG-002-03c:** Dado endereço inválido ou não encontrado pelo serviço, quando geolocalização é tentada (lote ou botão ao lado das coords), então sistema exibe alerta com endereço completo para verificação, sem gravar coordenadas incorretas.
- **CANG-002-04:** Dado alteração em **Informação do Cliente**, quando operador confirma **Gravar**, então dados persistem; **Cancelar** descarta.
- **CANG-002-05:** Dado consulta, quando operador abre cliente, então formulário é somente leitura com **Retornar** (sem Gravar).
- **CANG-002-06:** Dado tabela de preço **Preço (C)** configurada, quando pedido é criado para o cliente, então precificação usa tabela C.
- **CANG-002-07:** Dado cliente selecionado na lista, quando operador clica **Pedidos**, então sistema exibe resumo financeiro e histórico de pedidos daquele cliente.
- **CANG-002-08:** Dado pedido listado, quando operador aciona **Detalhe**, então abre consulta do pedido (integração com RN-004).
- **CANG-002-09:** Dado cliente selecionado, quando operador clica **Titulos**, então sistema lista parcelas em aberto com portador e vencimento.
- **CANG-002-10:** Dado títulos vencidos, quando operador aciona **Juros**, então coluna **Valor da Parcela** passa a refletir valor atualizado com juros.
- **CANG-002-11:** Dado título selecionado, quando operador aciona **Baixa** e confirma pagamento, então título é liquidado com valor pago e valor calculado (incl. juros quando aplicável).
- **CANG-002-12:** Dado cliente sem cheques devolvidos, quando operador aciona **Devolvidos** na lista, então sistema informa que não localizou registros.
- **CANG-002-13:** Dado texto em **Historico**, quando operador aciona **Gravar Historico**, então observações persistem para consulta futura.
- **CANG-002-14:** Dado **Visualizar Titulos** no histórico, quando acionado, então exibe consulta read-only dos títulos (sem baixa/juros).
- **CANG-002-15:** Dado janela de visita cadastrada (dia + semanas + datas), quando operador grava **Consulta de Visita**, então agenda respeita representante vinculado ao cliente.
- **CANG-002-16:** Dado origem e destino informados, quando operador confirma **Transferencia de Representantes**, então carteira migra conforme período e opção **Transfere os Pedidos**.
- **CANG-002-17:** Dado parâmetros MixPedido alterados e **Confirmar Informações**, então liberação, crédito e portadores bloqueados persistem para o cliente.
- **CANG-002-18:** Dado inclusão com dados nas 6 abas, quando operador aciona **Gravar**, então cliente é persistido com endereços alternativos, responsáveis, referências, parâmetros financeiros e veterinários conforme preenchido.
- **CANG-002-19:** Dado cliente **Prospecto** (`D_SITU`), quando convertido para cliente ativo, então política de crédito e bloqueios passam a valer em pedidos e títulos.
- **CANG-002-20:** Dado fila vazia, quando operador aciona **Clientes Novos** ou **Clientes Liberados**, então sistema informa que não localizou registros e encerra sem abrir listagem.
- **CANG-002-21:** Dado clientes com coordenadas/endereços válidos, quando operador aciona **Clientes Chrome**, então Google Maps exibe marcadores na região dos clientes.

## Validação legado — demo (2026-06-01)

### Lista (`Cadastro de Clientes`)

| Coluna / painel | Exemplo demo |
|-----------------|--------------|
| Cliente | DISTRIBUIDORA BUENO LTDA - EPP |
| CNPJ | 00.000.000/0000-10 (`D_DEST`) |
| Código | C0082 |
| Telefone | (99) 9999-9999 |
| Representante 1 | V0002 — TESTE |
| MixPedido | Liberado para pedidos internet |
| Limite crédito | R$ 0,00 |
| Geolocalização | -23,5808993 / -46,6344613 |

**2 clientes** na base demo `C:\Hidra`.

### Pesquisa Geral (botão **Geral**)

Acesso: **Cadastro de Clientes** → botão **Geral** → tela **Pesquisa Geral**.

| Elemento | Demo |
|----------|------|
| Lista | Coluna **Nome do Cliente** — BUENO (selecionado), FLORIANO |
| **Informação do Cliente** | DISTRIBUIDORA BUENO LTDA - EPP |
| Fantasia | BUENO (`D_FANT`) |
| Endereço | RUA MACHADO DE ASSIS, 358 |
| Cidade / UF / CEP | SAO PAULO / SP / 05281-010 |
| Bairro / Telefone | VILA MARIANA / (99) 9999-9999 |
| E-mail | distribuidor@gmail.com |
| Contador | **Encontrados: 2** |
| Ações | **Selecionar**, **Retornar** |

Visão consolidada por nome — complementa a pesquisa por destinatário no campo superior da lista. *Comportamento de **Selecionar** na grid principal não testado nesta sessão.*

### Geolocalização

Painel na lista exibe latitude/longitude (`D_LATI`, `D_LONG`) e duas ações:

| Ação | Onde | Comportamento validado |
|------|------|------------------------|
| **Botão ao lado das coordenadas** | Painel Geolocalização | Geocodifica **cliente selecionado**; demo C0082 → alerta RNG-002-08b |
| **Geral** | Mesmo painel | Processamento **em lote** (distinto do botão Geral = Pesquisa Geral) |

#### Processamento geral (lote)

**Resultado da geolocalização** (demo — 2 clientes na base):

| Contador | Demo |
|----------|------|
| Clientes | 000002 |
| Clientes processados | 000002 |
| Clientes Localizados | 000000 |
| Clientes Não Localizados | 000000 |
| Problemas de Conexão e/ou Localização | 000000 |
| Pesquisa sem Latitude × Longitude | 000000 |

Botão **OK** fecha o resumo. *Demo sem serviço de mapas ativo; C0082 já possui coords (-23,5808993 / -46,6344613) de cadastro anterior.*

#### Geocodificação individual (botão ao lado das coords)

Acesso: selecionar cliente na lista → painel **Geolocalização** → clicar **ao lado das coordenadas** (ícone/botão `...`).

**Demo C0082 (BUENO):** abre alerta **Atenção**:

| Elemento | Conteúdo |
|----------|----------|
| Mensagem | *Erro - Verificar Endereço para Localização.!* |
| Endereço exibido | RUA MACHADO DE ASSIS, 358 - VILA MARIANA, SAO PAULO - SP, 05281-010, Brasil |

Endereço montado a partir de `D_ENDE`, `D_BAIR`, `D_XMUN`, `D_ESTA`, `D_CEPN`. **OK** fecha. Mesmo alerta observado no processamento em lote.

*Com serviço de mapas ativo e endereço válido, espera-se atualização de coords e/ou abertura de mapa (`scd3map.prg`) — não reproduzido no demo.*

### Formulário — Dados Cadastrais (cliente C0082)

| Campo UI | Valor demo | Campo DBF |
|----------|------------|-----------|
| Código | C0082 | `D_CODI` |
| Tipo / CNPJ | Jurídica / 00.000.000/0000-10 | `D_DEST` |
| Empresa | DISTRIBUIDORA BUENO LTDA - EPP | `D_NOME` |
| Endereço | RUA MACHADO DE ASSIS, 358 | `D_ENDE` |
| Cidade / UF / CEP | SAO PAULO / SP / 05281-010 | `D_XMUN`, `D_ESTA`, `D_CEPN` |
| Fantasia | BUENO | `D_FANT` |
| Contato / Fone | EDISA / (99) 9999-9999 | `D_CON1`, `D_FONE` |
| Repres (1) | V0002 — TESTE | `D_REP1` |
| Divisão | 1-Revendedor | `D_DIVI` |
| Tabela | Preço (C) | `D_TABE` (=3) |
| Frete | 0-Emitente | `D_FRET` |

Endereços de cobrança/entrega repetem principal no DBF (`D_ENDC`, `D_ENDN`); IBGE município `D_CMUN=3554102`.

### Formulário — 6 abas *(Inclusão do Cliente — 2026-06-01)*

Caminho: **Cadastros → Clientes → Incluir** → **Informação do Cliente**. Cabeçalho fixo em todas as abas; **Gravar** / **Cancelar** no rodapé. Atalhos de teclado nas abas (ex.: Alt+C Complemento, Alt+R Responsaveis).

#### Aba Dados Cadastrais *(inclusão — campos vazios, defaults)*

| Campo UI | Default inclusão | Campo DBF |
|----------|------------------|-----------|
| Fantasia, Site, Contato, Fone, Celular | vazios | `D_FANT`, site, `D_CON1`, `D_FONE` |
| Distrito, Setor, Roteiro | vazios | distrito, setor, `D_ROTE` |
| Repres (1)…(4) | lookup código + nome | `D_REP1`…`D_REP4` |
| Transporte | lookup | `D_TRAN` |
| Divisão | 1-Revendedor | `D_DIVI` |
| Tabela | Preço (A) | `D_TABE` |
| Frete | 0-Emitente | `D_FRET` |

*Alterar C0082:* Tabela **Preço (C)** — default de inclusão difere do cliente existente.

#### Aba Complemento

| Grupo | Campos |
|-------|--------|
| Cobrança | Endereço, Cidade, Estado, Cep |
| Entrega | Endereço, Cidade, Estado, Cep |
| Classificação | Classe 1, Classe 2, Atividade (lookup `...`) |
| Fiscal | Cod.Ibge, Suframa |
| Alvarás | Validade Anvisa + Nº Alvará; Validade Mapa + Nº Alvará |

#### Aba Responsaveis

Três blocos idênticos **Proprietario** (não veterinário):

| Por bloco | Campos |
|-----------|--------|
| Identificação | Proprietario, Observação |
| Documentos | Nascimento, CPF, RG |

#### Aba Referencias

| Seção | Campos |
|-------|--------|
| Referências Comerciais | Empresa 1–3 + Telefone (máscara `( ) -`) |
| Referências Bancárias | Banco 1–2 + AG + CC + Telefone |

#### Aba Financeiro *(defaults inclusão)*

| Campo | Default |
|-------|---------|
| Cadastro | 01/06/2026 |
| LimiteC | 0,00 |
| Desconto | 0,00% |
| Licença / Data | vazios |
| Comissão | 0,00% |
| Sistema | Prospecto |
| Juros | Não Cobrar Juros |
| Meta/Mes | 0,00 |
| Subs. Trib. | Calculo Externo |
| Motivo | Devolução Parcial |
| Devolução | 0,00% |
| Serasa/Spc | vazio |
| Bloquear Fabricantes | lookup (`....`) |
| Qual Motivo / Lembrete | vazios |
| Venix | Enviar |
| MixPedido | Enviar |
| Bloquear Portadores | Depósito, Boleto, Cheque — nenhum |
| Venix e MixPedido | Dinheiro, Cartão — nenhum |

**Nota:** Venix/MixPedido e portadores também aparecem na tela **MixPedido** (RNG-002-05b) com **Nova Senha** e **Credito R$** — duas entradas de UI para parametrização B2B/campo.

#### Aba Diversos *(domínio pet/vet)*

Dois blocos **Veterinario** + campo empresa:

| Por veterinário | Campos |
|-----------------|--------|
| Profissional | Veterinario, Nascimento, CPF, RG, Crmv, Telefone |
| Empresa | Crmv Emp |

### Atalho Pedidos (cliente C0082)

Acesso: lista de clientes → botão **Pedidos**.

**Painel Cliente:** nome, endereço, código C0082, CNPJ, contato, telefone.

**Painel Financeiro:**

| Indicador | Demo |
|-----------|------|
| Status liberação | **PARCIALMENTE LIBERADO** |
| Vencidos | 2 títulos — R$ 855,57 |
| A vencer / Devolvidos / Cheques pré | 0 |
| Total devedor | 2 — R$ 855,57 |
| Limite de crédito | R$ 0,00 |
| Vencidos com juros | R$ 10.855,76 (+ R$ 1.168,83) |
| Data cadastro | 08/05/2015 |
| Última compra | 23/09/2016 — R$ 10,59 |
| Maior compra | 23/09/2016 — R$ 855,57 |

**Grid Pedidos:**

| Número | Valor | Situação | Repres | Transp |
|--------|-------|----------|--------|--------|
| 9000975 | 855,57 | Entrega | V0002 | T0020 |
| 9000976 | 10,59 | Entrega | V0002 | — |

Botões: **Detalhe**, **Finalizar**. Nota fiscal `0` nos dois registros demo.

**DBF:** `D_LLIB=2` coerente com “parcialmente liberado”; `D_CADT=2015-05-08`.

### Atalho Titulos (cliente C0082)

Acesso: lista de clientes → botão **Titulos**. Painel financeiro idêntico ao atalho Pedidos.

**Grid Titulos:**

| Documento | Emissão | Vencimento | Valor parcela | Portador | Banco / Boleto |
|-----------|---------|------------|---------------|----------|----------------|
| 9000975/1 | 23/09/2016 | 21/10/2016 | 427,79 | 3-Cheque | — |
| 9000975/2 | 23/09/2016 | 28/10/2016 | 427,78 | 2-Boleto | 237 / 237000000002P |

**Ações validadas:**

| Botão | Comportamento |
|-------|---------------|
| **Juros** | Recalcula juros; **atualiza Valor da Parcela** na grid (regra informada pelo operador) |
| **Extrato** | Preferências: Analítico, ordem Vencimento, portador/conteúdo Todos, período, representante → relatório ANALITICO POR VENCIMENTO |
| **Baixa** | Título 9000975/1: RECEBER, CHEQUE, valor 427,79, venc. 21/10/2016, 28 dias; pagamento 01/06/2026 → **valor calculado 5.432,93** (juros acumulados demo) |
| **Finalizar** | Fecha tela |

Total parcelas (427,79 + 427,78 ≈ 855,57) bate com resumo **Vencidos** do painel financeiro.

### Atalho Devolvidos (cliente C0082)

| Entrada | Resultado demo |
|---------|----------------|
| Lista → **Devolvidos** | Alerta: *Cheques Devolvidos não Localizados.!!!* |
| Histórico → **Visualizar Devolvidos** | Tela **Cheques devolvidos do Cliente** — grid vazio |

### Atalho Historico (cliente C0082)

Acesso: lista → **Historico**.

| Elemento | Demo |
|----------|------|
| Painel Cliente + Financeiro | Igual Pedidos/Titulos |
| Área **Historico** | Vazia (`SCD_HIST` sem registros; `D_OBS1`–`D_OBS3` vazios) |
| **Gravar Historico** | Persiste texto (não testado gravação) |
| **Bloquear Pedido** / **Bloquear Cliente** | Ações de bloqueio no contexto do cliente |
| **Visualizar Titulos** | Grid read-only: 9000975/1 e /2, Repres V0002, status **N** |
| **Visualizar Devolvidos** | Grid vazio — colunas Documento, Emissão, Vencimento, Valor, Portador, Bco, Boleto, Repres, S |
| **Retornar** | Fecha tela |

**Visualizar Titulos** vs. atalho **Titulos** na lista: consulta sem Extrato, Baixa, Juros.

### Atalho Visitas (cliente C0082)

Acesso: lista → **Visitas** → tela **Consulta de Visita**.

| Campo | Demo |
|-------|------|
| Cliente | DISTRIBUIDORA BUENO LTDA - EPP |
| Representante 1 | V0002 — TESTE |
| Dia da Visita | Segunda |
| Semanas | 1ª–5ª (checkboxes; nenhuma marcada no demo) |
| Venix / Proxima / Livre / Hora | Vazios |
| Regra exibida | Data enviada ao rep. = mais próxima de **01/06/2026** das datas acima |

Botões: **Gravar**, **Retornar**. DBF: `D_REP1=V0002`; `D_SEM1`–`D_SEM4` = `2000001  0`; demais campos visita vazios.

### Atalho Substituir Representantes

Acesso: lista → **Substituir Representantes** → **Transferencia de Representantes**.

| Campo | Observação |
|-------|------------|
| Representante — Origem | Lookup (código + nome) |
| Representante — Destino | Lookup (código + nome) |
| Transfere os Pedidos? | Dropdown — demo: **Não** |
| Período de Cadastro | Data inicial **até** data final |
| Ações | CONFIRMAR TRANSFERENCIA / CANCELAR TRANSFERENCIA |

*Transferência não executada no demo — apenas tela validada.*

### MixPedido por cliente (C0082)

Acesso: lista → painel MixPedido → **Alterar Informações** → tela **MixPedido**.

| Campo | Demo |
|-------|------|
| MixPedido | **Enviar** |
| Nova Senha | **Não** |
| Credito R$ | **0,00** |
| Bloquear Portadores | Depósito, Boleto, Cheque, Dinheiro, Cartão — nenhum marcado |

Lista exibe: *CLIENTE LIBERADO PARA EFETUAR PEDIDOS NA INTERNET (MIXPEDIDO)* + limite R$ 0,00. DBF: `D_LIBE=1`, `D_LIMI=0`.

Emitente define termo, site e tabela enviada (RN-001 aba MixPedido); cliente define envio, senha, crédito e portadores bloqueados.

### Integração — Clientes Novos / Liberados

Acesso: **Cadastros → Integração**.

| Item menu | Filtro | Demo |
|-----------|--------|------|
| **Clientes Novos** | Pendentes de liberação/cadastro (integração) | Alerta *Clientes Novos não Localizados...Finalizar* → **OK** |
| **Clientes Liberados** | Já liberados | **Mesma lista/tela** — demo vazio, mesmo comportamento |

Uma única consulta de integração com filtro distinto por menu. Com registros na fila, espera-se grid de clientes *(não reproduzido no demo)*.

### Relatórios — Clientes Chrome (mapa)

Acesso: **Relatórios → … → Positivação → Específicos → Clientes Chrome**.

Abre **Google Maps** no navegador (Chrome), centrado na região dos clientes com endereço/coordenadas cadastrados.

**Demo (base com 2 clientes — São Paulo):**

| Elemento | Observação |
|----------|------------|
| Mapa | Região SP — Pinheiros, **Vila Mariana**, Ipiranga, Mooca |
| Marcadores | Vários pins coloridos (ex.: amarelo, vermelho, roxo) — clientes geolocalizados |
| Controles | Mapa padrão Google (zoom, tela cheia) |
| API | Marca d'água *For development purposes only* — chave/billing Google Maps em modo dev |

Plotagem usa `D_LATI`/`D_LONG` (ou geocodificação do endereço). BUENO (Vila Mariana) coerente com coords `-23,5808993 / -46,6344613`.

**Clientes Internet** — mesmo bloco **Específicos**; *não testado nesta sessão*.

## Regras e restrições conhecidas

- Cliente sempre vinculado ao emitente (`D_EMIT`).
- Metas por cliente (`D_META`, `D_MET1`…`D_MET4`) existem no DBF; UI de metas não vista nesta sessão.
- Permissões de cliente seguem matriz de usuário (I/A/E/C em **Acessos** — RN-001).

## Funcionalidades pendentes (checklist de exploração)

Use esta lista para completar o RN-002. Envie **1 print por item** (ou por aba).

### Prioridade 1 — Abas do formulário *(Informação do Cliente → Incluir/Alterar)*

| # | Aba | Status |
|---|-----|--------|
| 1 | **Dados Cadastrais** | ✅ Validado (inclusão + C0082) |
| 2 | **Complemento** | ✅ Validado (inclusão) |
| 3 | **Responsaveis** | ✅ Validado (inclusão — 3 proprietários) |
| 4 | **Referencias** | ✅ Validado (inclusão) |
| 5 | **Financeiro** | ✅ Validado (inclusão — defaults) |
| 6 | **Diversos** | ✅ Validado (inclusão — 2 veterinários + Crmv Emp) |

### Prioridade 2 — Ações na lista *(Cadastro de Clientes)*

| # | Onde clicar | O que validar |
|---|-------------|---------------|
| 6 | **Alterar Informações** (MixPedido) | Parametros por cliente | ✅ Validado |
| 7 | **Geral** (botão superior) | Pesquisa Geral — lista por nome + painel cliente | ✅ Validado |
| 8a | **Geral** (painel geolocalização) | Processamento em lote + Resultado | ✅ Validado |
| 8b | Botão **ao lado das coords** | Geocodificação individual → alerta endereço | ✅ Validado (demo) |
| 9 | **Substituir Representantes** | RN-009 | ✅ Validado |
| 10 | **Historico** / **Visitas** | RN-011 | Historico ✅ / Visitas ✅ |

### Prioridade 3 — Atalhos inferiores *(cliente selecionado)*

| # | Botão | RN relacionado | Status |
|---|-------|----------------|--------|
| 11 | **Pedidos** | RN-002 / RN-004 | ✅ Validado |
| 12 | **Titulos** | RN-002 / RN-008 | ✅ Validado |
| 13 | **Devolvidos** | RN-002 / RN-008 | ✅ Validado (demo vazio — alerta) |

### Prioridade 4 — Menus fora de Cadastro de Clientes *(caminhos no `scd.exe`)*

Itens **não** aparecem na lista **Cadastro de Clientes**. Localização inferida do menu legado:

| # | Função | Onde clicar no Hidra | RNG | Status |
|---|--------|----------------------|-----|--------|
| 14 | **Bloquear Cliente** / **Liberar Cliente** | Lista → **Historico** → botões no rodapé (não é menu principal) | RNG-002-05a / RNG-002-16c | ✅ Historico |
| 15 | **Clientes Novos** / **Liberados** | **Cadastros → Integração** — mesma lista, filtros distintos | RNG-002-10a/b | ✅ Validado (demo vazio) |
| 15c | **Clientes Chrome** | **Relatórios → Específicos → Clientes Chrome** | RNG-002-10c | ✅ Mapa Google (demo SP) |
| 15d | **Clientes Internet** | **Relatórios → Específicos** | RNG-002-10d | ⏸ Não abriu no demo |
| 16 | **Alterar Email deste Cliente** | Ações de e-mail: *Enviar cliente Selecionado* / *Alterar Email deste Cliente* *(cliente selecionado; provável menu Financeiro/e-mail ou contexto da lista)* | RNG-002-11 | Não validado |
| 17 | **Atendimento ao Cliente** | **Financeiro → Boletos** *(perto de Processar Geral, Arquivo Remessa)* | RN-011 | Não validado |
| 18 | **Atividades de Clientes** | **Cadastros → Atividades de Clientes** *(tabela auxiliar, não filtro de clientes)* | RN-011 | Não validado |

**Permissões:** se **Integração** ou **Relatórios** estiverem bloqueados em **Cadastros → Usuarios → Acessos** (RN-001), esses itens somem para o usuário logado.

### Já documentado

- [x] Lista + grid (HIST, BLOQ, TAB)
- [x] **Pesquisa Geral** (botão Geral da barra superior)
- [x] **Geolocalização** — coords, lote (Geral), individual (botão ao lado das coords)
- [x] Incluir / Alterar / Consultar — cabeçalho + **6 abas**
- [x] Aba **Dados Cadastrais**
- [x] Aba **Complemento** — cobrança/entrega, alvarás Anvisa/Mapa
- [x] Aba **Responsaveis** — 3 proprietários
- [x] Aba **Referencias** — comerciais e bancárias
- [x] Aba **Financeiro** — crédito, juros, Venix/MixPedido
- [x] Aba **Diversos** — veterinários + Crmv Emp
- [x] Painel MixPedido + **Alterar Informações** (parametros por cliente)
- [x] Geolocalização — coordenadas na lista
- [x] Atalho **Pedidos**
- [x] Atalho **Titulos** (Extrato, Baixa, Juros)
- [x] Atalho **Devolvidos** + **Visualizar Devolvidos**
- [x] Atalho **Historico**
- [x] Atalho **Visitas** — Consulta de Visita
- [x] Atalho **Substituir Representantes** — Transferencia de Representantes
- [x] **Clientes Novos / Liberados** (Integração — mesma lista, demo vazio)
- [x] **Clientes Chrome** — mapa Google Maps (Relatórios → Específicos)
---

## Dúvidas em aberto

- [x] Conteúdo das **6 abas** do formulário (Inclusão — 2026-06-01).
- [ ] Gravação efetiva na inclusão (não testado **Gravar** com cliente novo).
- [x] Atalho **Pedidos** — painel financeiro + grid (C0082).
- [x] Atalho **Titulos** — grid parcelas, Juros, Extrato, Baixa (C0082).
- [x] Atalho **Devolvidos** — alerta (lista) + grid vazio (via Histórico).
- [x] Atalho **Historico** — observações, bloqueios, visualizar títulos/devolvidos.
- [ ] Significado exato **PARCIALMENTE LIBERADO** vs. `D_LLIB`, `D_LIBE`, `D_BLOC`.
- [x] **Clientes Novos / Liberados** — mesma lista Integração, fila vazia no demo.
- [x] **Clientes Chrome** — mapa com endereços/coords (Google Maps).
- [ ] **Clientes Internet** — não acessível no demo (2026-06-01).
- [ ] Mapa via botão coords (geocodificação individual bem-sucedida).
- [ ] Tela **Devolvidos** com grid — requer cliente com cheque devolvido na base.

## Referências legado

- Tabelas: `SCD_DEST`, `SCD_CONT`, `SCD_ATIV`
- Programas: `scd1cli.prg`, `scd2cli.prg`, `scd3cli.prg`, `scd3map.prg`
- Sessão: [SESSAO-2026-06-01-RN-002.md](../legado/registro-sessoes/SESSAO-2026-06-01-RN-002.md)
- RNs relacionados: RN-004 (pedido), RN-008 (títulos), RN-011 (visitas), RN-012 (MixPedido)
