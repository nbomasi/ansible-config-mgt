# AANSIBLE REFACTORING, ASSIGNMENTS & IMPORTS


**Note: This project is an enhancement to project 11, I will maintain the same repo (ansible-config-mgt). This project will introduce us to the following concept: Refactoring, assignment, imports, and role. this means that all must be up and running well before I can implement project 10.**

**Keywords:**
* **IMPORT:** A functionality that allows ansible playbook code to be reused.
It also make codes more organised.

* **REFACTORING:** is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

## Cloud Infracture for the Project

* **Cloud Platform:** AWS
* **OS**: 5 Red Hat Enterprise Linux 8; 3 Ubuntu 20.04

**Server Applications:**
* Webserver (Red Hat Enterprise Linux 8): 2 Apache2
* Load balancer Server (Ubuntu 20.04): NGINX load balancer
* Database server (Ubuntu 20.04): MYSQL
* Storage Server (Red Hat Enterprise Linux 8): NFS server
* CI/CD Server (Ubuntu 20.04): Jenkins-Ansible
* UAT Webserver (Red Hat Enterprise Linux 8):

**Project setup diagram:**

## Step 1: Jenkins job enhancement

There is a setback with the configuration earlier done on Jenkins, "any change" made to the source codes builds automatically and create a separate directory in the artifats directory, imagine making like 1k change per day, that means 1k files created, which will consume lots of space in Jenkins server.

Solution:
1. On Jenkins-Ansible server, create a new directory:

```markdown
sudo mkdir /home/ubuntu/ansible-config-artifact
chmod -R 0777 /home/ubuntu/ansible-config-artifact
```
2. Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins
3. Create a new Freestyle project and name it save_artifacts.
4. This project will be triggered by completion of your existing ansible project. Configure it accordingly:

Steps taken to change default artifats build directory to the new created directory is shown in images below.

Issue: the save-artifact refused to build, even after much editing and research

Solution: I had to change the permission of all the directories and subdirectorys of /home/ubuntu/ansible-config-artifact to 777, before it builts.
