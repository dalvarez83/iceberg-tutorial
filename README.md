Iceberg Tutorial

This repo is to run a quick demo for how to spin up an Apache Iceberg application.

Overview

Apache Iceberg is a table format specification. It defines a standard for how metadata is 
written in tables. It includes a set of APIs and libraries for interaction with that 
table format specification. These libraries are leveraged in other engines and tools 
that allow them to interact with Iceberg tables. For example, scanning and performing 
analytics of actual files is done by an analytics tools (i.e. Spark, SQL).

Iceberg is not:
- a storage engine
- an execution engine
- a service
- a file format 


This repository is structured as follows:

Project Organization
------------

    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make train`
    ├── README.md          <- The top-level README for developers using this project
    ├── docker-compose.yml <- Specifies the services, networks and ports for launching applications
    ├── docs               <- documents and images
    						  
    						  
    

--------


##  Setup
---
## You will need Docker installed. If not already installed, simply head over to 
## docker.com and install it. 

`Git clone` this repository and execute commands the main folder in this repository.

```
cd iceberg-tutorial
```

## Setting Up Dremio/Nessie/Minio

Spin up dremio
```
docker-compose up dremio
```

After a few minutes, you can access Dremio in your browser at localhost:9047. 
While Dremio starts up, open another terminal window and create your storage layer with 
Minio with the following command:

```
docker-compose up minio
```

Then start up a Nessie server which will be the catalog to track our Apache Iceberg 
tables.


```
docker-compose up nessie
```

Once all three are up and running, head over to localhost:9000 to log in to Minio.

####This step runs in Safari browser currently

Enter username: `admin`
Enter password: `password`
 

From the menu on the left, click on the `buckets` section and create a bucket named `warehouse`.

Minio is an S3-compatible storage layer, so a bucket is essentially where you can save 
files in object storage solutions like S3 and Minio.

Then head over to localhost:9047 and set up your Dremio account until you get to the 
Dremio dashboard.

Then click on “add source” and select `Nessie`.

- Set the name of the source to `nessie`
- Set the endpoint URL to `http://nessie:19120/api/v2`
- Set the authentication to `none`

- Navigate to the storage tab, by clicking on “storage” on the left
- For your access key, set `admin`
- For your secret key, set `password`
- Set root path to `/warehouse`
- Set the following connection properties:
- `fs.s3a.path.style.access` to "true"
- `fs.s3a.endpoint` to "minio:9000"
- `dremio.s3.compat` to "true"
- Uncheck "encrypt connection" (since our local Nessie instance is running on http)

![New Nessie Source]("/docs/new-nessie-source.png")
