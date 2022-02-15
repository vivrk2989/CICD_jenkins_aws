# CICD with Jenkins and AWS
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/cicd_with_jenkins_AWS.png)


Step 1 - Create ssh connection from location to github
1.1 - generate ssh key-pair on localhost
1.2 - put the lock on github - copy the public ssh key to github . file.pub is the key pair to access github
- create a new repo for CI/CD on our github

## Steps to link github and local host securely through keys

- Go to your gitbash terminal and `cd` into `.ssh` and type the below command
- `ssh-keygen -t rsa -b 4096 -C "vivrk2989@gmail.com"`
- This will ask for a name for the file. Once you provide the name for it, two file will be created in `.ssh` which will contain the key.
- the files will be of the following types: `filename` and `filename.pub` where .pub stands for public key

- Now on github, got to settings
- from the left hand side menu, choose `SSH and GPG keys` and click on `new ssh keys`.
- here you can add the key using a title of your choice. Make sure u copy the keys from the .pub file in your .ssh and paste it on github.

- create a new folder - `eng103a_cicd` in the destination of your choice.
- create a readme.md file using `nano README.md` and type some information. We can use this README.md to check if we could successfully link our computer to github using the ssh keys.
- then use `git init` and then check status using `git status`
- use `git add .` and then `git commit -m"...."`
- then create a new branch using `git branch -M main`
- then use `git remote add origin git@github.com:vivrk2989/CICD_jenkins_aws.git`
- use `git push -u origin main`. If this says permission denied, follow the below steps:

1. Got to your .ssh folder and use `eval "$(ssh-agent -s)"` . This will assign a new id.
2. use `ssh-add ~/.ssh/file-with-key` for example: ssh-add ~/.ssh/103a
3. Now go back to the `eng103a_cicd` folder you created and push the repository using `git push -u origin main`

## Jenkins

Jenkins is an Automation Server built by Java. 

### Steps to build your own project in Jenkins.
- Use the right ip address to sign into Jenkins.
- Once inside, click on `new item` to create a job.
- Enter the name of the item. For example: `vivek_jenkins_test`
- Click on `Freestyle Project` an then click `OK` on the bottom of the page. 
- This will open up a new page. Provide a description to the Job.
- Scroll down and click on `Add build step` and choose `Execute shell`
- To check the environment and the location of this, use `uname -a` and `date`.
- Click `Save`
- To run the Job, click on `build now` and u will be able to see it below in the build `console`
- click on the item and choose `console output` to see the output.

### Automating/Triggering multiple Jobs
- Lets say the above Job already exists. 
- We can create another Job - `vivek_test2` similarily 
- Now go back to the first Job - `vivek_jenkins_test` and choose `configure`.
-  Scroll down to the end of the page and choose `Post-build Actions` and choose `vivek_test2` and select `trigger only if build is stable` and then `Save` 


### cloning github repository

- create a new directory using `mkdir` and then use `git clone URL(http://...)`. Get the URL from github - `code`
- If the cloned folder contains .git file and if you dont want to be tracked to the parent directory, use `git rm -rf .git`

### CICD using jenkins
1. Create a new job in jenkins to create a constant integration pipeline
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Building%20a%20job%20in%20jenkins.png)
2. click `new item` and the following page appears where u can provide a name to ur project and choose `Freestyle Project`
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/How%20to%20build%20job.png)
3. Now we can provide the required details like below
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Jenkins%20job%20-%20constant%20integration%20with%20github.png)
4. Now we need to generate a key. For doing this, go into ur `.ssh` directory path and follow the `steps to link github and local host securely through keys` from above
5. Once created, we need to copy the public key and then go to your github and click on `settings` on the top next to the repository name
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Deploying%20keys.png)
6. Provide a name for the key and paste the `keyname.pub` into it
7. Now we need to put the private key into Jenkins
8. Go back to Jenkins and provide the http key of your github repository like below
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Providing%20the%20github%20http%20in%20jenkins.png)
9. Scroll down and under `Office 365 connector`, select `Restrict where this project can be run` like below. This is done so that we run the tests in the `Agent node` and not on the `Master node`.
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Restricting%20the%20tests%20in%20agent%20node.png)
10. Now on the next SCM section, choose `Git` and copy your repository URL (SSH) from github into it like below
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Using%20ssh%20from%20github%20for%20git%20repository%20URL.png) 
---
#### Creating webhook
- go to your github and go to `settings` and choose `webhook` like below
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Webhook1.png)
- on the payload URL, copy `JENKINS URL/github-webhook/` and select content type as `application/json`. Under events, choose `send me everything`
- Make sure to `git pull` after any changes you make to your file in the local host and then `git add` and then `git commit` and then `git push`
---
11. Now for the `credentials` section, click on `Add` and this will open up a new box. Here choose `SSH Username with private key` like below
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Jenkins%20Credentials%20for%20git1%20.png)
12. Now select `Private Key -> Enter directly` and copy the private key from .ssh and paste it under `Key` like below
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Entering%20private%20key%20as%20credentials%20for%20git2.png)
13. Under Branches to build section, type `main` instead of the default `master`
14. Under the `Build environment` section, choose `Provide Node & npm bin/folder to PATH` like below
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Build%20Environment.png)
15. Under `Build` , choose `Execute shell` and type the following commands
![Image Link](https://github.com/vivrk2989/CICD_jenkins_aws/blob/main/Images/Execute%20shell%20commands.png)
16. Click `Apply` and `Save`
17. Now on your terminal create a new branch `dev` using `git checkout -b dev`
18. Now go back into JENKINS and create a new job for merging the dev branch. Follow the same steps as above. 
19. Under the Branches to build section, type `Dev` instead of `main`
20. 



