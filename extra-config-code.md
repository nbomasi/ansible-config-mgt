
**SSH-KEY:**

```markdown
$ sudo chmod 400 osas.pem

```


**Git:**
$ codes: git checkout -b <branch name> -To create ```markdown
branch and check into it
$ url/github-webhook/
$ git switch main
```

**Copying archive directory**

 ```markdown
 $ cp -r /var/lib/jenkins/jobs/ansible/builds/5/archive/README.md /home/ubuntu
 ```

 **To confirm wireshark installation:**

 ```markdown
 $ rpm -qa | grep -i wireshark
 ```

 ```markdown
 $ date 
 ```
 
 **Last test task**
 - name: Create directory, file and set timezone on all servsers
   hostS: webservers, nfs, db, lb
   become: yes
   tasks:
     - name: create a directory
       file:
         path: /home/sample-folder
         state: directory

     - name: create a file
       file: 
         path: /home/sample-folder/sample-file.txt
         state: touch

     - name: set timezone
       timezone:
         name: Africa/Lagos
        