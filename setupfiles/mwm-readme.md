# Steps used to create lab environment:

## PaaS Services

1. Used deployment script from here [https://github.com/microsoft/IgniteTheTour/tree/master/DEV%20-%20Building%20your%20Applications%20for%20the%20Cloud/DEV10#deployment](https://github.com/microsoft/IgniteTheTour/tree/master/DEV%20-%20Building%20your%20Applications%20for%20the%20Cloud/DEV10#deployment) to run the depoy.sh script via Cloud Shell

## Mongo DB on VM instance


1. Created MongoDB VM server from using instructions from here [https://github.com/mmckechney/2019AzureMigrateYourApps/tree/master/setupfiles](https://github.com/mmckechney/2019AzureMigrateYourApps/tree/master/setupfiles)

    - leveraged the MongoDB image from Bitnami to avoid any MongoDB licensing fees
2. was unable to install mongo tools with `sudo apt install mongo-clients` instead used `sudo apt install mongo-dev`

3. downloaded bson and json files with:

```bash
  wget https://raw.githubusercontent.com/chadgms/2019AzureMigrateYourApps/master/setupfiles/inventory.metadata.json
  wget https://github.com/chadgms/2019AzureMigrateYourApps/raw/master/setupfiles/inventory.bson
```

4. Got mongo credentials by SSH to machine and run `cat ./bitnami_credentials`
    - The default username and password is 'root' and 'SaNoFo3FdAx4'.

5. Restored database with:

   - Would need to replace with your proper values for username and password as well as IP address of the host

```bash
mongorestore inventory.bson --db=tailwind -u root -p SaNoFo3FdAx4 -h 52.151.0.33
```

6. Added lab user with

```bash
  mongo
  use admin
  db.auth("root","SaNoFo3FdAx4")
  db.createUser({user: "labuser", pwd:"AzureMigrateTraining2019#", roles:[{role: "read", db:"tailwind"}]})
```
## Azure Migration Service

1. Created Azure Migration Service in my resource group via the Azure portal

## Cosmos DB
https://github.com/chadgms/2019AzureMigrateYourApps/tree/master/Lab1-MigrateYourData#setup-4---create-the-azure-sql-database-instance

1. Created CosmosDB via cloud shell

```bash
RESOURCE_GROUP_COSMOS=MigrateYourApp-rg
LOCATION_COSMOS="westus2"
ACCOUNT_NAME_COSMOS=myamigrationcosmos
az cosmosdb create --resource-group $RESOURCE_GROUP_COSMOS --name $ACCOUNT_NAME_COSMOS --kind MongoDB --locations regionName=$LOCATION_COSMOS
```

## SQL Server on VM Instance 

1. From Marketplace, used SQL Server 2019 on Windows 2019 base image
2. Connect to the new VM using RDP 
3. Open a PowerShell window
4. Download .bacpac file

```PowerShell
    mkdir C:\temp
    Invoke-WebRequest https://github.com/mmckechney/2019AzureMigrateYourApps/raw/master/setupfiles/TailwindInventory.bacpac -O c:\temp\TailwindInventory.bacpac1
```

5. Using SQL server Management Studio, click on Databases -> Import Data-tier Application and follow the wizard using `c:\temp\TailwindInventory.bacpac1` as the source