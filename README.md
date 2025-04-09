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
