# Deploy `json-server` to `Azure Web App`

Borrowed from https://github.com/rymdar/json-server-heroku

> Instructions how to deploy the full fake REST API [json-server](https://github.com/typicode/json-server) to various free hosting sites. Should only be used in development purpose but can act as a simpler database for smaller applications.

* [Deploy Directly from Git Repo](#deploy-directly-from-git-repo)
* [Deploy from file](#deploy-from-file)
* [To remove the app](#To-remove-the-app)

## Signup for free tier Azure
https://azure.microsoft.com/en-ca/free/ 

The free Azure Tier allows you to have up to 10 web apps at no cost. Not a bad deal!


## Deploy Directly from Git Repo

You can also take this repo as is and deploy it directly to Azure in a few minutes.

```PowerShell
# set app name, app service plan ,resource group and git repo
$webappname="mysamplejson123"
$appserviceplan="BasicAppServicePlan"
$resourcegroupname="rg1"
$gitrepo="https://github.com/meatsac/json-server-azure"

# create a resource group
az group create --location centralus --name $resourcegroupname

# create an App Service plan
az appservice plan create --name $appserviceplan --resource-group $resourcegroupname --sku FREE

# create a Web App
az webapp create --name $webappname --resource-group $resourcegroupname --plan $appserviceplan

# deploy code from a Git repository
az webapp deployment source config --name $webappname --resource-group $resourcegroupname --repo-url $gitrepo --branch master --manual-integration
```

Now visit the the site (this will take a few minutes), i.e. https://mysamplejson123.azurewebsites.net/

## Deploy from file

```PowerShell
cd ~/Documents
git clone https://github.com/meatsac/json-server-azure.git
cd json-server-azure
```
Within the cloned folder, alter or replace the db.json file to your liking.

```PowerShell
az group create --name rg01 --location centralus
az webapp up --name mysamplejson123 --plan BasicAppServicePlan --resource-group rg01 --sku FREE
```

Once the app is deployed (this will take a few minutes), open a browser and go to https://mysamplejson123.azurewebsites.net/ 


Choose **App Services** in the sidebar to the left and the choose your app in the list that appears then go to **Deployment Credentials** to change your password for deployment:<br>
https://docs.microsoft.com/en-us/azure/app-service/app-service-deployment-credentials

## To remove the app

```PowerShell
$webappname="mysamplejson123"
$appserviceplan="BasicAppServicePlan"
$resourcegroupname="rg1"

az webapp delete --name $webappname --resource-group $resourcegroupname
az appservice plan delete --name $appserviceplan --resource-group $resourcegroupname --yes
az group delete -n $resourcegroupname --yes
```
Or you can use https://portal.azure.com
