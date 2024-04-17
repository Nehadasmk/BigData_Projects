# Real-Time Sales Forecasting Platform

## Overview

This project implements a real-time sales forecasting platform utilizing various AWS services for efficient data processing and analysis, utilizing DynamoDB for instant data storage, Kinesis for efficient streaming, S3 for scalable storage, and Athena for dynamic on-the-fly analysis.

## Architecture

1. Mock Data Generation

Generate mock sales data and store it in DynamoDB for instant data storage.

2. DynamoDB Stream to EventBridge

DynamoDB Streams capture changes to the data in real-time and publish these changes to an EventBridge pipe. This ensures that any updates, inserts, or deletions in the sales data are immediately propagated throughout the system.

3. Kinesis Stream Consumption

Kinesis streams efficiently consume the data changes from the EventBridge pipe, ensuring that the system can handle large volumes of data and process it in real-time without delays.

4. Lambda for Transformation and Filtering

Lambda functions are triggered by Kinesis streams to apply sophisticated transformations and filtering on the data. In this case, the Lambda function is specifically designed to handle records from DynamoDB Streams. Here's what it does:

    a. Decoding and Transformation: 
    The Lambda function decodes the base64 encoded data and transforms it into a JSON format.

    b. Data Extraction: It accesses specific data within the DynamoDB record, such as order ID, product name, quantity, and price.

    c. Transformation: The function then transforms the extracted data into a desired format, possibly changing data types or structure.

    d. Encoding and Output: Finally, it encodes the transformed data back to base64 and prepares it to be forwarded to the next stage of the pipeline.

5. Kinesis Firehose for Batching

Kinesis Firehose efficiently batches the transformed data and delivers it to an S3 location. By batching the data, Firehose optimizes the storage and delivery process, reducing costs and improving performance.

6. Crawler for S3 Updates

that the system stays up-to-date with any changes in the data schema or structure, allowing for seamless adaptation and maintenance.

7. Athena for Dynamic Analysis

Athena enables dynamic, on-the-fly analysis of the sales data stored in S3. With Athena, users can run SQL queries against the data without needing to set up complex data warehouses or infrastructure, enabling quick and insightful data exploration.

8. Glue Catalog for Metadata Storage

The Glue Catalog stores metadata about the data stored in S3, including schema information, partitioning details, and data formats. This metadata management ensures that the data is properly organized and cataloged, making it easy to query and analyze

![Product_sale_projection](https://github.com/Nehadasmk/BigData_Projects/blob/main/ProductSaleProjection/images/ProductionSaleIngestion.drawio.png)
