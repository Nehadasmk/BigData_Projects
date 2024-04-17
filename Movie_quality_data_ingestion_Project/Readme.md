# Quality Movie Data Ingestion

## Overview

The "Quality Movie Data Ingestion" project involves batch processing of data retrieved from IMDB. The processed data is ingested into Amazon Redshift, with a focus on data quality checks throughout the pipeline.

- Built batch pipeline for processing IMDB movie data sourced from Amazon S3 and load it into Redshift for data storage, ensuring accurate and reliable data ingestion.Implemented robust quality checks in the pipeline, guaranteeing precise and reliable ingested data.
- Leveraged AWS Glue for optimized ETL processes, achieving 30% faster data transformation and loading.
- Integrated Amazon SNS (Simple Notification Service) to provide real-time monitoring and notification alerts, ensuring timely detection and resolution of any pipeline issues or anomalies and Athena for data analysis and insights generation.

## Implementation Steps

1. **Data Ingestion from S3:**
   - Consume raw movie data stored in Amazon S3.
   - Retrieve data files from designated S3 buckets.

2. **Data Quality Check (DQC):**
   - Data undergoes a quality check to ensure completeness, accuracy, consistency, and adherence to quality rules.
   - Notification sent to SNS if check fails.
   - Outcome of the check stored in S3.
   - Dataset marked with error status if check fails.

3. **Notification System:**
   - EventBridge configured to capture DQC failure events.
   - Email notifications via SNS triggered for failure events.
   - Detailed error information provided in notification emails.

## Rules and Decision Making

- AWS Glue Data Quality events published to Amazon EventBridge.
- Conditional check: DataQualityEvaluationResult == FAILURE.
- Data routed to either "Output Group" or "Default Group" based on check outcome.

### Output Group

- Dataset with significant errors stored in S3 for further analysis or manual cleaning.

### Default Group

- Dataset with minor errors processed further for loading into Redshift.

## Development Steps

![Alt Text Center](images\ETL-pipeline.png)


1. **S3 Bucket Creation:**
   - Create S3 bucket to store IMDB movie data.
   - Upload relevant data files.

2. **Redshift Cluster Setup:**
   - Create Redshift cluster with two dc2.large nodes.
   - Define IAM roles and policies.
   - Associate IAM roles with the Redshift cluster.

3. **IAM Role Configuration for Glue:**
   - Create IAM role for AWS Glue.
   - Attach AWSGlueServiceRole policy.

4. **Metadata Database Creation:**
   - Create metadata database cataloge in AWS Glue.

5. **Create Crawler for S3 and Redshift:**
   - Set up Glue Crawlers to discover and catalog data.
   - Configure crawlers to populate metadata.

6. **Redshift JDBC Connection:**
   - Establish JDBC connection to Redshift cluster.

7. **S3 VPC Endpoint:**
   - Create VPC endpoint for S3 to allow Redshift access without public internet routing.

8. **Glue VPC Endpoint:**
   - Establish VPC endpoint for Glue to facilitate communication within the same VPC.

9. **CloudWatch VPC Endpoint:**
   - Create VPC endpoint for CloudWatch to enhance monitoring capabilities.

## Working Principle

- VPC endpoints enable secure communication within the same VPC.
- Eliminates the need for public internet access, enhancing security and minimizing latency.

## Troubleshooting

1. **Initial Querying Issue:**
   - Data appears post Glue job execution.

2. **Glue Job Execution:**
   - Fetches data from S3, performs ETL transformations, and loads data into Redshift.

3. **Result Storage in S3:**
   - Successful records loaded into designated folder.
   - Failed records stored in "bad outcomes" folder.

4. **Email Notification:**
   - AWS Glue configured to trigger event on failure.# Quality Movie Data Ingestion
