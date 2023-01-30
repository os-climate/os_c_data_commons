# OS-Climate Data Commons Developer Guide: Data Transformation

We leverage DBT to build and execite advanced data transformation pipelines with clean and maintainable models, and version control all DBT artifacts in the pipeline repository, following our data-as-code principle. Before going through these instructions, we recommend going through a DBT overview and training and a good link for this is [DBT For Data Engineers provided by timeflow academy][1]. This is a free training and cover the DBT capabilities we need for building our pipelines. 

The following steps required for data transformation would typically be implemented in an Airflow DAG for a production run, but we cover here the details of each step from the point of view of a manual run in a development environment. 

## 1. Setup your development environment and repository

Considering the required structure for DBT projects to run, and the typical development process which may requires re-run of DBT pipelines specific to data schemas, we recommend to set a specific DBT project for each specific data schema that the data pipeline is working on (as opposed to centralizing all DBT pipelines into a dedicated repository). This means we advise to:

- Have a `profiles.yml` file to manage all DBT connections, broken down by DBT project / schema. Here is a link to a sample of base [profiles.yml](profiles.yml) file
- In your data pipeline repository, create one subdirectory with the name of the DBT project / schema. Another subdirectory that will be automatically created here is the `logs` directory.
- In the project subdirectory, a lot of logs data is typically generated as part of dbt command run. We typically want to exclude these from git versioning, having a `.gitignore` defined as per below (this has been done by default for you in our data pipeline project template):

```
target/
!target
target/*
!target/manifest.json
!target/catalog.json
dbt_packages/
logs/
```

## 2. Setup your DBT pipeline

We leverage baseline DBT capabilities here with a few key aspects to note:

- In `dbt_project.yml`, we prioritize outputs leveraging materialized views rather than new tables when possible if transformations are simple. If transformation involve combining data from multiple schemas and / or complex transformation rules, it can be acceptable to use new physical tables as an output but bear in mind this means having duplicated data stored.
- In the `models` subdirectory, we leverage the YAML file for schema definition to manage the data set metadata definition, and maintain / version it as code in our repository. This means if you means to access metadata from an external source, we recommend having one step in the data pipeline to extract metadata and automatically produce the YAML file. This allows scalable maintenance of the metadata model by adding steps in the pipeline to augmnent the metadata from source while keeping every change versioned if needed.
- Some simple data quality tests can be run in the DBT pipeline itself, if defined together with the schema. We recommend leveraging this for simple data checks performed for intermediate transformation step. The testing done to assess the quality of the final data set, in particular business rules, should be done with great_expectations (as shown in next step).
- The various steps in the pipeline and SQL scripts will be automatically acquired by OpenMetadata in the next step to build a lineage visible from our catalogue in OpenMetadata itself. So we recommend more simple steps in sequence and simple SQL statements rather than a single, convoluted SQL statement for the pipeline as it increases visibility for external data users.

## 3. Execute DBT pipeline

Finally the execution of the DBT pipeline can be run from the command line in 4 steps:

```
dbt debug --profiles-dir /opt/app-root/src/
dbt run --profiles-dir /opt/app-root/src/
dbt test --profiles-dir /opt/app-root/src/
dbt docs generate --profiles-dir /opt/app-root/src/
```

## Next Step

[Data Quality Validation and Metadata Management](./metadata-management.md)

[1]: https://timeflow.academy/categories/dbt
