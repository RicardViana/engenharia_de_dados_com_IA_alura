# Aviação ANAC — VoeBem Analytics

Materiais de estudo da turma da Imersão Engenharia de Dados — setembro/2026, com dados da ANAC, Python, SQL e Databricks.

A VoeBem Analytics é a consultoria fictícia usada no projeto: o objetivo é investigar atrasos, cancelamentos e pontualidade dos voos. Este repositório é uma iniciativa colaborativa da turma, sem vínculo oficial com a Alura.

## Primeira vez por aqui?

Siga o **[Comece aqui — guia para iniciantes](docs/comece-aqui.md)**: do download dos materiais à primeira consulta. O percurso é pelo navegador, com os dados incluídos e sem instalar Python ou Git no computador.

**Estado do material:** preparado para apoiar as aulas. A execução completa em um workspace novo ainda precisa ser validada.

## Estrutura

- `dados/`: 15 CSVs da ANAC incluídos no material original: 12 meses de VRA (agosto/2025 a julho/2026) e três cadastros de referência.
- `notebooks/`: ingestão Bronze, transformação Silver e governança Gold. Os arquivos `.py` estão no formato de notebooks Databricks.
- `pipelines/qualidade/`: regras de qualidade e quarentena para diagnóstico.
- `sql/gold/`: dimensão de aeroportos, fato de voos e tabela de consumo (OBT).
- `sql/gabarito/`: consultas para as perguntas de negócio.
- `scripts/`: download dos dados e utilitários opcionais de execução e Genie.
- `genie/`: instruções e configuração de exemplo para perguntas em linguagem natural.
- `docs/`: fontes, perguntas de negócio e registro de aceitação fornecido no material original.

## Preparação

A instalação local é necessária somente se você optar pelos scripts de terminal. Use Python 3.10 ou superior para esses scripts. Eles utilizam a biblioteca padrão. Os notebooks dependem do ambiente Spark/Databricks e não devem ser executados como scripts Python locais.

1. Baixe ou clone este repositório e abra um terminal na pasta do projeto.
2. Os CSVs já estão incluídos em `dados/`, portanto não é necessário baixá-los novamente. Para recuperar arquivos ausentes, execute `python scripts/baixar_anac.py`. O script usa a janela agosto/2025 a julho/2026, reaproveita arquivos existentes e atualiza `docs/fontes.md` com o registro do download. A disponibilidade de novos downloads depende do portal de origem.
3. Em seu ambiente de estudos Databricks, execute `sql/00_preparar_ambiente.sql`. É necessário ter permissão para criar catálogo, schemas e volume.
4. Envie o conteúdo de `dados/vra/` para `/Volumes/voebem/bronze/arquivos/vra/` e o conteúdo de `dados/referencias/` para `/Volumes/voebem/bronze/arquivos/referencias/`.
5. Importe os arquivos de `notebooks/` como notebooks Databricks.

O código usa o catálogo `voebem`. Se escolher outro nome, ajuste as referências nos notebooks, SQL e configuração do Genie. As cargas usam sobrescrita ou `CREATE OR REPLACE`: execute no ambiente destinado a este projeto.

## Sequência de estudo e execução

1. `notebooks/03_bronze_vra.py`
2. `notebooks/04_bronze_referencias.py`
3. `notebooks/05_silver_espelho.py`
4. Configure um pipeline de qualidade com os três arquivos de `pipelines/qualidade/`, catálogo `voebem` e schema `silver`, e execute-o no Databricks.
   Para usar `sql/metricas_qualidade.sql`, configure também a publicação do event log do pipeline na tabela `voebem.silver.eventos_qualidade`.
5. Execute `sql/gold/01_dim_aeroporto.sql`, `02_fato_voos.sql` e `03_obt_voos.sql`, nessa ordem.
6. Execute `notebooks/09_governanca_gold.py`.
7. Explore as consultas em `sql/gabarito/` e as perguntas em `docs/perguntas-de-negocio.md`.
8. Opcional: configure um espaço Genie com `voebem.gold.obt_voos`, usando os exemplos e instruções de `genie/`. O script `python scripts/montar_genie_space.py` regenera o JSON; ele não cria o espaço no serviço.

A numeração original foi preservada; os números ausentes não representam arquivos faltando neste pacote.

## Utilitários opcionais

Os scripts que acessam o Databricks exigem a CLI instalada e autenticada no seu próprio workspace. O perfil padrão é `alura-imersao`, substituível pela variável `DATABRICKS_CONFIG_PROFILE`.

Para consultar seu espaço Genie no PowerShell:

```powershell
$env:DATABRICKS_CONFIG_PROFILE = "alura-imersao"
$env:GENIE_SPACE_ID = "ID_DO_SEU_ESPACO"
python scripts/perguntar_genie.py "Quais aeroportos concentram os maiores atrasos?"
```

Os scripts `.sh` exigem Bash, como Git Bash ou WSL. Para `rodar_notebook.sh`, defina também `DATABRICKS_WORKSPACE_PATH` com a pasta dos notebooks no workspace. O script `rodar_pipeline.sh` recebe o ID do seu pipeline como primeiro argumento. As variáveis devem ser definidas no terminal em que o script será executado.

## Validação e contribuições

Compare as respostas do Genie com as consultas SQL. Os números e resultados em `docs/teste-aceitacao.md` são registros do material original; não são uma validação de uma execução no seu ambiente e podem variar com a atualização dos dados.

Para contribuir, descreva a alteração e como verificou o resultado em um pull request. Não inclua credenciais ou configurações pessoais. Ao alterar os CSVs, informe a fonte, o período e o motivo da atualização para preservar a reprodução dos exercícios.

## Créditos

Contexto educacional: Imersão Engenharia de Dados da Alura, setembro/2026. Fonte dos dados: ANAC, conforme `docs/fontes.md`. O código-base foi preservado a partir do material compartilhado da imersão, com ajustes de preparação para publicação. 

---

## Anotações 

### Aula 01 - Comece no Databricks e veja o que você vai construir

---

### Aula 02 - Domine a Ingestão de Dados Brutos 

Nesta aula:

- Entender a arquitetura medalhão (Bronze, Silver e Gold) e sua importância na governança de dados.

- Descobrir como utilizar o Genie (IA do Databricks) para acelerar o desenvolvimento de códigos e análises.

- Configurar catálogos, esquemas e volumes para organizar o seu Data Lake.

- Iniciar o projeto prático da VoeBem Analytics com dados reais da ANAC.

- Compreender os conceitos de Lakehouse, Delta Lake e a orquestração de pipelines automatizados.

- Implementar a camada Bronze da Arquitetura Medalhão, compreendendo seu papel na ingestão e organização dos dados em um pipeline

---

Links importantes:

- [Databricks](https://www.databricks.com/)

- [Base de Voos](https://www.gov.br/anac/pt-br/assuntos/dados-e-estatisticas/historico-de-voos) 

- [Arquivos do Dataset](https://github.com/alura-cursos/imers-o_engenhariadedados_ia)

- [Códigos da Imersão](https://github.com/Alura-Imersao-Engenharia-de-dados-2026/projeto-aviacao-anac)

---

Aprofundar nos seguintes tópicos:

[O que é um pipeline de dados?](https://www.alura.com.br/artigos/o-que-pipeline-dados)

[Engenharia de Dados](https://www.alura.com.br/artigos/engenharia-dados)

[Aplicações de SQL em diversas áreas](https://www.alura.com.br/artigos/aplicacoes-sql-diversas-areas)

[O que é a Arquitetura Medallion? | Databricks](https://www.databricks.com/br/blog/what-is-medallion-architecture)

---

### Aula 03 - Domine a Ingestão de Dados Brutos 

Nesta aula:

- Entender o papel da camada Silver na governança, qualidade e padronização dos dados.

- Aprender a tipar colunas, tratar valores nulos e calcular métricas derivadas, como o atraso real dos voos.

- Descobrir como unificar tabelas de diferentes fontes em uma única visão consolidada.

- Adicionar metadados de governança (data de carga, usuário e versão do pipeline) para rastreabilidade e auditoria.

- Conhecer a orquestração de pipelines via Jobs do Databricks e a governança com Unity Catalog.

---

### Aula 04 - Crie seu Agente de IA no Databricks

Nesta aula:

- Utilizar o Genie Code como especialista em governança para identificar tratamentos de qualidade nos dados da Silver.

- Construir um processo de Data Quality que impede o carregamento de dados inválidos, como códigos ICAO vazios.

- Conhecer o Spark Declarative Pipelines e suas três etapas: dados marcados, auditados e em quarentena.

- Aplicar constraints e expectations em SQL para validar regras de negócio automaticamente.
Modelar a camada Gold utilizando o conceito de One Big Table (OBT), unindo fatos e dimensões em uma única tabela.

- Explorar rastreabilidade e linhagem de dados através do Unity Catalog, incluindo o registro de eventos de qualidade.

---

Aprofundar nos seguintes tópicos:

- [Storytelling com dados: transforme seus dados em narrativas envolventes](https://www.alura.com.br/artigos/storytelling-com-dados)

- [Transformando dados em insights: como criar um relatório baseado em análises de dados](https://www.alura.com.br/artigos/transformar-dados-em-insights)

---

### Aula 05 - Publique seu pipeline e desbrave sua Carreira em Dados com IA
