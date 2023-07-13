# Spotify End to End Data Engineering Project

### Introduction
In this project I have built an ETL(Extract Transform Load) pipeline using the Spotify API on AWS. The pipeline will retrieve the data from the Spotify API, Transform it to the desired format, and load it into the AWS data storage.

### Architecture
![Architecture diagram](https://github.com/sreedharchalavadi/spotify-etl-de-prj/blob/main/spotify_de_architecture_diagram.png)

### About Dataset API
This API contains the information about the Albums, songs and artists.
[Spotify API Documentation](https://developer.spotify.com/documentation/web-api)

### Services used
1. **AWS S3 (Simple Storage Service) :** Its an object storage service that provides manufacturing scalability, data availability, security, and performance. Its a highly scalable object storage service that can store and retrieve any amount of data from anywhere on the web.It is commonly used to store and distribute large media files, data backups and static website files.
2. **AWS IAM :** This is nothing but identity and access management which enables us to manage access to AWS services and resources securely.
3. **AWS Glue :** A serverless data integration service that makes it easy to discover, prepare, and combine data for analytics, machine learning, and application development.
4. **AWS Glue Crawler :** AWS Glue Crawler is a fully managed service that automatically crawls the data sources, identifies data formats and infers schemas to create an AWS Glue Data Catalog.
5. **AWS Glue Data Catalog :** AWS Glue Data Catalog is a fully managed metadata repository that makes it easy to discover and manage data in AWS. We can use the Glue Data Catalog with other AWS services, such as Athena
6. **AWS Lambda :** Lambda is a computing service that allows programmers to run code without creating or managing servers.We can use lambda to run code in response to events like changes in s3, DynamoDB, or other AWS services
7. **AWS Athena :** Athena is an interactive query service that makes it easy to analyze data in amazon s3 using standard SQL. We can use Athena to analyze data in Glue Data catalog or in other s3 buckets
8. **AWS Cloud Watch :** Amazon cloudwatch is a monitoring service for AWS resources and the applications that run on them. We can use CloudWatch to collect and track metrics, collect and monitor log files and set alarms.
9. **AWS Glue Studio ETL (Extract, Transform, Load) :** These jobs are a visual interface within AWS Glue that enables users to design and build data transformation workflows without writing code. It simplifies the process of data extraction, transformation, and loading by providing a drag-and-drop interface for creating and managing ETL pipelines.

### Installed Packages

```
!pip install spotipy
!pip install pandas
!pip install numpy
!pip install bs4
!pip install requests
```

### Project Execution Flow
Extract data from API  --> Lambda Trigger (every 1 hour) --> Run extract code --> Store Raw data --> Trigger Transform function --> Transform the data and load it to s3 --> Glue crawler to create the data catalogs on s3 --> Query using Athena
