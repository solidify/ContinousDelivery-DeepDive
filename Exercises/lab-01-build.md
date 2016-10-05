# Exercise 1 - Build and Package

Expected time for this exercise: 60 minutes.

## Learnings
In this exercise you will learn to:

1. Install and configure a build environment
2. Setup a build process
3. Extend the build process with custom activities

## Setup / prerequisites
In order to do this lab you need have completed the setup exercise where you setup your 
development environment. 

Login to [https://deep-dive.visualstudio.com](https://deep-dive.visualstudio.com) with your 
assigned user (UserNN@DevOpsTraining.onmicrosoft.com).

## Configure the build environment
For this workshop we will use local build servers. In this step you will setup a local 
agent on your own machine to simulate an internal build server running against VSTS.

### Create a build pool
1. Go to the VSTS team settings and select the Agent Queue hub.
2. Click the "New queue..." button and create a queue and also a new pool.
![Create queue](images/lab1/create-queue.png)

### Create a Personal Access Token for the build service
1. Create a personal access token (PAT) to be used for the VSTS agent to access 
build resources in VSTS.
[Create VSTS PAT](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate)

zgh66vsbuxwqfh6h4rr5y6tbrbsrv2yae3jctuo7kobqx3bdz3qq

### Install a local build agent
1. Go to the VSTS team settings and select the Agent Queue hub.
2. Click on the "Download agent" button to download the agent. Review the steps in the dialog 
since it may have change since this text was written...
3. Open a PowerShell command prompt as administrator.
4. Run the following commands to extract the agent:

    `mkdir c:\vsts-agent`

    `cd c:\vsts-agent`

    `Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\Downloads\vsts-agent-win7-x64-2.107.0.zip", "$PWD")`

5. Configure the agent

    `.\config.cmd`

6. Run the agent interatively

    `.\run.cmd`

7. Now the agent should be up and running. Switch back to the VSTS team settings and look at
the status in the agent queue.

## Create a build definition

### CI build

### Release build

* Artifacts
* Extensions

## Further ideas
By now you should have completed the core steps of setting up a build pipeline. Well done! 
If you still have time and in the mood for some additional challenges here are a couple of 
ideas to explore.

* Use GitVersion to handle the build version number.

    [Using GitVersion with TFS Build vNext](https://gitversion.readthedocs.io/en/latest/build-server-support/build-server/tfs-build-vnext/)

