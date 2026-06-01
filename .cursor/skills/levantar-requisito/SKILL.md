---
name: levantar-requisito
description: >-
  Levanta requisitos de negócio (RN) do Ceopet — TransformaTech (TTech) com características do domínio
  mapeadas. Use para requisito, necessidade, levantamento, RN, RF, nova demanda
  antes da especificação técnica.
disable-model-invocation: true
---

# Levantar requisito de negócio — Ceopet — TransformaTech (TTech)

## Artefatos

- **Saída:** `Documentação/requisitos-negocio/RN-XXX-titulo.md`
- **Template:** `Documentação/requisitos-negocio/_TEMPLATE-RN.md`
- **Contexto global:** `Documentação/requisitos-negocio/RN-000-visao-e-caracteristicas-negocio.md`
- **Índice:** `Documentação/README.md`

Não escrever API, tabelas ou código nesta etapa — isso é `especificar-requisito-tecnico`.

## Fluxo

1. Ler `levantamento.md`, RN-000 e RN existentes.
2. Atribuir `RN-XXX` (funcional) ou `RNNF-XXX` (não funcional).
3. Preencher seção **Característica do negócio mapeada** (atores, canal, objeto, regra comercial, estoque).
4. Definir `RNG-*` e `CANG-*` (critérios de aceite de negócio).
5. Listar dúvidas; não inventar regras críticas sem base.
6. Atualizar índice em `Documentação/README.md`.
7. Sugerir: `especificar-requisito-tecnico` para o mesmo número (`RT-XXX`).

## Regras

- Um RN por arquivo.
- Mesmo número que o antigo RF (`RF-009` → `RN-009`).
- Fase: `B2B-1` | `B2C-2` | `ambos`.
- Português.

## Referência domínio

Ver RN-000 e `Documentação/levantamento.md`.
