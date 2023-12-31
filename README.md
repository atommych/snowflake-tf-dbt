# snowflake-tf-dbt
Demo Code showing Snowflake and AWS Integration with Terraform and dbt.

Terraform guide for snowflake:
https://quickstarts.snowflake.com/guide/terraforming_snowflake/index.html

Privileges to customize roles:
https://docs.snowflake.com/en/user-guide/security-access-control-privileges

Architecture based on: 
https://www.getdbt.com/blog/how-we-configure-snowflake/
![Architecture](https://cdn-images-1.medium.com/max/2400/1*FPxDaqugiCChkv5QxsoN7w.png)

###  Requirements
Terraform Account and Cli:
- https://app.terraform.io/public/signup/account
- https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli

AWS Account and Cli:
- https://portal.aws.amazon.com/billing/signup
- https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

Snowflake Account and Cli:
- https://signup.snowflake.com/
- https://docs.snowflake.com/pt/user-guide/snowsql-install-config

### Setup cli credentials
    #Set AWS Access Key ID and AWS Secret Access Key        
    aws configure

    #Check credentials 
    aws configure list
    vi ~/.aws/credentials

### Config a new environment
    #Create new python env 
    python3 -m venv snowflake 
    source snowflake/bin/activate

    #Install python dependencies
    pip install -r requirements.txt
    
    #Edit file with your credentials
    source setenv.sh

### Build / Run
    #Provide Infrastructure: AWS S3 
    ENVIRONMENT=dev make build-datalake

    #Provide Infrastructure: Snowflake Warehouses, Database, Schemas, Users, Roles and Grants
    ENVIRONMENT=dev make build-snowflake-db
    
    #Create snowflake_stage, snowflake_pipe, snowflake_table
    #Create aws_iam_role, aws_iam_role_policy, aws_s3_bucket_notification, aws_sns_topic, aws_sns_topic_policy, snowflake_storage_integration, snowflake_notification_integration
    ENVIRONMENT=dev make build-snowflake-pipe
    
    #Initialize dbt profile, upload samples to s3, run dbt workflows and generate documentation
    ENVIRONMENT=dev make run-all

  
### Generated Infrastructure
- **AWS S3 Bucket**: ___prefix___-datalake-___env___
- **Connection**: ___account.region___.snowflakecomputing.com
- **Users**: admin, LOADER_USER, TRANSFORMER_USER, REPORTER_USER
- **Roles**: PUBLIC, LOADER_ROLE, TRANSFORMER_ROLE, REPORTER_ROLE
- **Warehouses**: LANDING_ZONE, TRANSFORMING_WH, REPORTING_WH
- **Databases**: DEV_RAW_DB, DEV_GOLD_DB
- **Schemas**: LANDING_ZONE, ANALYTICS
  - **Tables**: SUBSCRIPTION
  - **Stages**: SUBSCRIPTION_STAGE
  - **Pipes**: PIPE_SUBSCRIPTION
 
### Destroy Infrastructure
    #Destroy Infrastructure: AWS S3, Snowflake Warehouses, Database, Schemas, Users, Roles and Grants
    ENVIRONMENT=dev make destroy-all
