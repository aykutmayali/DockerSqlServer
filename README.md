# DockerSqlServer
# Running MSSQL from Docker Container 

- - **-- Switch to Windows Containers from Docker Desktop 

stop all containers:
docker kill $(docker ps -q)

remove all containers
docker rm $(docker ps -a -q)

remove all docker images
docker rmi $(docker images -q)

docker pull microsoft/mssql-server-windows-developer

docker run --name SQLServer -d -p 1433:1433 -e sa_password=Password_01 -e ACCEPT_EULA=Y microsoft/mssql-server-windows-developer

Stop-Service docker
Stop-service hns
Start-service hns
Start-Service docker
docker network prune

docker run --name SQLServer -d -p 1433:1433 -e sa_password=Password_01 -e ACCEPT_EULA=Y microsoft/mssql-server-windows-developer

- - **-- go advanced "options" and put TrustServerCertificate=True to Additional Connection Parameters [login to SSMS]

docker stop SQLServer
 (docker rm SQLServer) + 
 (docker run --name SQLServer -d -p 1433:1433 -e sa_password=Password_01 -e ACCEPT_EULA=Y microsoft/mssql-server-windows-developer)

docker start SQLServer

- - **-- [from SSMS]
Server Name: 127.0.0.1,1443
Authentication: SQL Server Authentication
Login: SA
Password: whatever password you passed in as an Environment flag.
- - **-- 

docker run --name SQLServer -d -p 1433:1433 --volume c:\Docker\Volumes\SQLServer:c:\SQLData -e sa_password=Password_01 -e ACCEPT_EULA=Y microsoft/mssql-server-windows-developer

docker exec -it SQLServer sqlcmd -S localhost -U sa -P 'Password_01' 
1> create database deneme1
2> go
1> use deneme1
2> go
Changed database context to 'deneme1'.
1> create table dbo.beacon
2> (id int,type varchar(50),mac varchar(50),name varchar(50))
3> go
1> create table dbo.gateway
2> (id int,type varchar(50),mac varchar(50),location varchar(50))
3> go
1> insert into dbo.beacon (id,type,mac,name) values (1,'Unknown','02DDDD66F49D','Beacon1')
2~ go
(1 rows affected)
1> insert into dbo.gateway (id,type,mac,location) values (1,'gateway','AC233FC04A91','Ofis')
2> go

(1 rows affected)
1> select * from dbo.beacon
2> go
id          type                                               mac                                                name
----------- -------------------------------------------------- -------------------------------------------------- --------------------------------------------------
          2 Unknown                                            02DDDD66F49D                                       Beacon2

(1 rows affected)
1> select * from dbo.gateway
2> go
id          type                                               mac                                                location
----------- -------------------------------------------------- -------------------------------------------------- --------------------------------------------------
          1 gateway                                            AC233FC04A91                                       Ofis

(1 rows affected)
1>
------------------------------SSMS CreateDB-----------------------------------------------------------------------
USE [master]
GO

/****** Object:  Database [deneme1]    Script Date: 8/13/2020 3:23:34 PM ******/
CREATE DATABASE [deneme1]
 CONTAINMENT = NONE
 ON  PRIMARY 
( NAME = N'deneme1', FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\deneme1.mdf' , SIZE = 8192KB , MAXSIZE = UNLIMITED, FILEGROWTH = 65536KB )
 LOG ON 
( NAME = N'deneme1_log', FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\deneme1_log.ldf' , SIZE = 8192KB , MAXSIZE = 2048GB , FILEGROWTH = 65536KB )
GO

IF (1 = FULLTEXTSERVICEPROPERTY('IsFullTextInstalled'))
begin
EXEC [deneme1].[dbo].[sp_fulltext_database] @action = 'enable'
end
GO

ALTER DATABASE [deneme1] SET ANSI_NULL_DEFAULT OFF 
GO

ALTER DATABASE [deneme1] SET ANSI_NULLS OFF 
GO

ALTER DATABASE [deneme1] SET ANSI_PADDING OFF 
GO

ALTER DATABASE [deneme1] SET ANSI_WARNINGS OFF 
GO

ALTER DATABASE [deneme1] SET ARITHABORT OFF 
GO

ALTER DATABASE [deneme1] SET AUTO_CLOSE OFF 
GO

ALTER DATABASE [deneme1] SET AUTO_SHRINK OFF 
GO

ALTER DATABASE [deneme1] SET AUTO_UPDATE_STATISTICS ON 
GO

ALTER DATABASE [deneme1] SET CURSOR_CLOSE_ON_COMMIT OFF 
GO

ALTER DATABASE [deneme1] SET CURSOR_DEFAULT  GLOBAL 
GO

ALTER DATABASE [deneme1] SET CONCAT_NULL_YIELDS_NULL OFF 
GO

ALTER DATABASE [deneme1] SET NUMERIC_ROUNDABORT OFF 
GO

ALTER DATABASE [deneme1] SET QUOTED_IDENTIFIER OFF 
GO

ALTER DATABASE [deneme1] SET RECURSIVE_TRIGGERS OFF 
GO

ALTER DATABASE [deneme1] SET  ENABLE_BROKER 
GO

ALTER DATABASE [deneme1] SET AUTO_UPDATE_STATISTICS_ASYNC OFF 
GO

ALTER DATABASE [deneme1] SET DATE_CORRELATION_OPTIMIZATION OFF 
GO

ALTER DATABASE [deneme1] SET TRUSTWORTHY OFF 
GO

ALTER DATABASE [deneme1] SET ALLOW_SNAPSHOT_ISOLATION OFF 
GO

ALTER DATABASE [deneme1] SET PARAMETERIZATION SIMPLE 
GO

ALTER DATABASE [deneme1] SET READ_COMMITTED_SNAPSHOT OFF 
GO

ALTER DATABASE [deneme1] SET HONOR_BROKER_PRIORITY OFF 
GO

ALTER DATABASE [deneme1] SET RECOVERY FULL 
GO

ALTER DATABASE [deneme1] SET  MULTI_USER 
GO

ALTER DATABASE [deneme1] SET PAGE_VERIFY CHECKSUM  
GO

ALTER DATABASE [deneme1] SET DB_CHAINING OFF 
GO

ALTER DATABASE [deneme1] SET FILESTREAM( NON_TRANSACTED_ACCESS = OFF ) 
GO

ALTER DATABASE [deneme1] SET TARGET_RECOVERY_TIME = 60 SECONDS 
GO

ALTER DATABASE [deneme1] SET DELAYED_DURABILITY = DISABLED 
GO

ALTER DATABASE [deneme1] SET QUERY_STORE = OFF
GO

ALTER DATABASE [deneme1] SET  READ_WRITE 
GO








---------------------------------SSMS-CreateDB-------------------------------------------------------
CREATE DATABASE [OctopusDeploy]
 CONTAINMENT = NONE
 ON  PRIMARY
( NAME = N'OctopusDeploy', FILENAME = N'C:\SQLData\OctopusDeploy.mdf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
 LOG ON
( NAME = N'OctopusDeploy_log', FILENAME = N'C:\SQLData\OctopusDeploy_log.ldf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
GO
CREATE DATABASE [TeamCity]
 CONTAINMENT = NONE
 ON  PRIMARY
( NAME = N'TeamCity', FILENAME = N'C:\SQLData\TeamCity.mdf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
 LOG ON
( NAME = N'TeamCity_log', FILENAME = N'C:\SQLData\TeamCity_log.ldf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
GO
---------------------------------SSMS-----------------------------------------------------------

docker stop SQLServer
docker rm SQLServer
$attachDbs = "[{'dbName':'OctopusDeploy','dbFiles':['C:\\SQLData\\OctopusDeploy.mdf','C:\\SQLData\\OctopusDeploy_log.ldf']},{'dbName':'TeamCity','dbFiles':['C:\\SQLData\\TeamCity.mdf','C:\\SQLData\\TeamCity_log.ldf']}]"
docker run --name SQLServer -d -p 1433:1433 --volume c:\Docker\Volumes\SQLServer:c:\SQLData -e sa_password=Password_01 -e ACCEPT_EULA=Y -e attach_dbs=$attachDbs microsoft/mssql-server-windows-developer

---------------------------Docker Compose-----------------------------------------------------
version: '3.7'
services:
  SQLServer:
   image: microsoft/mssql-server-windows-developer
   environment:
     - ACCEPT_EULA=Y
     - SA_PASSWORD=Password_01   
     - attach_dbs=[{'dbName':'OctopusDeploy','dbFiles':['C:\\SQLData\\OctopusDeploy.mdf','C:\\SQLData\\OctopusDeploy_log.ldf']},{'dbName':'TeamCity','dbFiles':['C:\\SQLData\\TeamCity.mdf','C:\\SQLData\\TeamCity_log.ldf']}]
   ports:
     - '1433:1433'
   volumes:
     - c:\Docker\Volumes\SQLServer:c:\SQLData
	 
---------------------------Docker Compose--------------------------------------------------------------------
Set-Location C:\Docker
docker-compose up -d
	 







-----------------------------------------------------
https://dotnetplaybook.com/run-and-connect-to-a-sql-server-docker-container/
https://octopus.com/blog/running-sql-server-developer-install-with-docker


