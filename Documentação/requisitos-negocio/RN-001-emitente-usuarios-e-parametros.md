# RN-001 — Emitente, usuários e parâmetros

| Campo | Valor |
|-------|-------|
| **ID** | RN-001 |
| **Status** | Validado legado (demo) — produção Ceopet pendente |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_EMIT`, `SCD_SENH`, `scd1emi.prg`, `scd1sen.prg` |
| **RT relacionado** | RT-001 (pendente) |
| **Última validação** | 2026-06-01 — [Sessão RN-001](../legado/registro-sessoes/SESSAO-2026-06-01-RN-001.md) |

## Objetivo

Permitir configurar a empresa emitente, usuários do sistema e parâmetros globais que governam vendas, fiscal, financeiro, integrações e comportamento operacional do ERP.

## Contexto

Toda operação no Hidra está vinculada a um **emitente**. O fluxo legado separa **consulta/seleção** (`Relação de Emitentes`) de **manutenção** (`Informação do Emitente`, 10 abas). Usuários possuem login, número interno, perfil (`S_OPER`) e **duas camadas de permissão** na UI: **Acessos** (bloqueio por módulo, com CRUD I/A/E/C nos cadastros principais) e **Restrição** (exceções de negócio). No DBF, isso persiste principalmente em `S_ACES` (~150 posições) e flags do emitente em `E_AVIS`.

**Escopo desta validação:** base demo em `C:\Hidra`, sem dados de produção Ceopet.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Administrador, TI, operador comercial, separação/expedição |
| **Canal** | Desktop |
| **Objeto de negócio** | Emitente, usuário, parâmetro operacional, feature flags |
| **Regra comercial** | Multi-emitente na mesma base; sequências e políticas por emitente |
| **Estoque** | Indireto (flags Diretrizes, pastas, separação/logística) |

## Fluxo de telas — Emitente

```text
Cadastros ? Emitentes
  ?? Relação de Emitentes (grid + detalhe + Selecionar / Alterar)
  ?? Informação do Emitente
        ?? Dados do Emitente (cabeçalho fixo)
        ?? Abas: Complemento | Configuração | Pastas | Venix | MixPedido
                 | Diversos | Diretrizes I–IV
```

## Fluxo de telas — Usuários

```text
Cadastros ? Usuarios
  ?? Relação de Usuarios (Senhas) — CRUD, pesquisa, Copiar/Colar perfil
  ?? Tabela de Acessos — Bloquear Acessos (módulos + I/A/E/C)
  ?? Tabela de Acessos — Restrição ao Usuario (exceções de negócio)
```

## Requisitos funcionais (RNG)

### Emitente — cadastro e seleção

- **RNG-001-01:** Cadastrar e manter emitente com tipo pessoa (Jurídica/Física), CNPJ/CPF (`E_EMIT`), razão social (`E_NOME`), endereço, IE (`E_IEST`), e-mail (`E_EMAI`), telefone (`E_FONE`), país (`E_PAIS`), código IBGE (`E_CMUN`) e nome fantasia (`E_FANT`).
- **RNG-001-02:** Parametrizar NF-e por emitente: série (`E_SERI`), próximo número (`E_NUNF`), versão layout (`E_VERS`), regime tributário (`E_CRTN`), forma de emissão (`E_FEMI`), alíquota ICMS padrão (`E_ICMS`), tributos NF-e, casas decimais, layout de pedido e **dados adicionais** impressos na NF-e.
- **RNG-001-03:** Configurar pastas operacionais: remessa (`E_REMS`), retorno (`E_RETO`), lixeira (`E_LIXE`), saída XML NF-e (`E_ANFE`), boletos (`E_ABOL`) e diversos/logo (`E_LOGO`), com seletor de diretório na UI.
- **RNG-001-08:** Listar emitentes e **selecionar emitente ativo** da sessão (`Selecionar Emitente`); alteração via `Alterar dados do Emitente` com **Gravar** / **Cancelar**.

### Emitente — aba Configuração (comercial)

- **RNG-001-06:** Definir política de desconto (ex.: porcentagem), comissão (ex.: geral proporcional) e tabela de pedido (ex.: por valor).
- **RNG-001-06a:** Configurar até **5 faixas** de desconto com valor de tabela, percentual de desconto e peso (`E_FAD*`, `E_FAP*`).
- **RNG-001-06b:** Habilitar **portadores** (formas de pagamento): depósito, boleto, cheque, dinheiro, cartão (`E_DEPO`, `E_BOLE`, `E_CHEQ`, `E_DINH`, `E_CART`).
- **RNG-001-06c:** Manter **numeradores** por emitente: compras/entradas, contas a pagar, pedidos de compra, pedidos integração, romaneio e **pedido mínimo** (`E_PEDI`, `E_PPAG`, `E_PPED`, `E_NPED`, `E_NROM`, `E_MINI`).

### Emitente — aba Diversos

- **RNG-001-06d:** Parametrizar romaneio padrão, rodapé de pedido, transportadora/banco padrão, dias para liberar atraso, descontos Venix/comissão, bonificação, juros bancários e de cheques (`E_BONU`, `E_JURB`, `E_JURC`).
- **RNG-001-06e:** Configurar PIS/COFINS padrão para vendas e outras operações (`E_PPIS`, `E_PCOF`, códigos e isenções).
- **RNG-001-06f:** Manter sequenciais de cadastro (cliente, representante, fornecedor, transportadora, produto) e parâmetros de estoque (data base, aviso de validade, nome do computador servidor).

### Emitente — Diretrizes I a IV (comportamento)

- **RNG-001-09:** Configurar **flags operacionais** por emitente (~58 opções em 4 abas), persistidas em `E_AVIS`, agrupadas por domínio:
  - **Comercial/pedido:** desconto na venda, tabela de preços, reserva como entrega futura, código de barras, margem, impostos no pedido.
  - **Financeiro/boletos:** rateio contas a receber, atualização por entrega, reprocessamento de boletos, NF no boleto, baixa com valor menor, estornos.
  - **Fiscal/NF-e:** datas na NF, validade/fabricação, parcelas na NFe, FCP, ST bonificação, código de barras na NFe.
  - **Estoque/lotes/separação:** controle de lotes, ajuste por saldo, rotina de separação e logística, margem bruta no item.
  - **Impressão/expedição:** impressoras, romaneio, orçamento em duas vias, totais em relatórios.

### Emitente — integrações (escopo condicional)

- **RNG-001-10:** Parametrizar integração **Venix** (processo de digitação, limite de sincronismo, mensagens de envio, flags de sync/bloqueio) — *validar com Ceopet se permanece no escopo*.
- **RNG-001-11:** Parametrizar **MixPedido** (site, contato, liberação por e-mail, limite R$, tabela de preço enviada, termo de compra padrão) — *validar com Ceopet se permanece no escopo*.

### E-mail e SMTP

- **RNG-001-04:** Parametrizar textos de e-mail (`E_ASSU`, `E_ADEB`, remetente `E_REME`/`E_MPAD`).
- **RNG-001-05:** Configurar SMTP por emitente (`E_CDES`, `E_CPOR`, `E_CSSL`, `E_CAUT`, credenciais `E_SENH`).
- **Nota:** campos SMTP existem no DBF demo; **não localizados nas 10 abas** da UI explorada — pendente confirmar tela ou versão.

### Usuários e permissões

- **RNG-001-07:** Cadastrar usuários com login (`S_USUA`), senha (`S_SENH`), nome (`S_NOME`), número interno (`S_NUME`), vínculo ao emitente (`S_EMIT`) e pesquisa na relação.
- **RNG-001-07a:** Manter usuários (incluir, alterar, excluir) e **copiar/colar perfil** de permissões entre usuários.
- **RNG-001-07b:** Configurar **Acessos** — bloqueio por módulo; para Clientes, Fornecedores, Representantes, Produtos e Transportadoras, bloqueio granular **Inclusão / Alteração / Exclusão / Consulta** (I/A/E/C).
- **RNG-001-07c:** Configurar **Restrições** — exceções como bloquear exclusão global, liberar crédito/cobrança, alterar data do pedido, alterar margens de custo, estornos financeiros, movimentação de produtos, etc.

## Critérios de aceite de negócio (CANG)

- **CANG-001-01:** Dado emitente não selecionado, quando operador tenta operação que exige emitente, então sistema exibe mensagem para selecionar ou cadastrar emitente.
- **CANG-001-02:** Dado módulo bloqueado em **Acessos**, quando usuário tenta a função, então acesso é negado.
- **CANG-001-02a:** Dado CRUD bloqueado (ex.: Alteração de Clientes), quando usuário tenta alterar, então operação é impedida mesmo com consulta liberada.
- **CANG-001-02b:** Dado restrição não marcada como “Liberar…”, quando usuário tenta ação sensível (ex.: alterar data do pedido), então operação é bloqueada.
- **CANG-001-03:** Dado **Numero NFe** configurado, quando próxima NF-e é emitida, então numeração inicia no valor configurado.
- **CANG-001-04:** Dado alteração em qualquer aba do emitente, quando operador confirma **Gravar**, então parâmetros persistem; **Cancelar** descarta alterações.
- **CANG-001-05:** Dado portador desabilitado na aba Configuração, quando operador tenta usá-lo em pedido/financeiro, então forma de pagamento não está disponível.

## Validação legado — demo (2026-06-01)

### Emitente (1 registro)

| Item | Valor demo |
|------|------------|
| CNPJ / razão | `22222222222259` — DISTRIBUIDOR DE PRODUTOS VETERINARIOS LTDA |
| NF-e | Série 2, nº 1071, versão 3.10.37, regime Normal |
| Pastas | `D:\sistema\remessa\`, `retorno\`, `Lixeira\`, `saidatxt\…`, `boleto\`, `diversos\` |
| Configuração | Faixa1: 14,5% / 500 kg; portadores todos habilitados; pedido mín. R$ 250; romaneio seq. 92 |
| Diversos | Bonificação 40%; juros banc. 10% / cheques 15%; PIS 1,65% / COFINS 7,6% |
| MixPedido | Limite R$ 2.000; termo sujeito a crédito e estoque na entrega |
| Diretrizes | ~58 flags; ex.: separação/logística, lotes, rateio CR, NF no boleto — marcadas conforme prints da sessão |

### Usuários (UI demo: JUNIOR, ROBINSON, SEP)

| Elemento | Observação |
|----------|------------|
| Senha na grid | Exibida em texto claro (`111`) — **não replicar** na solução nova |
| Acessos | Dezenas de módulos + I/A/E/C nos 5 cadastros principais |
| Restrição | ~20 regras de exceção (bloquear/liberar operações sensíveis) |
| DBF local | 2 usuários ativos (ROBINSON, SEP); UI pode refletir base distinta ou inclusão posterior |

## Regras e restrições conhecidas

- Paths absolutos Windows no legado — configuráveis por ambiente na modernização.
- `E_AVIS` concentra dezenas de flags booleanas; na solução nova, preferir permissões e feature flags nomeadas por domínio.
- Senhas e SMTP no DBF sem criptografia adequada — exigir hash e cofre de segredos.
- Venix e MixPedido existem no emitente; escopo Ceopet **não confirmado**.

## Dúvidas em aberto (Ceopet / produção)

- [ ] Quantos emitentes em produção?
- [ ] Paths `D:\sistema\` e SMTP — ainda utilizados?
- [ ] Venix e MixPedido — módulos ativos?
- [ ] Quais **Diretrizes** são obrigatórias na operação (priorizar para MVP)?
- [ ] Onde configurar SMTP na UI atual (não visto nas abas)?
- [ ] Significado de `S_DIEN`, `S_OPER` e coluna **S** na relação de emitentes.
- [ ] Usuário **JUNIOR** — perfil real ou apenas demo?

## Referências legado

- Tabelas: `SCD_EMIT`, `SCD_SENH`
- Programas: `scd1emi.prg`, `scd1sen.prg`, `scd1ger.prg`
- Sessão: [SESSAO-2026-06-01-RN-001.md](../legado/registro-sessoes/SESSAO-2026-06-01-RN-001.md)
- RNs relacionados: RN-004 (pedido), RN-007 (NF-e), RN-008 (financeiro), RN-012 (MixPedido), RN-014 (Venix/pastas)
