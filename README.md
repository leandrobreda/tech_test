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

## Running the playbook:

```
lebreda:tech_test$ ansible-playbook tech_challenge.yaml --ask-vault-password
Vault password: 
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Manage EC2 instances] **************************************************************************************************************

TASK [aws-manage : Create the "webgroup" security group] *********************************************************************************
changed: [localhost]

TASK [aws-manage : Create a key pair based on the personal I have] ***********************************************************************
changed: [localhost]

TASK [aws-manage : Provision an AWS instances] *******************************************************************************************
changed: [localhost]

PLAY RECAP *******************************************************************************************************************************
localhost                  : ok=3    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

lebreda:tech_test$ 
```

... my AWS environment after having run this playbook:

![image](https://user-images.githubusercontent.com/108529920/200128012-6d3a7462-571e-4d36-924d-11830f7a46e2.png)


#### The new security group webgroup was created...
![image](https://user-images.githubusercontent.com/108529920/200128138-78fd6b79-d809-4100-9aee-b959b959001d.png)

#### … allowing access to 22, 80 and 443 ports:
![image](https://user-images.githubusercontent.com/108529920/200128198-37e62a98-bb45-40be-9a23-27b03bdd1fb3.png)

#### A tag to identify that it was created by Ansible was also added:
![image](https://user-images.githubusercontent.com/108529920/200128732-9f54c393-332f-44e3-8371-6961ec9f3984.png)

#### Same for my personal key uploaded:
![image](https://user-images.githubusercontent.com/108529920/200128320-ff4ad3f3-02d8-4eb8-8e5a-44f3008f73aa.png)

#### The new EC2 instance called webserver1 was successfully created:
![image](https://user-images.githubusercontent.com/108529920/200128441-b866b113-fdf8-4b37-9005-4390f8fd987a.png)

#### ...Tags were added to the new created EC2 instance:
![image](https://user-images.githubusercontent.com/108529920/200128487-ab081456-0ebd-4896-a93a-8e12b3f01ec7.png)

#### ...Tested access to the EC2 instance webserver1 with my personal key and verified that the httpd service is running:
![image](https://user-images.githubusercontent.com/108529920/200128626-c27536d5-8ccf-4d6b-89fe-4910c2d3479e.png)

#### Verified httpd connectivity:
![image](https://user-images.githubusercontent.com/108529920/200128683-6e4b86e4-2a18-42d8-aa87-cd0be8ecb857.png)
