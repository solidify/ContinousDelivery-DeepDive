# Exercise 0 - Setup

In this lab you will setup your environment to prepare for the rest of the excercises.
A team project has already been created for you.

Expected time for this exercise: 15 minutes.

## Preparations

1. Open a web browser and browse to https://deepdive.visualstudio.com/

2. Login using your assigned user login and password
 
3. Connect to the team project that has been assigned to you/your team

    ![](./images/lab0/setup1.png)

4. Go to the *Code* hub group

5. From the repository selector drop-down, click *Import repository*

    ![](./images/lab0/setup2.png)

6. In the *Clone URL* field, enter the following URL:

```
https://github.com/solidifysv/QBox.git

``` 

7. Click *Import* to import the QBox repository into a new Git repo in your team project

8. We will also setup a *branch policy* in order to protect our master branch. 

9. Click on the *Team Settings* wheel at the top right

10. Select the *Version Control* tab and select the QBox repository

11. Select the *master* branch and then click on the *Branch policies* section

12. Select the *Require a minimum number of reviewers per pull request* checkbox

13. Set the minimum number of reviewers to 1 and check the *Allow users to approve their own changes*

    ![](./images/lab0/setup4.png)

14. Save the branch policy. 

15. Verify that the policy is working by trying to commit and push something to the master branch, either from Visual Studio or straight from VSTS. 
You should receive an error message stating that you must submit your changes using a pull request.
