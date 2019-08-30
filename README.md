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
