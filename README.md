# AzureDataFactory- azure SQL linked service authenication methods (without storing connection string/key in code)
Azure data factory related connectors/authentication methods

#1. Connecting to Azure SQL Database

#1.1 Authentication Type: Managed Identity

Managed identity is great way to authentication data facory to connect to resources as you dont have to store connection string/key in code. Azure Data factory supports MI authenication (you dont have create an identity separately). Managed Identity can directly authenicate with SQL server database.
    
    --Setting up managed identoty permissions in Azure Sql database
    
    --Note: Setting up permissions would require you to connect Az sql DB with AD authentication. If you have not setup before, please setup, you can use portal--> [Azure SQL Server   resource] --> In Overview tab--> essentials section--> set Active Directory admin option.
    
    -- For login <login_name, sysname, login_name>, create a user in the database
    CREATE USER <DataFactory Managed Identity> FROM EXTERNAL PROVIDER
    GO

    -- Add user to the database owner role (or a DB role as suited)
    EXEC sp_addrolemember N'db_owner', N'<DataFactory Managed Identity>'
    GO
    
#1.2 Authentication Type: SQL Authentication
With SQL Authenitcation we can store password in Key vault with user name specified in linked service. We can create a secret in Azure key vault and authorising Azure active directory (with managed Identity) to access key vaut.

To access az key vault
1. Create a azure key vault linked service.
2. Note your Data factory's managed identity and assign permissions to key vault -using Access Policies blade--> add access policies (provide get and list permissions, in select principal select data factory MI)
3. Add your secret to KV (key vault)
 
 
#1.3 Authentication Type: Service Principal
Service principal is a great way to have one single principal/crentials to be used for accessing various resources in azure . If we have not created, we can create a new service principle in azure AD.
Azure Active Directory--> App registrations -->New Registration (it can signle or multitenant)
Once App registration is done generate a secret from Certificates & Secret blade.
Now secret (password) can be stored in Az key vault.
Use step 1.1 to grant permission to app/service principal on azure SQL database and 1.2 to retrive secret key


#1.3 Connection sting in Azure key vault
You can also save your compele connection string in azure Key vault. Use above steps to create key vault linked service.




