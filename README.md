# Ansible Playbook for Stage Deployments #

This Playbook proceeds tasks which are needed to be done before containerized services deployments to Stage server. Briefly it is responsible for preparing the environment.

![](src/ansible.png)

### Parts of this repository ###

* [Backend Playbook](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main/ansible-backend-playbook.yaml)
* [Frontend Playbook](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main//ansible-stage-frontend-playbook.yaml)
* [Inventory](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main/src/master/hosts)
* [Pipeline](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main/bitbucket-pipelines.yml)
* [Template Nginx.conf](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main/src/master/nginx/)

### How does it work? ###

1. [Backend Playbook](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main/ansible-stage-backend-playbook.yaml) consists of two main parts: Stg Ops Part is responsible for performing tasks on Stage Server, whereas Mysql Ops Part is responsible for database related tasks.

* Stg Ops: As we comment out in the code stg ops performs the following tasks on the Stage Server: Adding App User, File Management related tasks in user's home path, Public Key Operation, Adding Nginx Conf File for app, Storing mysql credentials.

* Mysql Ops: This part is running on the localhost. It is responsible for Creating a database for APP , Assigning the pass generated in pipeline step to the user and Granting necessary roles on the user.

2. [Frontend Playbook](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main/ansible-stage-frontend-playbook.yaml) consists of one main part: Stg Ops Part is responsible for performing tasks on Stage Server.

* Stg Ops: As we comment out in the code stg ops performs the following tasks on the Stage Server: Adding App User, File Management related tasks in user's home path, Public Key Operation, Adding Nginx Conf File for app.

3. [Inventory](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main/src/master/hosts) consists of the servers that our playbook looking for.  **'ansible_ssh_private_key_file'** parameter enables us to make a connection to the host. 


4. In [Pipeline](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main/bitbucket-pipelines.yml) we use custom mode to trigger pipelines whenever we need it; especially when a new service has been emerged. Caching part enables us to fasten the installation of dependencies. Using script part we can run the Playbook.


5. [Template Nginx.conf](https://github.com/elif-apaydin/bf-project-deploy-ansible/blob/main/src/master/nginx.conf) consists of the nginx templates used by backend/frontend services; we manipulate content of it with playbook.

### Variables ###

Before running pipeline, you should check current values of variables such **APP_NAME**, **PORT** and **B64_APP**. 

B64_APP must be the encoded public key (base64) generated for app. You can simply encode and copy of the content of the public key with the following command:

` openssl base64 < public-key | tr -d '\n' | pbcopy `

![](src/src1.png) 
