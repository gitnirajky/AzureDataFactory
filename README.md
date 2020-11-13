# AzureDataFactory
Azure data factory related connectors/authentication methods

#1. Connecting to Azure SQL Database

#1.1 Authentication Method: Managed Identity

Managed identity is great way to authentication data facory to connect to resources as you dont have to store connection string/key in code. Azure Data factory supports MI authenication. Managed Identity can directly authenicate with SQL server database.
    
    --Setting up managed identoty permissions in Azure Sql database
    
    --Note: Setting up permissions would require you to connect Az sql DB with AD authentication. If you have not setup before, please setup, you can use portal--> [Azure SQL Server   resource] --> In Overview tab--> essentials section--> set Active Directory admin option.
    
    -- For login <login_name, sysname, login_name>, create a user in the database
    CREATE USER <DataFactory Managed Identity> FROM EXTERNAL PROVIDER
    GO

    -- Add user to the database owner role (or a DB role as suited)
    EXEC sp_addrolemember N'db_owner', N'<DataFactory Managed Identity>'
    GO
