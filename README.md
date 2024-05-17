# Stock Market Kafka Real-Time Data Engineering Project

## Introduction
In this project, I executes an end-to-end data engineering project on Real-Time Stock Market Data using Kafka. The goal is to build a data pipeline that handles real-time stock market data efficiently and effectively.

 I used various technologies such as Python, Amazon Web Services (AWS), Apache Kafka, Glue, Athena, and SQL to accomplish this project.

## Architecture

![Architecture](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/eadc0d81-3f5d-4027-8347-8c85ae033ade)


## Technology Used  
**Programming Language**: 
- Python
    (Python will be used for scripting and automating various parts of the data pipeline)  

**Amazon Web Services (AWS)**: 

- S3 (Simple Storage Service): For storing raw and processed data.  

- Athena: For querying the processed data.  

- Glue Crawler: To create and update the data catalog.

- Glue Catalog: To maintain metadata about the data stored in S3. 

- EC2: For running Kafka and other processing jobs.  

**Apache Kafka**

  - Kafka will be used for real-time data streaming and processing.

  ![Apache kafka](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/20c0c7a3-3f68-4ad4-9c45-be2ea86781ff)

#### Apache Kafka Workflow Engine

  ![kafka work flow Engine](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/6f7436a4-65b8-473d-ba0f-8ce8bb90cb34)


## Dataset
The dataset used in this project : Stock Market Data


## Project Description
This project involves setting up a real-time data pipeline for stock market data using Apache Kafka, AWS services, and Python. The pipeline will collect, process, and store data, allowing for efficient querying and analysis.


## Use Cases
1. Real-Time Data Collection
    - Using Kafka to stream stock market data in real-time.
2. Data Processing
    - Utilizing Python scripts to process the incoming data.
3. Data Storage
    - Storing raw and processed data in AWS S3.
4. Data Cataloging
    - Using Glue Crawler and Glue Catalog to manage metadata.
5. Data Querying
    - Querying processed data using AWS Athena.

## Detailed Steps
### 1. Real-Time Data Collection with Kafka
- Set up a Kafka cluster on AWS EC2.

  ![Screenshot (182)](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/e07df166-9f65-4014-81d9-1a6ebf6de77b)

- Create Kafka topics for stock market data.
  ### Producer
  
    ![Screenshot (175)](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/d789fdc9-a16c-475c-9516-86ae9e57fbde)

  ### Consumer
  
    ![Screenshot (176)](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/ed624edc-8b47-4ea7-89ec-d225a3fe086b)

- Produce data to Kafka topics using Python.

    ![Code Producer](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/43e71e96-10bd-4b26-ba26-56893e574ea8)


### 2. Data Processing with Python
- Consume data from Kafka topics using Python scripts.
- Process and transform the data.
- Store processed data in AWS S3.

### 3. Data Storage in AWS S3
- Create S3 buckets for raw and processed data.

    ![Screenshot (179)](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/c750967b-ac2b-4e8b-9cc9-4d65d8850f0b)

- Store incoming raw data and processed data in respective buckets.

### 4. Data Cataloging with AWS Glue
- Use Glue Crawler to scan S3 buckets and create/update the data catalog.
- Manage metadata using Glue Catalog.

  ![image](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/22a77d10-03d1-4a5d-aaa9-0800537fca6a)


### 5. Data Querying with AWS Athena
- Configure Athena to query processed data in S3.
- Use SQL queries for data analysis and insights.

    ![2ss](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/87f1ccfd-12bf-4fea-bfbb-843f8a2c9805)

  
## Conclusion
This project provides a comprehensive approach to handling Real-Time Stock Market Data using a combination of Apache Kafka, AWS services, and Python, SQL.


  ![image](https://github.com/izhangit/Stock-Market-Real-Time-Data-Analysis/assets/108143680/045caa6f-6823-4957-a5ba-0c4eb8332106)
