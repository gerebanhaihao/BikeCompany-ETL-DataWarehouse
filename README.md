# BikeCompany-ETL-DataWarehouse

## Overview

A small-scale data warehouse project built with **Kettle (Pentaho Data Integration)** . It extracts data from the source database `BikeCompany.bak`, transforms it through **ETL (Extract, Transform & Load)** pipelines, and loads it into the data warehouse `BikeCompany_DW.bak`, which follows a **constellation schema**. The project includes dimension tables, fact tables, and complete ETL job flows.

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
│   └── flows/
│       ├── load_dims.png
│       ├── load_facts.png   
│       └── kjb_job.png              
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

## Data Warehouse Architecture

The data warehouse follows a **constellation schema** (fact constellation), consisting of:

| Fact Table | Description |
|------------|-------------|
| `fact_sales` | Stores financial metrics per order (subtotal, tax, freight, profit) |
| `fact_order_duration` | Tracks order fulfillment timelines (order → ship → delivery) |

These two fact tables share the following **conformed dimensions**:
`dim_date`, `dim_time`, `dim_employee`, `dim_customer`, `dim_territory`, `dim_product`, `dim_subcategory`

## ETL Pipeline Overview

The ETL process follows a **dimension-first, fact-second** loading order to maintain referential integrity:

1. **Load static dimensions** (`dim_date`, `dim_time`, `dim_territory`, `dim_subcategory`)
2. **Load dynamic dimensions** (`dim_employee`, `dim_customer`, `dim_product`)
3. **Load fact tables** (`fact_sales`, `fact_order_duration`)

## How to Run

1. **Restore the source database**  
   Restore `BikeCompany.bak` in SQL Server as the source OLTP database.  
   > The database name is referenced in the Kettle connections. Please ensure it matches your environment.

2. **Create the target data warehouse database**  
   Create a new database named `BikeCompany_DW` in SQL Server.  
   > Do **not** restore `BikeCompany_DW.bak`. This is an empty target database where the ETL process will load the transformed data.

3. **Place the ETL files in your Kettle workspace**  
   Copy all `.ktr` and `.kjb` files to your local Kettle repository or working directory.  
   > Kettle transformations (`*.ktr`) and jobs (`*.kjb`) must be accessible from your local file system or Kettle environment before execution.

4. **Configure database connections in Kettle**  
   - Open Spoon (Kettle GUI client).  
   - Set up two database connections:  
     - Source: `BikeCompany` (OLTP)  
     - Target: `BikeCompany_DW` (Data Warehouse)  
   - Ensure both connections are tested successfully.

5. **Run the main job**  
   Open `kjb_job.kjb` in Spoon and run it.  
   > This job orchestrates the entire ETL process: it will call all dimension table loads (`load_dim_*.ktr`) first, followed by fact table loads (`load_fact_*.ktr`) in the correct dependency order.

6. **Verify the results**  
   After the job completes, check the `BikeCompany_DW` database to confirm that all dimension and fact tables are populated successfully.

