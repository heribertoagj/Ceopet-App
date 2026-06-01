# Levantamento — Sistema Hidra (legado Ceopet)

Histórico do mapeamento inicial para modernização tecnológica. Especificações formais estão nos RNs; este arquivo registra **como** o conhecimento foi obtido e **o que falta validar**.

## Objetivo do projeto

Documentar o sistema **Hidra** (ERP legado em xHarbour) do **cliente Ceopet** para permitir a atualização tecnológica da operação de distribuição veterinária B2B, preservando regras de negócio essenciais. Levantamento e documentação conduzidos pela **TransformaTech (TTech)**.

## Fontes analisadas

| Fonte | Local | O que revelou |
|-------|-------|---------------|
| Executável | `Documentacao/Hidra/scd.exe` | Menus, mensagens, referências a 138 arquivos `.prg` |
| Bases DBF | `Documentacao/Hidra/*.DBF` | 37 tabelas, schemas e dados demo Ceopet |
| Log de erro | `Documentacao/Hidra/Error.log` | Stack xHarbour, módulos `scd.prg`, `scd1emi.prg` |
| Config DBU | `Documentacao/Hidra/EMAGDBU.INI` | RDD DBFCDX, histórico de arquivos abertos |
| Plugin boletos | `Documentacao/Hidra/Boleto/bplugin/` | Emissão bancária separada |
| Pastas operacionais | `Remessa/`, `Retorno/`, `Saidatxt/`, `Lixeira/` | Integração CNAB / arquivos fiscais |
| Script de análise | `Documentacao/_tools/setup_hidra.ps1` | Setup local em `C:\Hidra` para executar o legado |

## O que **não** estava disponível

- Código-fonte `.prg` descompactado (presente apenas como referência dentro do EXE; arquivo `Hidra.rar` no diretório não foi extraído — falta ferramenta RAR no ambiente).
- Manual de usuário ou documentação original do fornecedor.
- Ambiente em execução com usuários-chave para validação de fluxos.

## Domínio identificado

- **Segmento:** distribuição B2B de produtos veterinários / pet shop atacado.
- **Operador demo:** CEOPET Consultoria / EXACTA (dados em `SCD_EMIT`).
- **Clientes:** distribuidores, clínicas — cadastro com veterinários, CRMV, rotas e visitas.
- **Canal:** desktop interno, representantes comerciais, PDA de campo, integrações (e-mail, mapas, Venix/Accera).

## Metodologia aplicada

1. Inventário de arquivos e tipos no diretório legado.
2. Extração de strings de menu do executável (`scd.exe`).
3. Leitura de schema e amostra de registros DBF (Python + `dbfread`).
4. Agrupamento funcional por prefixo de programas (`scd1*` cadastro, `scd2*` operação, `scd3*` relatórios).
5. Derivação de RNs por módulo de negócio, marcando incertezas como dúvidas.

## Principais descobertas

- Sistema **multi-emitente** (campo emitente em todas as entidades principais).
- Ciclo comercial completo: pedido → liberação/bloqueio → separação → romaneio → NFe → boleto.
- Estoque com lotes, pendências e consolidação.
- Forte componente de **representantes** (metas, comissão, positivação, supervisores).
- Fiscal brasileiro: NCM, CFOP, NFe, importação XML.
- Financeiro: boletos com remessa/retorno, cheques, contas a pagar.

## Próximas validações recomendadas

1. Extrair `Hidra.rar` e incluir fontes no repositório (ou pasta `Documentacao/Hidra/fontes/`).
2. Sessão com usuário de negócio Ceopet para validar RNs 001–014.
3. Confirmar quais módulos entram na **fase 1** da modernização vs. descontinuados.
4. Definir se PDA/Venix/Accera permanecem no escopo do novo sistema.
5. Priorizar RTs a partir dos RNs validados.

## Execução local do Hidra (validado 2026-06-01)

### Resultado

| Item | Resultado |
|------|-----------|
| scd.exe em C:\Hidra | **Abre** — tela Identificação do Usuário (login) |
| Erro antigo (Alias does not exist: emit) | Causa: faltava ind_emit.cdx na cópia inicial |
| Hidra.rar | Contém ind_emit.cdx e pastas Remessa, Retorno, Lixeira, Saidatxt |
| OneDrive | Evitar executar direto de Documentacao/Hidra — usar C:\Hidra |

### Procedimento

powershell -ExecutionPolicy Bypass -File Documentacao\_tools\setup_hidra.ps1
cd C:\Hidra
.\scd.exe

### Login demo (SCD_SENH.DBF)

- ROBINSON (admin demo)
- SEPARADOR (expedição demo)

Senhas: confirmar com cliente Ceopet.

### Artefatos no repositório

- Documentacao/Hidra/ind_emit.cdx
- Pastas Remessa/, Retorno/, Lixeira/, Saidatxt/
- Documentacao/_tools/setup_hidra.ps1

## Histórico

| Data | Evento |
|------|--------|
| 2026-06-01 | Mapeamento inicial automatizado + estrutura RN criada a partir de `Documentacao/Hidra/` |
| 2026-06-01 | Roteiro de exploração guiada (legado/roteiro-exploracao-hidra.md) |
| 2026-06-01 | Setup `C:\Hidra` — RAR extraído, `ind_emit.cdx` corrigido, `scd.exe` abre tela de login |
