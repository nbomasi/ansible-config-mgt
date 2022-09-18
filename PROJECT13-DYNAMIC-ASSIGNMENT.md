Still using same ansible machine, same git repository.

1. Create a branch under ansible-config-mgt
2. Create a folder, name it dynamic-assignments, create a file named env-vars.yml
3. Paste the code bellow in env-vars.yml

```markdown
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always
```
Note: include plays the same role as import in stattic assignment. but include is use in dynamic assignment.
When the import module is used, all statements are pre-processed at the time playbooks are parsed. Meaning, when you execute site.yml playbook, Ansible will process all the playbooks referenced during the time it is parsing the statements. This also means that, during actual execution, if any statement changes, such statements will not be considered. Hence, it is static.

On the other hand, when include module is used, all statements are processed only during execution of the playbook. Meaning, after the statements are parsed, any changes to the statements encountered during execution will be used.

Take note that in most cases it is recommended to use static assignments for playbooks, because it is more reliable. With dynamic ones, it is hard to debug playbook problems due to its dynamic nature. However, you can use dynamic assignments for environment specific variables as we will be introducing in this project.

**4. Updte site.yml with dynamic assignment**

Update site.yml which is our usual ansible entering point.

```markdown
---
- hosts: all
- name: Include dynamic variables 
  tasks:
  import_playbook: ../static-assignments/common.yml 
  include: ../dynamic-assignments/env-vars.yml
  tags:
    - always

-  hosts: webservers
- name: Webserver assignment
  import_playbook: ../static-assignments/webservers.yml
```
* Community Roles
Now it is time to create a role for MySQL database – it should install the MySQL package, create a database and configure users. But why should we re-invent the wheel? There are tons of roles that have already been developed by other open source engineers out there. This already made code can help us rather than start from the scratch. I will be using my sql role created by geerlingguy. Note that to do this you will need ansible-galaxy package to do this.

cd into  ansible-config-mgt.git then, create a folder and name it role, then create a new git branch:

```markdown
git branch roles-feature
git switch roles-feature
```
Cd into the roles directory and install community mysql rename the default installation folder as **mysql**
```markdown
ansible-galaxy install geerlingguy.mysql 
mv geerlingguy.mysql/ mysql
```