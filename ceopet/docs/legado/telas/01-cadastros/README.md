# Cadastros

Menu principal **Cadastros** do Hidra. Escopo MVP B2B-1: RN-001, RN-002, RN-003, RN-009, RN-015 ([RN-CAD-001](../../requisitos-negocio/RN-CAD-001-cadastros-mvp-consolidado.md)).

| Pasta | Menu Hidra | RN |
|-------|------------|-----|
| [01-emitentes](01-emitentes/) | Cadastros → Emitentes | RN-001 |
| [02-usuarios-acessos](02-usuarios-acessos/) | Cadastros → Usuários / Acessos | RN-001 |
| [03-clientes](03-clientes/) | Cadastros → Clientes | RN-002 |
| [04-produtos](04-produtos/) | Cadastros → Produtos | RN-003 |
| [05-produtos-auxiliares](05-produtos-auxiliares/) | Cadastros → Produtos (submenus) | RN-003 |
| [06-fornecedores](06-fornecedores/) | Cadastros → Fornecedores | RN-015 |
| [07-representantes](07-representantes/) | Cadastros → Representantes | RN-009 |
| [08-transportadoras](08-transportadoras/) | Cadastros → Transportadoras | — |
| [09-bancos](09-bancos/) | Cadastros → Bancos | RN-001 / RN-008 |
| [10-cfop](10-cfop/) | Cadastros → Codigos Fiscais CFOP | RN-007 |
| [11-condicoes-pagamento](11-condicoes-pagamento/) | Cadastros → Condições de pagamento | RN-004 |

## Padrão transversal (G2)

Todos os cadastros principais (cliente, produto, representante, fornecedor) seguem:

1. Lista com pesquisa + grid + painel detalhe
2. Barra **Incluir · Alterar · Excluir · Consulta**
3. Modal **Informação do …** no Incluir/Alterar

Capturar pelo menos: **lista**, **incluir**, **alterar (demo)**, **consulta** (se layout distinto).
