# OS-Climate Data Commons Developer Guide: Data Quality Validation and Metadata Management

## 1. Data Quality Validation

From the previous page you have already run `dbt test --profiles-dir /opt/app-root/src/` which provides a preliminary data quality check.

What more needs to be said?

## 2. Metadata Management

## 3. Data Profiling and Sample Data

## 4. Sources and Exposures

[Sources and Exposures](https://timeflow.academy/dbt/labs/sources-exposures) put the DBT pipeline into a larger context.  Source tables and views can be referenced in our DBT pipelines, but they are never created or materialsied by DBT.  Exposures are tables containing the data we actually want our user community to use and which meet our desired standards for accuracy and completeness.  We can add metadata to Exposures to expose not only what the data is, but also how it is used or expected to be used (a dashboard, report or notebook), and an email address for the owner of the downstream consumer, facilitating communication and collaboration as new data arrives and as schemas evolve.

# Next Step

[Data Quality Validation and Metadata Management](./metadata-management.md)
