# OS-C Data Commons Architecture Blueprint

## Overview

![OS-C Data Commons Platform Overview](https://github.com/os-climate/os_c_data_commons/blob/main/images/architecture/COP26-Overview-Technical.png)

## Guiding Principles

These guiding principles are used for a consistent approach in linking together not just the platform / tools components but also the people and process aspects of data as managed through OS-C Data Commons:

1. **Data as code:** All data and data handling logic should be treated as code. Creation, deprecation, and critical changes to data artifacts should go through a design review process with appropriate written documents where the community of contributors as well as data consumers’ views are taken into account. Changes in data sources, schema, pipeline logic have mandatory reviewers who sign off before changes are landed. Data artifacts have tests associated with them and are continuously tested. The same practices we apply to software code versioning and management are employed.

2. **Data is owned:** Data and data pipelines are code and all code must be owned. Source data loading and normalisation pipelines are managed in dedicated code repositories with a clear owner, purpose and list of contributors. Likewise, data analytics pipelines providing inputs to scenario analysis and alignment tooling are managed in dedicated code repositories with a clear owner, purpose, list of contributors and defined data management lifecycle including data security and documented process for publishing, sharing and using the data.

3. **Data quality is measured, reviewed and managed:** Data artifacts must have SLAs for data quality, SLAs for data issues remediation, and incident reporting and management just like for any technology service. This means that for machine learning models, it is required to implement business driven data validation rules on the independent and target variables as part of a continuous integration and deployment pipeline, by monitoring the statistical properties of variables over the period of time and by continuously re-fitting the models to handle potential drift. The owner is responsible for quality and upholding those SLAs to the rest of the OS-C Data Commons user community.

4. **Accelerate data productivity:** The Data Commons platform and data tooling provided must be designed to optimise collaboration between data producers and consumers. Data tooling includes ETL tools, data integration, data management including data storage, data lineage and data access management, data pipeline development tools supporting the writing and execution of tests, automation of the data pipeline by following MLOps methodology, and integration with the existing monitoring / auditing capabilities of the platform. All the tooling used should be open source so as to not restrict usage for any contributing organization, and delivered following the Operate First principle (more at [https://www.operate-first.cloud/](https://www.operate-first.cloud/)) which means the platform development and productisation process must include building the required operational knowledge for deploying and managing the platform, and encapsulating it in the software itself.

5. **Organise for data:** Teams contributing to data pipeline development on OS-C Data Commons platform should be self-sufficient in managing the whole lifecycle of development which includes onboarding contributors, provisioning required infrastructure for development, testing and data processing, managing functional dependencies such as dependencies on other data pipeline streams, and managing their code repositories. In order to support this, the OS-C Data Commons platform follows a model of self-service built around the OS-C Data Commons platform GitHub repository where contributors and all code (platform, data pipelines) is managed and working instances of the Data Commons platform running on self-service infrastructure and tightly integrated with the code base.

## Architectural Domain Drivers

This section covers key design domains in the platform, and for each of these domains the key design drivers behind the target architecture blueprint.

1. [Data Infrastructure-as-a-Platform](./docs/add-data-infra-as-a-platform.md)
2. [Data Pipeline Management](./docs/add-data-pipeline-management.md)
3. [Federated Data Management and Governance](./docs/add-federated-governance.md)
