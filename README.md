#  TML Blocks - Redshift Storage & Performance

SpotApps are ThoughtSpot’s out-of-the-box solution templates built for specific use cases and data sources. They are built on ThoughtSpot Modeling Language (TML) Blocks, which are pre-built pieces of code that are easy to download and implement directly from the product.

The Redshift Performance and Consumption SpotApp mimics the Redshift data model. When you deploy it, ThoughtSpot creates several Worksheets, Answers, and Liveboards, based on your Redshift data in your cloud data warehouse. 

In every Redshift database, there is a schema called pg_catalog that includes system tables that store storage and performance data from the entire data warehouse. ThoughtSpot has selected the tables most meaningful for analyzing storage and performance trends and made this data easily searchable via ThoughtSpot. There is always room for customization to any template, but the DDL, Worksheets and Liveboard content that comprise this SpotApp definitely will provide a solid starting point for anyone who wants to explore and analyze their Redshift Storage & Performance data.

# Artifacts 
- **Amazon Redshift Management Dashboard_schema.csv**: The following table describes the schema for the Redshift Performance and Consumption SpotApp.
TML template files for the SpotApp.
- **Amazon Redshift TML Block.zip**: TML template files for the SpotApp.
  
# Prerequisites for Deploying the Redshift Performance and Consumption SpotApp

Before you can deploy the Redshift Performance and Consumption SpotApp, you must complete the following prerequisites:

## Review and Sync Data

- **Review Required Data**: Review the required tables and columns for the SpotApp.
- **Ensure Column Compatibility**: Ensure that your columns match the required column type listed in the schema for your SpotApp.
- **Sync Data**: Sync all tables and columns from Redshift to your cloud data warehouse. While only specific tables and columns may be required, ThoughtSpot recommends syncing all tables and columns from Redshift to your CDW. The columns can be Redshift’s out-of-the-box columns, or any custom columns that you are using instead of the out-of-the-box columns.
- **Collaborate on Data Movement**: If you are using an ETL/ELT tool or working with another team in your organization to move data, sync all columns from the tables listed in the SpotApp.

## Credentials and Access

- **Obtain Credentials and SYSADMIN Privileges**: Obtain the necessary credentials and SYSADMIN privileges to connect to Redshift. Ensure that the cloud data warehouse contains the data you would like ThoughtSpot to use to create Answers, Liveboards, and Worksheets. Refer to the Connection reference for Redshift for more information about required credentials.
- **Unique Connection Name**: Ensure that the connection name for each new SpotApp is unique.
- **Administrator Access to Redshift**: Maintain administrator access to manage Redshift resources.

## Create or Access Views

You must create views based on the following Redshift tables in your cloud data warehouse. If you already created the views, ensure access to these views, not just the tables. Refer to the [Redshift Performance and Consumption SpotApp schema](https://github.com/thoughtspot/tmlblock-redshift_storage_and_perf/blob/main/Amazon%20Redshift%20Management%20Dashboard_schema.csv) for more details.

- `pg_user`
- `stl_query`
- `stl_wlm_query`
- `stv_wlm_service_class_config`
- `svl_qlog`
- `stl_alert_event_log`
- `svv_table_info`
- `stv_sessions`
- `stl_load_errors`
- `stv_node_storage_capacity`
- `stl_ddltext`
- `stl_plan_info`
- `svl_query_metrics_summary`
- `svl_s3query_summary`

### Access to the following Redshift views in your cloud data warehouse:

- `pg_user_vw`
- `stl_query_vw`
- `stl_wlm_query_vw`
- `stv_wlm_service_class_config_vw`
- `svl_qlog_vw`
- `stl_alert_event_log_vw`
- `svv_table_info_vw`
- `stv_sessions_vw`
- `stl_load_errors_vw`
- `stv_node_storage_capacity_vw`
- `stl_ddltext_vw`
- `stl_plan_info_vw`
- `svl_query_metrics_summary_vw`
- `svl_s3query_summary_vw`

## Run SQL Commands

The tables that appear in the pg_catalog schema in any Redshift database are not available for direct selection from ThoughtSpot Embrace. This means that, in order to search the data included in these tables, they must first be made accessible to ThoughtSpot. The method to make this data accessible is to create a view from each of the relevant pg_catalog tables and then grant select permission to these views to the Redshift user that will be specified in the Embrace connection.

Run the required SQL commands in your cloud data warehouse to create the necessary views based on the required tables. The following is an example for the Redshift cloud data warehouse; you may need to modify the code for the SQL requirements of your specific cloud data warehouse.

```
{ create view dev.public.pg_user_vw as select * from dev.pg_catalog.pg_user;
create view dev.public.stl_alert_event_log_vw as select * from dev.pg_catalog.stl_alert_event_log;
create view dev.public.stl_ddltext_vw as select * from dev.pg_catalog.stl_ddltext;
create view dev.public.stl_load_errors_vw as select * from dev.pg_catalog.stl_load_errors;
create view dev.public.stl_plan_info_vw as select * from dev.pg_catalog.stl_plan_info;
create view dev.public.stl_query_vw as select * from dev.pg_catalog.stl_query;
create view dev.public.stl_wlm_query_vw as select * from dev.pg_catalog.stl_wlm_query;
create view dev.public.stv_node_storage_capacity_vw as select * from dev.pg_catalog.stv_node_storage_capacity;
create view dev.public.stv_sessions_vw as select * from dev.pg_catalog.stv_sessions;
create view dev.public.stv_wlm_service_class_config_vw as select * from dev.pg_catalog.stv_wlm_service_class_config;
create view dev.public.svl_qlog_vw as select * from dev.pg_catalog.svl_qlog;
create view dev.public.svl_query_metrics_summary_vw as select * from dev.pg_catalog.svl_query_metrics_summary;
create view dev.public.svl_s3query_summary_vw as select * from dev.pg_catalog.svl_s3query_summary;
create view dev.public.svv_table_info_vw as select * from dev.pg_catalog.svv_table_info;
]
```

After the views have been created, make sure the Redshift user has the proper permission to query the views by executing the following SQL statements. **(Please replace “your_username” with your username and, if necessary, replace “public” with the name of the appropriate schema).**

`
grant usage on schema public to your_username;
grant select on all tables in schema public to your_username;
`

# Redshift Storage & Performance SpotApp Implementation Steps

Once you have downloaded the Zip file and have verified its contents, the implementation steps are as follows:

2. Log into your ThoughtSpot instance and create an Embrace connection to all of the relevant views.
4. Import the TML for the worksheets and verify that it has all been imported without any errors.
5. Import the TML for the pinboard and verify that it has all been imported without any errors.
6. You are ready to start searching your Redshift data!

### Import TML
 
- Combine all worksheet TML files into a ZIP file: 
  - Worksheet_Manifest.yaml
  - Active Queries.worksheet.tml
  - Active Sessions.worksheet.tml
  - Alerts.worksheet.tml
  - Clusters.worksheet.tml
  - Errors.worksheet.tml
  - Query History.worksheet.tml
  - Storage by Table.worksheet.tml

- Combine all pinboard/liveboard TML files into a seperate ZIP file: 
  - pinboard_Manifest.yaml
  - Amazon Redshift: Management Dashboard.pinboard.tml
  - Amazon Redshift: Performance and Consumption.pinboard.tml
 
- Import the zipped file containing TML for the worksheets and verify that it has all been imported without any errors.
- Import the zipped file for the liveboards and verify that it has all been imported without any errors.

# Review Formula Values

After you deploy the Redshift Performance and Consumption SpotApp, it is crucial to review the default formula values ThoughtSpot used in several formulas in the Amazon Redshift Clusters Worksheet. Since your values may differ from the default values used by ThoughtSpot, reviewing these values will ensure the SpotApp is as useful for your specific data as possible.

## Formulas to Review in the Amazon Redshift Clusters Worksheet

- **Instance Type**: By default, this is set to `dc2.large`. If necessary, replace this value with the node/instance type for your Redshift cluster and region.
- **Price Per Node Per Hour (Currency)**: If necessary, replace this value with the value for your node type and region. Refer to "On-demand pricing" in the Redshift pricing documentation.
- **Spectrum Price Per TB (Currency)**: If necessary, replace this value with the value for your region. Refer to "Amazon Redshift Spectrum pricing" in the Redshift pricing documentation.
- **Concurrency Price per Second (Currency)**: If necessary, replace this value with the value for your region. Refer to "Concurrency Scaling Pricing" in the Redshift pricing documentation.

### Note
All currency values are in USD. If necessary, convert them to your currency.


# Liveboard Screenshots 

## Amazon Redshift: Management Dashboard

<img width="1200" alt="Screen Shot 2022-03-31 at 2 34 16 PM" src="https://user-images.githubusercontent.com/102629468/161161790-08aa3ec4-99f1-425d-a9c1-87aece10d70c.png">

## Amazon Redshift: Performance and Consumption 

<img width="1200" alt="Screen Shot 2022-03-31 at 2 33 29 PM" src="https://user-images.githubusercontent.com/102629468/161161781-edd6fa81-406e-479f-87b4-1841d00b2683.png">


# Liveboard Screenshots 




