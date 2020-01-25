# Middleware configuration

## SQL Server on Docker with persistent volume

```sh
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Passw0rd" -p 1433:1433 --name dockersql -v "C:\ContainerData\mssql:/var/opt/mssql"  -d mcr.microsoft.com/mssql/server:2017-CU8-ubuntu
```

```sql
CREATE DATABASE DockerContainer
SELECT Name from sys.Databases
GO

SELECT Name from sys.Databases
Use DockerContainer

CREATE LOGIN DockerLogin WITH PASSWORD = 'Passw0rd'
GO  

-- Creates a database user for the login created above.  
CREATE USER DockerUser FOR LOGIN DockerLogin
GO  

GRANT ALTER,CONTROL, CREATE SEQUENCE, DELETE, EXECUTE, INSERT, REFERENCES, SELECT, TAKE OWNERSHIP, UPDATE on SCHEMA::dbo to DockerUser
Go
```

You can log in with DockerLogin from Azure Data Studio.

```sql
create table block
(
    id int IDENTITY(1,1) not null,
    hash VARCHAR(128) not null DEFAULT('')
)

select * from block
```

## Mongo DB

```sh
docker run -d --name mongo-container -p 27017:27017 `
-e MONGO_INITDB_ROOT_USERNAME=mongoadmin `
-e MONGO_INITDB_ROOT_PASSWORD=secret mongo
```

## Redis

```sh
docker run -d --name redisdocker -p 6379:6379 redis
```

## Azure Storage with Azurite

## RabbitMQ

[Tutorial](https://www.rabbitmq.com/tutorials/tutorial-one-dotnet.html)
[API Reference](https://www.rabbitmq.com/dotnet-api-guide.html#major-api-elements)

```sh
docker run -d --hostname dockerrabbit --name dockerrabbit -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

Go to [management](http://localhost:15672). You can log in with `guest / guest`

## Neo4j

```sh
docker run -p 7474:7474 -p 7687:7687 --volume="C:\ContainerData\neo4j:/data" neo4j
```

## Cassandra

[Official](http://cassandra.apache.org/doc/latest/getting_started/index.html)
[Docker Hub](https://hub.docker.com/_/cassandra?tab=description)

```sh
docker run --name docker-cassandra -p 7000:7000 -p 7001:7001 -p 7199:7199 -p 9042:9042 -p 9160:9160 -d cassandra:latest
```

default username : cassandra / password : cassandra

## Docker Compose

## Postgres

## Kafka

Clone repo from [Github](https://github.com/wurstmeister/kafka-docker)

```sh
docker-compose up -d
```

## Cockroach DB

[Official](https://www.cockroachlabs.com/docs/stable/start-a-local-cluster-in-docker.html)
