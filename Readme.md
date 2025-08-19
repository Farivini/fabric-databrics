📘 Projeto FX USD/BRL – Data Lakehouse
📂 Estrutura de Notebooks

Este repositório contém três notebooks principais que implementam o fluxo Bronze → Silver → Gold seguindo a arquitetura de camadas Delta Lake:

01_ingest_fx_usdbrl_bronze.ipynb

Camada Bronze (Raw)

Objetivo: ingestão de dados de câmbio (USD/BRL) a partir de API externa.

Características:

Estrutura append-only (dados imutáveis, sem updates/deletes).

Armazenamento em Delta Table.

Conexão com Azure Data Lake Storage (ADLS) via SAS Token ou Secret Scope.

02_transform_fx_usdbrl_silver.ipynb

Camada Silver (Curated/Enriched)

Objetivo: padronizar, limpar e enriquecer os dados de câmbio.

Principais transformações:

Conversão de tipos.

Normalização de schema.

Deduplicação de registros.

Estrutura de merge incremental (upsert) com Delta Lake.

03_gold_fx_usdbrl_dashboard.ipynb

Camada Gold (Analytics/BI)

Objetivo: disponibilizar dataset pronto para consumo em dashboards.

Características:

Dataset final agregado por dia.

Preparado para ser consumido em ferramentas de BI (Power BI, Fabric Lakehouse, etc).

🔄 Fluxo de Dados
flowchart LR
    A[API FX Provider] -->|Ingestão| B[Bronze]
    B -->|Limpeza/Transformação| C[Silver]
    C -->|Agregação/Curadoria| D[Gold]
    D -->|Consumo| E[Dashboard / Power BI]

⚙️ Execução

Configure os segredos:

Usando Databricks Secret Scope ou Azure Key Vault para armazenar tokens de acesso (recomendado).

Exemplo de uso no notebook:

sas = dbutils.secrets.get(scope="storage-secrets", key="sas-token")


Execute os notebooks na ordem:

01_ingest_fx_usdbrl_bronze.ipynb

02_transform_fx_usdbrl_silver.ipynb

03_gold_fx_usdbrl_dashboard.ipynb

Verifique no Data Explorer / OneLake / ADLS se as tabelas Delta foram criadas corretamente.

📊 Estrutura de Tabelas

Bronze: fx_usdbrl_raw (dados brutos, append-only).

Silver: fx_usdbrl_clean (dados deduplicados e normalizados).

Gold: fx_usdbrl_dashboard (dataset final pronto para análise).

🚀 Próximos Passos

Automatizar execução via Azure Data Factory ou Fabric Pipelines.

Implementar testes de qualidade de dados (DQ checks).

Configurar alertas e monitoramento em caso de falha de ingestão.
