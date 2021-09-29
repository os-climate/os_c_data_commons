# OS-Climate Data Commons Developer Guide

This developer guide is for data engineers, data scientists and developers of the OS-Climate community who are looking at leveraging the OS-Climate Data Commons to build data ingestion and processing pipelines. It shows step-by-step how to configure your development environment, structure data pipeline projects, and manage data and code in a way that complies with our Architecture Blueprint. 

## Tools

In this guide the following technologies are showcased:

- [JupyterHub][4] to launch images with Jupyter tooling
- [Kubeflow Pipelines][9] for end to end experiments using pipelines

## GitOps for reproducibility, portability, traceability with AI support

Nowadays, developers (including data scientists) use Git and GitOps practices to store and share code on development platforms such as GitHub. GitOps best practices allow for reproducibility and traceability in projects. For this reason, we have decided to adopt a GitOps approach toward managing the platform, data pipeline code as well as data and related artifacts.

One of the most important requirements to ensure data quality through reproducibility is dependency management. Having dependencies clearly managed in audited configuration artifacts allows portability of notebooks, so they can be shared safely with others and reused in other projects.

## Project templates

The project template used as a starting point for new repositories can be found here: [project template][1]. Based on the [Cookiecutter Data Science][2] approach, it ties together data scientist needs (e.g. data, notebooks, models) and DevOps engineers needs (e.g. manifests). Having structure in a project ensures all the pieces required for the ML and DevOps lifecycles are present and easily discoverable.

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

### DevOps Lifecycle

7. [Setup and Deploy Inference Application](./docs/deploy-model.md)

8. [Test Deployed inference application](./docs/test-model.md)

9. [Monitor your inference application deployed](./docs/monitor-model.md)


[1]: https://github.com/aicoe-aiops/project-template
[2]: https://drivendata.github.io/cookiecutter-data-science/
[4]: https://jupyter.org/hub
[9]: https://www.kubeflow.org/docs/pipelines/overview/pipelines-overview/
