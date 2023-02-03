# OS-Climate Data Commons Developer Guide: Data Extraction

Note that for the examples in this documentation, we are primarily using a data pipeline example from our [WRI Global Power Plant Database ingestion pipeline repository](https://github.com/os-climate/wri-gppd-ingestion-pipeline). Other repositories and code bases will be referenced specifically for specific handling not used in the context of the WRI Global Power Plant Database ingestion pipeline.

We cover below the most common use case of data extraction from a managed file source. For federated data, specific connection to the data source would have to be handled based on the specific client used to connect (preferably a python library). Ultimately, for all types of source, we want to be able to extract and version the source data, and be able to load the data into a Pandas DataFrame for the next step which is the data loading into Trino, before transformation can happen.

## 1. Connect to and retrieve source data from a S3 source data bucket

This is the most common case of source data extraction for large, batch-based extractions which need to be triggered on a regular but not continuous basis. S3 source data bucket are read-only buckets used purely for source data retrieval. This can be done from:

1. A private S3 bucket for an OS-Climate Source Data Landing Zone. We have a S3 bucket (redhat-osc-physical-landing-647521352890) for public and private sources that need to be downloaded physically for ingestion, where we restrict the ability to read / write data to authorized processes. Access to the S3 bucket is managed via dedicated secrets embedding the required credentials - if you need access to specific source data please check the section on [OS-Climate Data Commons Developer Guide: Pre-Requisite](./pre-requisite.md). For retrieval of data files we advise using the boto3 resource API which provides efficient handling of bucket and object-level data. The following code shows how to access a given bucket:

```
    s3_resource = boto3.resource(
        service_name="s3",
        endpoint_url=os.environ['S3_LANDING_ENDPOINT'],
        aws_access_key_id=os.environ['S3_LANDING_ACCESS_KEY'],
        aws_secret_access_key=os.environ['S3_LANDING_SECRET_KEY'],
    )
    bucket = s3_resource.Bucket(os.environ['S3_LANDING_BUCKET'])
```

From there, a specific file object such as a csv file can be retrieved with the following code:

```
    csv_object = s3_resource.Object(os.environ['S3_LANDING_BUCKET'], 'filepath')
    csv_file = BytesIO(csv_object.get()['Body'].read())
```

2. A public S3 bucket for any data source that can be directly "federated" in the ingestion process from the source data provider. In this case, you can typically access the bucket through unsigned access as shown ou our [GLEIF Data Ingestion Sample](https://github.com/os-climate/data-platform-demo/blob/master/notebooks/gleif_ingestion_sample.ipynb).
   
3. As a side note, we also include in the [Template for Data Pipeline projects][1] a simple data directory structure for simple file-based upload from the GitHub repository of the pipeline itself. This structure can be used as an alternative for simple pipelines where:
   - The data is public and can be made available on our public repositories.
   - The data is not large (<10000 records)
   - The data is rarely updated
   - There is value in having the whole pipeline inclusive of the data available for external parties to replicate the example, for example in the case of pipelines used as demonstration for data engineering training

    In this structure, we have 4 folders:
    - external: used for data from third party sources which is needed in the pipeline (e.g. lookup) but is not the main processing target
    - raw: the original data to be processed (should be immutable)
    - interim: intermediate data that has been transformed but not to be used for analysis
    - processed: the final, canonical data set for analysis

## 2. Version the source data on Pachyderm

All source data triggering a data pipeline should then be versioned in a dedicated data versioning repository on Pachyderm service. Pachyderm provides version control and lineage management for source data and a good example of this can be found in [data extraction notebook for WRI Global Power Plant Database][2]. The code leverages the python_pachyderm library and versioning a source file requires only 3 steps:

1. Creating a Pachyderm client based on connection details set in your credentials.env

```
client = python_pachyderm.Client(os.environ['PACH_ENDPOINT'], os.environ['PACH_PORT'])
```

2. Creating a new repository (if required) - following the naming of your pipeline repository is recommended here

```
client.create_repo("wri-gppd")
````

3. A simple commit of the file can then be done

```
with client.commit("wri-gppd", "master") as commit:
        # Add all the files, recursively inserting from the directory
        # Alternatively, we could use `client.put_file_url` or
        # `client_put_file_bytes`.
        python_pachyderm.put_files(client, path, commit, "/global_power_plant_database_v_1_3/")
```
Note that we do not condition the type of file format to be used for versioning purpose. A recommendation is to use column-oriented data file format (such as Parquet, ORC) while bearing in mind that the data loading step will require reading the data into a Pandas dataframe for subsequent loading into Trino.

It should also be noted that for data that should carry units (i.e., metric tons of CO2e, petajoules, Euros, etc.), Pint-Pandas works best with column data all of the same type.  Thus a timeseries of companies in rows and years in columns is an anti-pattern (because different companies have units particular to their sectors) while companies in columns and years in rows typically leads to homogeneous (and thus PintArray-friendly) data columns.

## Next Step

[Data Loading](./data-loading.md)

[1]: https://github.com/os-climate/data-pipeline-template
[2]: https://github.com/os-climate/wri-gppd-ingestion-pipeline/blob/master/notebooks/wri-gppd-01-extraction.ipynb
