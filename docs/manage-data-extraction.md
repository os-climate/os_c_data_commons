# OS-Climate Data Commons Developer Guide: Create ingestion pipeline

To go through a typical ingestion pipeline, we will use an ingestion example from our [data-platform-demo repository](https://github.com/os-climate/data-platform-demo), which is a demo notebook showing how the ingestion process from source data buckets to a Trino schema in our Development or Production Data Buckets. The sample we will use is [PUDL Data Ingestion Sample](https://github.com/os-climate/data-platform-demo/blob/master/notebooks/pudl_ingestion_sample.ipynb) which shows the following steps required in ingesting data:

## 0. Create a branch in which you will do your work.

Creating branches for different changes makes it a lot easier to collaborate with others and use CD/CI tools.  If you only ever check in changes to the `main` branch, you will likely find yourself the only one able to work on that code base.

## 1. Connect to and retrieve source data from a S3 source data bucket

S3 source data bucket are read-only buckets used purely for source data retrieval. This can be done from:

1. A private S3 bucket for an OS-Climate Source Data Landing Zone. We have a private bucket (redhat-osc-physical-landing-647521352890) for public sources that need to be downloaded physically for ingestion, and private buckets by data source for each private data source that is contributed to our initiative. Access to the S3 bucket is then managed via a dedicated secret embedding the required credentials - if you need access to specific source data please check the section on [OS-Climate Data Commons Developer Guide: Pre-Requisite](./pre-requisite.md). For retrieval of data files we advise using the boto3 resource API which provides efficient handling of bucket and object-level data. The following code shows how to access a given bucket:

```
    s3_resource = boto3.resource(
        service_name="s3",
        endpoint_url=os.environ['S3_LANDING_ENDPOINT'],
        aws_access_key_id=os.environ['S3_LANDING_ACCESS_KEY'],
        aws_secret_access_key=os.environ['S3_LANDING_SECRET_KEY'],
    )
    bucket = s3_resource.Bucket(os.environ['S3_LANDING_BUCKET'])
```

1. A public S3 bucket for any data source that can be directly "federated" in the ingestion process from the source data provider. In this case, you can typically access the bucket through unsigned access as shown ou our [GLEIF Data Ingestion Sample](https://github.com/os-climate/data-platform-demo/blob/master/notebooks/gleif_ingestion_sample.ipynb).

## 2. Transfer the data into a Parquet file on S3 development or production data bucket

This is done by extracting the data to be ingested into a Pandas data frame - this often requires using *pandas.read_csv* or *pandas.read_excel* - then generating a Parquet file from the data frame. To upload to the S3 development or production data bucket, follow this five step process (examplified here: https://github.com/os-climate/wri-gppd-ingestion-pipeline/blob/master/notebooks/WRI-gppd-ingest.ipynb):

### 2.1 Convert the pandas dataframe to pyarrow for type conversion and basic metadata
### 2.2 Since pyarrow tables are immutable, create a new table with additional combined metadata
### 2.3 Convert table to parquet format (which cannot be written directly to s3)
### 2.4 Put the parquet-ified data into an S3 bucket for trino.
### 2.5 Create the trino table backed by our parquet files enhanced by field-level and table-level metadata

These buckets are used for storing data to be ingested into Trino, the open source distributed SQL query engine that we use to federate data across tabular sources for ingested and processed data. The process requires:

1. Creating a boto3 client connection to S3 development or production data bucket using the credentials provided in the relevant secret for the particular bucket.
   
```
    s3 = boto3.client(
        service_name="s3",
        endpoint_url=os.environ['S3_DEV_ENDPOINT'],
        aws_access_key_id=os.environ['S3_DEV_ACCESS_KEY'],
        aws_secret_access_key=os.environ['S3_DEV_SECRET_KEY'],
    )
```

1. Uploading the parquet file for ingestion into the bucket, using the following folder structure /trino/{trino_schema_name}/{trino_table_name}/{uuid}/{trino_table_name}.parquet. It should be noted that the convention for data ingestion is to use the name of the data source as the schema name, and the name of the data set as table name.

2. Open a Trino connection using the Trino connection credentials in your credentials.env based on your [environment setup as described here](./setup-initial-environment.md). Note that the Trino token to be retrieved has a one-week validity and you may require to regenerate the token and update your credentials.env accordingly.

```
    conn = trino.dbapi.connect(
        host=os.environ['TRINO_HOST'],
        port=int(os.environ['TRINO_PORT']),
        user=os.environ['TRINO_USER'],
        http_scheme='https',
        auth=trino.auth.JWTAuthentication(os.environ['TRINO_PASSWD']),
        verify=True,
    )
    cur = conn.cursor()
```

3. Create a new schema as required if the ingestion source is new. This is done by creating a schema under osc_datacommons_dev catalogue (the Trino development catalogue) or osc_datacommons_prod (the Trino production catalogue) via an SQL command through Trino connection as per this example:

```
    cur.execute('create schema if not exists osc_datacommons_dev.pudl')
    cur.fetchall()
```

4. Generate the Trino table schema from the dataframe, using the function generate_table_schema_pairs on the data frame with the source data.

5. Finally the data can be ingested into Trino from the parquet file using the schema generated in step 5.

```
    tabledef = """create table if not exists osc_datacommons_dev.pudl.{tname}(
        {schema}
        ) with (
        format = 'parquet',
        external_location = 's3a://{bucket}/trino/{sname}/{tname}/{uuid}'
    )""".format(schema=schema,bucket=os.environ['S3_DEV_BUCKET'],sname=schemaname,tname=tablename,uuid=ingest_uuid)
    print(tabledef)
    cur.execute(tabledef)
    cur.fetchall()
````
