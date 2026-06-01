# RN-014 — Integrações externas

| Campo | Valor |
|-------|-------|
| **ID** | RN-014 |
| **Status** | Rascunho |
| **Fase** | B2B-1 |
| **Origem legado** | pastas Remessa/Retorno, SMTP emitente, menus import/export |
| **RT relacionado** | RT-014 (pendente) |

## Objetivo

Integrar o ERP com bancos, SEFAZ (via XML), e-mail, mapas e parceiros comerciais (Venix, Accera).

## Contexto

Integrações são parte central da operação — não são opcionais para financeiro e fiscal.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | TI, financeiro, fiscal |
| **Canal** | Arquivos, SMTP, APIs legadas |
| **Objeto de negócio** | Arquivo remessa/retorno, XML, e-mail |
| **Regra comercial** | Pastas configuráveis por emitente |
| **Estoque** | Indireto (XML entrada mercadoria) |

## Requisitos funcionais (RNG)

- **RNG-014-01:** Integração bancária CNAB — remessa e retorno de boletos.
- **RNG-014-02:** Importação/exportação XML NF-e.
- **RNG-014-03:** Envio de e-mail transacional (NF-e, cobrança, comunicados).
- **RNG-014-04:** Integração **Google Maps** — geocodificação em lote (RNG-002-08a), mapa por coords (RNG-002-08c) e visualização **Clientes Chrome** (RNG-002-10c; `scd3map.prg`).
- **RNG-014-05:** Importar arquivos Venix.
- **RNG-014-06:** Export/import Accera (referência menu).
- **RNG-014-07:** Exportar representantes, supervisores e mix pedido para parceiros.
- **RNG-014-08:** Reorganizar/importar arquivos de clientes recebidos.

## Critérios de aceite de negócio (CANG)

- **CANG-014-01:** Dado remessa gerada, quando enviada ao banco e retorno importado, então ciclo fecha sem intervenção manual nos títulos afetados.
- **CANG-014-02:** Dado SMTP configurado, quando e-mail de NF-e é disparado, então destinatário recebe anexo/XML conforme configuração.

## Dúvidas em aberto

- [ ] Venix e Accera ainda contratados?
- [ ] Substituir Google Maps por outro provedor na modernização?

## Referências legado

- Pastas: `Documentacao/Hidra/Remessa`, `Retorno`, `Saidatxt`, `Lixeira`
- Programas: `scd2xml.prg`, `scd3map.prg`
- Config: paths em `SCD_EMIT` (`E_REMS`, `E_RETO`, SMTP)
