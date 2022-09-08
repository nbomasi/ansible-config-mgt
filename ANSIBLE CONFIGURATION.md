# ANSIBLE CONFIGURATION MANAGEMENT – AUTOMATE PROJECT 7 TO 10 UNDER THE REPO "Bomasi-DevOps"


**Note: This project is an enhancement to project 7,8,9 and 10, this means that all must be up and running well before I can implement project 10. We are still working with the existing setup, but ansible will be added but will share the same OS with Jenkins, the the ec2 will be renamed to Jenkins-Ansible A new Repo has been created on github named "ansible-config-mgt"**


## Cloud Infracture for the Project

* **Cloud Platform:** AWS
* **OS**: 3 Red Hat Enterprise Linux 8; 3 Ubuntu 20.04

**Server Applications:**
* Webserver (Red Hat Enterprise Linux 8): 2 Apache2
* Load balancer Server (Ubuntu 20.04): NGINX load balancer
* Database server (Ubuntu 20.04): MYSQL
* Storage Server (Red Hat Enterprise Linux 8): NFS server
* CI/CD Server (Ubuntu 20.04): Jenkins-Ansible

**Project setup diagram:**

## STEP 1: INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

1. Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible.
2. In your GitHub account create a new repository and name it ansible-config-mgt.
3. Instal Ansible

```markdown
sudo apt update

sudo apt install ansiblcle
```
4. Configure Jenkins build job to save repo content every time every time I change it.
 * Webhooks created, to link my github repo to Jenkins servser.
 * To create a freestyle project called 'ansible'
 * Build triger and post triger action, such that built file can be saved in archive, and any change to git repo will triger a built.

 ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
 setup tested and its up and running

 ## STEP 2: BEGIN ANSIBLE DEVELOPMENT