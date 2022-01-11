# Component Architecture: Data Management Platform

## Overview

![Data Management Plaform Component Architecture](https://github.com/os-climate/os_c_data_commons/blob/main/images/architecture/Data-Commons-Data-Platform-Component-Architecture.png)

The above view is a component architecture diagram of the Data Commons platform focusing on data management capabilities. Our approach is to closely integrate technology components across 3 layers:

- Physical layer: responsible for storage and accessibility of raw data, both structured and unstructured.

- Virtual layer: responsible for data virtualization, federation and access to data pipelines and other internal technology services.

- Access layer: responsible for data distribution including access control and compliance management for external data consumers (including external applications integrating with the Data Commons)

In addition, a single layer of Identify & Access Management driving data access controls, security and compliance is implemented across the physical, virtual and access layers to ensure consistency across, and all data transactions are audited in real-time through a single consolidated real-time monitoring pipeline.

## Data Access Management Components

The key components of our data platform architecture are as follows:

1. Object storage: Responsible for secure storage of raw data and object-level data access for data ingestion / loading. This is a system layer where data transactions are system-based, considered to be privileged activity and typically handled via automated processes. The data security at this layer is to be governed through secure code management validating the logic of the intended data transaction and secrets management to authenticate all access requests for privileged credentials and then enforce data security policies.

2. Data serving: Open standard, high-performance table format responsible for managing the availability, integrity and consistency of data transactions and data schema for huge analytic datasets. This includes the management of schema evolution (supporting add, drop, update, rename), transparent management of hidden partitioning / partitioning layout for data objects, support of ACID data transactions (particularly multiple concurrent writer processes) through eventually-consistent cloud-object stores, and time travel / version rollback for data. This layer is critical to our data-as-code approach as it enables reproducible queries based on any past table snapshot as well as examination of historical data changes.

3. Distributed SQL Query Engine: Centralized data access and analytics layer with query federation capability. This component ensures that authentication and data access query management is standardized and centralized for all data clients consuming data, by providing management capabilities for role-based access control at the data element level in Data Commons. This being a federation layer it can support query across multiple external distributed data sources for cross-querying requirements.

4. Metadata management: Component responsible for data pipeline and metadata management. By tying together data pipeline code and execution, it provides file-based automated data and metadata versioning across all stages of our data pipelines (including intermediate results), including immutable data lineage tracking for every version of code, models and data to ensure full reproducibility of data and code for transparency and compliance purpose.

5. Data security management: Framework responsible for the monitoring and management of comprehensive data security across the entire Data Commons, as a multi-tenant environment. It provides centralized security administration for all security related tasks in a management UI, fine-grained authorization for specific action / operation on data at the data set / data element level including metadata-based authorization and controls. It also centralizes the auditing of user / process-level access for all data transactions. Ultimately, we want this layer to provide data access management and visibility directly to the data / data product owners as a self-service capability. 

6. API Gateway: API management tool that aggregates the access to various external data APIs used in distributing data to external clients. It is responsible for managing external authentication, authorization to external services integrating with Data Commons, in particular external partner application and tools.
