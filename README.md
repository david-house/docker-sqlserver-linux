# docker-sqlserver-linux
Run MS SQL Server on a Linux Docker container with a persisted sample database
Example using Ubuntu 16.04

### Install Docker
See the instructions from Digital Ocean here. https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04

You might be tempted to use the `apt-get install` but be aware the version level can be severely behind.


```
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=sample_password_1' -p 1433:1433 -d microsoft/mssql-server-linux:2017-latest
```
