# Projeto Databricks - Pipeline FX USD/BRL

Este repositório contém notebooks organizados em camadas **Bronze → Silver → Gold**, seguindo boas práticas de Data Lakehouse no Databricks.

---

## 📂 Estrutura de Notebooks

### 1. `01_ingest_fx_usdbrl_bronze.ipynb`
- **Objetivo:** Ingestão da taxa USD/BRL a partir de API pública.  
- **Camada:** Bronze (append-only, histórico bruto).  
- **Principais pontos:**
  - Consumo da API `frankfurter.app`.
  - Criação de partição de ingestão (`ingestion_epoch`).
  - Escrita no Delta Lake em modo **append**.

---

### 2. `02_transform_fx_usdbrl_silver.ipynb`
- **Objetivo:** Padronizar e limpar os dados da camada Bronze.  
- **Camada:** Silver (dados tratados e consistentes).  
- **Principais pontos:**
  - Deduplicação e normalização dos campos.
  - Conversão de tipos (date, decimal).
  - Garantia de unicidade por `date`.

---

### 3. `03_gold_fx_usdbrl_dashboard.ipynb`
- **Objetivo:** Gerar visão analítica pronta para consumo.  
- **Camada:** Gold (camada de negócio).  
- **Principais pontos:**
  - Cálculo de estatísticas (média, mínimo, máximo).
  - Agregações temporais (por mês/ano).
  - Exportação para visualização e dashboards.

---

## 🔄 Fluxo do Pipeline

```text
API → Bronze (raw, append-only) → Silver (curado) → Gold (analítico)
