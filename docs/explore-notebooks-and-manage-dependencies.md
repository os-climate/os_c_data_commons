# OS-Climate Data Commons Developer Guide: Explore notebooks and manage dependencies

1. Add dependencies to Pipfile in the [packages] section.
2. Create a notebook with basic ingest flow:
   1. Load credentials from *credentials.env* file
   2. Create S3 resource for source file or files.  Credentials give access to OS-Climate private buckets.  To access a public S3 bucket from *e.g.* WRI, use this:
      ```
      s3_resource = boto3.resource(
          service_name="s3",
          endpoint_url=os.environ['S3_LANDING_ENDPOINT'],
          config=Config(signature_version=UNSIGNED)
      )
      bucket = s3_resource.Bucket('wri-projects')
      ```
   3. Load data to be ingested (often using *pandas.read_csv* or *pandas.read_excel* which often requires *(engine=openpxyl)* and the *openpxyl* library
   4. Tidy/Process data as required
   5. Create a trino connection / cursor
   6. Create / Set schema name according to last identifier of your repository name
   7. For each input table
      1. Map input datatypes to trino's SQL datatypes
      2. Create the trino table using an appropriate data format (such as *parquet*)
      3. Update access control rules (HOW?)
      4. Update metadata information (HOW!?)
      5. Update version information (HOW!?)
 3. Test your work (HOW?!)
 4. Done!
