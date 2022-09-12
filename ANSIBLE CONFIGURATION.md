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

![Project-11-setup-diagram](https://user-images.githubusercontent.com/65962095/189554930-1e1d8abb-656a-4ba0-aa71-30d6c69945aa.png)


## STEP 1: INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

1. Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible.
2. In your GitHub account create a new repository and name it ansible-config-mgt.
3. Instal Ansible

```markdown
sudo apt update

sudo apt install ansible
```
4. Configure Jenkins build job to save repo content every time every time I change it.
 * Webhooks created, to link my github repo to Jenkins servser.
 * To create a freestyle project called 'ansible'
 * Build triger and post triger action, such that built file can be saved in archive, and any change to git repo will triger a built.

 ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
 setup tested and its up and running

 ## STEP 2: BEGIN ANSIBLE DEVELOPMENT
 1. In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature (Ansible-new-feature)
 2. Checkout the newly created feature branch to your local machine and start building your code and directory structure
 3. Create a directory and name it playbooks – it will be used to store all your playbook files.
 4. Create a directory and name it inventory – it will be used to keep your hosts organised.
 5. Within the playbooks folder, create your first playbook, and name it common.yml
 7. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) **dev, staging, uat, and prod** respectively.

 ## STEP 3: USE THE CONCEPT OF SSH-AGENT TO ENSURE THAT THE ANSIBLE MACHINE IS ABLE TO COMMUNICATE WITH THE THE REST MACHINE.
 
 1.  On the vscode git-bash terninal use the following 
 
 ```eval `ssh-agent -s`
 ssh-add <path-to-private-key>
 ssh-add -l
 ssh -A ubuntu@public-ip ```
 
The above will allow communication between VS code and ansible machine and hence ansible machine will be able to send command to other machines via ssh.

Now go back to the git branch where the inventory/dev.yml is to create inventory of the machines that the ansible will automate. The code are is the **branch**

## STEP 4: CREATE A COMMON PLAYBOOK 
Ansible is actually tailored towards tasks that are repeated on multiple machines.

Go to the branch to update playbooks/common.yml with command that ansible will execute on the other machines.

## STEP 5: TO COMMIT AND PUSH BRANCH CODE TO MAIN (MASTER) SO THAT JENKINS CAN BUILD CODE 

1. Use git commands to add, commit and push your branch to GitHub.

```markdown
git status

git add <selected files>

git commit -m "commit message"
```
Or You can commit through vs code GUI.

2. Create a Pull request (PR)

3. Wear a hat of another developer for a second, and act as a reviewer.

4. If the reviewer is happy with your new feature development, merge the code to the master branch.

5. Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.

Once your code changes appear in master branch – Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server. Built is successful

**Issue: **Jenkins did not build the the files updated to git.

**Solution:** I forgot that I used dynamic IP not elastic one, so I had to update the current IP on webhook.

## STEP 6: RUN FIRST ANSIBLE TEST

Now I will execute the playbook commands through the vs code with no need to remotely login into the ansible machine.

Do the following on git cli.

To ececute the command, I have to cd into the archive where the playbooks and the inventory are, else, playbook won,t run

```markdown
cd ansible-config-mgt
ansible-playbook -i inventory/dev.yml playbooks/common.yml
```
Project 11 achieved.

![Project-11-final](https://user-images.githubusercontent.com/65962095/189554946-6e2914e9-0240-48cc-b9b4-ba39e781cb71.png)
