# Telas fora do menu principal

Telas acessadas por **botão**, **atalho**, **histórico** ou **contexto** — não por item direto do menu raiz.

Organizar em subpastas por entidade ou função:

```text
_fora-do-menu/
├── clientes-historico/          ← Bloquear/Liberar via Histórico (RN-002)
├── clientes-integracao/         ← Clientes Novos, Liberados (Cadastros → Integração)
├── pedidos-contexto/            ← Ações contextuais em pedidos
└── …
```

## Convenção de nome

Mesmo padrão RN: `RN-002-14-historico-bloqueio.png`

## Quando usar

| Situação | Exemplo |
|----------|---------|
| Menu real ≠ pasta “óbvia” | Clientes Chrome → Relatórios, não Cadastros |
| Modal sobre outra tela | Consulta financeira a partir da lista |
| Ação no rodapé | Baixa título, Juros |

Documentar no README da pasta **origem** o caminho alternativo e linkar para cá.
