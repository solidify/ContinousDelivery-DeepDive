# Exercise 2 - Release Management

TODO: Add images
      Add further ideas

## Learnings
1. Creating a release pipeline for deploying your application to multiple environments
2. Understand how to management configuration across environments
3. ...

## Setup / prerequisites
Before starting on this excercise, you need to complete the previous excercise in which you created a release build definition. This build definition produces versioned artefacts that we will use in this excercise

## Creating a Release Definition
In Visual Studio Release Management, a deployment pipeline is implemented by creating a release definition. The definition contains information about the different environments, how the application should be deployed to each environment and which users should (if necessary) verify the deployments

1. Go to the Release hub
2. Click the + button and select *Create release definition*
3. Note that there are several different templates that can be used for certain scenarios. In this excercise, we will start from scratch so click on *Empty* at the bottom
4. Click Next, then select the build definition that you created in the previous excercise.
5. Check the *Continuous deployment...* checkbox to automatically deploy the application every time a release build finishes
6. Select the agent queue that you created when installing the build agent
7. Press *Create*. 
8. Press *Save* and name the release definition *QuizBox.Release*

### Create environments
Create test and production environments
Add pre-deployment approval in production

1. Rename the created environment to *Test*
2. Click *Add new environment* and select *Create new environment*
3. Select *Empty* and *Next*
4. In the pre-deployment approval section, select *Specific users* and select your own account
5. Verify that the *Trigger* setting specifies that deployment to this environment should start as soon as a deployment to the Test environment has completed successfully
6. Select the agent queue that you created previously
7. Rename the new environment to *Production* and save the release definition

### Define configuration variables
Now we need to define the configuration variables for this release definition. Configuration variables contain the settings that either differ between the different environment, or for values that should be reused across multiple environments. 

For this release definition, we need the following variables:

* Frontend web site name
* Backend web site name
* Backend URL
* Connection string
...

1. Click the context menu for the Test environment(The ...) and select *Configure variables*
2. Add the following variables and values
- XXX YYY
- XXXX YYYY
3. Repeat for the *Production* environment, with the following variables:
- XXX YYY
- XXX YYY
4. Save the release definition


### Add deployment tasks 
Now it's time to specify how the deployment steps for QuizBox. We will be using an ARM (Azure Resource Manager) template for creating the environment. In this case, the template will contain 2 web sites, one SQL server and a SQL database. Then we will use a couple of deployment tasks to deploy the web sites and the database.

1. 
Deploy 2 Azure web apps + 1 SQL database (ARM template)
Deploy web

### Queue a release
Let's try out our release definition.

1. Queue a release by clicking on the *+Release* button and select *Create release*, then select OK.
2. 


Run deployment
Verify test deployment
Approve
Verify production deployment



## Further ideas
* Releases can be deployed in parallell across multiple environments. Try changing the release definition so that the release is deployed to the Test and Production simultaneously
* Releases can contain a Manual Intervention step, to allow for manual steps. Try adding this and see how this affect the release process


