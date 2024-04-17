# Real-Time Sales Data Pipeline with Google Cloud Platform (GCP)

## Overview

The E-commerce Store Analysis project implements a scalable Pub/Sub pipeline for real-time e-commerce analysis. It utilizes Google Cloud Platform (GCP) services to optimize storage with Cassandra for efficient retrieval of large-scale e-commerce data. Additionally, the system features a Dead-Letter Queue (DLQ) for payments, capturing and analyzing unmatched messages, thereby enhancing data integrity and reliability.

## Prerequisites

Before setting up and using the system, ensure you have the following prerequisites:

1. **Google Cloud SDK**: Make sure the Google Cloud SDK is installed to access `gcloud` CLI commands.

2. **Python Packages**: Install the required Python packages:

        pip3 install google-cloud-pubsub
        pip3 install cassandra-driver


3. **Docker**: Install Docker for running the Cassandra database container.

## Setup Instructions

Follow these steps to set up the system:

1. **Create Pub/Sub Topics**: Use the following commands to create Pub/Sub topics named `orders_data` and `payments_data` with default subscribers:
gcloud pubsub topics create orders_data
gcloud pubsub topics create payments_data


2. **Authentication**: Authenticate your GCP account from the terminal using the command:
gcloud auth application-default login


3. **Service Account Setup**:

- Create a new service account in the IAM & Admin service.
- Add Pub/Sub producer and Pub/Sub subscriber roles for the service account.
- Create keys for the service account, and download the JSON file.
- Replace the content of `application_default_credentials.json` located at `/Users/username/.config/gcloud/` with the content of the downloaded JSON file.

4. **Start Cassandra Database**: Use Docker to start the Cassandra container by running the following command:

        docker compose -f docker-compose-cassandra.yml up -d

5. **Cassandra Table Creation**: Open the terminal of the Cassandra Docker container, and execute the following commands to create the keyspace and table:
        
        CREATE KEYSPACE IF NOT EXISTS ecom_store WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
        CREATE TABLE ecom_store.orders_payments_facts (
        order_id BIGINT PRIMARY KEY,
        customer_id BIGINT,
        item TEXT,
        quantity BIGINT,
        price DOUBLE,
        shipping_address TEXT,
        order_status TEXT,
        creation_date TEXT,
        payment_id BIGINT,
        payment_method TEXT,
        card_last_four TEXT,
        payment_status TEXT,
        payment_datetime TEXT
        );

## Usage

- **Producer Code**: Use the provided producer code to generate mock data for orders and payments.
- **Dead Letter Queue**: If due to some lag, order details are not available in the fact table, they will be added to the `dead_letter_queue` topic in GCP.

## Architecture Flow

![E-Commerce store](https://github.com/Nehadasmk/BigData_Projects/blob/main/Ecommerce_store_analysis_project/images/ecom_store_analysis.drawio.png)
