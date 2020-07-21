#!/bin/bash

# Replace the following URL with a public GitHub repo URL
gitrepo=https://github.com/Azure-Samples/php-docs-hello-world
webappservice=bbenztailwind-frontend
webappstaging=bbenztailwindstaging
webappproduction=bbenztailwindproduction

# Create a resource group.
az group create --location westus2 --name $webappservice

# Create an App Service plan in STANDARD tier (minimum required by deployment slots).
az appservice plan create --name $webappstaging --resource-group $webappservice --sku S1

# Create a web app.
az webapp create --name $webappstaging --resource-group $webappservice --plan $webappstaging
az webapp create --name $webappproduction --resource-group $webappservice --plan $webappstaging

#Create a deployment slot with the name "canary".
az webapp deployment slot create --name $webappproduction --resource-group $webappservice --slot canary

# Deploy sample code to staging Web App slot from GitHub.
az webapp deployment source config --name $webappstaging --resource-group $webappservice --repo-url $gitrepo --branch main --manual-integration

# Deploy sample code to "production" slot from GitHub.
az webapp deployment source config --name $webappproduction --resource-group $webappservice --repo-url $gitrepo --branch main --manual-integration
az webapp deployment source config --name $webappproduction --resource-group $webappservice --slot staging --repo-url $gitrepo --branch main --manual-integration

# Copy the results of the following commands into a browser to see the deployments.
echo http://$webappstaging.azurewebsites.net
echo http://$webappproduction.azurewebsites.net
echo http://$webappproduction-canary.azurewebsites.net

# Example for Later - Swap the canary slot into production.
#az webapp deployment slot swap --name $webappproduction --resource-group $webappservice --slot canary

# Copy the results of the following commands into a browser to see the web app deployment slot swap.
echo http://$webappproduction.azurewebsites.net
echo http://$webappproduction-canary.azurewebsites.net
