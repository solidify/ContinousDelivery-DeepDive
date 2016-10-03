# Exercise 2 - Release Management

**TODO**: 
* Add images
* Add further ideas

## Learnings
1. Creating a release pipeline for deploying your application to multiple environments
2. Understand how to management configuration across environments
3. ...

## Setup / prerequisites
Before starting on this excercise, you need to complete the previous excercise in which you created a release build definition. This build definition produces versioned artefacts that we will use in this excercise

## Creating an Azure Service Endpoint
All build and deployment tasks that communicate with an Azure subscription does so using an Service Endpoint. A service endpoint wraps all the information necessary in order to communicate with an external system (such as Azure, Jenkins or GitHub) so that 
this does not have to be done for each release definition. Also, it makes it possible to keep sensitive information such as passwords stored in one place.

In this lab, we will create and use an *Azure Resource Manager Service Endpoint* which is based on the Azure Resource Manager (ARM). If the VSTS account is backed by Azure AD (AAD), this is very easy. 
If not, you need to run a PowerShell script that will create the necessary artifacts and produce the information
 


## Creating a Release Definition
In Visual Studio Release Management, a deployment pipeline is implemented by creating a release definition. 
The definition contains information about the different environments, how the application should be deployed to each environment and which users should (if necessary) verify the deployments

1. Go to the Release hub
2. Click the + button and select *Create release definition*
3. Note that there are several different templates that can be used for certain scenarios. In this excercise, we will start from scratch so click on *Empty* at the bottom

![](./images/lab2/createrelease.png  "Logo Title Text 1")

4. Click *Next*, then select the build definition that you created in the previous excercise.
5. Check the *Continuous deployment...* checkbox to automatically deploy the application every time a release build finishes
6. Select the agent queue that you created when installing the build agent
7. Press *Create*. 
8. Rename the empty environment to *Test*
9. Press *Save* and name the release definition *QuizBox*

### Configure the test environment
Now we need to define the configuration variables for this release definition. 
Configuration variables contain the settings that either differ between the different environment, or for values that should be reused across multiple environments. 

For this release definition, we need the following variables:

| Variable        | Value           | Secured |
| ------------- |-------------||
| apiUrl      | http://quizboxtestapiXX.azurewebsites.net/api ||
| databaseLogin      | quizboxadmin ||
| databasePassword      | P2ssw0rd | Yes |
| resourceGroupName      | quizboxtest ||
| resourceGroupLocation      | North Europe ||
| hostingPlanName      | quizboxtest ||
| databaseName      | quizbox ||
| databaseServer      | quizboxtestXX.database.windows.net ||
| databaseServerName      | quizboxtestXX ||
| webSiteName      | quizboxtestXX ||
| backendWebSiteName      | quizboxtestapiXX ||


1. Click the context menu for the Test environment(The ...) and select *Configure variables*
2. Add all the variables listed above
3. Save the release definition

### Add deployment tasks 
Now it's time to specify how the deployment steps for QuizBox. We will be using an ARM (Azure Resource Manager) template for creating the environment. In this case, the template will contain 2 web sites, one SQL server and a SQL database. Then we will use a couple of deployment tasks to deploy the web sites and the database.*
1. Select the Test environment
2. Select *Add tasks*
3. Add a *Replace Tokens* task (in the Utility section)
4. Enter the following variables:

| Variable        | Value           |
| ------------- |-------------|
| Source Path      | $(Agent.ReleaseDirectory)|
| Target File Pattern      | \*\*\\\*.SetParameters.xml|

5.  Add an *Azure Resource Group Deployment* task (in the Deploy category)
6. Set the following variables

| Variable        | Value           |
| ------------- |-------------|
| Azure Connection Type      | Azure Resource Manager|
| Azure RM Subscription      | Azure Service Endpoint |
| Action      | Create or Update Resource Group|
| Resource Group      | $(resourceGroupName) |
| Location      | $(resourceGroupLocation)|
| Template      | $(System.DefaultWorkingDirectory)/QBox.Release/ARMTemplate/WebSiteSQLDatabase.json|
| Template Parameters      | $(System.DefaultWorkingDirectory)/QBox.Release/ARMTemplate/WebSiteSQLDatabase.parameters.json|
| Override Template Parameters      | -administratorLoginPassword (ConvertTo-SecureString -String '$(databasePassword)' -AsPlainText -Force) -administratorLogin $(databaseLogin) -hostingPlanName $(hostingPlanName) -databaseName $(databaseName) -webSiteName $(webSiteName) -databaseServer $(databaseServerName) -backendWebSiteName $(backendWebSiteName)
|

7. Add an *Azure SQL Database Deployment* task (in the Deploy category)
8. Set the following variables:

| Variable        | Value           |
| ------------- |-------------|
| Azure Connection Type      | Azure Resource Manager|
| Azure RM Subscription      | Azure Service Endpoint|
| Azure SQL Server Name      | $(databaseServer)|
| Database Name      | $(databaseName)|
| Server Admin Login      | $(databaseLogin)|
| Password      | $(databasePassword)|
| Type      | SQL DACPAC File|
| DACPAC File      | $(System.DefaultWorkingDirectory)/QBox.Release/database/QuizBoxDB.dacpac|

9. Add an *AzureRM App Service Deployment* task (in the Deploy category
10. Set the following variables

| Variable        | Value           |
| ------------- |-------------|
| Azure RM Subscription      | Azure Service Endpoint|
| App Service Name      | $(webSiteName) |
| Package or Folder      | $(System.DefaultWorkingDirectory)/QBox.Release/frontend/QBox.Web.zip |
| Publish using Web Deploy      | Checked |
| SetParameters File      | $(System.DefaultWorkingDirectory)/QBox.Release/frontend/QBox.Web.SetParameters.xml |

11. Add another *AzureRM App Service Deployment* task (in the Deploy category
12. Set the following variables

| Variable        | Value           |
| ------------- |-------------|
| Azure RM Subscription      | Azure Service Endpoint|
| App Service Name      | $(backendWebSiteName) |
| Package or Folder      | $(System.DefaultWorkingDirectory)/QBox.Release/backend/QBox.Api.zip |
| Publish using Web Deploy      | Checked |
| SetParameters File      | $(System.DefaultWorkingDirectory)/QBox.Release/backend/QBox.Api.SetParameters.xml |

13. Save the release definition

### Create Production environment
Now that the Test environment is configured, we need to add the production environment. The deployment steps should be identical to the Test environment, we just need to specify different configuration values.
Therefor, we can quickly create the production environment by cloning the Test environment.This will copy all the settings, and then we can just change the production values.

1. Click on the context menu for the *Test* environment and select *Clone environment".
2. Rename the environment to *Production*
3. Click on the context menu for the *Production* environment and select *Assign approvers*
4. For the pre-deployment approval step, select *Specific users* and select your own account
5. 

### Queue a release
Let's try out our release definition.

1. Queue a release by clicking on the *+Release* button and select *Create release*, then select OK.
2. Select the *Logs* tab on the running release to see the progress and the log output
3. After the release has been deployed to the *Test* environment, the release should stop and wait for your approval
4. Verify that the application has been deployed to the Test environment and is working as expected
5. Approve the release and verify that it is deployed to the production environment


## Further ideas
* Releases can be deployed in parallell across multiple environments. Try changing the release definition so that the release is deployed to the Test and Production simultaneously
* Releases can contain a Manual Intervention step, to allow for manual steps. Try adding this and see how this affect the release process


