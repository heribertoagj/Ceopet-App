---
name: sincronizar-documentacao
description: >-
  Mantém README, RN-000, decisoes.md e índice RN/RT do Ceopet — TransformaTech (TTech) consistentes.
  Use após mudança de escopo ou conclusão de requisitos.
disable-model-invocation: true
---

# Sincronizar documentação — Ceopet — TransformaTech (TTech)

## Arquivos oficiais

| Arquivo | Papel |
|---------|--------|
| `Documentação/README.md` | Índice RN + RT |
| `Documentação/requisitos-negocio/RN-000-*.md` | Características do negócio |
| `Documentação/requisitos-negocio/RN-XXX-*.md` | Requisitos de negócio |
| `Documentação/requisitos-tecnicos/RT-XXX-*.md` | Requisitos técnicos |
| `Documentação/levantamento.md` | Histórico — não duplicar specs |
| `Documentação/decisoes.md` | ADRs |

## Regra RN ↔ RT

- Mesmo número: RN-005 ↔ RT-005.
- Status independente: RN pode estar **Validado negócio** enquanto RT está **Rascunho**.

## Status

**RN:** Rascunho | Validado negócio | Obsoleto  
**RT:** Pendente | Rascunho | Especificado | Implementado
