# Ingestão de Dados: Balanços Internacionais - Argentina

Este repositório contém o pipeline de Engenharia de Dados responsável pela captura automatizada e estruturação dos dados de produção de gás natural das bacias sedimentares da Argentina.

O projeto faz parte da arquitetura de dados baseada em **Lakehouse (Medalhão)** no **Microsoft Fabric**, processando e consolidando as informações na camada **Bronze**.

## 📋 Informações do Script
* **Notebook:** `bronze__ingerir__balancos_internacionais_argentina`
* **Camada:** Bronze
* **Autor:** Annara Myrella Moura da Silva Sousa
* **Data de Criação:** 18 de setembro de 2026

## 🎯 Objetivo
Realizar a ingestão dos dados de produção acumulada de gás natural das bacias sedimentares alvo a partir de **2019 até o período corrente**. O pipeline garante uma atualização incremental contínua na camada Bronze à medida que novos registros são publicados na origem, acoplando metadados técnicos para garantir total rastreabilidade.


## ⚙️ Bacias Monitoradas
O pipeline filtra e consolida dados unicamente de gás natural para as 5 bacias principais:
1. **AUSTRAL**
2. **GOLFO SAN JORGE**
3. **NEUQUINA**
4. **NOROESTE**
5. **CUYANA**

## 🔄 Fluxo de Dados (I/O)

### 📥 Entradas (Origem)
* **Fonte:** Secretaría de Energía da República Argentina
* **Tecnologia:** API CKAN SQL (`datastore_search_sql`)
* **ID do Recurso:** `876b3746-85e2-4039-adeb-b1354436159f`

### 📤 Saídas (Destino)
O processo resulta em dois formatos de armazenamento no Fabric Lakehouse (`datalake_bronze_dgn_v01`):
1. **Tabela Delta (Managed Table):**
   * `datalake_bronze_dgn_v01.balancos_internacionais.producao_gas_natural_bacias_argentina`
2. **Backup Físico (CSV):**
   * `Files/Arquivos_brutos/argentina/producao_gas_natural_bacias_argentina/producao_gas_natural_bacias_argentina.csv`

## 🛡️ Metadados de Rastreabilidade adicionados
Durante o processamento das respostas JSON da API, o script injeta colunas de controle técnica no DataFrame final para governança de dados:
* `dt_ingestao`: Timestamp UTC do momento exato em que a coleta rodou.
* `fonte`: Identificador do provedor (`secretaria_energia_argentina`).
* `canal_ingestao`: Identificador técnico do mecanismo (`api_ckan_datastore_search_sql`).
* `url_origem`: Endereço de endpoint utilizado na requisição `requests`.

## 🛠️ Tecnologias Utilizadas
* **Linguagem:** Python / PySpark (Ambiente Microsoft Fabric Synapse)
* **Bibliotecas Principais:** `requests`, `pandas`, `pyspark.sql`
* **Utilitários Fabric:** `mssparkutils` para manipulação de arquivos do File System (ABFSS).
