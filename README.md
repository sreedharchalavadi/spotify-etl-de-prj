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

#### Steps Executed
Every week we need to extract the top global songs to understand what kind of songs are trending. Extracted data from spotify api using python.

1. Register our application in spotify api for the data sources, get the client id and the secret key. Used the package spotipy to extract data from spotify api. 

2. Created the buckets and folders as follows.
    Bucket : sree-spotify-etl-project
    raw    : sree-spotify-etl-project/raw_data/to_be_processed/
    		     sree-spotify-etl-project/raw_data/processed/
		 
    Transformed folders : sree-spotify-etl-project/transformed_data/album_data
                          sree-spotify-etl-project/transformed_data/songs_data
                          sree-spotify-etl-project/transformed_data/artists_data
					  
3. Deployed the code to the aws lambda , so that it will extract data from the spotify api. We have used the spotipy layer in lambda to read data from spotify api. Added the cloud watch trigger to run the code on daily basis, This will extract the data and store it in s3.

    lambda function : spotify_api_data_extract.
    The above data is stored as it is in the raw folder 
    Folder : sree-spotify-etl-project/raw_data/to_be_processed/

4. Created one more lambda function. Whenver there is a data in the raw folder of s3. We will trigger this transformation code, in this code following things will be performed.

    --> created 3 functions to extract the album_data,songs_data and artists_data in a 3 lists.
    --> Converted these 3 lists into dataframes.
    --> All these dataframes are stored in the csv format and stored in the following folders respectively
    sree-spotify-etl-project/transformed_data/
    album_data
    songs_data
    artists_data

    --> Move the raw data from the to_be_processed/  to the processed folder.
    This can be done by copying the data to the processed folder and delete from the to_be_processed/ folder.
    lambda function : spotify_transformation_load_function

5. Once we have data in transformed folder in s3, we will create the 3 glue crawler job to create the the glue catalog tables on top of this folders. We can query this data using Athena. 

    spotify_album_crawler
    spotify_artist_crawler
    spotify_song_crawler

6. Data analysis quries in athena.
    SELECT count(*) FROM "spotify_db"."album_data" limit 10;
    select name, sum(total_tracks) FROM "spotify_db"."album_data" group by name order by sum("total_tracks") desc;
    SELECT count(*) FROM "spotify_db"."artist_data" limit 10;
    SELECT count(*) FROM "spotify_db"."songs_data" limit 10;


