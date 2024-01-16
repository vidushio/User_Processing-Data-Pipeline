
# User Processing DAG with Apache Airflow

This repository contains an Apache Airflow DAG for processing user data obtained from an API and storing it in a PostgreSQL database.

## Overview

The DAG (user_processing.py) automates the extraction of user data from an API, processing the information, and storing it in a PostgreSQL database. Below are the key components of the DAG:

#### DAG Definition: 
The DAG is named 'user_processing' and is scheduled to run daily, starting from January 1, 2022, with no catch-up.

#### Tasks:

create_table: Creates a PostgreSQL table named "users" with columns for first name, last name, country, username, password, and email.

is_api_available: Checks the availability of the API by making an HTTP request to the '/api/' endpoint.

extract_user: Fetches user data from the API using an HTTP GET request to the '/api/' endpoint. The response is filtered and converted to JSON.

process_user: Python task that processes the user data received from the API. It uses the Pandas library to normalize the JSON data and extracts specific fields (first name, last name, country, username, password, and email). The processed data is then saved to a CSV file.

store_user: Uses a PostgreSQL hook to copy the processed user data from the CSV file into the "users" table in the PostgreSQL database.

## Getting Started

#### Prerequisites

Apache Airflow installed.

PostgreSQL database with appropriate credentials.

Python libraries specified in requirements.txt installed.

#### Configuration

Set up your Airflow environment by defining connections and variables. Ensure the following:

PostgreSQL Connection (postgres): Configure the connection with the necessary credentials for the PostgreSQL database.

API Connection (user_api): Establish a connection with the API by providing the required details in the Airflow UI.

#### Airflow and DAG Definition

In Apache Airflow, a DAG represents a workflow and is defined using Python scripts. 

#### Usage

Place the DAG file (user_processing.py) in the Airflow DAGs directory.

Trigger the DAG through the Airflow UI or using the Airflow CLI.

## DAG scheduled

The DAG is scheduled to run daily, starting from January 1, 2022, with no catch-up.

## Pipeline Execution and Technical Flow

The User Processing pipeline is orchestrated through an Apache Airflow DAG, comprising multiple tasks to seamlessly handle the extraction, transformation, and loading of user data. The technical flow of the pipeline can be broken down into the following key steps:

#### Table Creation (create_table):

The pipeline begins by ensuring the existence of a PostgreSQL table named "users." This task defines the table schema with columns for first name, last name, country, username, password, and email. If the table doesn't exist, it is created to store the processed user data.

#### API Availability Check (is_api_available):

Before extracting user data from the API, the pipeline checks the availability of the API by making an HTTP request to the '/api/' endpoint. This ensures that the subsequent extraction process occurs only when the API is accessible.

#### User Data Extraction (extract_user):

Upon confirming the availability of the API, the pipeline proceeds to fetch user data by making an HTTP GET request to the '/api/' endpoint. The response, which is in JSON format, is then filtered and processed to extract relevant user information.

#### User Data Processing (process_user):

The extracted user data, in JSON format, undergoes processing in a Python task. Using the Pandas library, the data is normalized to a tabular structure, extracting specific fields such as first name, last name, country, username, password, and email. The processed data is then saved to a CSV file for further use.

#### User Data Storage (store_user):

Finally, the processed user data stored in the CSV file is loaded into the PostgreSQL database. This is achieved using the PostgresHook, which allows the pipeline to efficiently copy the data into the "users" table with the specified schema.

The tasks are interconnected in a sequential order, ensuring that each task is executed only after the successful completion of its predecessor. This orchestrated flow guarantees the integrity and consistency of the data processing pipeline. Connections to the PostgreSQL database and the user API, established in the Airflow environment, enable seamless interaction with external systems.

## Conclusion

The User Processing DAG automates the extraction, processing, and storage of user data from an API into a PostgreSQL database. The defined tasks ensure a seamless flow of data through the pipeline, providing a robust solution for daily user data updates.
