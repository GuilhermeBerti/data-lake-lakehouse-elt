# 📊 Projeto Data Lake - IoT Sensors

## 📌 Visão Geral

Este projeto demonstra a implementação de um pipeline de dados simples
utilizando arquitetura de Data Lake na AWS.

O objetivo é ingerir um arquivo CSV com leituras de sensores IoT,
armazenar os dados no Amazon S3 e realizar consultas utilizando o
Dremio.

------------------------------------------------------------------------

## 🏗 Arquitetura do Projeto

### 🔄 Fluxo de Dados

1.  **Fonte de Dados**\
    Arquivo `iot_sensors_readings.csv` armazenado em um repositório no
    GitHub.

2.  **Ingestão de Dados**\
    O Airbyte realiza a conexão com o GitHub e envia os dados para o
    Amazon S3.

    ![Airbyte](https://raw.githubusercontent.com/GuilhermeBerti/GaleriaDataEngineer/main/airbyte.png)


3.  **Armazenamento (Data Lake)**\
    Bucket utilizado:

        datalake-guilherme-dev

    Estrutura de pastas:

        raw/
          └── iot_sensors_readings/
              └── iot_sensors_readings/
                  ├── 2026_02_11_1770773683516_0.csv
                  └── 2026_02_11_1770773322528_0.parquet

    ![Amazon S3](https://raw.githubusercontent.com/GuilhermeBerti/GaleriaDataEngineer/main/AmazonS3.png)

4.  **Consulta dos Dados**\
    O Dremio conecta diretamente ao S3 e consulta os arquivos no formato
    Parquet.

    ![Dremio](https://raw.githubusercontent.com/GuilhermeBerti/GaleriaDataEngineer/main/dremio.png)

------------------------------------------------------------------------

## 📂 Camada RAW

Os dados são armazenados na camada `raw`, mantendo o formato original da
ingestão.

Características:

-   Arquivos versionados por timestamp
-   Armazenamento em CSV e Parquet
-   Dados sem transformação
-   Estrutura organizada por dataset

Caminho no S3:

    s3://datalake-guilherme-dev/raw/iot_sensors_readings/iot_sensors_readings/

------------------------------------------------------------------------

## 📦 Formato dos Arquivos

-   **CSV**: formato original ingerido.
-   **Parquet**: formato colunar otimizado para consulta analítica.

O Parquet é utilizado para melhorar a performance das consultas
realizadas pelo Dremio.

------------------------------------------------------------------------

## 🔎 Exemplo de Consulta no Dremio

``` sql
SELECT *
FROM "datalake-s3"."datalake-guilherme-dev".raw."iot_sensors_readings"."iot_sensors_readings"."2026_02_11_1770773322528_0.parquet";
```

Essa consulta permite visualizar todos os registros armazenados no Data
Lake.

## 📊 Análise de Leituras por Status

A consulta abaixo realiza uma agregação dos dados armazenados no Data Lake, agrupando os registros pelo campo `status` dos sensores.

O objetivo é identificar a quantidade de leituras por status, contabilizando:

- Total de `sensor_id`
- Total de registros de `timestamp`

```sql
SELECT 
    status, 
    COUNT(sensor_id) AS Count_sensor_id, 
    COUNT("2026_02_11_1770773322528_0.parquet"."timestamp") AS Count_timestamp
FROM "datalake-s3"."datalake-guilherme-dev".raw.iot_sensors_readings.iot_sensors_readings."2026_02_11_1770773322528_0.parquet" AS "2026_02_11_1770773322528_0.parquet"
GROUP BY status;
```

Essa consulta permite analisar a distribuição dos estados dos sensores e validar a integridade dos dados ingeridos no Data Lake.

------------------------------------------------------------------------

## 🛠 Tecnologias Utilizadas

-   GitHub (Fonte de Dados)
-   Airbyte (Ingestão)
-   AWS S3 (Armazenamento)
-   Parquet (Formato de Dados)
-   Dremio (Consulta SQL)

------------------------------------------------------------------------

## 👨‍💻 Autor

Guilherme Berti\
Engenheiro de Dados
