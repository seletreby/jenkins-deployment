# jenkins-deployment
The aim of to have a jenkin deployment on dedicated Redhat Linux VM and use it to deploy application on Redhat OpenShift environment

## Create a dedicated VM on Softlayer
I created Redhat Enterprise linux vm with the following specs

| IP Address | Hostname | CPU | Memory
| ---  | ---|---|---|
| 158.177.112.231 | jenkins.example.com | 8 | 32 GB|

### Install Jenkins
First we need to install a JAVA. I choose to install openSDK java using the following command:
```
sudo yum -y install java-1.8.0-openjdk
```
Redhat yum repository doesn't have Jenkins package. you need to import Jenkins repository on our vm using the following commands:
```
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
```
**Install Jenkins**
```
sudo yum install jenkins -y
````
**Enable Jenkins to start automatic**
```
sudo systemctl enable jenkins
sudo systemctl start jenkins
```
### Verify Jenkin installation
Access jenkin using its IP and port 8080
```
http://158.177.112.231:8080
````
you will be providing by password verification. copy the string of file ``` /var/lib/jenkins/secrets/initialAdminPassword ``` and post it on the box. Then select install recommended plugins. The next step is to provide Jenkins admin user and its password. I used a user called jenkins. Once finished you will be login to Jenkins and see the first Jenkin welcome page

![](./images/Jenkins-welcome.png)

## Setup Jenkins Project
Jenkins projects are at the core of doing any kind of automation in Jenkins. The CI process for an application is generally configured in Jenkins as a Jenkins project. 

A simple way to implement a CI build in Jenkins is with a **freestyle project**

I will use a github project hosted on ``` https://github.com/linuxacademy/cicd-pipeline-train-schedule-jenkins.git ``` and use it to build freestyle project.
Click on __new item__ type a project name, select freestyle and press ok

![](./images/new_item_Free-style.png)

Click ok and From Source Code Management choose Git and enter git hub url ```  https://github.com/linuxacademy/cicd-pipeline-train-schedule-jenkins ```  

![](./images/source-code.png)


Then go down and from build - Add build steps - Our build here is build on Gradle. Then select Use Gradle Wrapper and enter build on the tasks

![](./images/buid.png)

The next step is to archieve build as artificate. On the Post-build Actions, select Archieve the artificats. And the file will be on dist/trainingSchedule.zip and then click save
![](./images/post-build.png)

Then we can run the build by clicking on build now

## Setting up GitHub webhooks in Jenkins.

One of the most important aspects of a good CI process is quick feedback whenever there is a change. This means that it is important to execute builds as soon as possible after a code change is pushed to source control. One of the best ways to do this with GitHub and Jenkins is to use webhooks to have GitHub notify Jenkins when there is a change so that Jenkins can automatically start the build. 
Configure webhooks in jenkins is relatively easy. We need to:
* Create an access token in GitHub that has permission to read and create webhooks.
* Add GitHub Server in Jenkins for GitHub.com.
* Create a Jenkins credential with the token and configure the GitHub server configuration to use it.
* Check " Manage Hooks" for the GitHub Server configuration.
* In the project configuration, under "Build Triggers", Select "GitHub hook trigger for GITScm polling".


