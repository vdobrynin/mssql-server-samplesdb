<div class="social_links">
    <a href="https://www.clouddataninjas.com"><img src="https://img.shields.io/website?down_color=red&down_message=down&label=clouddataninjas.com&up_color=46C018&url=https%3A%2F%2Fwww.clouddataninjas.com&style=for-the-badge" alt="Cloud Data Ninjas"></a>
    <a href="https://github.com/enriquecatala" target="_blank"><img  src="https://img.shields.io/github/followers/enriquecatala?label=GitHub&style=for-the-badge" alt="GitHub followers of Enrique Catalá" ></a>
    <a href="https://github.com/sponsors/enriquecatala" target="_blank"><img src="https://img.shields.io/badge/GitHub_Sponsors--_.svg?style=for-the-badge&logo=github&logoColor=EA4AAA" alt="Sponsor Enrique Catalá on GitHub" ></a>    
    <a href="https://www.linkedin.com/in/enriquecatala" target="_blank"><img src="https://img.shields.io/badge/LinkedIn--_.svg?style=for-the-badge&logo=linkedin" alt="LinkedIn Enrique Catalá" ></a>        
    <a href="https://twitter.com/enriquecatala" target="_blank"><img src="https://img.shields.io/twitter/follow/enriquecatala?color=blue&label=twitter&style=for-the-badge" alt="Twitter @enriquecatala" ></a>    
    <a href="https://enriquecatala.com"><img src="https://img.shields.io/website?down_color=red&down_message=down&label=enriquecatala.com&up_color=46C018&url=https%3A%2F%2Fenriquecatala.com&style=for-the-badge" alt="Data Engineering with Enrique Catalá"></a>
    <a href="https://youtube.com/enriquecatala"><img src="https://raw.githubusercontent.com/enriquecatala/enriquecatala/master/img/youtube.png" alt="Canal de Enrique Catalá" height=20></a>
</div> 

<div style="display: flex; align-items: center; justify-content: center;">
  <a href="https://www.credly.com/badges/cde0dbd2-8d03-4ca7-8284-d471d65d0e5f">
      <img src="https://raw.githubusercontent.com/enriquecatala/enriquecatala/master/img/MVP_Logo_horizontal.png" 
           alt="Microsoft DataPlatform and AI MVP Enrique Catalá"
           style="min-height: 50px; max-height: 70px; min-width: 100px">
  </a>
  <a href="https://www.clouddataninjas.com">
          <img src="https://raw.githubusercontent.com/enriquecatala/enriquecatala.github.io/master/img/CLOUDDATANINJAS.png" 
          alt="Cloud Data Ninjas" 
          style="min-height: 50px; max-height: 70px; min-width: 250px "/>
  </a>
</div>

# mssql-server-samplesdb

Easily deploy a Docker instance with SQL Server and all Microsoft Sample databases. Choose between stateless or stateful deployment for your SQL Server needs. Perfect for developers and DBAs looking for a quick and reliable database setup.


## Quick Start

Run the image instantly with `make all`. For more control use the following commands:

```bash
make prerequisites # setup
make build         # build the image
docker compose up  # run the image
```

Connect to your SQL Server instance at `localhost,14330` using `sa` and `PaSSw0rd` (configurable in `docker-compose.yml`).

**Table of Contents:**
- [mssql-server-samplesdb](#mssql-server-samplesdb)
  - [Quick Start](#quick-start)
  - [Features](#features)
  - [Installation](#installation)
    - [Prerequisites](#prerequisites)
  - [Steps](#steps)
  - [How to run the image](#how-to-run-the-image)
    - [Databases included](#databases-included)
  - [Enable all databases](#enable-all-databases)
  - [Stateless deployment](#stateless-deployment)
  - [Stateful deployment](#stateful-deployment)
    - [Permissions](#permissions)
      - [Setting Up Local Directories for Container Mounts](#setting-up-local-directories-for-container-mounts)
      - [How to Use the Script](#how-to-use-the-script)
    - [Force Attach (optional)](#force-attach-optional)
  - [Customization](#customization)
    - [How to change the SQL Server base image](#how-to-change-the-sql-server-base-image)
    - [How to add new databases to the image](#how-to-add-new-databases-to-the-image)
    - [How to change the sa password](#how-to-change-the-sa-password)
  - [FAQ](#faq)
    - [How does it works?](#how-does-it-works)
      - [Restoring databases](#restoring-databases)
        - [Entrypoint](#entrypoint)
      - [Avoid container to stop after deploy](#avoid-container-to-stop-after-deploy)


## Features
- **Easy Setup**: Deploy SQL Server in Docker with a simple command.
- **Multiple Databases**: Includes popular databases like - Northwind, Pubs, and AdventureWorks.
- **Customizable**: Options for stateless and stateful deployment.
- **Community Driven**: Open for contributions and enhancements.

## Installation
### Prerequisites
- Docker
- Make (optional)

## Steps
1) Clone the repository: git clone https://github.com/enriquecatala/mssql-server-samplesdb.
2) Navigate to the directory and run make prerequisites.
3) Build the image: make build.
4) Start the container: docker compose up.


## How to run the image

You can run the image with just executing **`make all`**, but if you want more control, you can execute the following commands:

```bash
# Create the folder where the databases will be restored and download the databases
# into ./Backups folder
# 
make prerequisites

# Build the image
make build

# Run the image
docker compose up
```


Now you can open your favorite SQL Server client and connect to your local SQL Server instance. By default:
- Server localhost,14330
- user:sa
- Password: PaSSw0rd

>NOTE: You can find the credentials in the docker-compose.yml file

```yaml
    environment:
      MSSQL_SA_PASSWORD: "PaSSw0rd"      
    ports:
      - "14330:1433"  
```

### Databases included

Databases included:
- Pubs
- Northwind
- WideWorldImporters
- WideWorldImportersDW
- AdventureWorks2017
- _AdventureWorksDW2017*_
- _AdventureWorks2016*_
- _AdventureWorks2014*_
- _AdventureWorks2012*_
- _StackOverflow2010*_


> NOTE: Databases marked with * must be [switched on during build](#enable-all-databases)

## Enable all databases

Only common databases are deployed by default. To deploy ALL databases in your container, please edit the **[.env](.env)** file and set the following variable to 1:

```bash
INCLUDE_ALL_DATABASES=1
```

```cmd
# to make sure that all databases are deployed, you can execute
make clean
# to build the image and run it
make all
```

**IMPORTANT:** StackOverflow2010 database is huge and it will require a couple of minutes to initialize. Please be patient. You can work and play within the other databases while the StackOverflow database is being prepared

## Stateless deployment

Edit the [docker-compose.yml](./docker-compose.yml) file and comment the following lines:

```yml
#volumes:
#      - ${LOCAL_MOUNTPOINT}:/var/opt/mssql/data
```
>NOTE: Doing that, will disable mounting the local folder specified in the **[.env](.env)** file

Then, you can create and run the image with the following command:

```cmd
docker compose up --build
```


**IMPORTANT:** StackOverflow2010 database is huge and it will require a couple of minutes to initialize. Please be patient. You can work and play within the other databases while the StackOverflow database is being prepared

## Stateful deployment

With the [docker-compose.yml](./docker-compose.yml) file you will deploy all databases in a persistent folder in the host (remind to configure the [.env](/.env) file with a valid local folder):

- LOCAL_MOUNTPOINT 

   The folder must exist ( for example: /home/enrique/your/path/to/volume/)

- SHARED_FOLDER

   The folder must exists. This shared folder can be used for example, to deploy backups or easily copy-paste between container and host

> IMPORTANT: There is some kind of bug with WSL2 and if you want to use stateful deployment, you need to start your container inside the wsl2 image. You cant execute docker-compose up from windows

### Permissions

When working with Docker containers that mount local volumes, managing file and directory permissions is crucial. These permissions ensure that the container has the appropriate access rights to the data stored on these volumes. To simplify this process, we have a script named [prerequisites.create_local_directories.sh](./prerequisites.create_local_directories.sh) that automatically sets up the necessary directories and permissions.

>NOTE: This is automatically done when you execute **`make prerequisites`**

#### Setting Up Local Directories for Container Mounts

The [prerequisites.create_local_directories.sh](./prerequisites.create_local_directories.sh) script is designed to create local directories and configure their permissions to match the requirements of the Docker container. By running this script, you avoid the manual process of setting up these directories and permissions.

To understand what the script does, here's an overview of the steps involved:

1. **Create Local Directories**: The script creates directories on your host system that will be mounted into the Docker container. This includes data and shared folders.

    ```bash
    mkdir -p ./local_mountpoint/data/
    mkdir -p ./local_mountpoint/shared_folder/
    ```

2. **Set Ownership**: It changes the ownership of these directories to the user ID (`UID`) and group ID (`GID`) that the SQL Server in the Docker container runs as. This is typically `UID 10001` and `GID 0`.

    ```bash
    sudo chown 10001:0 ./local_mountpoint/data/
    sudo chown 10001:0 ./local_mountpoint/shared_folder/
    ```

3. **Adjust Permissions**: The script sets the necessary read, write, and execute permissions on these directories to ensure that the container can access and modify the data as required.

    ```bash
    sudo chmod +rwx ./local_mountpoint/data/
    sudo chmod +rwx ./local_mountpoint/shared_folder/
    ```

#### How to Use the Script

>NOTE: This is automatically done when you execute **`make prerequisites`**


Simply run the `prerequisites.create_local_directories.sh` script to automatically set up the directories and permissions:

```bash
./prerequisites.create_local_directories.sh
```

This approach streamlines the setup process and ensures consistency in the permissions, allowing your Docker container to function correctly with the mounted volumes.

And now, in the docker-compose.yml, you can reference that path, for example

```yaml
    volumes:
       - ${LOCAL_MOUNTPOINT}:/var/opt/mssql/data
```

Now, when you start the container, you will see how the files are deployed locally 

```bash
mssql-server-samplesdb | 2020-05-25 16:23:11.74 Server      Setup step is copying system data file 'C:\templatedata\master.mdf' to '/var/opt/mssql/data/master.mdf'.
2020-05-25 16:23:12.05 Server      Did not find an existing master data file /var/opt/mssql/data/master.mdf, copying the missing default master and other system database files. If you have moved the database location, but not moved the database files, startup may fail. To repair: shutdown SQL Server, move the master database to configured location, and restart.
2020-05-25 16:23:12.11 Server      Setup step is copying system data file 'C:\templatedata\mastlog.ldf' to '/var/opt/mssql/data/mastlog.ldf'.
2020-05-25 16:23:12.15 Server      Setup step is copying system data file 'C:\templatedata\model.mdf' to '/var/opt/mssql/data/model.mdf'.
....
```


### Force Attach (optional)

>NOTE: This is a hack for anyone who is still using Windows10 with WSL2 (win11 is fixed)

- FORCE_ATTACH_IF_MDF_EXISTS

  1 -> if you don´t want to "restore" and the files exists, you can attach those databases
  0 -> if you did´nt executed docker-compose down, you can still "up" your container with previously restored databases

You can create and run the image with the following command:

```cmd
docker compose up --build
```

## Customization

### How to change the SQL Server base image

The [Dockerfile](./Dockerfile) specifies which base SQL Server Instance you want to use for your image. 

In case you want to change the version of the SQL Server used, please go edit the first line of the [Dockerfile](./Dockerfile)  and select your prefered version. For example

Change 

```docker
FROM mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

To

```docker
FROM mcr.microsoft.com/mssql/server:2017-latest-ubuntu
```

To get the latest SQL Server 2017 version with applied CU

__NOTE:__ To see which SQL Server versions, please go [here](https://hub.docker.com/_/microsoft-mssql-server) and select your "tag"

### How to add new databases to the image

It´s as easy as modifying the [Dockerfile](./Dockerfile), and adding the new backups you want to restore, and modifying the [setup.sql](./setup.sql) file with the RESTORE command.

### How to change the sa password

The password for the "sa" account is specified at the [docker-compose.yml](./docker-compose.yml) file.

## FAQ
### How does it works?

[![deploy sql server in docker with mssql-server-samplesdb](http://img.youtube.com/vi/ULL5nntWn1A/0.jpg)](http://www.youtube.com/watch?v=ULL5nntWn1A "mssql-server-samplesdb")


> NOTE: If you want me to make a translation of this video to english, please show me  a little of your support! and when I reach 150€ I´ll do it!  <a href="https://github.com/sponsors/enriquecatala"><img src="https://img.shields.io/badge/GitHub_Sponsors--_.svg?style=social&logo=github&logoColor=EA4AAA" alt="GitHub Sponsors"></a> 

As you can see, its a little tricky but when you find how it works, its very simple and stable:

[Dockerfile](/Dockerfile) makes 3 mayor steps

#### Restoring databases

This is the tricky part since involves 2 scripts and the final command to keep alive the image

##### Entrypoint

```docker
COPY setup.* ./
COPY entrypoint.sh ./

RUN chmod +x setup.sh
RUN chmod +x entrypoint.sh

# This entrypoint start sql server, restores data and waits infinitely
ENTRYPOINT ["./entrypoint.sh"]
```

#### Avoid container to stop after deploy

To avoid the container to stop after first run, you need to ensure that is waiting for something. the best solution is to add a sleep infinity...as simple as it sounds :)

```docker
CMD ["sleep infinity"]
```

# mssql-server-samplesdb
