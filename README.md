# Lift and Shift your Smoelenboek

## Introduction
This lab is the hands-on part of the Azure Fundamental Introduction module. In this lab you will deploy a simple web application (ASP.NET Core 3.1 MVC) to Azure. You will learn how to create an App Service Plan, a Web App and an Azure SQL database. And you will learn how to deploy the web application to Azure using Visual Studio Code.

## Pre-requisites
To perform this lab you will need:

- Visual Studio Code (get it from https://code.visualstudio.com/download) with the Azure App Service extension installed (get it from https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
- .NET Core 3.1 SDK
- Owner / administrator access to an Azure Resource Group (which is provided to you because you registered for the Azure Fundamentals Introduction module)
- The code of the application (which is in this GitHub repository)
## Steps

### 1. Create the Infrastructure

In this part of the lab you will create the infrastructure that will host the application in Azure. Basically, you will perform the following actions:

1. Create an App Service Plan
2. Create a Web App
3. Create an Azure SQL Database, including a new server.

#### 1.1 Navigate to your resource group

- Go to the Azure portal (https://portal.azure.com) and log on with your credentials.
- Navigate to your resource group:
 - Click on the menu button on the left top of the screen.
 - Click "Resource groups".
 - Your resource group name starts with "rg-we-afls-". Enter "rg-we-afls-" in the "Filter by name..." text box and wait until filtering is done.
 - Click on the resource group name in the list.

#### 1.2 Create an App Service Plan

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

#### 1.3 Create a Web App

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

#### 1.4 Create an Azure SQL Database, including a new server

- Navigate to your resource group (when you are still on the App Service blade, you can find a link to your resource group just below the buttons on the top of the blade).
- Click "+ Add" in the resource group blade.
- Search for "Azure SQL", select it.
- Click "Create".
- We will use the cheapest variant of an Azure SQL database, which is a single database running on a fully managed environment. Select "Single database" from the combo box in the "SQL databases" box.
- Click "Create".
- Now, we first have to create a new SQL Database Server:
  - Click on "Create new" below the server selection dropdown box.
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

### 2. Deploy the smoelenboek application

First, we will explore the smoelenboek application a little and run it on our own machine. Then we will change the connection to the database and deploy to Azure.

#### 2.1 Get the application's code

In the upper right part of the github form you will find the "Clone or download" button. Click on this button and choose "Download ZIP". When the file is downloaded, unzip it on your computer.

#### 2.2 Explore the smoelenboek application

- Open Visual Studio Code and open the folder where you unzipped this github repository.

- Open the "Centric.Learning.Smoelenboek" folder and notice there are two main folders; "src" and "tst". 

- Open de "src" folder and notice there are four projects in the smoelenboek application solution:

  - Centric.Learning.Smoelenboek

    Contains the web application itself.

  - Centric.Learning.Smoelenboek.Business

    Contains the main model and support libraries.

  - Centric.Learning.Smoelenboek.Storage.People.Database

    Contains an implementation of the IPeopleRepository interface that uses a SQL Server database.

  - Centric.Learning.Smoelenboek.Storage.Photos.Filesystem

    Contains an implementation of the IPhotoRepository interface that uses the local filesystem to store photo's.

- Right-click the "Centric.Learning.Smoelenboek" folder and select "Open in terminal". Click once in the new terminal window.
- Start the application by entering the command "dotnet run" in the terminal window. Notice the url's that are shown in the window.
- Open a browser and navigate to one of the url's, i.e. "http://localhost:5000". Try the application.
- Shut down the browser and stop the application by pressing "Ctrl + C" in the terminal window.

#### 2.3 Change the connection string

Before we can deploy the application to Azure, we have to make sure that it will connect to the Azure SQL Database we created earlier. We have to change the connection string of the web application to point to the correct database. To do this, we must search for the connection string in the Azure portal and copy it in the "appsettings.json" file of the web application.

- Navigate to the resource group in the Azure portal.

- Click on the link to the database (not the database server).
- On the menu of the database, click "Connection strings". Note that you will see a connection string to the database, but that you have to change the password manually (hope you remember the password you have given the database user in 1.4).
- Copy the ADO.NET connection string to the clipboard.
- Switch back to Visual Studio Code and open the "appsettings.json" file of the web application. It should look like this:

```json
{

    "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=aspnet-Centric.Learning.Smoelenboek-A4CDB26E-944A-4258-8BD3-C24C4271BC91;Trusted_Connection=True;MultipleActiveResultSets=true"
    },
    "Logging": {
        "LogLevel": {
            "Default": "Information",
            "Microsoft": "Warning",
            "Microsoft.Hosting.Lifetime": "Information"
        }
    },
	"AllowedHosts": "*"
}
```

- Change the value of the "DefaultConnection" under "ConnectionStrings" to the connection string in your clipboard (i.e. select the text between the brackets and press Ctrl + V.
- Change the "{your_password}" of the copied connection string to the password that you created earlier.

#### 2.3 Publish the smoelenboek application

Before we can deploy the application to the Azure Web App we first have to make a publishable version.

- From the terminal window enter "dotnet publish -o pub".

The publishable version is now available in the folder "pub" within the Web Application folder.

- Open the Azure App Service extension in Visual Studio Code by pressing "Ctrl + Shift + A". Log on to Azure with you credentials if necessary. Notice all subscriptions you have access to.
- Navigate to the subscription that contains the web application you created in step 1.3. Notice all web applications you have access to.
- Right click on the web application you created in step 1.3. Select "Deploy to Web App".
- In the upper center of the screen, select "Browse" and browse to the pub folder you created by the dotnet publish command. Do not open the folder, but select it.
- Open the "output window" (follow the link in the message box) to review progress. When deployment is completed, click the "Browse Website" button presented by the wizard.