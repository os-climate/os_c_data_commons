# Architectural Domain Driver: Data Pipeline Management

- **DPM-001 - Distributed data mesh architecture:** Pipeline management in OS-C Data Commons platform is based on a distributed data mesh architectural paradigm, in order to support rapid onboarding of an ever-growing number of distributed domain data sets, and the expected proliferation of consumption scenarios such as reporting, analytical tools and machine learning across the growing OS-C community. This means having small units of data pipelines that are highly re-usable across multiple development streams with well-defined and documented integration / consumption models, all leveraging a shared infrastructure that takes care of scalability, governance and security.

![OS-C Data Commons Data Pipeline Architecture](https://github.com/os-climate/os_c_data_commons/blob/main/images/architecture/Data-Commons-Pipeline.png)

- **DPM-002 - Query-driven data mesh implementation strategy:** The primary way data products should be made accessible to the organization is query-driven. In Data Commons query-driven data mesh, interested consumers are expected to query the data through a data federation layer using SQL, including queries that can federate multiple sources of data including both external federated data sources as well as loaded / transformed data made stored in the Data Commons data vault. This has several advantages including:

    Improving data quality: By keeping the data close to data product owner and limiting data copy across multiple repositories, we can reduce data errors as well as the cost associated with data movement.

    Improving availability / usability: As most data sciences users are already familiar with SQL, they can use it to get insights by combiining a wide range of heterogeneous data sources.

    Increasing agility: Having the ability to create adhoc queries gives users the freedom to easily integrate and leverage data sources in other domains.

- **DPM-003 - Date pipelines based on a FLT / ELT approach:** For data pipelines the use of an FLT (Federate / Load / Transform) or ELT (Extract / Load / Transform) approach is preferred over a traditional ETL approach. This means that:

    We prioritize loading data from a federated source as much as possible in order to promote use of the data maintained by the domain owner, and limit data duplication.

    We perform transformation only in consumer data pipelines that require the transformed data for their own use (e.g. feeding a model, or data distribution). In this case data is loaded only once by the consumer data pipeline and all transformation logic is managed according to our data pipeline management principles, in particular data-as-code for transparency reproducibility. In addition, the consumer pipeline typically is developed from the output-bacward and only loading relevant data from source.

- **DPM-004 - Use a multi-layered approach for data processing / engineering:** Data Engineering Pipelines which focus on handling ELT from multimodal external data sources and data normalisation, shall have a dedicated pipeline by business domain layer, managed in a dedicated code repository. The data processing should follow a multi-layered approach for decoupling and easy maintenance of the processing logic:

    Ingestion: This layer handles the collection of raw data in a wide variety (structured, semistructured, and unstructured) from various sources (such as external file-based systems, external APIs, IoT devices) at any speed (batch or stream) and scale.

    Storage: This layer takes care of storing the data in secure, flexible, and efficient storage for internal processing. This can typically be a relational database, a graph database, a NoSQL database, an in-memory data cache or even an event streaming pipeline. This should include managing relevant business and technical metadata that allows us to understand the data’s origin, format, lineage, and how it is organised, classified and connected.

    Processing: This layer turns the raw data into consumable, by sorting, filtering, splitting, enriching, aggregating, joining, and applying any required business logic to produce new meaningful data sets. At this layer, we harmonise and simplify the disparate data sources by combining various data sets and build a unified business domain layer, which can be reused for various analytics and reporting use cases.

    Distribution: This layer provides the consumer of the data the ability to use the post-processed data, by performing ad-hoc queries, producing views which are organised into reports and dashboards or upstream it for ML use in other pipelines. This layer is designed for reusability, discoverability, and backfilling.

- **DPM-005 - Manage data pipelines and training data sets as code for reproducibility:** Data pipelines design should ensure both audit-ability and reproducibility, which is the ability to re-process the same source data with the same workflow / model version to reach the same conclusion as a previous work state. This means in particular for pipelines leveraging machine learning, the data pipeline implementation should support a snapshot of the raw, curated and model input data to be saved / versioned / metadata-tagged every time a model is trained and associated with a specific version of pipeline source code (maintained in the Github repository). Training data should also be made available for external consumption by other work streams requiring similar model training.

- **DPM-006 - Recommended patterns for ingestion:** Based on the data source, we recommend 3 main types of ingestion patterns driving the underlying design of the data pipelines, namely:

    1. [Batch ingestion pattern](add-data-pipeline-ingestion-patterns-batch.md): Used when entire, consistent data sets need to be loaded into Data Commons consistently with the source. 
    2. [Event-driven ingestion pattern](add-data-pipeline-ingestion-patterns-event.md): Used when there is a need to synchronize the data to an external source that goes through regular data changes, be it as append-only changes that need to be captured as a persistent and unchangeable log, or data unit level transactions that would impact existing data.
    3. [Data federation pattern](add-data-pipeline-ingestion-patterns-federation.md): Used when the external data set is vast, available through direct connectivity, and the data required in the pipeline is typically a subset (data elements / records) of the entire data set. 
