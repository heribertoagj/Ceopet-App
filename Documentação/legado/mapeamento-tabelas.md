# Mapeamento de tabelas DBF — Hidra

Referência rápida para derivar RTs. Gerado por análise de schema DBF.

## Entidades centrais

### SCD_EMIT — Emitente (empresa operadora)

Configura a operação inteira: identificação fiscal, SMTP, pastas de integração bancária/NFe, sequências, percentuais padrão, textos de e-mail e pedido.

Campos relevantes (amostra): `E_EMIT`, `E_NOME`, `E_FANT`, endereço, `E_CNAE`, `E_CMUN`, IE, e-mail, parâmetros NFe (`E_NUNF`, `E_SERI`, `E_VERS`), pastas (`E_REMS`, `E_RETO`, `E_LIXE`, `E_ANFE`), juros, bonificação, flags operacionais.

### SCD_DEST — Cliente / destinatário

Cadastro rico para distribuição B2B com foco veterinário: múltiplos endereços, representantes (até 4), rotas, tabela de preço, limite de crédito, bloqueio/liberação, metas, veterinários responsáveis (`D_VET*`, `D_CRM*`), geolocalização (`D_LATI`, `D_LONG`).

### SCD_PROD — Produto

Código, EAN, NCM, unidade, embalagem, estoque mín/máx, fabricante, família, tributação (ICMS, IPI, PIS/COFINS), múltiplas faixas de preço (`P_FX*`, `P_PVE*`, `P_PPR*`), comissões por faixa, saldo (`P_SALD`, `P_EPEN`, `P_EEFU`), controle de lote/validade.

### SCD_PEDI — Pedido (cabeçalho)

`PED_SIST` discrimina tipo (ex.: `C` = comercial/venda). Liga cliente (`PED_CLIE`), representante, transportadora, datas (pedido, liberação, saída, entrega), totais, descontos, frete, status (`PED_STAT`), romaneio (`PED_ROMA`), flags faturamento/separação.

### SCD_ITEM — Itens do pedido

Produto, quantidades (pedida, baixada, recebida), valores, descontos, lote, CFOP, impostos por item.

## Tabelas auxiliares

| Tabela | Descrição |
|--------|-----------|
| `SCD_ATIV` | Atividades de clientes |
| `SCD_BANC` | Bancos |
| `SCD_CAIX` | Caixa |
| `SCD_CATE` | Categorias de produto |
| `SCD_CFOP` | CFOP |
| `SCD_CHEQ` | Cheques |
| `SCD_COND` | Condições de pagamento |
| `SCD_CONT` | Contatos |
| `SCD_DESP` | Despesas |
| `SCD_ESTA` | Estatísticas / estados auxiliares |
| `SCD_FABR` | Fabricantes |
| `SCD_FAMI` | Famílias de produto |
| `SCD_FORN` | Fornecedores |
| `SCD_HIST` | Histórico |
| `SCD_HVIS` / `SCD_MVIS` | Visitas |
| `SCD_IBGE` | Municípios IBGE |
| `SCD_ICOM` | Itens de compra |
| `SCD_LOTE` / `SCD_MLOT` | Lotes |
| `SCD_MCAI` | Movimentos de caixa |
| `SCD_META` | Metas |
| `SCD_PCOM` | Pedidos de compra |
| `SCD_REPR` | Representantes |
| `SCD_ROMA` | Romaneios |
| `SCD_SENH` | Usuários e permissões (`S_ACES`) |
| `SCD_TNCM` | Tabela NCM |
| `SCD_TRAN` | Transportadoras |

## Fiscal e PDA

| Tabela | Descrição |
|--------|-----------|
| `NFe_NOTA` | Cabeçalho NF-e |
| `NFe_ITEM` | Itens NF-e |
| `PDA_PEDI` / `PDA_ITEM` / `PDA_CLIE` | Pedidos originados em dispositivo de campo |

## Relacionamentos inferidos

- Emitente (`*_EMIT` / `E_EMIT`) → escopo multi-empresa por base.
- Cliente + Produto + Emitente → Pedido → Itens → (opcional) NFe → Boleto.
- Compra → Entrada → atualização estoque produto.
- Representante → Cliente → Meta → Comissão.

Detalhamento campo a campo: ver JSON em `Documentacao/_tools/hidra_inventory.json`.
