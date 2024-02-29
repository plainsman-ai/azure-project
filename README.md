# Azure Database Migration Project

Setting Up - Provided with login details by AiCore to log in with their subscription.

------------------------

Setting Up Production Environment - Provisioned a Virtual Machine with RDP protocol on Port 3389, which allowed me to open the VM with an .exe file. Installed SQL Server and SSMS directly on VM. Downloaded the .bak file for AdventureWorks and restored it with SSMS.

-------------------------

Migrating to Azure SQL Database - Set up an Azure SQL Database. Established connection using Azure Data Studio (on VM) to the production environment with SQL login. Established connection to Azure SQL Database using Windows Authentication (this requires login via browser on VM). Installed SQL Server Schema Compare extension to migrate the schema from the production environment to the Azure SQL Database, which also made migrating easier. Then installed the Azure SQL Migration extension which let me migrate the whole database to the Azure SQL Database. I validated this by comparing the schema and sampling a few of the tables to see if they matched in the original and the migrated databases.

--------------------------

Data Backup and Restore - Generated a backup of the production database using SSMS. Configured a Blob Storage account on Azure and uploaded the .bak file there. Created a sandbox environment to replicate the other VM (including download of SSMS and login to Azure on the VM to access the storage account) and restored database from Blob Storage. Configured a weekly backup plan to Blob Storage account using SSMS and Azure access keys.

--------------------------

Disaster Recovery Simulation - In my production environment I simulated data loss by corrupting the data with some SQL commands. They were as follows (with description of what the commands collectively do):

1. Drop a table

        [This lists the constraints which mention Person.Address because I want to drop the foreign keys in other tables]
   
        sp_help 'Person.Address';

        ALTER TABLE Ai_Production.Person.BusinessEntityAddress
        DROP CONSTRAINT FK_BusinessEntityAddress_Address_AddressID

        ALTER TABLE Ai_Production.Sales.SalesOrderHeader
        DROP CONSTRAINT  FK_SalesOrderHeader_Address_BillToAddressID

        ALTER TABLE Ai_Production.Sales.SalesOrderHeader
        DROP CONSTRAINT  FK_SalesOrderHeader_Address_ShipToAddressID
   
        [This lets me drop the table Person.Address]
        DROP TABLE Person.Address;  


2. Alter a column

        [This changes all the comments on the reviews]
        UPDATE Production.ProductReview
        SET Comments = 'Hello World!';  


3. Drop a column from a table

        [This drops a column from the table]
        ALTER TABLE HumanResources.Employee
        DROP COLUMN JobTitle;  


After running these commands, I downloaded the weekly backup I had made in the previous step and restored it to my production environment, thereby conducting a full disaster recovery simulation.

----------------------------

Geo-Replication and Failover - Created a geo-replica of my original database server in Central India and setup both in the same failover group. Initiated a failover to Central Indian server and a tailback to original (UK South 1).

----------------------------

Entra ID Integration - Made a group on Entra ID and an Admin for user management and access control. Then created a new user account called db_reader111 and assigned it the db_datareader role in Azure Data Studio on the original VM. This allowed me to read the databases in the original production environment.


