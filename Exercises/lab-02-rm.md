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
Create variables for test and production

### Add deployment tasks 
Deploy 2 Azure web apps + 1 SQL database

### Queue a release
Run deployment
Verify test deployment
Approve
Verify production deployment



## Further ideas


