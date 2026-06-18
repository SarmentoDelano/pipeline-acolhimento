# Pipeline de Tratamento de Dados de Acolhimento

Este repositório contém um notebook em Python/Google Colab para tratamento, padronização, consolidação e validação de planilhas de acolhimento.

A pipeline foi criada para receber bases atualizadas de acolhimento, aplicar os tratamentos necessários e gerar uma planilha final padronizada para análise, controle e uso em dashboards.

## Objetivo

Automatizar o tratamento das planilhas de acolhimento, evitando retrabalho manual e garantindo que os dados finais estejam organizados, padronizados e prontos para uso.

A pipeline realiza:

* leitura das bases de acolhimento;
* padronização dos nomes das crianças;
* preservação da situação original;
* criação da coluna `Idade (anos)`;
* separação do órgão julgador em município e vara;
* consolidação das informações das duas bases;
* inserção do `ID Instituição`;
* auditorias e validações;
* exportação da planilha final formatada.

## Arquivos de entrada

A pipeline espera encontrar os seguintes arquivos na mesma pasta do notebook:

```text
acolhimento.xlsx
acolhimento2.xlsx
INSTITUICOES_PIPELINE.xlsx
```

### Descrição dos arquivos

| Arquivo                      | Descrição                                                                  |
| ---------------------------- | -------------------------------------------------------------------------- |
| `acolhimento.xlsx`           | Base principal de acolhimento, usada como referência principal da pipeline |
| `acolhimento2.xlsx`          | Segunda base de acolhimento, usada para complementar dados                 |
| `INSTITUICOES_PIPELINE.xlsx` | Base de instituições, usada para buscar o `ID Instituição`                 |

## Arquivo de saída

Ao final da execução, a pipeline gera o arquivo:

```text
PLANILHA_FINAL_PIPELINE_TRATADA.xlsx
```

O arquivo final contém a planilha consolidada e abas de auditoria/validação.

## Principais tratamentos realizados

### 1. Padronização da coluna Criança

A pipeline identifica colunas como:

```text
criança
Criança
nome
Nome
```

E padroniza para:

```text
Criança
```

Também remove números indevidos nos nomes e aplica capitalização.

### 2. Situação

A coluna `Situação` é mantida da mesma forma que vem nas planilhas originais.

A pipeline não separa mais a coluna `Situação` e não cria a coluna `Detalhe`.

### 3. Idade

A coluna original `Idade` é mantida.

Além dela, a pipeline cria a coluna:

```text
Idade (anos)
```

Regras aplicadas:

| Valor original    | Valor em `Idade (anos)` |
| ----------------- | ----------------------- |
| `14 anos`         | `14`                    |
| `1 ano e 3 meses` | `1`                     |
| `8 meses`         | `0`                     |
| `15 dias`         | `0`                     |
| vazio             | vazio                   |

### 4. Separação do Órgão Julgador

A coluna `Órgão Julgador` é separada conforme a origem da base.

Para a base `acolhimento.xlsx`:

```text
Município Medida Protetiva
Vara Medida Protetiva
```

Para a base `acolhimento2.xlsx`:

```text
Município Destituição
Vara Destituição
```

Após a separação, a coluna `Órgão Julgador` é removida.

### 5. Tempo de Acolhimento

A pipeline mantém a coluna original:

```text
Tempo de Acolhimento
```

E cria a coluna:

```text
Tempo de Acolhimento (anos)
```

Regras aplicadas:

| Valor original         | Valor convertido |
| ---------------------- | ---------------- |
| `4 ano(s), 2 mês(es)`  | `4`              |
| `1 ano(s), 3 mês(es)`  | `1`              |
| `9 mês(es), 28 dia(s)` | `0`              |
| `15 dia(s)`            | `0`              |
| vazio                  | vazio            |

### 6. ID Instituição

A pipeline cria a coluna:

```text
ID Instituição
```

O ID é buscado na planilha:

```text
INSTITUICOES_PIPELINE.xlsx
```

A identificação considera:

* nome da instituição;
* município;
* vara;
* regras específicas para instituições regionais;
* mapa regional quando necessário.

A coluna auxiliar `Método ID Instituição` é removida antes da exportação final.

## Estrutura do notebook

O notebook está organizado em células sequenciais:

```text
Célula 1  - Imports e verificação dos arquivos na pasta
Célula 2  - Configurações dos nomes dos arquivos
Célula 3  - Funções auxiliares gerais
Célula 4  - Leitura das planilhas
Célula 5  - Padronização de colunas, nomes e órgão julgador
Célula 6  - Auditoria de nomes parecidos
Célula 7  - Mesclagem segura dos acolhimentos 1 e 2
Célula 8  - Preparação da base final de acolhimento
Célula 9  - Consolidação da base final
Célula 10 - Tratamento da coluna Situação
Célula 11 - Correção de Idade (anos)
Célula 12 - Conferência das colunas de município e vara
Célula 13 - Capitalização das colunas de município
Célula 14 - Criação de Tempo de Acolhimento (anos)
Célula 15 - Remoção de colunas auxiliares
Célula 15.1 - Inserção do ID Instituição
Célula 15.2 - Correção de IDs de unidades regionais
Célula 15.3 - Remoção da coluna Método ID Instituição
Célula 16 - Validações e auditorias
Célula 17 - Exportação final formatada
```

## Como usar

1. Abra o notebook no Google Colab.
2. Coloque os arquivos de entrada na mesma pasta do notebook.
3. Execute as células na ordem.
4. Ao final, baixe o arquivo gerado:

```text
PLANILHA_FINAL_PIPELINE_TRATADA.xlsx
```

## Cuidados importantes

Não altere os nomes dos arquivos de entrada sem atualizar a célula de configuração.

Os arquivos esperados são:

```text
acolhimento.xlsx
acolhimento2.xlsx
INSTITUICOES_PIPELINE.xlsx
```

Também é recomendado não publicar bases reais de dados no GitHub, especialmente se houver informações sensíveis.

## Sugestão de `.gitignore`

Para evitar o envio de planilhas e arquivos sensíveis ao GitHub, recomenda-se criar um arquivo `.gitignore` com:

```gitignore
*.xlsx
*.xls
*.csv
*.pdf
.ipynb_checkpoints/
__pycache__/
```

## Tecnologias utilizadas

* Python
* Pandas
* OpenPyXL
* Google Colab

## Status

Pipeline em uso e preparada para receber novas bases de acolhimento, mantendo os tratamentos padronizados e a geração automática da planilha final.
