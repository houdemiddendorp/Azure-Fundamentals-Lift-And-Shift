# Lift and Shift your Smoelenboek

## Introduction
This lab is the hands-on part of the Azure Fundamental Introduction module. In this lab you will deploy a simple web application (ASP.NET Core 3.1 MVC) to Azure. You will learn how to create an App Service Plan, a Web App and an Azure SQL database. And you will learn how to deploy the web application to Azure using Visual Studio Code.

## Pre-requisites
To perform this lab you will need:

- Visual Studio Code (get it from https://code.visualstudio.com/download) with the Azure App Service extension installed (get it from https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
- Owner / administrator access to an Azure Resource Group (which is provided to you because you registered for the Azure Fundamentals Introduction module)
- The code of the application (which is in this GitHub repository)
## Steps

### Navigate to your resource group

- Go to the Azure portal (https://portal.azure.com) and log on with your credentials.
- Navigate to your resource group:
 - Click on the menu button on the left top of the screen.
 - Click "Resource groups".
 - Your resource group name starts with "rg-we-afls-". Enter "rg-we-afls-" in the "Filter by name..." text box and wait until filtering is done.
 - Click on the resource group name in the list.

### Create an App Service Plan

- Click "+ Add" in the resource group blade.

- Search for "App Service Plan", select it.

- Click "Create".

  Note that some information is already entered or selected in the new form that comes up.

- Select "Windows" as operating system and "West Europe" as region. Give the app service plan a name, best practice is to use a standard prefix (asp). So your name should be something like "asp-we-sb001".

- Now scroll down to "Pricing Tier". This is where the cost come in. For this lab we will select the "free" version of an app service plan. Click "Change size".

- In the "Spec Picker" select the "Dev / Test" tab.

- Select the "F1" pricing tier, which is free of charge and then "Apply".

- Back in the previous form, click "Review + create".

- In the "Review + create" form, click "Create".

  Your App Service Plan is now created. This will take a little time.

- When finished, click "Go to resource" and notice the App Service Plan blade.

### Create a Web App

- Navigate to your resource group (when you are still on the App Service Plan blade, you can find a link to your resource group just below the "Delete" button on the top of the blade).
- Click "+ Add" in the resource group blade.
- Search for "Web App", select it.
- Click "Create".
- Give the Web App a name.
- Make sure "Code" is the publish mechanism.
- Select ".NET Core 3.1" for runtime stack.
- Make sure "Windows" is your operating system (necessary for selecting the right App Service Plan).
- Select "West Europe" as region.
- Scroll down to "App Service Plan" and select the App Service Plan you just created.
- Click "Review + create". Notice that on the bottom of the screen also Monitoring is enabled.
- Click "Create". Again a deployment is started. This will take a little time.
- When finished, click "Go to resource" and notice the App Service blade.

### Create an Azure SQL Database

- Navigate to your resource group (when you are still on the App Service blade, you can find a link to your resource group just below the buttons on the top of the blade).
- Click "+ Add" in the resource group blade.
- Search for "Azure SQL", select it.
- Click "Create".
- We will use the cheapest variant of an Azure SQL database, which is a single database running on a fully managed environment. Select "Single database" from the combo box in the "SQL databases" box.
- Click "Create".
- Now, we first have to create a new SQL Database Server:
  - Click on "Create new" below the server selection combobox.
  - Enter a name, e.g. "ss-we-sb001", an administrator login name and password (remember these!!!!) and select region "West Europe".
  - Click "OK".
- Back in the "Create SQL Database" form, enter a database name.
- Make sure that you do not use SQL elastic pool.
- Next to "Compute + storage", click "Configure database".
- In the Configure page, select the Basic configuration and leave the defaults. Click "Apply".

Now we will make sure that we can reach the database from our current location and from Azure services.

- In the Create SQL Database page, click "Next: Networking".

- Select Public endpoint for "Connectivity method". Select "Yes" for "Allow Azure services and resources to access this server" and "Yes" for "Add current client IP address".

- Click "Review + create".

- Click "Create".

  The server and database are now deployed. This will take a little time.