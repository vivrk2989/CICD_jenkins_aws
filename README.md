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


