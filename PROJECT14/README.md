# CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP

## ANSIBLE ROLES FOR CI ENVIRONMENT

**Some terminology/Technology**

* Reverse Proxy: Is the application that sits in front of back-end applications and forwards client (e.g. browser) requests to those applications. Reverse proxies help increase scalability, performance, resilience and security. The resources returned to the client appear as if they originated from the web server itself.

* SonarQube: SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality, it is used to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities. Watch a short description here.

* Artifactory: Artifactory is a product by JFrog that serves as a binary repository manager. The binary repository is a natural extension to the source code repository, in that the outcome of your build process is stored. It can be used for certain other automation, but we will it strictly to manage our build artifacts.

## Configuring Ansible For Jenkins Deployment

**Step1. Install Jenkins**

```markdown
yum install java-11-openjdk-devel
yum install wget
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum install jenkins
sudo systemctl start jinkins
sudo systemctl status jinkins
```
Image of running jenkins:

1. In jenkins Install & Open Blue Ocean Jenkins Plugin

2. Create a new pipeline name it: