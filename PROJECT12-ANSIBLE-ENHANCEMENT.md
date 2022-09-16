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

Solution: I had to change the permission of all the directories and subdirectorys of /home/ubuntu/ansible-config-artifact to 755, before it builts, when all folders and sub-folders were on 755, code built, but I could not ssh to the machine, so I had to change all permission to 755 before I could build save-artifact.

## Step 2: REFACTOR ANSIBLE CODE BY IMPORTING OTHER PLAYBOOKS INTO SITE.YML

1. Within playbooks folder, create a new file and name it site.yml
2. Create a new folder in root of the repository and name it static-assignments
3. Move common.yml file into the newly created static-assignments folder.
4. Since I have ansible playbook that installed wireshark, I will need the one that will uninstall it by introducing import
5. Create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility. 

Note: The playbook code for deletion is in common-del.yml.
6. update site.yml with 
```markdown
- import_playbook: ../static-assignments/common-del.yml
``` 
instead of common.yml and run it against dev servers.cd /home/ubuntu/ansible-config-mgt/

7. Run the following to play the playbook.

```markdown
cd /home/ubuntu/ansible-config-mgt/

ansible-playbook -i inventory/dev.yml playbooks/site.yaml
```
Image of session 2:

## Step 3: CONFIGURE UAT WEBSERVERS WITH A ROLE ‘WEBSERVER’

1. Launch 2 fresh EC2 instances using RHEL 8 image, we will use them as our uat servers, so give them names accordingly – Web1-UAT and Web2-UA.
2. To create a role, you must create a directory called roles/, relative to the playbook file or in /etc/ansible/ directory. (Github)

3. You can choose to automate creation of webserver directory or manually, I chose manual because it was quite easy for me to create under our working repo.
the webserver directory tree can be seen on the file pane of the vscode window.
4. Update your inventory ansible-config-mgt/inventory/uat.yml file with IP addresses of your 2 UAT Web servers.


6. It is time to start adding some logic to the webserver role. Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:

Install and configure Apache (httpd service)
Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.
Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
Make sure httpd service is started. This task is in the webserver/task/main.yml

## STEP 4: REFERENCE WEBSERVER ROLE

1. Reference ‘Webserver’ role
Within the static-assignments folder, create a new assignment for uat-webservers uat-webservers.yml.Z

```markdown
---
- hosts: uat-webservers
  roles:
     - webserver
```
What this means is that ansible will host that we named "**uat-webservers**' under the inventory. After gathering the information about the host, it take up the roles that are defined in webserver.

2. update **site.yml**, since- its the entering point to the ansible configuration

```markdown
- hosts: uat-webservers
- import_playbook: ../static-assignments/uat-webservers.yml
```
What the above code means is that ansible do the followings:
* get the playbook code meant to be executed on **uat-webservers** from the file **/static-assignments/uat-webservers.yml**
* However, the uat-webservers.yml is referencing the webserver that has the actual ansible code in **main.yml**

3. In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path    = /home/ubuntu/ansible-config-mgt/, so Ansible could know where to find configured roles.**(In my case, my ansble server does not have ansible.cfg file)** 

Every effort to get around this issue proved abortive, so I had to downgrade to Ubuntu 20.00, with this, I got the ansible config file as expected. Also I did not install Jenkins in the new machine, meaning I had to clone the **ansible-config-mgt** after all the configurations above were done.

4. Run the playbook under **ansible-config-mgt**

```markdown
sudo ansible-playbook -i /home/ubuntu/ansible-config-mgt/inventory/uat.yml /home/ubuntu/ansible-config-mgt/playbooks/site.yaml
```

Summary of how the ansible will move from file to file:

```markdown
inventory/uat.yml > /static-assignments/uat-webservers.yml > webserver/task/main.yml
```
End of project 12,

Image of the final end goal.