# azure-project

05/01/2024
Writing this from the Azure VM. Have installed SQL Server and SSMS. Have also restored AdventureWorks with SSMS.


07/01/2024
Corrected the date for prev entry to the proper format. I connected to AdventureWorks using Windows Authentication on Azure Data Studio. I connected to the database on Azure by using details from the Azure portal. I downloaded the addons in Azure Data Studio and used them to migrate all the info from one database to the other - although I had a bit of trouble here trying to get the Database Migration Service to work, this was solved with the help of the team. Then I checked to see everything had been transferred well.

15/01/2024
Provisioned a new VM (duplicate-machine) which was a copy of the old one. Installed all the necessary software on here, logged into Azure, downloaded the .bak file and mov3ed it to the correct folder in Program Files. Then I restored this on the new VM and spent a lot of time confused because I thought I needed to connect it to Azure or to some other account - the confusion was compounded by the fact that the Server was rendered as "duplicate-machi...".

More trouble when I had to automate updates from the development environment to the Azure storage account - a lot of 400 and 403 errors. I think this was because the blob in the container was set to private. I also ticked a box in SSMS that said "ignore error messages." Tried restoring one of the backups sent after this change and it worked alright.

21/01/2024
Had been working on Milestone 5 but administrative error meant I had to restart the whole project. Restart followed pretty much the same except in the lastcase there was no trouble when setting up automatic backups - I think this is because I set up the storage account properly

The SQL commands I have used are:
- sp_help 'Person.Address' - [This lists the constraints which mention Person.Address because I want to drop the foreign keys in other tables]
- ALTER TABLE Ai_Production.Person.BusinessEntityAddress.
DROP CONSTRAINT FK_BusinessEntityAddress_Address_AddressID
ALTER TABLE Ai_Production.Sales.SalesOrderHeader
DROP CONSTRAINT  FK_SalesOrderHeader_Address_BillToAddressID
ALTER TABLE Ai_Production.Sales.SalesOrderHeader
DROP CONSTRAINT  FK_SalesOrderHeader_Address_ShipToAddressID

DROP TABLE Person.Address; - [This lets me drop the table Person.Address]

UPDATE Production.ProductReview
SET Comments = 'Hello World!'; - [This changes all the comments on the reviews]

ALTER TABLE HumanResources.Employee
DROP COLUMN JobTitle; - [This drops a column from the table]

After running these queries, I restored the database from Azure.

22/01/2024
For the geo-replication I set up a replica of my original database server called "original-server" in Central India and setup both in the same failover group. I initiated a failover and my primary server switched, and then another failover, and they returned to their original positions.

Then for the Entra ID, I didn't set up any groups on my own, because I was already avaliable as an AiCore member. Then I navigated to my original virtual machine, disconnected from the server in the previous point, and reconnected using Microsoft Entra - I did not get a popup asking me to provide login details, because I had already logged in while setting up Azure Data Studio on that machine.

Then I made a group on Entra Id and a user called db_reader111. I modified the connection in Azure Data Studio to allow this user to read the databases and it worked.

