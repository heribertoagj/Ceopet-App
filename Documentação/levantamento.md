# Levantamento � Sistema Hidra (legado Ceopet)

Hist�rico do mapeamento inicial para moderniza��o tecnol�gica. Especifica��es formais est�o nos RNs; este arquivo registra **como** o conhecimento foi obtido e **o que falta validar**.

## Objetivo do projeto

Documentar o sistema **Hidra** (ERP legado em xHarbour) do **cliente Ceopet** para permitir a atualiza��o tecnol�gica da opera��o de distribui��o veterin�ria B2B, preservando regras de neg�cio essenciais. Levantamento e documenta��o conduzidos pela **TransformaTech (TTech)**.

## Fontes analisadas

| Fonte | Local | O que revelou |
|-------|-------|---------------|
| Execut�vel | `Documentacao/Hidra/scd.exe` | Menus, mensagens, refer�ncias a 138 arquivos `.prg` |
| Bases DBF | `Documentacao/Hidra/*.DBF` | 37 tabelas, schemas e dados demo Ceopet |
| Log de erro | `Documentacao/Hidra/Error.log` | Stack xHarbour, m�dulos `scd.prg`, `scd1emi.prg` |
| Config DBU | `Documentacao/Hidra/EMAGDBU.INI` | RDD DBFCDX, hist�rico de arquivos abertos |
| Plugin boletos | `Documentacao/Hidra/Boleto/bplugin/` | Emiss�o banc�ria separada |
| Pastas operacionais | `Remessa/`, `Retorno/`, `Saidatxt/`, `Lixeira/` | Integra��o CNAB / arquivos fiscais |
| Script de an�lise | `Documentacao/_tools/setup_hidra.ps1` | Setup local em `C:\Hidra` para executar o legado |

## O que **n�o** estava dispon�vel

- C�digo-fonte `.prg` descompactado (presente apenas como refer�ncia dentro do EXE; arquivo `Hidra.rar` no diret�rio n�o foi extra�do � falta ferramenta RAR no ambiente).
- Manual de usu�rio ou documenta��o original do fornecedor.
- Ambiente em execu��o com usu�rios-chave para valida��o de fluxos.

## Dom�nio identificado

- **Segmento:** distribui��o B2B de produtos veterin�rios / pet shop atacado.
- **Operador demo:** CEOPET Consultoria / EXACTA (dados em `SCD_EMIT`).
- **Clientes:** distribuidores, cl�nicas � cadastro com veterin�rios, CRMV, rotas e visitas.
- **Canal:** desktop interno, representantes comerciais, PDA de campo, integra��es (e-mail, mapas, Venix/Accera).

## Metodologia aplicada

1. Invent�rio de arquivos e tipos no diret�rio legado.
2. Extra��o de strings de menu do execut�vel (`scd.exe`).
3. Leitura de schema e amostra de registros DBF (Python + `dbfread`).
4. Agrupamento funcional por prefixo de programas (`scd1*` cadastro, `scd2*` opera��o, `scd3*` relat�rios).
5. Deriva��o de RNs por m�dulo de neg�cio, marcando incertezas como d�vidas.

## Principais descobertas

- Sistema **multi-emitente** (campo emitente em todas as entidades principais).
- Ciclo comercial completo: pedido ? libera��o/bloqueio ? separa��o ? romaneio ? NFe ? boleto.
- Estoque com lotes, pend�ncias e consolida��o.
- Forte componente de **representantes** (metas, comiss�o, positiva��o, supervisores).
- Fiscal brasileiro: NCM, CFOP, NFe, importa��o XML.
- Financeiro: boletos com remessa/retorno, cheques, contas a pagar.

## Pr�ximas valida��es recomendadas

1. Extrair `Hidra.rar` e incluir fontes no reposit�rio (ou pasta `Documentacao/Hidra/fontes/`).
2. Sess�o com usu�rio de neg�cio Ceopet para validar RNs 001�014.
3. Confirmar quais m�dulos entram na **fase 1** da moderniza��o vs. descontinuados.
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

## Hist�rico

| Data | Evento |
|------|--------|
| 2026-06-01 | Mapeamento inicial automatizado + estrutura RN criada a partir de `Documentacao/Hidra/` |
| 2026-06-01 | Roteiro de exploração guiada (legado/roteiro-exploracao-hidra.md) |
| 2026-06-01 | Setup `C:\Hidra` � RAR extra�do, `ind_emit.cdx` corrigido, `scd.exe` abre tela de login |
