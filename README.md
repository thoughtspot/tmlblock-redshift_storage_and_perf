#  TML Blocks - Redshift Storage & Performance

Monitor Redshift cluster performance and team usage on Thoughtspot. 

In every Redshift database, there is a schema called pg_catalog that includes system tables that store storage and performance data from the entire data warehouse. ThoughtSpot has selected the tables most meaningful for analyzing storage and performance trends and made this data easily searchable via ThoughtSpot. There is always room for customization to any template, but the DDL, worksheets and pinboard content that comprise this SpotApp definitely will provide a solid starting point for anyone who wants to explore and analyze their Redshift Storage & Performance data.

# Artifacts 

## Worksheets
- Clusters
- Storage by Table
- Query History
- Errors
- Alerts
- Active Queries

## Liveboards
- Amazon Redshift: Management Dashboard
- Amazon Redshift: Performance and Consumption 

# Redshift Storage & Performance SpotApp Implementation Steps

Once you have downloaded the Zip file and have verified its contents, the implementation steps are as follows:

1. Create the necessary views for the SpotApp referenced in the “Redshift Storage & Perform SpotApp pg_catalog Views” section below.
2. Log into your ThoughtSpot instance and create an Embrace connection to all of the relevant views.
3. Create all of the required joins between the views.
4. Import the TML for the worksheets and verify that it has all been imported without any errors.
5. Import the TML for the pinboard and verify that it has all been imported without any errors.
6. You are ready to start searching your Redshift data!

## Redshift Storage & Performance SpotApp pg_catalog Views

The tables that appear in the pg_catalog schema in any Redshift database are not available for direct selection from ThoughtSpot Embrace. This means that, in order to search the data included in these tables, they must first be made accessible to ThoughtSpot. The method to make this data accessible is to create a view from each of the relevant pg_catalog tables and then grant select permission to these views to the Redshift user that will be specified in the Embrace connection.

**To create these views, please run the following SQL queries in Redshift. (Please replace “your_db_name” with the name of the database that you are using).**

`
create view b2cmarketing.public.pg_user_vw as select * from b2cmarketing.pg_catalog.pg_user;
create view b2cmarketing.public.stl_alert_event_log_vw as select * from b2cmarketing.pg_catalog.stl_alert_event_log;
create view b2cmarketing.public.stl_ddltext_vw as select * from b2cmarketing.pg_catalog.stl_ddltext;
create view b2cmarketing.public.stl_load_errors_vw as select * from b2cmarketing.pg_catalog.stl_load_errors;
create view b2cmarketing.public.stl_plan_info_vw as select * from b2cmarketing.pg_catalog.stl_plan_info;
create view b2cmarketing.public.stl_query_vw as select * from b2cmarketing.pg_catalog.stl_query;
create view b2cmarketing.public.stl_wlm_query_vw as select * from b2cmarketing.pg_catalog.stl_wlm_query;
create view b2cmarketing.public.stv_node_storage_capacity_vw as select * from b2cmarketing.pg_catalog.stv_node_storage_capacity;
create view b2cmarketing.public.stv_recents_vw as select * from b2cmarketing.pg_catalog.stv_recents;
create view b2cmarketing.public.stv_sessions_vw as select * from b2cmarketing.pg_catalog.stv_sessions;
create view b2cmarketing.public.stv_wlm_service_class_config_vw as select * from b2cmarketing.pg_catalog.stv_wlm_service_class_config;
create view b2cmarketing.public.svl_qlog_vw as select * from b2cmarketing.pg_catalog.svl_qlog;
create view b2cmarketing.public.svl_query_metrics_summary_vw as select * from b2cmarketing.pg_catalog.svl_query_metrics_summary;
create view b2cmarketing.public.svl_s3query_summary_vw as select * from b2cmarketing.pg_catalog.svl_s3query_summary;
create view b2cmarketing.public.svv_table_info_vw as select * from b2cmarketing.pg_catalog.svv_table_info;
`

After the views have been created, make sure the Redshift user has the proper permission to query the views by executing the following SQL statements. **(Please replace “your_username” with your username and, if necessary, replace “public” with the name of the appropriate schema).**

`
grant usage on schema public to your_username;
grant select on all tables in schema public to your_username;
`

## Connect Redshift and Thoughtspot 
Log into your ThoughtSpot instance and create an Embrace connection to Redshift. You should have the following tables available in your ThoughtSpot account:
- pg_user_vw
- stl_alert_event_log_vw
- stl_ddltext_vw
- stl_load_errors_vw
- stl_plan_info_vw
- stl_query_vw
- stl_wlm_query_vw
- stv_node_storage_capacity_vw
- stv_recents_vw
- stv_sessions_vw
- stv_wlm_service_class_config_vw
- svl_qlog_vw
- svl_query_metrics_summary_vw
- svl_s3query_summary_vw
- svv_table_info_vw

## Thoughtspot Table Joins 

To ensure the successful import of the worksheet and pinboards, joins need to be added to the tables in ThoughtSpot. Because these are views in Redshift and the joins cannot be “inherited” directly from the data warehouse, the joins must be added manually in the interface.

Please add the following joins to the tables. (IMPORTANT: You will only have to do this once).

| **Source Table** | **Joins** |
| ----------- | ----------- |
| pg_user_vw | None |
| stl_alert_event_log_vw |  <img width="500" alt="Screen Shot 2022-03-31 at 2 05 25 PM" src="https://user-images.githubusercontent.com/102629468/161161164-f619ccb5-903d-493b-962d-ae49d048cf15.png">|
| stl_ddltext_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 09 52 PM" src="https://user-images.githubusercontent.com/102629468/161161198-9151e011-0146-41cd-9c25-42d959f5f600.png"> |
| stl_load_errors_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 10 02 PM" src="https://user-images.githubusercontent.com/102629468/161161218-ec8b4169-66b3-4a4e-b586-bf29427b6406.png"> |
| stl_plan_info_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 10 11 PM" src="https://user-images.githubusercontent.com/102629468/161161235-8f79cb01-4211-442a-ad97-959dcaf9c96a.png"> |
| stl_query_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 10 22 PM" src="https://user-images.githubusercontent.com/102629468/161161260-fee0a62b-1cee-442a-bee4-5fc19191d775.png"> |
| stl_query_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 10 28 PM" src="https://user-images.githubusercontent.com/102629468/161161269-ed815212-420a-42bb-bcf3-c464c8091690.png"> |
| stl_query_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 10 39 PM" src="https://user-images.githubusercontent.com/102629468/161161281-b50d7692-06f4-4133-a608-5576fa1274e4.png"> |
| stl_query_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 10 45 PM" src="https://user-images.githubusercontent.com/102629468/161161297-6440fa1e-abee-4154-a0e4-b9dc5214f5e6.png"> |
| stl_query_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 10 53 PM" src="https://user-images.githubusercontent.com/102629468/161161322-5b7b22c3-b5d6-4bf0-93c4-38ea7f3b518a.png"> |
| stl_wlm_query_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 11 01 PM" src="https://user-images.githubusercontent.com/102629468/161161341-c3879d20-ac52-487e-b45f-ddde6b23df50.png"> |
| stl_wlm_query_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 11 11 PM" src="https://user-images.githubusercontent.com/102629468/161161347-42d41083-51e2-424e-a187-ddc0dab74bea.png"> |
| stv_node_storage_capacity_vw | None |
| stv_recents_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 11 21 PM" src="https://user-images.githubusercontent.com/102629468/161161437-ab8b1bb2-11db-4acc-b38b-e0f29c3f9e72.png"> |
| stv_sessions_vw | <img width="500" alt="Screen Shot 2022-03-31 at 2 11 28 PM" src="https://user-images.githubusercontent.com/102629468/161161447-5cebfffb-a27f-4c0e-ad4d-c9a8aca1a5b8.png"> |
| stv_wlm_service_class_config_vw | None |
| svl_qlog_vw | None |
| svl_query_metrics_summary_vw | None |
| svl_s3query_summary_vw | None |
| svv_table_info_vw | None |

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

# Liveboard Screenshots 

## Amazon Redshift: Management Dashboard

<img width="1200" alt="Screen Shot 2022-03-31 at 2 34 16 PM" src="https://user-images.githubusercontent.com/102629468/161161790-08aa3ec4-99f1-425d-a9c1-87aece10d70c.png">

## Amazon Redshift: Performance and Consumption 

<img width="1200" alt="Screen Shot 2022-03-31 at 2 33 29 PM" src="https://user-images.githubusercontent.com/102629468/161161781-edd6fa81-406e-479f-87b4-1841d00b2683.png">



