# Diálogos e validações

Alertas, mensagens de erro e confirmações isoladas — úteis para documentar **CANG** e regras de validação.

## Convenção de nome

```text
RN-{NNN}-dialog-{slug}.png
```

Exemplos:

| Arquivo | Descrição |
|---------|-----------|
| `RN-002-dialog-bloqueio-motivo.png` | Alerta ao bloquear sem motivo |
| `RN-004-dialog-estoque-insuficiente.png` | Validação na digitação do item |
| `RN-001-dialog-emitente-obrigatorio.png` | “Selecionar ou Cadastrar emitente” |

## Quando capturar

- Mensagem exata exibida ao usuário (texto para RNG/CANG)
- Estado de erro reproduzível no demo
- Confirmações destrutivas (Excluir, Cancelar pedido)

Referenciar no RN a seção **Mensagens do sistema** e linkar o PNG.
