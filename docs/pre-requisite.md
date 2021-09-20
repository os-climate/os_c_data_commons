# OS-Climate Data Commons Developer Guide: Pre-Requisite

This list provides the requirements for newly onboarded contributors to the OS-Climate project who need to leverage the Data Commons platform for their data management requirements.

## GitHub Setup

1. A GitHub account with your name and contributing organization updated in your profile. This is the profile that will be invited to OS-Climate GitHub organization and will provide with immediate access (Read-only mode) to all repositories.

2. The access to every repository in the organization is managed through specific team structures. A project team is created for every delivery stream (naming convention: os-climate-stream-name-project-team) which provide the ability to raise and contribute issues in the applicable repository or set of repositories, sub-teams providing privileged administrative access to a given repository (naming convention: repository-name-admin) and sub-teams providing development access to a given repository (naming convention: repository-name-developers). Check that your team assignment is correct and if not, please raise an issue against [OS-C Data Commons Repository][1] with the subject as "Access Request for OS-C GitHub Organization repositories".

3. For data pipeline developers, access to the Data Commons platform built on Open Data Hub is configured to work with GitHub SSO and requires the user to be part of the team named odh-env-users. If you require access to the platform, please raise an issue against [OS-C Data Commons Repository][1] with the subject as "Access Request for Data Commons platform". The access provided will be based on developer teams you are assigned to at creation time (see item 2 above). In the same process, access to the relevant S3 buckets (for source data) and Trino catalogs (for data ingestion and processing) would be created for your user. Any additional access request or request for new S3 buckets and Trino catalogs to be created would have to be raised separately.

4. For data pipeline developers who require an SFTP access to a secure S3 bucket where source data is to be uploaded, you will also need to create specific SSH keys in your GitHub account. This can be done following the documented process: [Adding a new SSH key to your GitHub account](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account). Name the new key OS-Climate SFTP Key and raise an issue against [OS-C Data Commons Repository][1] with the subject as "Access request for Source Data SFTP", indicating the repository for the data pipeline and source of the data.

## Additional S3 Bucket and Trino Access

1. When providing development access to Data Commons platform (item 2 as part of the GitHub Setup above), a Read access to relevant source data, R/W access to the relevant S3 buckets for data ingestions & processing, and R/W access to relevant Trino catalogs and schemas should be created. For additional bucket or catalog creation, or for a Read access to other data sources (if you need to access the data in another pipeline or for reporting purpose), raise an issue against [OS-C Data Commons Repository][1] with the subject as "Access request for S3 / Trino" with the information about the access required including data sets if existing.

[1]: https://github.com/os-climate/os_c_data_commons
