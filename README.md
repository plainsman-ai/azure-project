# azure-project

05/01/2024
Writing this from the Azure VM. Have installed SQL Server and SSMS. Have also restored AdventureWorks with SSMS.


07/01/2024
Corrected the date for prev entry to the proper format. I connected to AdventureWorks using Windows Authentication on Azure Data Studio. I connected to the database on Azure by using details from the Azure portal. I downloaded the addons in Azure Data Studio and used them to migrate all the info from one database to the other - although I had a bit of trouble here trying to get the Database Migration Service to work, this was solved with the help of the team. Then I checked to see everything had been transferred well.

15/01/2024
Provisioned a new VM (duplicate-machine) which was a copy of the old one. Installed all the necessary software on here, logged into Azure, downloaded the .bak file and mov3ed it to the correct folder in Program Files. Then I restored this on the new VM and spent a lot of time confused because I thought I needed to connect it to Azure or to some other account - the confusion was compounded by the fact that the Server was rendered as "duplicate-machi...".

More trouble when I had to automate updates from the development environment to the Azure storage account - a lot of 400 and 403 errors. I think this was because the blob in the container was set to private. I also ticked a box in SSMS that said "ignore error messages." Tried restoring one of the backups sent after this change and it worked alright.
