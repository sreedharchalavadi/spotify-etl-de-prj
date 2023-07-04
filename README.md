# Spotify End to End Data Engineering Project

### Introduction
In this project we will build an ETL(Extract Transform Load) pipeline using the Spotify API on AWS. The pipeline will retrieve the data from the Spotify API, Transform it to the desired format, and load it into the AWS data storage.

### Architecture
![Architecture diagram](https://github.com/sreedharchalavadi/spotify-etl-de-prj/blob/main/spotify_de_architecture_diagram.png)

### About Dataset API
This API contains the information about the Albums, songs and artists.
[Spotify API Documentation](https://developer.spotify.com/documentation/web-api)

### Services used
1. **S3 (Simple Storage Service) :** Its an
   
2. **AWS Lambda :**

3. **Cloud Watch :**

4. **Glue Crawler :**

5. **Data  Catalog :** 

6. **Amazon Athena : **

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
