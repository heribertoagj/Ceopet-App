# Decisões de arquitetura (ADR)

Registro de decisões do projeto de modernização do sistema Hidra — **cliente Ceopet**, **TransformaTech (TTech)**.

| ID | Data | Decisão | Status |
|----|------|---------|--------|
| ADR-001 | 2026-06-01 | Documentação de requisitos deriva do legado Hidra via engenharia reversa (DBF + EXE), não de especificação greenfield | Aceita |
| ADR-002 | 2026-06-01 | Requisitos de negócio (RN) precedem requisitos técnicos (RT) conforme fluxo do repositório | Aceita |
| ADR-003 | 2026-06-01 | Ceopet é o cliente; TransformaTech (TTech) é a consultora — sem referências a Via&Cia | Aceita |

## ADR-001 — Fonte da verdade para requisitos legados

**Contexto:** Não há manual nem fontes `.prg` versionados inicialmente.

**Decisão:** Usar inventário em `Documentacao/_tools/hidra_inventory.json`, mapas em `Documentação/legado/` e validação iterativa com stakeholders.

**Consequências:** RNs nascem como **Rascunho** até confirmação; regras críticas ficam em "Dúvidas em aberto".

## ADR-002 — Fluxo RN → RT

**Decisão:** Manter numeração alinhada (RN-005 ↔ RT-005) e separar status de negócio vs. técnico.

**Consequências:** Implementação só após RT especificado; negócio pode validar RN antes do RT.

## ADR-003 — Partes do projeto

**Contexto:** Ceopet e Via&Cia são organizações distintas. Este repositório trata exclusivamente do legado Hidra do **cliente Ceopet**.

**Decisão:** Documentação e entregas creditam **TransformaTech (TTech)** como consultoria e **Ceopet** como cliente.

**Consequências:** PDFs, README e RNs seguem essa nomenclatura; não referenciar Via&Cia neste projeto.
