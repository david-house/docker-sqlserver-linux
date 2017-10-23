# docker-sqlserver-linux
How to set up MS SQL Server on a Linux Docker container with a persisted sample database. Example using Ubuntu 16.04, Docker, SQL Server 2017, and World Wide Importers sample database.

### Install Ubuntu 16.04 LTS

https://help.ubuntu.com/lts/installation-guide/index.html

### Install Docker
See the instructions from Digital Ocean here. https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04

You might be tempted to use `apt-get install` without following the above instructions, but be aware that the Docker version in the official Ubuntu repository can be very old.

### Create a persistent directory to store databases

```
sudo mkdir /var/sqldata
```

### Start the Docker container
The argument flags used:
```
 -e: Set environment variable
 -p: Map ports host:container
 -v: Map directories host:container
 -d: Run container in detached (vs. interactive shell) mode
 ```
 
```
sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=yourStrong(!)Password' -p 1433:1433 -v /var/sqldata:/var/sqldata -d microsoft/mssql-server-linux:2017-latest
```

### Download the World Wide Importers sample database backup file
```
cd /var/sqlata
sudo wget "https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak"
```

### Verify and restore the backup from SQL Server Management Studio (SSMS)

From your SSMS client, connect to the new server and open a query window in the `master` database.

Verify the backup
```
RESTORE VERIFYONLY FROM DISK = '/var/sqldata/WideWorldImporters-Full.bak'
```

List the database files in the backup
```
RESTORE FILELISTONLY FROM DISK = '/var/sqldata/WideWorldImporters-Full.bak'
```

Restore the database in the new /var/sqldata directory you created
```
RESTORE DATABASE WWI FROM DISK = '/var/sqldata/WideWorldImporters-Full.bak'
WITH 
MOVE 'WWI_Primary' TO '/var/sqldata/WideWorldImporters.mdf',
MOVE 'WWI_UserData' TO '/var/sqldata/WideWorldImporters_UserData.ndf',
MOVE 'WWI_Log' TO '/var/sqldata/WideWorldImporters.ldf',
MOVE 'WWI_InMemory_Data_1' TO '/var/sqldata/WideWorldImporters_InMemory_Data_1'
```

### To connect via SSMS

Use a TCP connection and specify the port. For example:
```
tcp:HOSTNAME,1433
```

Note that using the SQL Configuration Manager on the client you can assign an alias to use for linked server setup.

### Enjoy your minty fresh copy of the World Wide Importers database running on SQL Server in a Docker container on Ubuntu 16.04





