# OS-Climate Data Commons Developer Guide

This developer guide is for data engineers, data scientists and developers of the OS-Climate community who are looking at leveraging the OS-Climate Data Commons to build data ingestion and processing pipelines. It shows step-by-step how to configure your development environment, structure data pipeline projects, and manage data and code in a way that complies with our Architecture Blueprint.

 - **Need Help?** 
   - Outage/System Failures:  File an Linux Foundation (LF) [outage ticket](https://jira.linuxfoundation.org/plugins/servlet/desk/portal/2/create/30) (note: select OS-Climate from project list)
   - New infrastructure request (e.g., software upgrade):  File an LF [ticket](https://jira.linuxfoundation.org/plugins/servlet/desk/portal/2) (note: select OS-Climate from project list)
   - General infrastructure support:  Get help on Slack [ODH channel](https://operatefirst.slack.com/archives/C01RMPVUUK1)
   - Data Commons support: Get help on Slack [Data Commons channel](https://os-climate.slack.com/archives/C034SCF92BU)  OR [Developers channel](https://os-climate.slack.com/archives/C034SCQU919)

## Tools

In this guide the following technologies are showcased:

- [GitHub][2] and [GitHub Projects][3] ([link to Data Commons project board](https://github.com/orgs/os-climate/projects/7))
- [JupyterHub][4] to launch images with Jupyter tooling ([link to cl1 instance](https://jupyterhub-odh-jupyterhub.apps.odh-cl1.apps.os-climate.org/) :: [link to cl2 instance](https://jupyterhub-odh-jupyterhub.apps.odh-cl2.apps.os-climate.org/))
- [Kubeflow Pipelines][5] for end to end experiments using pipelines
- [SuperSet][6] data visualation dashboards ([link to superset cl1 instance](https://superset-secure-odh-superset.apps.odh-cl1.apps.os-climate.org/) :: [link to superset cl2 instance](https://superset-secure-odh-superset.apps.odh-cl2.apps.os-climate.org/))
- [Trino][7] distributed query engine encapsulating data storage, metadata, and access management

## GitOps for reproducibility, portability, traceability with AI support

Nowadays, developers (including data scientists) use Git and GitOps practices to store and share code on development platforms such as GitHub. GitOps best practices allow for reproducibility and traceability in projects. For this reason, we have decided to adopt a GitOps approach toward managing the platform, data pipeline code as well as data and related artifacts.

One of the most important requirements to ensure data quality through reproducibility is dependency management. Having dependencies clearly managed in audited configuration artifacts allows portability of notebooks, so they can be shared safely with others and reused in other projects.

## Project templates

The project template used as a starting point for new repositories can be found here: [project template][1]. It ties together data scientist needs (e.g. data, notebooks, models) and DevOps engineers needs (e.g. manifests). Having structure in a project ensures all the pieces required for the ML and DevOps lifecycles are present and easily discoverable.

## Tutorial Steps

0. [Pre-requisites](./docs/pre-requisite.md)

### ML Lifecycle / Source Lifecycle

1. [Setup your initial environment](./docs/setup-initial-environment.md)

2. [Explore notebooks and manage dependencies](./docs/explore-notebooks-and-manage-dependencies.md)

3. [Push changes to GitHub](./docs/push-changes.md)

4. [Setup pipelines to create releases, build images and enable dependency management](./docs/setup-gitops-pipeline.md)

### DataOps Lifecycle

5. [Create a Data Ingestion Pipeline](./docs/create-ingestion-pipeline.md)

6. [Create a Data Processing Pipeline](./docs/create-processing-pipeline.md)
   
7. [Manage Data Security & Compliance](.docs/manage-security-compliance.md)

### ModelOps Lifecycle

8. [Setup and Deploy Inference Application](./docs/deploy-model.md)

9. [Test Deployed inference application](./docs/test-model.md)

10. [Monitor your inference application deployed](./docs/monitor-model.md)

[1]: https://github.com/aicoe-aiops/project-template
[2]: https://github.com/
[3]: https://docs.github.com/en/issues/trying-out-the-new-projects-experience/about-projects
[4]: https://jupyter.org/hub
[5]: https://www.kubeflow.org/docs/pipelines/overview/pipelines-overview/
[6]: https://superset.apache.org/
[7]: https://trino.io/docs/current/overview/concepts.html
