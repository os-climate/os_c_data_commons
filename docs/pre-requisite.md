# OS-Climate Data Commons Developer Guide: Pre-Requisite

This list provides the requirements for newly onboarded contributors to the OS-Climate project who need to leverage the Data Commons platform for their data management requirements.

## GitHub Setup

1. A GitHub account with your name and contributing organization updated in your profile. This is the profile that will be invited to OS-Climate GitHub organization and will provide with immediate access (Read-only mode) to all repositories.

2. The access to every repository in the organization is managed through specific team structures. A project team is created for every delivery stream (naming convention: os-climate-stream-name-project-team) which provide the ability to raise and contribute issues in the applicable repository or set of repositories, sub-teams providing privileged administrative access to a given repository (naming convention: repository-name-admin) and sub-teams providing development access to a given repository (naming convention: repository-name-developers). Check that your team assignment is correct and if not, please raise an issue against [OS-Climate Data Commons Repository][1] with the subject as "Access Request for OS-C GitHub Organization repositories".

3. For data pipeline developers, access to the Data Commons platform built on Open Data Hub is configured to work with GitHub SSO and requires the user to be part of the team named odh-env-users. If you require access to the platform, please raise an issue against [OS-Climate Data Commons Repository][1] with the subject as "Access Request for Data Commons platform". The access provided will be based on developer teams you are assigned to at creation time (see item 2 above). In the same process, access to the relevant S3 buckets (for source data) and Trino catalogs (for data ingestion and processing) would be created for your user. Any additional access request or request for new S3 buckets and Trino catalogs to be created would have to be raised separately.

4. For data pipeline developers who require an SFTP access to a secure S3 bucket where source data is to be uploaded, you will also need to create specific SSH keys in your GitHub account for SFTP access to the bucket. This can be done following the documented process: [Adding a new SSH key to your GitHub account](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account). Name the new key OS-Climate SFTP Key and raise an issue against [OS-Climate Data Commons Repository][1] with the subject as "Access request for Source Data SFTP", indicating the repository for the data pipeline and source of the data.

## Access to S3 Buckets

1. When providing development access to Data Commons platform (item 2 as part of the GitHub Setup above) and a Read access to relevant source data, and R/W access to the relevant S3 buckets for data ingestions & processing, may be required. In order to get the required S3 credentials including access and secret keys, you should raise an issue against [OS-Climate Data Commons Repository][1] with the subject as "Access request for S3 Buckets" with the information about the access required including data pipeline repositories you need to contribute to and relevant data sets that need to be accessed.

2. There are two main buckets potentially required for data pipelines developers:
    - The S3 bucket which is our Source Data Landing Zone
    - The S3 Iceberg Development Data bucket which is used to load data into Trino

## Access to Trino

1. Access to relevant Trino catalogs and schemas is done via pull requests on relevant Operate First overlay files. A good example of such pull request can be found at [this link][2]

2. In order to perform the change, the following steps are required:
   - If required (i.e. a new pipeline and catalogs / schemas / tables have to be created), create a new group under group-mapping.properties with the users who need access to the schema. A specific group needs to be created for each type of access (e.g. if you need to segregate people who have Read access vs Read / Write access to a schema)
   - Add your GitHub User Name to required groups in group-mapping.properties
   - If required, use the file trino-acl-dsl.yaml to define the access rights for the new or existing groups into the new catalogs / schema / tables
   - Then on the command line use the trino-dsl-to-rules python module to produce and format the rules.json configuration file used by Trino:

  ```text
  trino-dsl-to-rules kfdefs/overlays/osc/osc-cl2/trino/configs/trino-acl-dsl.yaml > kfdefs/overlays/osc/osc-cl2/trino/configs/rules.json
  ```

  When the above is done, commit the pull request as per the example shared above. Do note there are commit hooks in place to ensure consistency between the DSL and rules.json to ensure that access controls follow the appropriate structure and syntax.

## Next Step

[Setup your initial environment](./setup-initial-environment.md)

[1]: https://github.com/os-climate/os_c_data_commons
[2]: https://github.com/operate-first/apps/commit/cc3d4c456d5d0c40882971f700a89ad16b0bc111
