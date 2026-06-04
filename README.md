# 📊 Business Intelligence Dashboard — Corporate Registry Analysis

**End-to-end BI project** | SQL dimensional modeling + Power BI visualization  
**Dataset:** 460,000+ registered companies | **Tools:** SQLite · Power BI · DAX

---

## 🗂️ Project Overview

This project analyzes commercial registry data from Colombian companies to uncover revenue patterns, enrollment trends, and business segmentation insights across sectors, geographic regions, and legal entity types.

Built as part of the **Big Data & Data Analytics Diploma** at Fundación Universitaria San José (2026).

---

## 📸 Dashboard Preview

![Corporate Registry Dashboard](dashboard_preview.png)

**Key KPIs displayed:**
- 461K total registered companies
- 23.67T in total reported income (last year)
- 457K active companies in the most recent period
- Dynamic filtering by enrollment status, society type, legal organization, and identification class

---

## 🏗️ Data Architecture — Star Schema

The project follows a **dimensional modeling approach (star schema)**, separating business dimensions from the central fact table for efficient querying and Power BI performance.

```
                    ┌─────────────────────┐
                    │   fact_empresas      │
                    │─────────────────────│
                    │ matricula (FK)       │
                    │ razon_social         │
                    │ nit                  │
                    │ fecha_matricula      │
                    │ ultimo_ano_renovado  │
                    │ ingresos             │
                    └──────────┬──────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                       │
┌───────▼──────┐   ┌───────────▼──────┐   ┌──────────▼───────┐
│ dim_camara   │   │ dim_estado       │   │ dim_tipo_sociedad │
│ _comercio    │   │ _matricula       │   │──────────────────│
│─────────────│   │─────────────────│   │ cod_tip_soci      │
│ cod_cam      │   │ cod_est_mt       │   │ tipo_sociedad     │
│ camara_      │   │ estado_matricula │   └──────────────────┘
│ comercio     │   └──────────────────┘
└─────────────┘
        │                      
┌───────▼──────┐   ┌───────────────────┐   ┌──────────────────┐
│ dim_         │   │ dim_orga_juridica │   │ dim_identificacion│
│ matricula    │   │───────────────────│   │──────────────────│
│─────────────│   │ cod_org_jr        │   │ cod_cl_id        │
│ cod_ct_matr  │   │ organizacion_     │   │ clase_           │
│ categoria_   │   │ juridica          │   │ identificacion   │
│ matricula    │   └───────────────────┘   └──────────────────┘
└─────────────┘
```

---

## 🔧 SQL Schema

```sql
-- DIMENSION TABLES
CREATE TABLE dim_camara_comercio(cod_cam BIGINT, camara_comercio VARCHAR);
CREATE TABLE dim_estado_matricula(cod_est_mt BIGINT, estado_matricula VARCHAR);
CREATE TABLE dim_identificacion(cod_cl_id BIGINT, clase_identificacion VARCHAR);
CREATE TABLE dim_matricula(cod_ct_matr BIGINT, categoria_matricula VARCHAR);
CREATE TABLE dim_orga_juridica(cod_org_jr BIGINT, organizacion_juridica VARCHAR);
CREATE TABLE dim_tipo_sociedad(cod_tip_soci BIGINT, tipo_sociedad VARCHAR);

-- FACT TABLE
CREATE TABLE fact_empresas(
    matricula VARCHAR,
    razon_social VARCHAR,
    sigla VARCHAR,
    numero_identificacion VARCHAR,
    nit VARCHAR,
    fecha_matricula VARCHAR,
    fecha_renovacion VARCHAR,
    ultimo_ano_renovado VARCHAR,
    fecha_vigencia VARCHAR,
    -- + additional operational and financial fields
);

-- ANALYTICAL VIEW
CREATE VIEW vw_empresas_ingresos AS
    SELECT
        e.matricula,
        e.razon_social,
        e.sigla,
        e.numero_identificacion,
        e.nit,
        e.digito_verificacion,
        e.fecha_matricula,
        e.fecha_renovacion,
        e.ultimo_ano_renovado,
        e.fecha_vigencia,
        e.fecha_cancelacion,
        e.codigo_organizacion,
        -- + dimension joins for BI consumption
    FROM empresas_final e;
```

---

## 📊 Power BI Dashboard Features

| Visual | Description |
|--------|-------------|
| KPI Cards | Total companies, active companies, total income |
| Bar Chart | Income by Chamber of Commerce (city) |
| Bar Chart | Income by enrollment category |
| Donut Chart | Distribution by enrollment status |
| Bar Chart | Average income by legal organization |
| Bar Chart | Income by identification class |
| Bar Chart | Income by society type |
| Slicer | Dynamic filter by enrollment status and year |

---

## 🛠️ Tech Stack

![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat&logo=sqlite&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![DBeaver](https://img.shields.io/badge/DBeaver-372923?style=flat&logo=dbeaver&logoColor=white)

- **Database:** SQLite via DBeaver
- **Modeling:** Star schema dimensional design
- **Visualization:** Power BI with advanced DAX measures and Power Query transformations
- **Dataset:** 460K+ company records from Colombian commercial registry

---

## 📁 Repository Structure

```
├── schema.sql              # Full database DDL (tables + view)
├── dashboard_preview.png   # Dashboard screenshot
├── trabajo_final.pbix      # Power BI report file
└── README.md
```

---

## 👤 Author

**Bryan Camilo Martínez Riveros**  
Data Analytics Engineer | Supply Chain & Business Intelligence  
📍 Bogotá, Colombia  
🔗 [linkedin.com/in/bryanmartinez96](https://linkedin.com/in/bryanmartinez96)
