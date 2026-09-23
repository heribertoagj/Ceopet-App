# RN-011 — Visitas comerciais e CRM

| Campo | Valor |
|-------|-------|
| **ID** | RN-011 |
| **Status** | Em validação — Consulta de Visita via cliente (demo) |
| **Fase** | B2B-1 |
| **Origem legado** | `SCD_HVIS`, `SCD_MVIS`, `scd1vis.prg`, `scd2ate.prg` |
| **RT relacionado** | RT-011 (pendente) |

## Objetivo

Registrar visitas a clientes, motivos de visita, atividades comerciais e atendimentos pós-venda.

## Contexto

Representantes planejam visitas por dia/horário (cadastro cliente). Histórico alimenta gestão comercial e metas.

## Característica do negócio mapeada

| Dimensão | Descrição |
|----------|-----------|
| **Atores** | Representante, comercial interno |
| **Canal** | Desktop; mapas (RN-014) |
| **Objeto de negócio** | Visita, atividade, atendimento |
| **Regra comercial** | Registro obrigatório para positivação |
| **Estoque** | Não impacta |

## Requisitos funcionais (RNG)

- **RNG-011-01:** Cadastrar motivos/tipos de visita.
- **RNG-011-02:** Registrar visita a cliente com data, representante e observações.
- **RNG-011-03:** Consultar histórico de visitas por cliente/representante.
- **RNG-011-04:** Registrar atividades de clientes (promoções, alvará, contatos).
- **RNG-011-05:** Atendimento ao cliente — registrar interações e follow-up.
- **RNG-011-06:** Consultar clientes por rota/geolocalização para planejamento.

- **RNG-011-02a:** Agendar **janela de visita** por cliente: dia da semana, semanas do mês (1ª–5ª), datas Venix/próxima/livre, hora; representante Rep 1; regra de proximidade de data para envio ao representante.

## Validação legado — demo (2026-06-01)

Via **Cadastros → Clientes → Visitas** (C0082) — tela **Consulta de Visita**:

| Campo | Demo |
|-------|------|
| Representante | V0002 — TESTE |
| Dia | Segunda |
| Semanas | 1ª–5ª (desmarcadas) |
| Regra | Data mais próxima de 01/06/2026 |

Ver [RN-002](../requisitos-negocio/RN-002-cadastro-de-clientes.md) (RNG-002-17).

## Critérios de aceite de negócio (CANG)

- **CANG-011-01:** Dado visita registrada, quando relatório de visitas é gerado, então aparece no histórico do cliente.
- **CANG-011-02:** Dado cliente com janela de visita cadastrada, quando consulta de rota é feita, então horários sugeridos respeitam cadastro.

## Dúvidas em aberto

- [ ] Integração visita × pedido (geração automática de pedido na visita?).

## Referências legado

- Tabelas: `SCD_HVIS`, `SCD_MVIS`, `SCD_ATIV`, `SCD_HIST`
- Programas: `scd1vis.prg`, `scd1ati.prg`, `scd2ate.prg`
