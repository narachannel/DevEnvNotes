# Middleware configuration

## SQL Server on Docker with persistent volume

```shell
$VOLUME_DIRECTORY='' # Volume directory on WSL2
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Passw0rd" -p 1433:1433 --name dockersql -v "$VOLUME_DIRECTORY:/var/opt/mssql"  -d mcr.microsoft.com/mssql/server:2019-CU11-ubuntu-20.04

sudo chown 10001 $VOLUME_DIRECTORY # Docker file specifies this uid as mssql user
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

```powershell
docker run -d --name mongo-container -p 27017:27017 `
-e MONGO_INITDB_ROOT_USERNAME=mongoadmin `
-e MONGO_INITDB_ROOT_PASSWORD=secret mongo
```

## Redis

```shell
docker run -d --name redisdocker -p 6379:6379 redis
sudo apt-get install redis-server
```

## MySQL

```shell
docker run --name dockermysql -v /home/narachannel/ContainerData/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=Passw0rd! -d -p 3306:3306 -p 33060:33060 mysql:latest 
```

Be careful with `-p` location. `-p` can be recognized as MySQL's command line option.

```txt
2022-07-31 14:31:27+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.30-1.el8 started.
2022-07-31 14:31:27+00:00 [ERROR] [Entrypoint]: mysqld failed while attempting to check config
        command was: mysqld -p 3306:3306 --verbose --help --log-bin-index=/tmp/tmp.dwUYhZoebN
        Enter password: mysqld: Can not perform keyring migration : Invalid --keyring-migration-source option.
2022-07-31T14:31:27.436424Z 0 [ERROR] [MY-011084] [Server] Keyring migration failed.
2022-07-31T14:31:27.437251Z 0 [ERROR] [MY-010119] [Server] Aborting
```

```sql
create database newdb;
use newdb;
create table newtable;
```

## Azure Storage with Azurite

## RabbitMQ

[Tutorial](https://www.rabbitmq.com/tutorials/tutorial-one-dotnet.html)
[API Reference](https://www.rabbitmq.com/dotnet-api-guide.html#major-api-elements)

```shell
docker run -d --hostname dockerrabbit --name dockerrabbit -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

Go to [management](http://localhost:15672). You can log in with `guest / guest`

## Neo4j

```shell
docker run -p 7474:7474 -p 7687:7687 --volume="C:\ContainerData\neo4j:/data" neo4j
```

## Cassandra

[Official](http://cassandra.apache.org/doc/latest/getting_started/index.html)
[Docker Hub](https://hub.docker.com/_/cassandra?tab=description)

```shell
docker run --name docker-cassandra -p 7000:7000 -p 7001:7001 -p 7199:7199 -p 9042:9042 -p 9160:9160 -d cassandra:latest
```

default username : cassandra / password : cassandra

## Docker Compose

## Postgres

## Kafka

Clone repo from [Github](https://github.com/wurstmeister/kafka-docker)

```shell
docker-compose up -d
```

## Cockroach DB

[Official](https://www.cockroachlabs.com/docs/stable/orchestrate-a-local-cluster-with-kubernetes)

How to install on local Kubernetes cluster

```shell
kubectl apply -f https://raw.githubusercontent.com/cockroachdb/cockroach-operator/v2.1.0/config/crd/bases/crdb.cockroachlabs.com_crdbclusters.yaml
kubectl apply -f https://raw.githubusercontent.com/cockroachdb/cockroach-operator/v2.1.0/manifests/operator.yaml
curl -O https://raw.githubusercontent.com/cockroachdb/cockroach-operator/v2.1.0/examples/example.yaml
kubectl apply -f example.yaml
kubectl create \
-f https://raw.githubusercontent.com/cockroachdb/cockroach-operator/master/examples/client-secure-operator.yaml
kubectl exec -it cockroachdb-client-secure -- ./cockroach sql --certs-dir=/cockroach/cockroach-certs --host=cockroachdb-public
# Run Some SQL to create dattabase, grant user, etc.
kubectl port-forward service/cockroachdb-public 8080 26257
```
