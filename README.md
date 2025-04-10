# Ansible Playbook to Install and Start NGINX
This project demonstrates a simple yet powerful use of Ansible to automate the installation and startup of the NGINX web server on a remote target machine. This is part of my hands-on DevOps learning journey.
## Scenario
We are equipped with two servers. Using the Ansible Server (AS), we need to remotely configure the Target Server by installing Nginx and starting the service.
## Server setup
* Create two EC2 instances on AWS namely Ansible Server and Target Server.
* In AS, install ansible.
  ```
  sudo apt update
  sudo apt install ansible
  ```
  ## Setup passwordless authentication
  Ansible should be able to communicate with taget servers withot any passwords.
  * In AS run
    ```
    ssh-keygen
    ```
    This will create files with private and public keys for you. We will use the public key to create connection to target server.
  * Open the public key file and copy the contents.
  * Run the keygen on TS as well.
  * In TS, open the authorized_keys file
    ```
    vim ~/.ssh/authorized_keys
    ```
  * Paste the public key of AS in here and save the file.
  * Copy the private ip address of TS.
  * In AS, run
    ```
    ssh (TS-private-ip)
    ```
    Now you will be authenticated in to TS.
## Create inventory file
We will create an inventory file in AS to store the ip addresses of target machines.
```
vim inventory
```
Inside this file, paste the private ip address pf the TS and save and quit the file.
## Playbook Description
The playbook is structured to perform the following tasks sequentially:

__1.Update APT cache__
Ensures the package manager has the latest list of packages.

__2.Install NGINX__
Installs the nginx package if it's not already present.

__3.Start and Enable NGINX Service__
Starts the nginx service so it runs immediately.
* Create a playbook file on AS
  ```
  vim playbook1.yml
  ```
* Enter the following YAML code in the playbook
```
---
- name: install and start nginx
  hosts: all
  become: true

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: install nginx
      apt:
        name: nginx
        state: present

    - name: start nginx
      service:
        name: nginx
        state: started
```
![playbook file](https://github.com/user-attachments/assets/1d553a25-8955-46a0-8442-29a8e56e8310)

* Execute the playbbook by running:
```
ansible-playbook -i inventory playbook1.yml
```
## Execution Output
Here's a sample output after successful execution:
![run playbook](https://github.com/user-attachments/assets/09bcb1a5-fbc2-40f3-a168-dfde7f02171d)
## Check nginx status
Now we can check the TS to see whether nginx has been installed and started successfully.
* In TS run
```
sudo systemctl status nginx
```
* If everything has been implemented successfully, the output should show that nginx is active and running.
![check for nginx in target](https://github.com/user-attachments/assets/f881df49-95cd-4d7e-ae1d-e61257230256)
## Debugging
If you want to debug the playbook and check for errors, run
```
ansible-playbook -vvv -i inventory playbook1.yml
```
## Skills Demonstrated
* Writing Ansible playbooks

* Managing package installations using Ansible modules

* Controlling remote servers with Ansible inventory

* Automating service management (start/stop/restart)
