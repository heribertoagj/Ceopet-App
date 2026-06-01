---
name: especificar-requisito-tecnico
description: >-
  Deriva requisitos técnicos (RT) a partir dos RN do Ceopet — TransformaTech (TTech) com modelo de dados,
  API, estados e critérios testáveis CAT. Use após RN validado, especificação técnica,
  RT-XXX, ou antes de planejar/implementar.
disable-model-invocation: true
---

# Especificar requisito técnico — Ceopet — TransformaTech (TTech)

## Pré-requisitos

- Existe `Documentação/requisitos-negocio/RN-XXX-*.md` com regras e CANG.
- Ler `RT-000-principios-tecnicos.md` para alinhamento transversal.

## Artefatos

- **Saída:** `Documentação/requisitos-tecnicos/RT-XXX-titulo.md` (ou `RTNF-XXX`)
- **Template:** `Documentação/requisitos-tecnicos/_TEMPLATE-RT.md`

## Fluxo

1. Abrir RN-XXX correspondente.
2. Mapear cada `CANG-*` → `CAT-*` testável (Dado/Quando/Então).
3. Mapear cada `RNG-*` → regra técnica ou validação na API.
4. Preencher: componentes, modelo, API, estados, permissões, dependências RT.
5. Status do RT → **Especificado** quando CAT cobrirem o RN.
6. Atualizar `Documentação/README.md`.
7. Sugerir `planejar-implementacao` no RT.

## Regras

- Todo RT referencia o RN no cabeçalho.
- Alteração de regra de negócio: alterar RN primeiro, depois RT.
- Não implementar código nesta etapa.
- Integrações: documentar MVP (mock) vs alvo.

## Substitui

A skill `especificar-feature` (legado `Documentação/specs/`) — preferir RN/RT.
