# OS-Climate Data Commons Developer Guide

This developer guide is for data engineers, data scientists and developers of the OS-Climate community who are looking at leveraging the OS-Climate Data Commons to build data ingestion and processing pipelines, as well as AI / ML pipelines. It shows step-by-step how to configure your development environment, structure projects, and manage data and code in a way that complies with our Architecture Blueprint.

**Need Help?**

- Outage / System failure:  File an Linux Foundation (LF) [outage ticket](https://jira.linuxfoundation.org/plugins/servlet/desk/portal/2/create/30) (note: select OS-Climate from project list)
- New infrastructure request (e.g. software upgrade):  File an LF [ticket](https://jira.linuxfoundation.org/plugins/servlet/desk/portal/2) (note: select OS-Climate from project list)
- General infrastructure support:  Get help on OS-Climate Slack [Data Commons channel](https://os-climate.slack.com/archives/C034SCF92BU)
- Data Commons developer support: Get help on OS-Climate Slack [Developers channel](https://os-climate.slack.com/archives/C034SCQU919)

## Tools

Pipeline development leverages a number of tools provided by Data Commons. The list below provides an overview of key technologies involved as well as links to development instances:

| Technology | Description | Link |
| ---------- | ----------- | ---- |
| [GitHub][2] | Version control tool used to maintain the pipelines as code | [OS-Climate GitHub](https://github.com/os-climate) |
| [GitHub Projects][3] | Project tracking tool that integrates issues and pull requests | [Data Commons Project Board](https://github.com/orgs/os-climate/projects/7) |
| [JupyterHub][4] | Self-service environment for Jupyter notebooks used to develop pipelines | [JupyterHub Development Instance](https://jupyterhub-odh-jupyterhub.apps.odh-cl2.apps.os-climate.org/) |
| [Kubeflow Pipelines][5] | MLOps tool to support model development, training, serving and automated machine learning | |
| [Trino][7] | Distributed SQL Query Engine for big data, used for data ingestion and distributed queries | [Trino Console](https://trino-secure-odh-trino.apps.odh-cl2.apps.os-climate.org/) |
| [CloudBeaver][8] | Web-based database GUI tool which provides rich web interface to Trino | [CloudBeaver Development Instance](https://cloudbeaver-odh-trino.apps.odh-cl2.apps.os-climate.org/) |
| [Pachyderm][9] | Data-driven pipeline management tool for machine learning, providing version control for data | |
| [dbt][10] | SQL-based data transformation tool providing git-enabled version control of data transformation pipelines | |
| [Great Expectations][11] | Data quality tool providing git-enabled data quality pipelines management | |
| [OpenMetadata][12] | Centralized metadata store providing data discovery, data collaboration, metadata versioning and data lineage | [OpenMetadata Development Instance](https://openmetadata-openmetadata.apps.odh-cl2.apps.os-climate.org) |
| [Airflow][13] | Workflow management platform for data engineering pipelines | [Airflow Development Instance](https://airflow-openmetadata.apps.odh-cl2.apps.os-climate.org/home) |
| [Apache Superset][6] | Data exploration and visualization platform | [Superset Development Instance](https://superset-secure-odh-superset.apps.odh-cl2.apps.os-climate.org/) |
| [Grafana][14] | Analytics and interactive visualization platform | [Grafana Development Instance](https://grafana-opf-monitoring.apps.odh-cl2.apps.os-climate.org/login)
| [INCEpTION][15] | Text-annotation environment primarily used by OS-C for machine learning-based data extraction | [INCEpTION Development Instance](https://inception-inception.apps.odh-cl2.apps.os-climate.org/) |

## GitOps for reproducibility, portability, traceability with AI support

Nowadays, developers (including data scientists) use Git and GitOps practices to store and share code on development platforms such as GitHub. GitOps best practices allow for reproducibility and traceability in projects. For this reason, we have decided to adopt a GitOps approach toward managing the platform, data pipeline code as well as data and related artifacts.

One of the most important requirements to ensure data quality through reproducibility is dependency management. Having dependencies clearly managed in audited configuration artifacts allows portability of notebooks, so they can be shared safely with others and reused in other projects.

## Project templates

We use two project templates as starting point for new repositories:

- A project template for data pipelines, specific to OS-Climate Data Commons, can be found here: [Data Pipelines Template][16]
- A project tempalte specifically for AI/ML pipelines can be found here: [Data Science Template][1].

Together the use of these templates ties data scientist needs (e.g. notebooks, models) and data engineers needs (e.g. data and metadata pipelines). Having structure in a project ensures all the pieces required for the Data and MLOps lifecycles are present and easily discoverable.

## Tutorial Steps

0. [Pre-requisites](./docs/pre-requisite.md)

### ML Lifecycle / Source Lifecycle

1. [Setup your initial environment](./docs/setup-initial-environment.md)

2. [Explore notebooks and manage dependencies](./docs/explore-notebooks-and-manage-dependencies.md)

3. [Push changes to GitHub](./docs/push-changes.md)

4. [Setup pipelines to create releases, build images and enable dependency management](./docs/setup-gitops-pipeline.md)

### DataOps Lifecycle

5. [Data Ingestion Pipeline Overview](./docs/data-ingestion-pipeline.md)

6. [Data Extraction](./docs/data-extraction.md)

7. [Data Loading](./docs/data-loading.md)

8. [Data Transformation](./docs/data-transformation.md)

9. [Metadata Management](./docs/metadata-management.md)

### ModelOps Lifecycle

10. [Setup and Deploy Inference Application](./docs/deploy-model.md)

11. [Test Deployed inference application](./docs/test-model.md)

12. [Monitor your inference application](./docs/monitor-model.md)

[1]: https://github.com/aicoe-aiops/project-template
[2]: https://github.com/
[3]: https://docs.github.com/en/issues/trying-out-the-new-projects-experience/about-projects
[4]: https://jupyter.org/hub
[5]: https://www.kubeflow.org/docs/pipelines/overview/pipelines-overview/
[6]: https://superset.apache.org/
[7]: https://trino.io/
[8]: https://dbeaver.com/
[9]: https://www.pachyderm.com/
[10]: https://www.getdbt.com/
[11]: https://greatexpectations.io/
[12]: https://open-metadata.org/
[13]: https://airflow.apache.org/
[14]: https://grafana.com/
[15]: https://inception-project.github.io/
[16]: https://github.com/os-climate/data-pipeline-template
