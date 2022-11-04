# Tech_test

## Playbook overview:

 This Ansible playbook and role has the main objective to create a security group on AWS VPC to allow http and https tcp ports, and an instance (EC2), where apache will be installed and running.

## Requirements:

 In order to run this playbook, following configuration are required:
 - Following packages must be installed in a linux machine/server
   - ansible
   - python3
   - pip3
   - booto3 (AWS SDK)
 - Install ansible AWS collection: ansible-galaxy collection install community.aws
 - Create a credential with Programmatic access on AWS and grant it AmazonEC2FullAccess role. You will need its Access key and Secret Key to stablish a connection with AWS. (Take note of boths, Access and Secret)

**Here I'm following Ansible best practices, so no plain text password will be used to stablish AWS connectivity, instead we will use ansible-vault to create a file with both info (Access and Secret keys) as variables for our playbook**

 ## Creating the vault file with AWS credentials:
 ```
  ansible-vault create vars/ec2adm_cred.yml
  Vault password: 
  acc_key: <put_your_access_key_here>
  sec_key: <put_your_secret_key_here>
 ```

## Playbook tasks:
 Bellow all tasks performed by this playbook are listed and explained:

```
lebreda:tech_test$ ansible-playbook tech_challenge.yaml --ask-vault-password --list-tasks
Vault password: 

playbook: tech_challenge.yaml

  play #1 (localhost): Manage EC2 instances	TAGS: []
    tasks:
      aws-manage : Create the "{{ secgroup }}" security group	TAGS: []  ***This task creates a security group on AWS VPC to allow http and https tcp ports connectivity.***
      aws-manage : Create a key pair based on the personal I have	TAGS: []  ***This task imports my personal ssh key pair to allow me access the EC2 instance when created just using my keys.***
      aws-manage : Provision an AWS instances	TAGS: []   ***This task creates a new EC2 instance on AWS, attaching it to the VPC and security group we just created. It will also install httpd and enable its service.***
lebreda:tech_test$
```

## AWS Environment:
My AWS environment before running this playbook:
![image](https://user-images.githubusercontent.com/108529920/200081633-c1ec4dc6-b8b8-4365-8e7b-894a28e293b4.png)



