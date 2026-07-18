# BikeCompany-ETL-DataWarehouse

## Overview

A small-scale data warehouse project built with **Kettle (Pentaho Data Integration)** . It extracts data from the source database `BikeCompany.bak`, transforms it through **ETL (Extract, Transform & Load)** pipelines, and loads it into the data warehouse `BikeCompany_DW.bak`, which follows a constellation schema. The project includes dimension tables, fact tables, and complete ETL job flows.

## Repository Structure

```
BikeCompany-Data-Warehouse-ETL/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── Docs/
│   ├── models/
│   │   ├── oltp_er_diagram.png  
│   │   └── dw_constellation_schema.png 
│   ├── flows/
│       ├── etl_overview_flow.png        # ETL 总流程图
│       ├── etl_dimensions_flow.png      # 维度表加载流程
│       └── etl_facts_flow.png              
│
├── Database/
│   ├── BikeCompany.bak                    
│   └── BikeCompany_DW.bak                 
│
└── ETL/
    ├── jobs/
    │   └── kjb_job.kjb                       
    │
    └── transformations/
        ├── dimensions/
        │   ├── load_dim_date.ktr
        │   ├── load_dim_customer.ktr
        │   ├── load_dim_employee.ktr
        │   ├── load_dim_product.ktr
        │   ├── load_dim_subcategory.ktr
        │   ├── load_dim_territory.ktr
        │   └── load_dim_time.ktr
        │
        └── facts/
            ├── load_fact_sales.ktr
            └── load_fact_order_duration.ktr

```

## How to Run
1. Restore `BikeCompany.bak` and `BikeCompany_DW.bak` in SQL Server.
2. Open `main.kjb` in Kettle (Spoon).
3. Configure database connections.
4. Run the job.

