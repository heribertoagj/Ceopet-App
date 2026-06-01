---
name: planejar-implementacao
description: >-
  Gera plano de implementação no RT do Ceopet — TransformaTech (TTech) a partir de requisitos técnicos.
  Use para planejamento, backlog técnico ou antes de implementar RT-XXX.
disable-model-invocation: true
---

# Planejar implementação — Ceopet — TransformaTech (TTech)

## Pré-requisitos

- `Documentação/requisitos-tecnicos/RT-XXX-*.md` com status **Especificado** (ou RN validado + RT em elaboração).
- Ler RN vinculado para não planejar fora do negócio.

## Fluxo

1. Abrir RT-XXX e listar **CAT-***.
2. Adicionar seção **Plano de implementação** no **RT** (não no RN).
3. Ordenar: dados → API → admin → app → integrações → testes.
4. Mapear mocks (ERP, pagamento) conforme RTNF.
5. Não codar nesta etapa.

## Template (no RT)

```markdown
## Plano de implementação
### Ordem sugerida
1. [ ] ...
### Tarefas
| # | Tarefa | Camada | Tamanho |
### Definition of Done
- [ ] Todos CAT-XX do RT
- [ ] CANG-XX do RN verificados
```

## Regras

- Implementação referencia **RT**, validação referencia **RN + RT**.
