# OS-Climate Data Commons Developer Guide: Data Loading

In the Data Commons architecture, the Distributed SQL Query engine provided by Trino is leverages both as a centralized layer for federated queries but also as a data loading engine for the data processed in our data pipelines. In the latter case, it fulfills the role of a data processing engine and abstracts the complexity of writing and versioning data over the underlying cloud object storage and Apache Iceberg, used to handle the partitioning and versioning of the data schema and the data itself. Trino and Iceberg basically bring the simplicity of SQL tables to the work we do with big data, and provide the reliability and scale we need for high volume data processing.

The steps required for data loading are demonstrated in the [data loading notebook for WRI Global Power Plant Database][1]. 

## 1. Use osc-ingest-tools to connect into the Hive ingestion bucket

For efficient data loading at scale it is advised to use our osc-ingest-tools python library and connect into the Hive ingestion bucket, as shown in the sample above. This requires proper setup of credentials.env as explained in our [environment setup guide](./setup-initial-environment.md)

```
import osc_ingest_trino as osc
hive_bucket = osc.attach_s3_bucket('S3_HIVE')
```

## 2. Open a Trino connection

In this case we use Trino as an ingestion engine, so we need to open a connection:

```
ingest_catalog = 'catalog_name'
engine = osc.attach_trino_engine(verbose=True, catalog=ingest_catalog)
```

## 3. Ingest the data via Trino

In this case we use Trino as an ingestion engine, which allows creating a new table schema with a SQL statement and directly loading the data from a partitioned set of parquet files writtten in our Hive storage bucket. We may first need to create a new schema if it does not exist. Note that we use `osc._do_sql` instead of calling `engine.execute` directly, because since Trino version 395 and the trino python client version 0.317.0, some calls are complete without returning rows, and others won't complete unless rows are fetched via fetchall.  The `osc._do_sql` command abstracts that away.

```
ingest_schema = 'schema_name'
ingest_table = 'table_name'
ingest_bucket = 'osc-datacommons-s3-bucket-dev02'
schema_create_sql = f"""
create schema if not exists {ingest_catalog}.{ingest_schema} with (
    location ='s3a://{ingest_bucket}/data/{ingest_schema}.db/'
)
"""
osc._do_sql(schema_create_sql, engine, verbose=True)
```

Once done, osc-ingest-tools provide a fast loading method via Hive:

```
osc.fast_pandas_ingest_via_hive(
    df_gppd,
    engine,
    ingest_catalog, ingest_schema, ingest_table,
    hive_bucket, hive_catalog, hive_schema,
    partition_columns = ['field_name'],
    overwrite = True,
    verbose = True
)
```
From there the data should be available in Trino and can further data processing can be done from there, in particular data transformation if required. Note that the data transformation setup need to be executed regardless in order to have the data set metadata ingested, as we leverage the data transformation part of the pipeline with dbt to push the data set metadata into our metadata catalogue based on OpenMetadata.

## 4. Using Seed Data

[As pointed out in previously mentioned dbt training modules](https://timeflow.academy/dbt/labs/dbt-seed-data-lab), having access to some small data tables (such as country codes) can greatly simplify the ingestion of principal data tables.  The `dbt seed` command allows the loading of small CSV files that can be stored in the dbt project repository.  Of course it is also possible to reference, load, and use such data within the ingention notebooks themselves, but if you think it makes more sense to use the data only within the context of dbt, the `dbt seed` command makes that possible.


## Next Step

[Data Transformation](./data-transformation.md)

[1]: https://github.com/os-climate/wri-gppd-ingestion-pipeline/blob/master/notebooks/wri-gppd-02-loading.ipynb
