---
name: especificar-feature
description: >-
  Legado: preferir especificar-requisito-tecnico para RT a partir de RN. Use apenas
  se o usuário citar especificar-feature ou pasta specs/ explicitamente.
disable-model-invocation: true
---

# Especificar feature (legado)

**Use em vez desta skill:** `especificar-requisito-tecnico`

Artefatos atuais:

- Negócio: `Documentação/requisitos-negocio/RN-XXX.md`
- Técnico: `Documentação/requisitos-tecnicos/RT-XXX.md`

Se o usuário pedir `especificar-feature` ou `RF-XXX`, mapear para o mesmo número em RN/RT e seguir o fluxo de `especificar-requisito-tecnico`.
