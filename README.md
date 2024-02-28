# azure-project

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

sp_help 'Person.Address'  [This lists the constraints which mention Person.Address because I want to drop the foreign keys in other tables]

ALTER TABLE Ai_Production.Person.BusinessEntityAddress
DROP CONSTRAINT FK_BusinessEntityAddress_Address_AddressID

ALTER TABLE Ai_Production.Sales.SalesOrderHeader
DROP CONSTRAINT  FK_SalesOrderHeader_Address_BillToAddressID

ALTER TABLE Ai_Production.Sales.SalesOrderHeader
DROP CONSTRAINT  FK_SalesOrderHeader_Address_ShipToAddressID

DROP TABLE Person.Address;  [This lets me drop the table Person.Address]

2. Alter a column

UPDATE Production.ProductReview
SET Comments = 'Hello World!';  [This changes all the comments on the reviews]

3. Drop a column from a table

ALTER TABLE HumanResources.Employee
DROP COLUMN JobTitle;  [This drops a column from the table]

After running these commands, I downloaded the weekly backup I had made in the previous step and restored it to my production environment, thereby conducting a full disaster recovery simulation.

----------------------------

Geo-Replication and Failover - 





22/01/2024
For the geo-replication I set up a replica of my original database server called "original-server" in Central India and setup both in the same failover group. I initiated a failover and my primary server switched, and then another failover, and they returned to their original positions.

Then for the Entra ID, I didn't set up any groups on my own, because I was already avaliable as an AiCore member. Then I navigated to my original virtual machine, disconnected from the server in the previous point, and reconnected using Microsoft Entra - I did not get a popup asking me to provide login details, because I had already logged in while setting up Azure Data Studio on that machine.

Then I made a group on Entra Id and a user called db_reader111. I modified the connection in Azure Data Studio to allow this user to read the databases and it worked.

