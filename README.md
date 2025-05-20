- [Introduction](#Introduction)

- [Ansible Arichitecture](#Ansible-Arichitecture)

  - [Modules | Ansible architecture](#Modules)
  
  - [Ansible uses YAML](#Ansible-uses-YAML)
  
  - [Ansible Playbooks](#Ansible-Playbooks)
  
  - [Ansible Inventory List](#Ansible-Inventory-List)
  
  - [Ansible for Docker](#Ansible-for-Docker)
 
  - [Ansible Tower](#Ansible-Tower)
 
- [Install Ansible](#Install-Ansible)

- [Set up Managed Server](#Set-up-Managed-Server)

- [Ansible Inventory and Ansible ad hoc commands](#Ansible-Inventory-and-Ansible-ad-hoc-commands)

  - [Ansible Inventory](#Ansible-Inventory)
 
  - [Ansible ad-hoc commands](#Ansible-ad-hoc-commands)
 
- [Configure AWS EC2 Server with Ansible](#Configure-AWS-EC2-Server-with-Ansible)

- [Managing Host Key Checking](#Managing-Host-Key-Checking)

  - [Authorized Keys and Known Hosts](#Authorized-Keys-and-Known-Hosts)
 
  - [Disalbe Host Key Checking](#Disalbe-Host-Key-Checking)
 
- [Introduction Ansible Playbook](#Introduction-Ansible-Playbook)

  - [Write a simple playbook](#Write-a-simple-playbook)
 
  - [Executing-Playbook](#Executing-Playbook)
 
  - [Install specific Version](#Install-specific-Version)
 
  - [Ansible itempotency](#Ansible-itempotency)
 
- [Modules and Collections in Ansible](#Modules-and-Collections-in-Ansible)

  - [What is a Collection](#What-is-a-Collection)
 
  - [Ansile Galaxy](#Ansile-Galaxy)
 
  - [Ansible Namespaces](#Ansible-Namespaces)
 
- [Deploy Nodejs application](#Deploy-Nodejs-application)

  - [Create Digital Droplet](#Create-Digital-Droplet)
 
  - [Write Playbook](#Write-Playbook)
 
  - [Install node and npm](#Install-node-and-npm)
 
  - [Copy and unpack tar file](#Copy-and-unpack-tar-file)
 
  - [Start Node App](#Start-Node-App)
 
  - [Ensure App is running](#Ensure-App-is-running)
 
  - [Ansible shell vs command Module](#Ansible-shell-vs-command-Module)
 
  - [Executing tasks with a different user](#Executing-tasks-with-a-different-user)
 
- [Ansible Variable](#Ansible-Variable)

  - [Registered Variable](#Registered-Variable)
 
  - [Parameterzie Playbook](#Parameterzie-Playbook)
 
  - [Variables defined in a Playbook](#Variables-defined-in-a-Playbook)
 
  - [Passing Variables on the Command Line](#Passing-Variables-on-the-Command-Line)
 
  - [External Variables File](#External-Variables-File)
 
- [Deploy Nexus](#Deploy-Nexus)

  - [Preparation](#Preparation)
 
  - [First Play](#First-Play)
 
  - [Second Play](#Second-Play)
 
  - [find Module](#find-Module)
 
  - [Conditionals in Ansible](#Conditionals-in-Ansible)
 
  - [Third Play](#Third-Play)
 
  - [Fourth Play](#Fouth-Play)
 
  - [Fifth Play](#Fifth-Play)
    
  - [Configure Git](#Configure-Git)
 
  - [Configure Inventory Default Location](#Configure-Inventory-Default-Location)
 
- [Ansible and Docker](#Ansible-and-Docker)

  - [Ansible and Docker](#Ansible-and-Docker)
 
  - [Adjust Inventory file](#Adjust-Inventory-file)
 
  - [Frist Play Install Docker](#Frist-Play-Install-Docker)
 
  - [Second Play Instal Docker Compose](#Second-Play-Instal-Docker-Compose)
 
  - [Third Play Start Docker Daemon](#Third-Play-Start-Docker-Daemon)
 
  - [Fourth Play Add ec2 user to Docker Group](#Fourth-Play-Add-ec2-user-to-Docker-Group)
 
  - [Refactor the Playbook](#Refactor-the-Playbook)
 
  - [Use community docker Ansible Collection](#Use-community-docker-Ansible-Collection)
 
  - [Pull Docker Image](#Pull-Docker-Image)
 
  - [Start Docker Containers](#Start-Docker-Containers)
 
  - [Make the Playbook more generic](#Make-the-Playbook-more-generic)
 
- [Ansible and Terraform](#Ansible-and-Terraform)

  - [Intergrate Ansible Playbook execution in Terraform](#Intergrate-Ansible-Playbook-execution-in-Terraform)
 
  - [Wait for EC2 Server to be fully initialized](#Wait-for-EC2-Server-to-be-fully-initialized)
 
  - [Using null resource](#Using-null-resource)
 
- [Dynamic Inventory](#Dynamic-Inventory)

  - [Terraform Create EC2](#Terraform-Create-EC2)
 
  - [Inventory Plugin and Inventory Script](#Inventory-Plugin-and-Inventory-Script)
 
  - [AWS EC2 Plugin](#AWS-EC2-Plugin)
 
  - [Write plugin configuration](#Write-plugin-configuration)
 
  - [Assign public DNS to EC2 Instances](#Assign-public-DNS-to-EC2-Instances)
 
  - [Configure Ansible to use dynamic inventory](#Configure-Ansible-to-use-dynamic-inventory)
 
  - [Target only Specific Servers](#Target-only-Specific-Servers)
 
  - [Create Dynamic Groups](#Create-Dynamic-Groups)
 
- [Deploying Application in Kubernetes](#Deploying-Application-in-Kubernetes)

  - [Create EKS Cluster with Terraform](#Create-EKS-Cluster-with-Terraform)
 
  - [Create a Namespace in EKS Cluster](#Create-a-Namespace-in-EKS-Cluster)
 
  - [Deploy App in new Namespace](#Deploy-App-in-new-Namespace)
 
  - [Set ENV for kubeconfig](#Set-ENV-for-kubeconfig)

- [Run Ansible from Jenkins Pipeline](#Run-Ansible-from-Jenkins-Pipeline)

  - [Prepare Ansible Control Node](#Prepare-Ansible-Control-Node)
 
  - [Create 2 EC2 Instances to be managed by Ansbile](#Create-2-EC2-Instances-to-be-managed-by-Ansbile)
 
- [Ansible Integration in Jenkins](#Ansible-Integration-in-Jenkins)
 
  - [Copy files from Jenkins to Ansible Server Jenkinfile](#Copy-files-from-Jenkins-to-Ansible-Server-Jenkinfile)
 
  - [Create Jenkins pipeline](#Create-Jenkins-pipeline)
 
  - [Execute Ansible Playbook from Jenkins](#Execute-Ansible-Playbook-from-Jenkins)
  

# Ansible-

## Introduction

Ansible is a tool that help automate different IT tasks 

For example: When I have a complex IT Infrastructure with 10 of Servers, some IT tasks can be time consuming and tedious to do manually and I have my application running on it, now I release new application version and I need to deploy that on all those Servers . 

 - Or I want to update Docker Version on all those 10 Servers . Sometime upgrades might contain multiple steps

 - Or Repetitive tasks that system administator perform and often take hours to do manually, such as updates, backups, system rebootsm, creating users, assigning groups ...

Ansible come in to make those task much more efficent and less time consuming in 4 ways : 

 - Execute tasks from my own machine like update and deployment etc on all the servers from my own machine remotely instead of ssh to the Server one by one

 - I can do it by writing all the steps of configuring, installing, deploying etc in a single YAML file . Instead of doing it with manual steps or combination of shell script and manual steps

 - I can reuse the same file to execute the same task sequence multiple times if needed on the same server when I am doing -upgrade or on different environemnt when I am trying to create the identical on dev, staging, and prod environment

 - More reliable and less likely for errors when configuring server or deploying applications

Right now the state of Ansible is that whenever IT or System Administator task I can imagine, starting from OS level updates to cloud provisioning, I can do all of them with Ansible bcs it really has expanded so much that it supports all operating system, a lot of different Cloud Providers, work with virtual cloud servers, as well as bare metal infrastructure

The only thing I have to do in order to execute these Ansible files on the target server from my own control machine is simple SSH access to the target Servers . This is a unique advantages of Ansible compared to alternative tools that is Agentless 

Nomarlly I want to use some tools on the machine I need to go to machine and install Agent for that tool . To use Ansible however I don't have to install anything on the target servers, I just install it on one machine, which is my control machine, could be even my laptop, and that machine can now manage the whole fleet of target machines remotely . This mean I don't have any deployment effort at the beginning and second I don't have an upgrade effort when moving to a new Ansible Version 

## Ansible Arichitecture

#### Modules

Ansible work with Module 

Module is a small program that do the actual work . Thet get sent from the control machine to the target server, so they get pushed out there . They do their job on the target server like installing application, stop processes, apply firewall rules etc .. when they done they get removed

Important to Note that Modules are very granular . One module does one small specific task, so I have a module to create or copy a file, module to install an Nginx server or module to start that installed Nginx, or to start Docker container with some arguments, or create Cloud instance . So all of these are specific task that represent a module, and Ansible has 100 of modules that each execute a specific task  

with Ansible I can execute any IT task bcs of these modules

#### Ansible uses YAML 

No need to learn any special language . It's super intuitive 

For example Jenkins YAML : 

<img width="629" alt="Screenshot 2025-05-01 at 11 51 04" src="https://github.com/user-attachments/assets/36786b06-a5d3-4557-ac56-ccb88f3dd62e" />

Docker YAML: 

<img width="629" alt="Screenshot 2025-05-01 at 11 51 50" src="https://github.com/user-attachments/assets/122bdce8-5f5f-47dc-97a5-c2b506ace092" />

Postgres YAML:

<img width="732" alt="Screenshot 2025-05-01 at 11 52 50" src="https://github.com/user-attachments/assets/fd48a17a-3b1c-4ca8-8e4f-8e7f425f07c3" />

 When I handle complex application I will need multiple of small modules in a certain sequence grouped together to represent that one whole configuration or all the steps to deploy the application, that is where Ansible Playbooks come in

#### Ansible Playbooks

Sequential modules are grouped in tasks, where each task makes sure the module gets executed with certain argument, Also describe the tasks using the name 

<img width="600" alt="Screenshot 2025-05-01 at 11 57 36" src="https://github.com/user-attachments/assets/0be70123-1a77-4532-8a6f-6ac272d16927" />

Above picture I have a example of group of task where table get rename the owner get set, and then the table gets truncate all using PostgresQL module . 

Or I can execute module in a sequence . I have module files that create a directory for nginx, then I have yum module that install nginx then I have service module that starts that nginx and that will get executed exactly in that sequence and that will be 1 configuration

<img width="600" alt="Screenshot 2025-05-01 at 12 40 24" src="https://github.com/user-attachments/assets/8ee43f18-78a0-4a92-a794-599b970497b6" />

 I also have to define where or on which target machines these tasks should be executed . And I do that using the `HOSTS` attribute . In this case said that the following tasks here should be executed on database hosts, and we also define with which user those tasks should be executed . `remote_user` is which user those tasks should be executed 

<img width="600" alt="Screenshot 2025-05-01 at 12 44 12" src="https://github.com/user-attachments/assets/e4d15afd-1f96-44ed-9199-b67633098b4b" />

Another concept is I can use variables for repeating values in ansible . And the way to do that is on the same level, I can define vars attributes

<img width="600" alt="Screenshot 2025-05-01 at 12 47 01" src="https://github.com/user-attachments/assets/fb3322a0-c6e2-45c1-bb07-1fcc64b31e75" />

This whole block define which task should be executing on which hosts and which users plus maybe some additional attributes is called a `Play` 

<img width="600" alt="Screenshot 2025-05-01 at 12 48 30" src="https://github.com/user-attachments/assets/7dad0144-9dd7-4e23-a135-322e20d0373e" />

In a single file I can have multiple such plays, which maybe depend on each other or needs to be executed in the sequence . For example I have a Play for Web server where these following tasks get executed on web server hosts and I have another play where a bunch of tasks get executed on database hosts . And file that contain one or multiple plays is called `Playbook`

<img width="600" alt="Screenshot 2025-05-01 at 12 56 54" src="https://github.com/user-attachments/assets/b4f5a367-000a-4a96-9b4f-9d90575faadc" />

`Playbook`is describe how and in which order and where, meaning on which machines the modules, which are part of the tasks should be executed. Or in other words it orchestrates the module execution, And in playbook I can name my Plays using the name attribute, just like I can name the tasks, which actually is good pratice bcs it kind of summarizeds in plain English what the play does 

<img width="600" alt="Screenshot 2025-05-01 at 12 57 45" src="https://github.com/user-attachments/assets/b8c9a2c4-b644-4469-a723-4f0a61003dec" />

#### Ansible Inventory List

The values of the `hosts` attribute come from  Ansible's inventory list, which looks like this  

<img width="600" alt="Screenshot 2025-05-01 at 12 59 19" src="https://github.com/user-attachments/assets/8ef0fd60-9a8c-4f27-83fb-5a91af0a03c7" />

In Ansible I have this `hosts file`  that basically keeps a list of inventory where inventory is all the machines that are involved in Ansible task executions . And I can define the entries in that host file as IP addresses or the hosts names . 

And this syntax `[webserver]` is basically group multiple IP addresses or hosts name together so that I can reference them as a group  

Let's say I have a Play that should execute on all my Databases and I have 20 of them so I can group them here and then reference it using that Group Name . And these hosts names or the IP address could be of cloud Instances, virtual or bare metal servers 

#### Ansible for Docker 

Usually when I want to create Docker container I write Dockfile which is prepare Application Environment . So 
I have the Application Artifact and then I define the Dockerfile which is config file should be copied into the environment maybe a directory should be created where the log files will be stored . I defined and set environmental . May I also copy a start script that should be executed before application start 

So All this basically prepares the Environtment for Application Start up . And with Dockefile that will produce a Docker container with application and its prepared environment 

With Ansible I can acutally create an alternative to Dockerfile, which is more powerful . I can take the same type of configuration with application artifact and all these things around it and instead of just creating Docke container I can basically create a docker container, I can create Vagrant container from it, I can deploy the application and all these Environment on a cloud Instance or even Bare Metal server 

<img width="600" alt="Screenshot 2025-05-01 at 13 14 43" src="https://github.com/user-attachments/assets/b0976080-0ee9-4b2f-8cfa-dd99b4a55a6e" />

What this mean is Ansible allow me to reproduce the Application accross many Environments and not only Docker container 

Another advantage of doing that is that I can not only manage the Docker container, but also the host where this container is running . For example Container have dependencies on the Storage of the host or the network using Ansible can manage all these different parts, which can make it possible to create one play in a playbook that execute tasks both on a container and the host level 

<img width="739" alt="Screenshot 2025-05-01 at 13 18 49" src="https://github.com/user-attachments/assets/1bbd9e86-8279-4c74-8601-7f1053a3f614" />

#### Ansible Tower 

Is a UI dashboard from Red Hat, which basically gives me a way to centrally store all my automation tasks across teams, where I can configure permissions for those teams who can access and execute what tasks . Where I can manage my inventory and have a good overview of which jobs have run adn their health status 

<img width="600" alt="Screenshot 2025-05-01 at 13 21 45" src="https://github.com/user-attachments/assets/58eeae47-05dc-4234-8bbc-d63ef5d103b4" />

## Install Ansible

Install Ansible Locally then I connect to target Server then I execute Ansible commands or Ansible playbooks to manage those target 

Another ways is  have separate remote server where Ansible installed, and then I can acutally manage all the other remote servers with Ansible installed on that specific remote server . Common use case for this is if all my Servers are in some kind of private network, I can only communicate with in that network so I might not be able to administer these Server from my laptop if it's outside that network . In that case I have dedicated server with Ansible installed . And I would execute all the Ansible commands from that Server 

Intall Locally on Mac : `brew install ansible`

!!! Note : Ansible is written in Python so that mean Ansible also needs Python to run . If I want to install Ansible in an alternative way, maybe on different operating system, I could also install it using Python 
Package manager called Pip : `pip install ansible`

## Set up Managed Server 

I will create 2 Server on Digital Ocean so that I can configure them with Ansible . 

Once I have 2 Server running I want to connect Ansible to these Remote Servers to be able to configure them to execute commands on them . 

Ansible connects to the target Servers using SSH it does not need any Agent installed on those Servers 

!!! NOTE: For Linux Servers there has to be Python installed . IF it is a Windows Server , then Powershell has to be installed on the Window Server  

## Ansible Inventory and Ansible ad hoc commands

#### Ansible Inventory 

Ansible Inventory File 

 - File containing data about the Ansible Client servers

 - `hosts` meaning the manged Servers

 - Default location for file : `/etc/ansible/hosts`

First I need to tell Ansible What are the IP Address or hosts name of the Servers it has to manage . And we do that using a file called hosts : `vim hosts`

 - In the `hosts` file I need all the IP address that I want to connect to

 - Also need credentials to authenticate with  the Server. I need either username, password for the server or a private SSH key . I can specify which private key Ansile should use for each host using Attribute called `ansible_ssh_private_key_file=~/.ssh/id_rsa`

 - Also I need to specify the username of the server, which is `root` by using `ansible_user=root` 

```
157.230.29.95 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root
157.230.23.199 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root
```

#### Ansible ad hoc commands

<img width="600" alt="Screenshot 2025-05-01 at 14 02 31" src="https://github.com/user-attachments/assets/97877c6a-0149-4372-88cb-b8e65850b302" />

`ansible [pattern] -m [moudle] -a "[module options]"`

 - [pattern] = targeting hosts and group

 - `-m` : Stand for `module` . Ansible has modules which are basically containers for one specific command

Whenever I want to execute Ansible commands for a list of server that I have in hosts file, I can do `ansible all -i hosts -m ping`

 - `all`: Default group, which contains every hosts . I want to execute this command for all the server or all the IP address and host names . Inside the hosts file . 

 - `-i hosts`: I also have to specify by which host file I want to use bcs it could be multiple

 - `-m ping`: Ansible module called ping

Imagine we had servers on multiple different platforms . Like 2 Servers on Digital Ocean, 10 Servers on AWS ... . In this case I may configure them differently base in which Servers they are . 

<img width="600" alt="Screenshot 2025-05-03 at 10 37 52" src="https://github.com/user-attachments/assets/e4d61299-b01a-4798-ae07-37d5e08ba443" />

Executing `all` command to basically target every single Server in the `host` is not alway work bcs we may need to address specific Servers 

I need to Group a Servers, so I can address as one group in Ansible by using `[]`, then give the Groups a name `[droplet]` . For example : 

```
[droplet]
157.230.29.95 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root
157.230.23.199 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root

[aws]
<aws address>
```

So now I know those address above which belong to which 

To execute command only to only target the Droplet Server : `ansible droplet -i hosts -m ping` .

 - This command say execute all the address group under `droplet`

Another the use case If I have multiple different types of Servers . For example we had 5 Servers that were database Servers and we had 5 server that were web Application Servers . I could do 

```
[database]
157.230.29.95 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root
157.230.23.199 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root

[web]
<web address>
```

What if I want to address 1 specific Server in that list even though it's part of droplet . I can do `ansible <ip-address> -i host -m ping`

With Ansible I can target Individule Servers, Group of Servers or All Servers 

Another example where we have 50 droplets on DigitalOcean and they, and they all configure with the same ssh key and they all use the same user . I can configure same ssh key and same user in a Group . Like this : ...

```
[droplet]
157.230.29.95 
157.230.23.199 

[droplet:vars]
ansible_ssh_private_key_file=~/.ssh/id_rsa
ansible_user=root
```

## Configure AWS EC2 Server with Ansible

Create another group call EC2 in the `hosts` file 

In AWS, for every Instances, in addition to the IP address, Private and Public , I also get the host names (DNS names) . In the Inventory file (hosts file) I can also DNS name of the Server 

The same way for Droplet Servers, we want to configure which OS user I want to connect with to the servers and which Private key 

SSH `.pem` private key need to modify stricter before I can connect to EC2 `chmod 400 ~/.ssh/tim.pem`

```
[droplet]
157.230.29.95 
157.230.23.199 

[droplet:vars]
ansible_ssh_private_key_file=~/.ssh/id_rsa
ansible_user=root

[ec2]
ip-10-0-1-121.us-west-1.compute.internal
ip-10-0-2-70.us-west-1.compute.internal

[ec2:vars]
ansible_ssh_private_key_file=~/.ssh/tim.pem
ansible_user=ec2-user
```

Now I can ssh to the Server `ansible ec2 -i hosts -m ping`

After executed I have a Warning that is related to the Python interpreter . So even though Python 3 is already installed on the Servers, Ansible is going to use its own interpreter discovery tool to try to find the Python installation on the host automatically . So this warning is a note to say that if Ansible find another interpreter path, For example for an older version of Python, then Ansible might use this path instead . And in the hosts files, I can acutally override that behavior and set the location of Python interpreter explicitly for the host 

```
[droplet]
157.230.29.95 
157.230.23.199 

[droplet:vars]
ansible_ssh_private_key_file=~/.ssh/id_rsa
ansible_user=root

[ec2]
ip-10-0-1-121.us-west-1.compute.internal
ip-10-0-2-70.us-west-1.compute.internal ansible_python_interpreter=/usr/bin/python3.9

[ec2:vars]
ansible_ssh_private_key_file=~/.ssh/tim.pem
ansible_user=ec2-user
```

## Managing Host Key Checking

I need to take care of the host key check prompt . If I want to automate configuration on the servers using Ansible, then we want this thing to be able to run automatically without any interaction 

How do I configure that ? 

#### Authorized Keys and Known Hosts

There is 2 ways to handle SSH key checks in General, wheather it is with Ansible or with other tools . 

First one is if I have servers that are long running, meaning that we create these Droplets once and we know that we are going to have this droplet servers for a long time and our application are going to run there for a long time . In this case I can acutally configure or do this SSH key check once for each Server from the machine where Ansible is running

When I ssh from my machine to target machine, both servers need to identify and authenticate each other . So the server must allow our machine to connect to it as well as our machine must recognize this server as a valid host . For this to happen, the Server which is connecting or SSH-ing into the target Server, must add target Server infomation a file called `known hosts` that file located in `ls ~/.ssh/known_host` . 

 - To add target Server information to `known_host` I use : `ssh-keyscan -H <host-ip-address> >> ~/.ssh/known_hosts` . So this will add an entry about the remote server in the known hosts file

Another steps is for the remote Server to authenticate the machine where I am trying to SSH from , for that this target Server has to have the Public SSH key `cat ~/.ssh/id_rsa.pub`, this key must already have our public or the connecting machines, public ssh key inside .

 -  Since I create the Droplet using the SSH key, Digital Ocean automatically added our public SSH key to the Server

 - To see that I can ssh to the Server `ssh root@<host-ip-address>` . I will not get a prompt bcs I added the Server to `known_host` file . Also on this that I just connected to, there is already my public SSH key in `ls .ssh/authorized_keys`

Another example : This time I will create a Droplet with Password . Not SSH key . 

 - Add target Server information to `known_host` I use : `ssh-keyscan -H <host-ip-address> >> ~/.ssh/known_hosts`

 - When I `ssh root@<ip-address>`, it is asking for a `root` password .

 - Inside droplet this `cat .ssh/authorized_key` is empty . However I can copy a Public SSH key into a target server `cat .ssh/authorized_key`

 - I can do that In my terminal `ssh-copy-id root@<ip-address>`. This will take the public key in the default location which is `cat .ssh/id_rsa.pub` and copy that to the droplet root users in this location `cat .ssh/authorized_key`

 - Now I can ssh to the target machine without proivde password

With those example above I would do on Servers that are long-term servers, that I spin up once and then use it for a long period of time 

#### Disalbe Host Key Checking

This way is less secure bcs the key check its security reasons, however if I have an infrastructure where I am creating and destroying servers all the time, so my infrastrucuture is not made up of static servers that I spin up once and use long term .

For example : For every test run maybe I create a new server, run the test there , I destroy the server, or maybe I have dynamic infrastructure where I scale up as my application needs more resources and then scale down when Application has less load 

For this use case it makes sense to disable SSH key check and it's not going to be such a security issue bcs the Servers are short-lived anyways

I will create new droplet with ssh key 

To disable SSH key check by using Ansible configuration file. 

In the earlier version of Ansible when I installed Ansible I actually got a default Ansible configuration file inside `ls /etc/ansible` . However on the recent versions this directory is not getting created and I don't have the configuration file we can create one ourselves. 

However there is default locations here Ansible will look for this configuration file, so I can create that configuration file ourselves in one of the default locations and then Ansible will pick it up . 

 - Location is `/etc/ansible/ansible.cfg`

 - Another one is `~/.ansible.cfg`

 - To create Ansible config file in home dir `vim ~/.ansible.cfg` . Ansible will look for it in this locations and pick up the configuration

```
[defaults]
host_key_checking = False 
```

So now I can ssh to a new server for the first time with Ansible it shouldn't give me any prompt bcs I have disabled . So now Ansible acutally will look in the `~/.ansible.cfg` will see this one and apply the configuration from there 

I can also set this configuration for specific Ansible project, so instead of creating `~/.ansible.cfg` in the user's home directory , I can create this configuration file in the Ansible project directory `vim ansible.cfg` . 

<img width="600" alt="Screenshot 2025-05-03 at 12 54 21" src="https://github.com/user-attachments/assets/2eb8931f-b7a5-4312-8a18-8bffbb82735f" />

## Introduction Ansible Playbook

Obviously in Ansible I want to configure the Server like install Aplication, Update, create files etc .... for that I will write `Ansible Playbook`

Since Ansible is IaC tools, it will treated like code 

I will create Project Folder and open editor and start creating Ansible configuration files . Finally we can create a git repository for this Ansible project and check all the code or all the configuration files into that repository , same as Terraform Project 

First I will create a `hosts` file . In every Ansible project I will always has a `hosts` file  

#### Write a simple playbook

Create `touch my-playbook.yaml`

Syntax of playbook : 

<img width="400" alt="Screenshot 2025-05-03 at 13 18 11" src="https://github.com/user-attachments/assets/257c12f1-4ec1-4971-9610-15201f2e0406" />

`---` this is a YAML syntax for separating . Basically blocks in the YAML file itself 

A `Playbook` can have multipke `Plays`

`Plays` is a group of task that I want to execute on several Servers

 - For example I can have a `plays` for DB server . And I can have another `plays` for Web server . And I would have both a this `plays` in one `Playbook`

To write a `play`:

  - `-name: Configure Nginx Web server` . Every `plays` has a `-name:` and it is optional where me and my team member who then work with my script or my Ansible project basically know what this `play` does

  - Every `play` have to have `hosts` define which Server from my `hosts` inventory I want to run this play on . I can provide individule Server by using IP Address or I can provide a Group Name with multiple Servers in there `hosts: webserver`. I can only define the host that from `hosts` file

  - `tasks` are a list of command that I want to execute on all the Server the I have define in the `hosts: ...` . `Tasks` is a list and YAML syntax for lists is start with `-`.

    - Every `tasks` has it own `- name: Install Nginx` . Can be any name
   
    - After `-name: ...` I want to specify what task or what command I want to execute . And this will be a place for `modules` . `Module` is just one specific command that I am executing . Think about if I want install nginx server myself if I ssh to server , I would do `apt install nginx` . So that I will use `apt` module to execute `apt` command bcs I am on Ubuntu . So for `apt` modules I have `name: nginx` which is the package I want to install . And I have a `state: latest` as in install the latest nginx package
   
    - As a Second `tasks` I want to `- name: install nginx`. And for this I will use another `module` called `service`, and `service` module has name `name: nginx` and state `state: started`
   
```
---
- name: Configure nginx server
  hosts: webserver
  tasks:  
  - name: install nginx server
    apt:
      name: nginx
      state: latest
  - name: start nginx server
    service:
      name: nginx
      state: started
```

**Wrap up**: Now I have a first `play` which have 2 tasks . First `task` install latest nginx using `apt` module , Second task start the nginx using `service` module . And these 2 `tasks` are executed on the hosts that belong to `webserver` group in the `hosts` file 

#### Executing Playbook

Before execute I can create Ansible config file in this project `touch ansible.cfg`

I can have own Ansible config file for every project so that all the configuration is specific to that one project

To execute ansible playbook : `ansible-playbook -i <hosts-file> <play-book.yaml file>`

If successed I will have ouput in the terminal : 

 - First I have the name `play` that got execute

 - Second I have `Task` which is call `Gathering Fact`

   - `Gathering Fact` is a module that automatically added and executed at the beginning of every `play` and it collects a lot of information about these remote servers that I am connecting to . Including wheather it is accessible, what is the Status, which OS and distribution it is using, and a lot others infomation that I can actually use in my `playbook`
  
 - Then I will see a first `task` said `changed`. Install nginx server was a changed in the Server , they have package that doesn't exist before . Ansible will identify that and show me as a `changed`
 
 - Then I have `Play Cap` which is a summary for that

Now I can connect to a server `ssh root@<ip-address>` and see if nginx is running `ps aux | grep nginx` 

#### Install specific Version 

Now I want to install specific version of a package on the Server, not the latest one 

To provide specific Version of a package that I want to install I can set `name: nginx=<version>` . `<version>` can be regular expression or specific . Then I would have `state: present`

To look up package version for different packages on ubuntu . Go to (https://launchpad.net/ubuntu)


```
---
- name: Configure nginx server
  hosts: webserver
  tasks:
  - name: install nginx server
    apt:
      name: nginx=1.24.0-1ubuntu1 ###
      state: present ###
  - name: start nginx server
    service:
      name: nginx
      state: started
```

Or with Regular expression 

```
---
- name: Configure nginx server
  hosts: webserver
  tasks:
  - name: install nginx server
    apt:
      name: nginx=1.24* ###
      state: present ###
  - name: start nginx server
    service:
      name: nginx
      state: started
```

#### Ansible itempotency 

Most Ansible modules check wheather the desired state has already been achieved . They exit without performing any action 

Ansible compared the Actual state and the Desired State  

Iempotency mean If I execute the same configuration multiples I will get the same results 


To stop Nignx Server 

```
---
- name: Configure nginx server
  hosts: webserver
  tasks:
  - name: uninstall nginx server
    apt:
      name: nginx
      state: absent ##
  - name: start nginx server
    service:
      name: nginx
      state: stopped ##
```

## Modules-and-Collections-in-Ansible

How to know these Attribute names like `apt`, `service` etc... and what the value of those attributes names should be ? 

Answer is by using Ansible modules which are part of Ansible Collections 

This is a huge lists of moudles : (https://docs.ansible.com/ansible/latest/collections/index_module.html) . 1 Specific entry is 1 module 
 
!!! NOTE : Each of those `modules` is part of a collection 

#### What is a Collection

Collection is actually a packaging and distribution format for all types of Ansible content and these type of Ansible content can be playbooks themselves, this could be modules, this could be plugins, documents .... Instead of having modules separately, and plugins separately and playbooks separately I can just pull all of this together and package them in a single bundle and that bundle is a `Collection` . So now it is easier to distribute and share with all these Ansible content inside 

A lists of collections : (https://docs.ansible.com/ansible/latest/collections/index.html)

The modules `apt` and `service` is a part of `ansible.builtin` Collections

`Ansible Plugins` is a pieces of Ansible code that add to functionality of Ansible itself or the modules that I use . So basically if I have a module for AWS EC2 that maybe creates or terminates EC2 instances on AWS, with a plugin I can enhance that module's functionality . So maybe I can add a feature that filter which EC2 instances I want to terminate .

The name of the modules is actually `ansible.builtin.apt`

Collections of `community.<something>` is actually collections that are maintained and managed by Ansible . These collections are available when I install Ansible 

For example to get a list of ansible collections : `ansible-galaxy collection list`

#### Ansile Galaxy 
 
Collections acutally group Ansible content, modules and plugins . Modules and Plug are code so they have to be listed somewhere, they have to be located somewhere so that I can download that code locally on my machine whenever I need that collection . 

So Where do Collections live ?  

One of the main hubs of collections is acutally Ansible Galaxy 

Link to Ansible Galaxy (https://galaxy.ansible.com/ui/)

Ansible Galaxy is where the code of Collections lives so whenever I need that I can download it from here 

It the same to registry of artifact for different technologies . I have Repositories for npm packages, I have repository for terraform modules, etc ... 

Ansible galaxy is just a command line tools that can fetch the collection from the Ansible galaxy and work with collection 

Whenever we need to install a Collection I can use `ansible-galaxy collection install <collections>`

What happens if specifics collection gets updated, new features come in , I don't have to acutally update my whole Ansible installation to upgrade my collections, I can just pick and choose which individual collection I want to update and intall newer version : `ansible-galaxy collection install <collections> --upgrade`

I can create my own `collections` that will be useful if I have a complex, large Ansible project that basically contains a lot of different types of ansible content. So I have couple of plugin , I have modules that I created, I have a bunch of playbooks, and if I want to share that and distribute that together in a bundle, then I would want to create a collections for that Ansible Project 

Ansible Collections have a predefined structure which is a standard structure that everyone has to stick with, which is a greate thing bcs when I work with collection and I know the standard structure, basically I will know how to navigate in the collections and how to find the right content I need and also adjust and change the right content 

<img width="600" alt="Screenshot 2025-05-06 at 19 16 14" src="https://github.com/user-attachments/assets/33610063-5244-48fe-a560-25d061b638e3" />

#### Ansible Namespaces

Might wonder what is the `community.docker.docker_image` mean, or whatever the name of the modules is 

The structure would be <namespace>.<collection>.<module name> 

Every modules in a collections and every collections are in a Namespace 

<img width="614" alt="Screenshot 2025-05-06 at 19 19 52" src="https://github.com/user-attachments/assets/53cd44c0-b97c-46d1-b44a-dddeb777918a" />

Fully Qualified Collection Name is `community.docker.docker_image` to specify the exact source of a module/plugin/... 

Ansible build in a a default namespace and collection name that Ansible assumes that the module is in . So for the built-in collection content like modules, I don't have to use the fully qualified name 

!!! NOTE: Should using the fully qualified name 

## Deploy Nodejs application 

I will automate installing a Nodejs application on a Digital Ocean Server . So I will Create A Droplet, then I will write an Ansible playbook that will install Node on the Server, Then I will copy a Nodejs tar file to a Server, unpack it there and start the Application using Node command on the Server . Finally I will check that the Application is successfully running 

#### Create Digital Droplet 

I will go to Digital Ocean and create a small Droplet 

#### Write Playbook 

I will add the new IP address of the Droplet in the `hosts` file . 

```
165.22.22.94 ansbile_ssh_private_key_file=~/.ssh/id_rsa ansbile_user=root
```

I will create a new Playbook file : `touch deploy-node.yaml`

#### Install node and npm 

They way I will run a Nodejs application on a server is first install dependencies that applications needed via NPM and I will start the Applications using Node command . That mean I need the NPM and Node install on the Server 

First `task` will be to do `apt update`, which basically updates the `apt` package manager repository and cache . 

 - I will do this using `apt` module . This is a part of `ansible.builtin` module which mean I don't need to use fullname

 - `apt` modules docs (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html#ansible-collections-ansible-builtin-apt-module)

 - `cache_valid_time=3600` : Mean that if I execute this immediately after, then it won't update the cache bcs I have some valid validity time set here 

Second `task` I will install nodejs in the server 
also using `apt` moudle

 - To install a list of packages I will use `pkg`

```
---
- name: Install node and npm
  hosts: 165.22.22.94
  tasks:
    - name: Update apt repo and cache
      apt:
        update_cache: yes
        force_apt_get=yes
        cache_valid_time=3600 #
    - name: Install nodejs and npm
      apt:
        pkg:
          - nodejs
          - npm
```

Playbooks can have multiple `Plays` . The block above preresent for 1 `Play` .

 - Every `Play` has `hosts` `name` of the play, and lists `tasks` by using different `modules`

#### Copy and unpack tar file

I will create another `Play` to deploy Nodejs Application 

First I want to copy a tar file of Nodejs application to the remote server then I want to unpack that in the remote server . And inside the package I will see what get acutally tarred or what inside 

To get a tar file : `npm pack` . This tar file when unpack will ge us package folder 

First task would be copy Nodejs folder to a Server 

 - To copy a file from Local Machine to Remote Server I use `copy` module

   - `copy` module will has a `src` (A file from local machine) and a `dest` (Remote Server)
  
   - Bcs I am using a root user on the remote Server . Every will happen on the root directory .
  
Second task I want to unpack tar file

 - I will use `unarchive` module

   - `unarchive` module also have a `src` and `dest`
  
   - The tar file will unpack as a `package` folder so I will be the name of the unpacked folder
  
   - `remote_src: yes` this mean that the `src` attribute that we have defined has to be on the remote server, If I do not specify this it will look for this tar file on our local machine 

```
- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Copy Nodejs folder to a Server
      copy:
        src: <absolute path of the file>
        dest: /root/<name of the file>
    - name: Unpack the Nodejs file
      unarchive:
        src: /root/<name of the file>
        dest: /root/
        remote_src: yes
``` 

To execute that `Playbook`: `ansible-playbook -i hosts deploy-node.yaml`

In the second Task I could do those 2 steps `copy` modules, `unarchive` modules in 1 step.  

 - I will use `unarchive` module to copy a file from my local machine to remote server and unpack it at the same time

```
- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: <absolute path of the file from local machine>
        dest: /root/
        remote_src: yes
```

!!! NOTE : Installing package and copying files or unarchiving archive file on remote server is going to be some of the common tasks with Ansible 

#### Start Node App

First I need to install all the Dependencies that are defined in `package.json` . Bcs I don't have `node_modules` folder inside the tar file, I just have `package.json` . So I need to install all the dependencies that will create `node_modules` folder with all the dependencies first

Then I can run node command which we have available bcs we installed it using Ansible . 

And then execute the `server.js` inside `/app` folder

Inside the same `play` in Deploy Nodejs App I will have another the step call Install Dependencies . In local I would do `npm install` : (https://docs.ansible.com/ansible/latest/collections/community/general/npm_module.html)

 - Very important to understand where each task actually get executed on our local machine or on the control Node , which is where we are executing ansible from or on the managed node which is the remote Server which ansible manages . Most of the command get executed on the remote server that we specify at `hosts: <ip_address>`

 - `npm` module come from the `community.general` . This one is also part of ansible installation. So we don't have to install it and also i don't have to provide full name 

Second step is Start the Application 

 - I will use a `command` module which let me execute any command on a server just like I would execute them myselfs manually

 - `command` module pretty flexible I can do alot of stuff with it and I have a lot of example in the Docs (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#ansible-collections-ansible-builtin-command-module)

```
- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: <absolute path of the file from local machine>
        dest: /root/
        remote_src: yes
    - name: Install Dependencies
      npm:
        path: /root/package
    - name: Start the Application
      command:
        chdir: /root/package/app
        cmd: node server
```

I will execute it and see what happen : `ansible-playbook -i hosts deploy-node.yaml`

 - Everything installed and run correctly on the remote-server

 - However my `Play` is hanging in the terminal . I started the Application but the `Playbook` not completing . The reason is `node server` command is acutally blocking the terminal this will never exit and to fix that I need to execute this command in the background . To make it run in the background I will need to add an attribute here called `async` and `poll` (https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_async.html)

 - These are acutually general attributes . So these are not part of the `command` module bcs we are not setting them as attributes here inside . We are setting it on the same level

 - I can run any task for any module basically asynchronously so that it doesn't block my current task execution


```
- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: <absolute path of the file from local machine>
        dest: /root/
        remote_src: yes
    - name: Install Dependencies
      npm:
        path: /root/package
    - name: Start the Application
      command:
        chdir: /root/package/app
        cmd: node server
      async: 1000
      poll: 0
```

 - So this will execute all the `Playbook` commands and it will run asynchoronously . Even after the Ansbile playbook execution is completed, the node.js application will still run on the server

#### Ensure App is running 

Let's say I want to know without ssh to the remote server and check the Application is running or make sure the application really is running . 

I want the output immediately in the Terminal 

I will use a `shell` module (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#ansible-collections-ansible-builtin-shell-module)

 - `shell` module pretty much the same as `command` module in terms of executing commands there . But the different is that `shell` module execute the command in the shell . So the Pipe Operator as well as redirect operators and Boolean operators and also environment vairalbes that I have available in shell. If I need to use them I should use a `shell` module

 - However `command` is more secure in term of having these commands isolated and not running them directly through `shell`. Whereas `shell` module is more open to shell injection . That mean whenever I have commands that don't need anything from `shell` itself like Operator or ENV etc ... I should use `command` module 

```
- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: <absolute path of the file from local machine>
        dest: /root/
        remote_src: yes
    - name: Install Dependencies
      npm:
        path: /root/package
    - name: Start the Application
      command:
        chdir: /root/package/app
        cmd: node server
      async: 1000
      poll: 0
    - name: Ensure app is running
      shell: ps aux | grep node
```

To get a result or print out the result of this `shell: ps aux | grep node` command execution and for that there is a concept in Ansible . This is not specific to any `modules` I can apply this to any module and that is `register` 

So I have register attribute available for any tasks, for any module and that is basically creates a variable and assigns that variable result of the module execution or the task execution . So result of this `ps aux | grep node` command execution will be assign to a variable call `app_status` . 

Next step I want to print that Variable by referencing a variable . And for that there is a module called `debug` . This modules is where I want to print a result of a tasks execution. This is a built-in `module` : (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/debug_module.html#ansible-collections-ansible-builtin-debug-module)

 The way we can reference variables in the Yaml file is using `{{<variable name>}}`
 
<img width="437" alt="Screenshot 2025-05-08 at 12 39 32" src="https://github.com/user-attachments/assets/4a632ace-f5e0-4d78-8bbd-557f7b4aa796" />


```
- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: <absolute path of the file from local machine>
        dest: /root/
        remote_src: yes
    - name: Install Dependencies
      npm:
        path: /root/package
    - name: Start the Application
      command:
        chdir: /root/package/app
        cmd: node server
      async: 1000
      poll: 0
    - name: Ensure app is running
      shell: ps aux | grep node
      register: app_status
    - debug: msg={{app_status}}
```

After Execute I will see the output like this : 

<img width="600" alt="Screenshot 2025-05-08 at 12 46 12" src="https://github.com/user-attachments/assets/27f02061-fe53-4e05-ad6c-a8511470293e" />

Every replay of the `Playbook`, the start application task gets executed . So I can see here that even though we did not make any change to the start application task, it has still been run again . The reason for that is `command` and `shell` modules are not stateful (not idempotent)

As mentioned before Ansible executes its tasks, it checks wheather something nees to be done or not, so bascially whether a change is needed or if everything is up to date so no need to do anything . 

Important to Note : Whenever I execute commands or tasks with `command` and `shell`, I don't have state management so Ansible doesn't know whether not to execute the command bcs the application is already running . Solution for this is `conditionals` in Ansible 

Also to mention as a comparision to Python . In Python whenever I executed commands, I needed some kind of checking mechanism for every execution that basically check the status first before executing the command . For example If I am creating a new directory on remote server on a second execution I am going to get an error saying the directory with this name already exist . 

 - That is a downside of using Python compare to Ansible or Terraform bcs even though I can install stuff with Python I can start server and services using Python, I don't have to do the state management at all

 - Ansible and Terraform handle that State check for me . It take care of the State management and checks the current and the desired state and decide itself in an intelligent way wheather something needs to be done or not 


#### Ansible shell vs command Module

In Ansible, both `shell` and `command` modules are used to run commands on remote systems. While they may seem similar, there are key differences in how they interpret and execute those commands.

---

#### Difference Between `shell` and `command`

| Feature                     | `shell` module                                  | `command` module                                  |
|----------------------------|--------------------------------------------------|---------------------------------------------------|
| Shell features supported   | ✅ Yes (pipes, redirects, variables, etc.)       | ❌ No (executes without a shell)                  |
| Environment variable usage | ✅ Supports `$VAR` and other shell variables      | ❌ Variables must be passed explicitly            |
| Safer to use?              | ❌ Less safe (risk of shell injection)           | ✅ Safer (no shell parsing)                       |
| Use cases                  | Complex shell commands                           | Simple, direct commands                          |

---

#### What is a Shell?

A **shell** is a command-line interface that allows users to interact with the operating system. It interprets and executes the commands entered by users or scripts.

Common Unix shells:
- `bash` (Bourne Again Shell)
- `sh` (Bourne Shell)
- `zsh`, `fish`, etc.

Shell features:
- **Pipes** (`|`)
- **Redirects** (`>`, `>>`, `2>&1`)
- **Boolean operators** (`&&`, `||`)
- **Environment variables** (`$HOME`, `$PATH`)

---

#### ✅ When to Use `shell`

Use the `shell` module when your command includes:

- Pipes (`|`)
- Redirects (`>`, `>>`)
- Chained commands (`&&`, `||`)
- Shell-specific syntax or environment variables

### Example:

```yaml
- name: Use shell to grep user from /etc/passwd
  ansible.builtin.shell: "cat /etc/passwd | grep root"
```

#### Executing tasks with a different user

Example above I am executing everythin with Root USer 

With security Best Practices I shouldn't be executing and starting application with a root user. 

Should creating own user for each applications or maybe users for each team member and I should then start application with a non root user 

To do that I will have to create a new user on our remote server bcs right now root is the only one that gets created by default and the one we are using so we will have to create a new user and then we will have to reconfigure this or make some small adjustments to run the application, to run our Nodejs using that new user 

I will leave a Install node and npm as root user 

After that I will create new linux user for node app by using `user` module (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html#ansible-collections-ansible-builtin-user-module)

Afther that I want to executing all the command use `tim` user. 

 - To let Ansible know that I want to execute with `tim` user by using `become_user` attribute .

 - `become_user` = set to user with desired privileges . Default is root

 - For `become_user` to need to enable `become: True`

 - So those 2 attributes tell Ansible that we want to execute all this with another user called `tim`


```
---
- name: Install node and npm 
  hosts: 165.22.22.94
  become: True ## 
  become_user: tim ##
  tasks:
    - name: Update apt repo and cache
      apt:
        update_cache: yes
        force_apt_get=yes
        cache_valid_time=3600 
    - name: Install nodejs and npm
      apt:
        pkg:
          - nodejs
          - npm

- name : Create new Linux user for node app
  hosts: 165.22.22.94
  tasks:
    - name: Create Linux User
      user:
        name: tim
        comment: Tim Admin
        group: admin

- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: <absolute path of the file from local machine>
        dest: /home/tim ###
        remote_src: yes
    - name: Install Dependencies
      npm:
        path: /home/tim/package
    - name: Start the Application
      command:
        chdir: /home/tim/package/app
        cmd: node server
      async: 1000
      poll: 0
    - name: Ensure app is running
      shell: ps aux | grep node
      register: app_status
    - debug: msg={{app_status}}
```

**Recap** : I ssh to server as a Root User. I am installing 2 packages using root user . Then I am creating new user using root user . And then I will use new Linux User to deploy my Nodejs Application

## Ansible Variable

#### Registered Variable 

In the previouse section above I already used one form of variable that is `register` . These are variables that I can register as results of a module or task execution . And I have them available for every single task, whichever module they are using 

Syntax for reference the variable is `{{}}` and the variable name . 

If variable is a Dictionary I can access variable like this `{{<variable-name>.<key>}}`

For example in : 

 - At the same level of the module  `name` . I will do `register: <name-of-variable>`. To access that  I use `debug: msg={{}}` module

 - The Information that variable print out is related to that specific module execution 

```
- name : Create new Linux user for node app
  hosts: 165.22.22.94
  tasks:
    - name: Create Linux User
      user:
        name: tim
        comment: Tim Admin
        group: admin
      register: user_creation_result
     - debug: msg={{user_creation_result}}
```

Wrap up : If I want to get some values as a result of task execution, I can get them using this register results 

I can see some of the Infomation in the Docs itself . Basically What information each module execution returns 

#### Parameterzie Playbook  

To make parameterzie by substituting the value with a variable : `{{}}`

For example of instead of `src: <absolute path of the file from local machine>` I can parameterize it like this : `src: {{node-file-location}}` . 

However in somecases we need to enclose these `{{}}` with `"{{}}"`  And that is whenever these `{{}}` are after a column . 

 - The reason for that is by default Ansible will actaully think that this is YAMLs dictionary syntax, and not a variable syntax

 - With `"{{node-file-location}}"` Ansible will know this is actually a variable

 - We only have this problem when we are using this `{{}}` immediately after the colume . For example : `src: /trinhnguyen/ansible/nodejs/nodejs-app.{{version}}.tgz` I don't need `""` for variable 

<img width="504" alt="Screenshot 2025-05-12 at 11 04 38" src="https://github.com/user-attachments/assets/5bda86a2-433f-4414-854c-782bccecd1ce" />

Now I defined a variables . How do I set their value 

 - If we execute now without setting the value . We will get error from Ansible saying location is `undefined`

```
---
- name: Install node and npm 
  hosts: 165.22.22.94
  become: True ## 
  become_user: tim ##
  tasks:
    - name: Update apt repo and cache
      apt:
        update_cache: yes
        force_apt_get=yes
        cache_valid_time=3600 
    - name: Install nodejs and npm
      apt:
        pkg:
          - nodejs
          - npm

- name : Create new Linux user for node app
  hosts: 165.22.22.94
  tasks:
    - name: Create Linux User
      user:
        name: tim
        comment: Tim Admin
        group: admin

- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: "{{node-file-location}}" ## 
        dest: "{{destination}}" ###
        remote_src: yes
    - name: Install Dependencies
      npm:
        path: "{{destination}}/package"
    - name: Start the Application
      command:
        chdir: "{{destination}}/package/app"
        cmd: node server
      async: 1000
      poll: 0
    - name: Ensure app is running
      shell: ps aux | grep node
      register: app_status
    - debug: msg={{app_status}}
```

#### Variables defined in a Playbook 

I can set variable at the level with `hosts` 

 - `vars`: This will be a list of variable  

```
vars: ##
  - location: <absolute-path>
  - version: 1.0.0
  - destination: /home/trinhnguyen
```

 ```
---
- name: Install node and npm 
  hosts: 165.22.22.94
  become: True 
  become_user: tim
  vars: ##
    - location: <absolute-path>
    - version: 1.0.0
    - destination: /home/trinhnguyen
  tasks:
    - name: Update apt repo and cache
      apt:
        update_cache: yes
        force_apt_get=yes
        cache_valid_time=3600 
    - name: Install nodejs and npm
      apt:
        pkg:
          - nodejs
          - npm

- name : Create new Linux user for node app
  hosts: 165.22.22.94
  tasks:
    - name: Create Linux User
      user:
        name: tim
        comment: Tim Admin
        group: admin

- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: "{{node-file-location}}" 
        dest: "{{destination}}" 
        remote_src: yes
    - name: Install Dependencies
      npm:
        path: "{{destination}}/package"
    - name: Start the Application
      command:
        chdir: "{{destination}}/package/app"
        cmd: node server
      async: 1000
      poll: 0
    - name: Ensure app is running
      shell: ps aux | grep node
      register: app_status
    - debug: msg={{app_status}}
```

#### Passing Variables on the Command Line 

I want the variable value to be configuration from outside the playbook, and that is will be another way of defining `variables` from outside the playbooks .

The way to set variable from out side is during runtime on the command execution . I will pass them as a variable in the the terminal 

To pass this as `variables` I will use a flag call `--extra-vars` shortcut is `-e` . and the variable will be in `""` as key value pair : `-e "version=1.0.0 location=/User/trinhgnuyen/ansible/nodejs-app"`

 - The  whole syntax would look like `ansible-playbook -i hosts deploy-node.yaml -e "version=1.0.0 location=/User/trinhgnuyen/ansible/nodejs-app"`

Another the benefit of passing the variables from outside and not defining them in YAML files is .

 - If I want to make a `user` parameterize . I want to defined them once and be abble to use it in whatever `playbook` I want . I don't want to define in each `play` seperately 


 
```
---
- name: Install node and npm 
  hosts: 165.22.22.94
  become: True 
  become_user: "{{name}}"
  vars: ##
    - location: <absolute-path>
    - version: 1.0.0
    - destination: /home/trinhnguyen
  tasks:
    - name: Update apt repo and cache
      apt:
        update_cache: yes
        force_apt_get=yes
        cache_valid_time=3600 
    - name: Install nodejs and npm
      apt:
        pkg:
          - nodejs
          - npm

- name : Create new Linux user for node app
  hosts: 165.22.22.94
  tasks:
    - name: Create Linux User
      user:
        name: "{{name}}"
        comment: "{{name}} Admin"
        group: admin

- name: Deploy Nodejs App
  hosts: 165.22.22.94
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: "{{node-file-location}}" 
        dest: "{{destination}}" 
        remote_src: yes
    - name: Install Dependencies
      npm:
        path: "{{destination}}/package"
    - name: Start the Application
      command:
        chdir: "{{destination}}/package/app"
        cmd: node server
      async: 1000
      poll: 0
    - name: Ensure app is running
      shell: ps aux | grep node
      register: app_status
    - debug: msg={{app_status}}
```


!!! NOTE : I will a warning `Found variable using reserved name: name` . 

 - I have the warning bcs I called my variable a `reserved word` . And I want to avoide doing that bcs it might give me some unexpected error  

<img width="500" alt="Screenshot 2025-05-12 at 11 40 06" src="https://github.com/user-attachments/assets/23124997-5066-4c40-8140-cd40be58cee7" />

#### External Variables File 

I want to paramerize everything in my configuration file by creating a Separate variable file and then tell Ansible to use that Variable file to get the values from 

I will create variable file `touch project-vars` . I can also do `touch project-vars.yaml` 

 - Variable file in Ansilbe actually has a YAML syntax, so instead of `=` we have to use the YAML syntax with `:` for key values pair .

```
version: 1.0.0
node-file-location: /Users/trinhnguyen/ansible/nodejs-app
linux_name: Tim
destination: /Users/trinh/ansible/nodejs-app{{version}}.tgz
```

Now I want to use that variable files in the project. In the same level with `hosts` I can specify : 

```
---
- name: Install node and npm 
  hosts: 165.22.22.94
  become: True 
  become_user: "{{linux_name}}"
  vars_files:
    project-vars
  tasks:
    - name: Update apt repo and cache
      apt:
        update_cache: yes
        force_apt_get=yes
        cache_valid_time=3600 
    - name: Install nodejs and npm
      apt:
        pkg:
          - nodejs
          - npm

- name : Create new Linux user for node app
  hosts: 165.22.22.94
  vars_files:
    project-vars
  tasks:
    - name: Create Linux User
      user:
        name: "{{linux_name}}"
        comment: "{{linux_name}} Admin"
        group: admin

- name: Deploy Nodejs App
  hosts: 165.22.22.94
  vars_files:
    project-vars
  tasks:
    - name: Unpack the Nodejs file
      unarchive:
        src: "{{node-file-location}}" 
        dest: "{{destination}}" 
        remote_src: yes
    - name: Install Dependencies
      npm:
        path: "{{destination}}/package"
    - name: Start the Application
      command:
        chdir: "{{destination}}/package/app"
        cmd: node server
      async: 1000
      poll: 0
    - name: Ensure app is running
      shell: ps aux | grep node
      register: app_status
    - debug: msg={{app_status}}
```

## Deploy Nexus

I will automate install and starting Nexus on a remote Server using Ansible 

To manually do it : 

 - I create a Droplet Server

 - SSH into a Server and executing commands manually to download the Nexus binary then unpack it
 
 - Configure the Nexus Application to run using Nexus user and start Nexus Application 

All of those thing above I can now automate using Ansible 

#### Preparation 

I will create a Droplet and get its Public IP address put it in a `hosts` file : `134.122.333.444 ansible_ssh_private_key=~/.ssh/id_rsa ansible_user=root`

I will create a new `Playbook` : `touch deploy-nexus.yaml`

I also have a `nexus.sh` installation for the reference : 

```
apt-get update
apt install openjdk-8-jre-headless
apt install net-tools

cd /opt
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
tar -zxvf latest-unix.tar.gz

adduser nexus
chown -R nexus:nexus nexus-3.65.0-02
chown -R nexus:nexus sonatype-work

vim nexus-3.65.0-02/bin/nexus.rc
run_as_user="nexus"

su - nexus
/opt/nexus-3.65.0-02/bin/nexus start

ps aux | grep nexus
netstat -lnpt
```

#### First Play

First I have to install Java version 8 bcs Nexus is using JVM or Java runtime 

Then I have to install `net-tools` which basically is just for debugging at the end to see that Nexus is running and listening on the port or Nexus port  

```
---
- name: Install java and net-tools
  hosts: 134.122.333.444
  tasks:
    - name: Update apt repo and cache
      apt: update_cache=yes force_apt_get=yes cache_valid_time=3600
    - name: Install Java version 8
      apt: name=openjdk-8-jre-headless
    - name: Install net-tools
      apt: name=net=tools
```

 - I could list all the packages that I want to install using this `pgk` attribute, or I can just list them individually

 - If I list them seprately I can see then basically I will see an output as a task for each installation 

#### Second Play 

I will change the dicrectory to `cd /opt` and inside `/opt` I will download the latest tar file of Nexus `wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz` and then I will unpack the tar file `tar -zxvf latest-unix.tar.gz` 

To download from internet from URL Ansible has module `get_url` (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/get_url_module.html#ansible-collections-ansible-builtin-get-url-module)

<img width="460" alt="Screenshot 2025-05-12 at 12 48 52" src="https://github.com/user-attachments/assets/011aca38-b228-4dca-a5cf-9802a3688f25" />

To untar a package I will use module `unarchive` : (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/unarchive_module.html#ansible-collections-ansible-builtin-unarchive-module)

 - If I want the `src` on the remote machine I have to specify `remote_src: True`

After untar Nexus Package I have :

 - `nexus-3.65.0-02` : for Nexus binary

 - `sonatype-work` : For storage, all the data of Nexus 
   
```
- name: Download and unpack Nexus installer
  hosts: 134.122.333.444
  tasks:
  - name: Download Nexus
    get_url:
      url: https://download.sonatype.com/nexus/3/latest-unix.tar.gz
      dest: /opt/
  - name: Untar Nexus installer
    unarchive:
      src: /opt/<name-of-tarfile>
      dest: /opt/
      remote_src: True
```

However the `src` of the `unarchive` is specific version of Nexus . So when we download this tar with `latest` it will give me the latest version of Nexus . And if we execute this a coupe of weeks after or a month after, we will get a different version and to know  aversion I had to actually ssh into the server and acutally see waht version got downloaded 

But I want everythin to automate it , that mean I have to now configure my `play` to set `<name-of-tarfile>` automatically 

 - I will save the result of exeucute the `get_url` module : `register: download_result` . Then I will add `debug` module to print out that result

 - Basically with printing out the result of the `download_result` I want to get the information about the name of the file without me have to ssh into the server and get a name there

 - In the print out result I have a `dest` attribute that has a value that actually contains full path of that Nexus tar file . So whatever version we download it will be a value of the `dest` key 


Now I want to use the information from one `module` to another `module`

 - In a `unarchive` module in the `src` attribute I will change the value from  `src: /opt/<name-of-tarfile>` to `src: "{{download_result.dest}}"`

 - And I can remove the `debug: msg={{download_result}}` bcs I already printed it out 

**Wrap up** : I saved the resoule in `get_url` module by using `register: download_result` and using the result in `unarchive` module `src: "{{download_result.dest}}"` . Now I am able to unpack the file without hard coding its values 

```
- name: Download and unpack Nexus installer
  hosts: 134.122.333.444
  tasks:
  - name: Download Nexus
    get_url:
      url: https://download.sonatype.com/nexus/3/latest-unix.tar.gz
      dest: /opt/
    register: download_result #
  - debug: msg={{download_result}}
  - name: Untar Nexus installer
    unarchive:
      src: "{{download_result.dest}}"
      dest: /opt/
      remote_src: True
```

#### find Module 

I want to rename the folder in the remote Server 

To do that I will create another task using `shell` module and execute `mv` command

 - For rename it I would need a dynamic value of the remote server folder name that just got unpacked

 - For that there is a module in Ansible which lets me find a folder or a file using a regular expression 

To get a name of the remote server folder . I will create a task and using `find` module to find a folder name 

`find` module useful for configuring stuff, creating folders, or maybe doing something with folders and files when I don't exactly know the name bcs it's either dynamic bcs of the version or maybe just gets updated (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/find_module.html#ansible-collections-ansible-builtin-find-module)

<img width="500" alt="Screenshot 2025-05-13 at 16 53 45" src="https://github.com/user-attachments/assets/a93bc269-9e57-4df6-bc90-7e40093923b5" />

 - `paths` Atribute I can specify where to look for that file or directory

 - `pattern` This is a regular expression to match files or folders with some pattern . In this case I know it will start with `nexus-` alway but don't know what come after that

 - `file_type`: by default is a file

 - With those 3 attributes above configured it will find the Nexus folder inside the `/opt`

 - Then I can save the result by using `register: find_result` . To see the result I can use `debug: msg={{find_result}}`

 - Now I can use that `register: find_result` in the `shell` module to rename that folder
 
```
- name: Download and unpack Nexus installer
  hosts: 134.122.333.444
  tasks:
  - name: Download Nexus
    get_url:
      url: https://download.sonatype.com/nexus/3/latest-unix.tar.gz
      dest: /opt/
    register: download_result #
  - debug: msg={{download_result}}
  - name: Untar Nexus installer
    unarchive:
      src: "{{download_result.dest}}"
      dest: /opt/
      remote_src: True
  - name: Find Nexus folder
    find:
      paths: /opt
      pattern: "nexus-*"
      file_type: directory
    register: find_result
  - name: Rename Nexus folder
    shell: mv {{fine_result.files[0].path}} /opt/nexus
```

#### Conditionals in Ansible

If I execute `Playbook` again I will see an error in rename Nexus folder task and it said can not  move Nexus with version to `/opt/nexus` directory not empty . Basically first it detected in `Untar Nexus installer` that the package with `nexus-*` name not exist anymore bcs I already change the name above and then it will untar it agian and try to move that folder into a Nexus folder . But Nexus folder is not empty that is why it is failed 

What's happening is that Ansible knows for all the other tasks that they already got executed so it doesn't acutally take try to execute them again . But for the `shell` task it executes on every single rerun

 - For the `shell` and `command` modules Ansible doesn't the current State and Desired State, so it executes them all the time.

That mean I need to tell Ansilbe if this `shell: mv {{fine_result.files[0].path}} /opt/nexus` already execute . 

 - I want it to be skipped if it already a Nexus folder inside `opt` directory I can do that using `conditionals`

<img width="500" alt="Screenshot 2025-05-13 at 17 23 40" src="https://github.com/user-attachments/assets/abb0d23e-4d23-4a3e-a8fc-fde4cb9ad678" />

First I have a `stat` module can give me information whether a folder or file already exists (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/stat_module.html#ansible-collections-ansible-builtin-stat-module)

 - `path` Atribute I can specify where to look for that file or directory

 - This will give me information about or statistis about this folder

 - Then I can use `register` to save the result . Then I can use `debug` module to print out the result . Then I can see the `exsist` attribute in `stat` module

Now to use that information to make Ansilbe skip the `shell` command steps if the `exist` is True 

 - I can do that by using `when` conditional . I can use that on any task

 - I can say execute this task if `stat_result.stat.exists` not exist by using `when: not stat_result.stat.exists`

 - This is going to be necessary for `shell` and `command` modules bcs, when I am creating an existing folder or moving to an existing folder or doing something that will result in an error if I execute it multiple times then I want to be able to skip it 

```
- name: Download and unpack Nexus installer
  hosts: 134.122.333.444
  tasks:
  - name: Download Nexus
    get_url:
      url: https://download.sonatype.com/nexus/3/latest-unix.tar.gz
      dest: /opt/
    register: download_result #
  - debug: msg={{download_result}}
  - name: Untar Nexus installer
    unarchive:
      src: "{{download_result.dest}}"
      dest: /opt/
      remote_src: True
  - name: Find Nexus folder
    find:
      paths: /opt
      pattern: "nexus-*"
      file_type: directory
    register: find_result
  - name: Check Nexus folder stats
    stat:
      path: /opt/nexus
    register: stat_result
  - name: Rename Nexus folder
    shell: mv {{fine_result.files[0].path}} /opt/nexus
    when: not stat_result.stat.exists
```

#### Third Play 

Now In this play I want to configure this section where I create a Nexus user and I make Nexus user owner of Nexus and Sonatype work folder.

```
adduser nexus
chown -R nexus:nexus nexus-3.65.0-02
chown -R nexus:nexus sonatype-work
```

I will create another `play` .

 - First task is to create Nexus and a group . This command `adduser nexus` is create nexus user and the default nexus group for that user

 - In Ansilbe I have to do that seperately . First I will create Nexus Group the I have to assign that user to the group

 - To create group I can use `group` module (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/group_module.html#ansible-collections-ansible-builtin-group-module)

<img width="500" alt="Screenshot 2025-05-13 at 17 56 02" src="https://github.com/user-attachments/assets/b0209d25-1144-427d-a7f1-532b189c8ae5" />

 - Then I will create Nexus user by using `user` module (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html#ansible-collections-ansible-builtin-user-module)

<img width="500" alt="Screenshot 2025-05-13 at 17 58 05" src="https://github.com/user-attachments/assets/c27ec419-fc08-4c0a-9575-8fd4d499a665" />

 - Then I can make `nexus user` a owner of `nexus` folder and `sonatype-work` folder for that I will use `file` module (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html#ansible-collections-ansible-builtin-file-module)


```
- name: Create nexus user to own nexus folder
  hosts: 134.122.333.444
  tasks:
    - name: Ensure group nexus exists
      group:
        name: nexus
        state: present
    - name: Create nexus user
      user:
        name: nexus
        group: nexus
    - name: Make nexus user owner of nexus folder
      file:
        path: /opt/nexus
        state: directory
        owner: nexus
        group: nexus
        recurse: yes
    - name: Make nexus user owner of sonatype-work folder
      file:
        path: /opt/sonatype-work
        state: directory
        owner: nexus
        group: nexus
        recurse: yes
```

#### Fourth Play

The 4th Play will be the execution of this command 

```
vim nexus-3.65.0-02/bin/nexus.rc
run_as_user="nexus"
```

In `ls /opt/nexus/bin/nexus.rc` file that have a value `run_as_user=""` commented out and I wanto to set this with `run_as_user="nexus"` 

I will create another the `Play` to do that . 

 - First I want to set `run_as_user="nexus"` into a `/opt/nexus/bin/nexus.rc` . I can use `blockinfile` module to change the content of the file (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/blockinfile_module.html#ansible-collections-ansible-builtin-blockinfile-module) .

   - `path` Attribute is will be the location of the file
  
   - `block` this is a block of content, so this `module` allows me to add multiple lines into a file, 
so if we had a configuration file and we wanted to add multiple lines in there, multiple configuration values, then we would use this blocking file and we would write all the configuration options line by line .

  - This `|` character representing a multi line string and the value that we are setting is a Nexus 

<img width="500" alt="Screenshot 2025-05-13 at 18 11 24" src="https://github.com/user-attachments/assets/5b17ff27-a381-46db-9fbb-645eb18bb470" />

Another module call `lineinfile` if I want to just write or replace an existing line with another line 

 - `path` Attribute location of the file I want to modify

 - `regexp` : '^#run_as_user=""' . This way I can tell Ansible I want to replace this line with `line: run_as_user="nexus"`

<img width="500" alt="Screenshot 2025-05-13 at 18 22 13" src="https://github.com/user-attachments/assets/61390a01-631e-48a6-bbfb-25786f235245" />

```
- name: Start Nexus with Nexus user
  hosts: 134.122.333.444
  tasks:
    - name: Set run_as_user nexus ### I can use one of this
      blockinfile: 
        path: /opt/nexus/bin/nexus.rc
        block: |
          run_as_user="nexus"
    - name: Set run_as_user nexus ### I can use one of this
      lineinfile:
        path: /opt/nexus/bin/nexus.rc
        regexp: '^#run_as_user=""'
        line: run_as_user="nexus"
```

The Untar Nexus task is getting executed everytime I replay my `Playbook` . I want to avoid that happen again 

 - I can do the same the as I did for the renamed Nexus folder task where I check whether the folder already exists and if it does, I don't  execute the task

 - Bcs in the same `Play` I can move `name: Check Nexus folder stats` to the top . Whenever I need to make the check I will just edit

 - So in `Untar Nexus installer` task I will add `when` conditionals and say only execute this task if there is no `/opt/nexus` folder it means that I already did all of this and I have that folder existing 

```
- name: Download and unpack Nexus installer
  hosts: 134.122.333.444
  tasks:
  - name: Check Nexus folder stats
    stat:
      path: /opt/nexus
    register: stat_result
  - name: Download Nexus
    get_url:
      url: https://download.sonatype.com/nexus/3/latest-unix.tar.gz
      dest: /opt/
    register: download_result #
  - name: Untar Nexus installer
    unarchive:
      src: "{{download_result.dest}}"
      dest: /opt/
      remote_src: True
    when: not stat_result.stat.exists
  - name: Find Nexus folder
    find:
      paths: /opt
      pattern: "nexus-*"
      file_type: directory
    register: find_result
  - name: Rename Nexus folder
    shell: mv {{fine_result.files[0].path}} /opt/nexus
    when: not stat_result.stat.exists
```

Back to `Start Nexus with Nexus user` Play . As a last step I just have to run Nexus by using `command` module

 - Before that I also have important thing which is switching to a Nexus user . Bcs I want to execute this `/opt/nexus-3.65.0-02/bin/nexus start` as a `nexus user`

 - So I have to tell Ansible to become a Nexus User by using `become: True`, and `become_user: nexus`

!!! Note: Ansible `Playbook` show success and I don't have any failed command, it doesn't always mean that application actually started successfully and that's why we actually did the check at the end of whether application is actually running . That mean we should always do some kind of validation to check that application really started successfully 

!!! Note: Second Problem is that application did not  start bcs of insufficient storage, bcs we are all the very small machine . I will create another bigger Droplet

```
- name: Start Nexus with Nexus user
  hosts: 134.122.333.444
  become: True
  become_user: nexus
  tasks:
    - name: Set run_as_user nexus ### I can use one of this
      blockinfile: 
        path: /opt/nexus/bin/nexus.rc
        block: |
          run_as_user="nexus"
    - name: Set run_as_user nexus ### I can use one of this
      lineinfile:
        path: /opt/nexus/bin/nexus.rc
        regexp: '^#run_as_user=""'
        line: run_as_user="nexus"
    - name: Start nexus
      command: /opt/nexus/bin/nexus start
```


Important: First I just destroy an old Droplet and I will use a new one a fresh clean server where I am gonna execute this playbook . If we did this manually like I did the first time in Nexus module we would basically execute all of the commands manually then we realized the Server is too small for Nexus and we have to destroy it and create a new one and manually install command again 

  - However with Ansible we gonna have finished completed script, so it doesn't matter how many times we have to recreate this Nexus configuration and start Nexus application, we just have to execute one command

Important: We have the `playbook` that has a couple of plays and in each play we have this hosts IP address . Whenever we create a new server we will have a new IP Address now we have to go through every `Play` and change the host name everywhere which is inefficient . And that why even for 1 single server to always name your servers 

 - To name nexus_server

```
[nexus_server]
207.154.204.213 ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_user=root
```

 - Now I can reference that one server using that name, even if we created new Server we will just have to update it in 1 place in the host file 

#### Fifth Play 

I want to see in the terminal from Ansible playbook execution itself for that I will add this 2 command  at the end

```
ps aux | grep nexus
netstat -lnpt
```

I will create another `play`

```
- name: Verify Nexus running
  hosts: nexus_server
  tasks:
    - name: Check with ps command
      shell: ps aux | grep nexus
      register: app_status
    - debug: msg={{app_status.stdout_lines}}
    - name: Check with netstat
      shell: netstat -plnt
      register: app_status
    - debug: msg={{app_status.stdout_lines}}
```

The output in the Terminal we don't see the Nexus Port bcs it take sometime at the beginning until it appear in the the result. If we execute the netstat command immediately after we started the application we are not gonna see that entry 

I can fix this timing issue by configure my `Play` to wait some time before executing the netstat command for that we have `pause` module (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/pause_module.html#ansible-collections-ansible-builtin-pause-module)

Another one is `wait_for` module (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/wait_for_module.html#ansible-collections-ansible-builtin-wait-for-module) . I can configure to wait for something to happen this could be timeout or wait for a port to open on the host 

```
- name: Verify Nexus running
  hosts: nexus_server
  tasks:
    - name: Check with ps command
      shell: ps aux | grep nexus
      register: app_status
    - debug: msg={{app_status.stdout_lines}}
    - name : Wait one minute
      pause:
        minutes: 1 ##
    - name: Check with netstat
      shell: netstat -plnt
      register: app_status
    - debug: msg={{app_status.stdout_lines}}
```

#### Configure Git

I will create a Git Repo for this Project 

My `Playbooks` should be stored safely in this remote repository . 

#### Configure Inventory Default Location 

Configuring default `hosts` file 

Until now whenever we executed our `Playbooks` we always has to pass that hosts file that we created here as a parameter . 

But what if we just created 1 `hosts` file in our Ansible project, in this specific project, and we want that `hosts` file to be just a default so that we don't have to pass it as a parameter of the command all the time, and be able to execute those `playbooks` just with the `playbooks` name 

To do that in my project I will create a `ansible.cfg` file  . 

 - I can configure default `hosts` file location

```
[defaults]
host_key_checking = False
inventory = <location-hosts-file>
```

Having Ansible configuration file insde our Ansible project makes it very easy to have separate configuration for each project . If each project has it's own hosts file, basically I will just configure it in that projects 

## Ansible and Docker

In this part we will write an Ansible `playbook` to install Docker and Docker-compost on an AWS EC2 Server and using Docker Compose file from one of our projects we will then start the containers on that EC2 Server  

First I will create AWS EC2 with Terraform 

Second I will connect to a Server using Ansible . Configure Inventory file to connect to AWS EC2 Instance . Then I will write a `Playbook` that installs Docker and Docker Compose on the Server, then copies a Docker Compose file to the Server and then start container that are defined in the Docker Compose on that remote EC2 server 

#### Create AWS EC2 Instance 

I want to change an `image_name` to an newer version 

In this Terraform configuration, we are creating a new key pair from our local public key, and that's an important information to SSH into the Server 

Another thing is remove the `user_data`

When I `terraform apply` I will have a server running with a IP address to ssh to server

#### Adjust Inventory file 

I will get a Public IP Address and pass it into `hosts` file 

```
[docker_server]
3.75.173.238 ansible_ssh_private_key_file=~/.ssh/id_
rsa ansible_user=ec2-user
```

#### Frist Play Install Docker 

I will create another `Playbooks`  call `touch deploy-docker.yaml`

To start with, I will write a play that just installs Docker on that EC2 Instance 

 - To install Docker i will use a `yum` module instead . Bcs this Amazon-Linux actually use `yum` as a package manager

 - As of recent Ansible versions (and modern Red Hat–based systems like CentOS 8+, RHEL 8+, Fedora), the yum module is officially a redirect to the dnf module. (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/dnf_module.html#ansible-collections-ansible-builtin-dnf-module)

 - Compare to the Droplet where we have root access, here I am using a admin user not root, so to install a package I need to switch to a root user . To do that I will use `become: yes` and `become_user: root` 
 
```
---
- name: Install Docker
  hosts: docker_server
  become: yes
  become_user: root
  tasks:
    - name: Install Docker
      ansible.builtin.dnf:
        name: docker
        update_cache: yes
        state: present
```

#### Second Play Instal Docker Compose

This `play` I will install Docker Compose 

In EC2 we can not install docker-compose using `yum` bcs it is now a `yum` package 

This time instead of installing Docker Compose as a standalone installation, we are going to install the docker compose CLI plugin which works more intuitively with ansible's docker modules . For `entry-script` we would do `yum update` and `yum install docker` 

From the Docs I need to create `mkdir -p $DOCKER_CONFIG/cli_plugins` first and then complete the installation 

So First task I will create a docker-compose directory using `file` module 

Secode task I will install docker-compose . To get some thing from the internet I use `get_url` module

<img width="442" alt="Screenshot 2025-05-14 at 11 39 01" src="https://github.com/user-attachments/assets/048d75d2-f906-4119-9ec6-1a7194d4544b" />

 - `uname -s` : print out Linux

 - `uname -m` : print out x86_64

 - `curl -SL https://github.com/docker/compose/releases/download/v2.36.0/docker-compose-$(uname -s)-$(uname -m)` basically these are not just string or part of the string of the URL these are the encapsulated command and the way that I have this here `-$(uname -s)-$(uname -m)` it will not work, this will be interpreted as part of the string and not as a command

 - I could hardcode it into this `curl -SL https://github.com/docker/compose/releases/download/v2.36.0/docker-compose-linux-x86_64`

 - But I also can handle this by takeing the ouput of `uname -m` command and register it with a variable in a separate ansible task, so I will use `shell` command to run the `uname -m` and register it a variable then I can print out the value of it as `x86_64`

 
Install Docker Compose Docs (https://docs.docker.com/compose/install/linux/)

```
---
- name: Install Docker-Compose
  hosts: docker_server
  tasks:
    - name: Create docker-compose directory
      file:
        path: ~/.docker/cli-plugins
        state: directory
    - name: Get architecture of remote machine
      shell: uname -m
      register: remote_arch
    - name: Install Docker-Compose
      get_url:
        url: curl -SL https://github.com/docker/compose/releases/download/v2.36.0/docker-compose-linux-{{ remote_arch.stdout }}
        dest: ~/.docker/cli-plugins/docker-compose
        mode: +x
```

#### Third Play Start Docker Daemon

I need to explicitly start docker with `sudo systemctl start docker` 

To do it in Ansible I will use `systemd` module  

  - `Systemd` module a redirect to the `ansible.builtin.systemd_service` module. (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/systemd_service_module.html#ansible-collections-ansible-builtin-systemd-service-module)

```
- name: Start docker
  hosts: docker_server
  become: yes
  tasks:
    - name: Start docker daemon
      ansible.builtin.systemd_service:
        name: docker
        state: started
```

#### Fourth Play Add ec2 user to Docker Group 

I need to add `ec2-user` to Docker Group so that we could then execute Docker commands with the `ec2-user` without using `sudo` like this : `sudo usermod -aG docker ec2-user`

I will do it in Ansible by using `user` module 

 - `user` module allow us to create new user or add existing to the groups (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html#ansible-collections-ansible-builtin-user-module)

After append `ec2-user` to a docker groups I need to disconnect the server and reconnect it again for it to work . I will create another task to reconnect to the server by using `meta` module (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/meta_module.html#ansible-collections-ansible-builtin-meta-module) . 

```
- name: Add ec2-user to Docker Group
  hosts: docker_server
  become: yes
  tasks:
    - name: Add ec2-user to Docker Group
      user:
        name: ec2-user
        groups: docker
        append: yes
    - name: Reconnect to server session
      meta: reset_connection 
```

#### Refactor the Playbook

In Ansible it is very common standard to name the tasks base on what I desire to be an outcome once Ansible executes 

Another thing is that we can basically decide depending on what makes the sense to me to group the tasks in the place in a different order 


#### Use community docker Ansible Collection

`command` module is for executing those commands that I can't cover using modules . This should be really the last resort when I can't execute those commands differently . 

However for Docker there is its own set of modules in Ansible 

`command` and  `shell` do not have state management inside, Ansible doesn't know whether to execute this command again or not . 

And we want that state management to have, especially when working with Docker and when executing and doing a lot of stuff with Docker . Plus of course the module are easier to use . I just need to define attribute key value pair and not a complete commands

All the modules start with `community.docker` belong to the same collection 

#### Pull Docker Image 

I will use `community.docker.docker_image` module to pull Docker Image (https://docs.ansible.com/ansible/latest/collections/community/docker/docker_image_module.html)

This module can pull the image, building the images, pushing the image to repository and so on 

I will create another `play` to pull docker image

 - I will use `docker_imange` module to pull the image .

 - Compare to `command` module Ansible detected or saw the redis already pulled so It not gonna run again

```
- name: Test docker pull
  hosts: docker_server
  tasks:
    - name: Pull redis
      docker_image:
        name: redis
        source: pull
```

#### Start Docker Containers
 
Above I just have tested `docker_image` module now I can remove it and write the actual one to start Docker Container  

First task I want to copy my docker-compose file from local host to EC2 Server . This docker compose is contain the image name of the container that I want to start on the server 

 - For copying file from local to the remote or from the control Node of Ansible to the managed node, we  can use `copy` module which has `src` and `dest` attribute

I need to Login to DockerHub or ECR Private Repository before I can pull the Private Image 

 - To do that I use `docker_login` module to login to Private Repository (https://docs.ansible.com/ansible/latest/collections/community/docker/docker_login_module.html)

 - I can specify registry URL which is the registry that I am logging into . The default is Docker Hub but also I can set this to ECR , Nexus

 - I also need to provide username and password as a Parameter .

 - I don't want to hardcode my password so I will use variable `password: "{{password}}"` . Then I can specify a variables file outside and set my password there  by using `vars_files` and set the name of the file 

<img width="500" alt="Screenshot 2025-05-14 at 13 06 34" src="https://github.com/user-attachments/assets/862bd5ea-2eb3-4b56-8239-9e9eb316ecef" />

 - Or one other way to set a variable value is using a variable prompt . So basically whe executing an Ansible Playbook I can prompt the user to enter the value and the user will have to enter that in the command line

 - To do that there is a attribute `vars_prompt`

 - I can use this either with `vars_prompt` and `vars_file`

<img width="500" alt="Screenshot 2025-05-14 at 13 11 54" src="https://github.com/user-attachments/assets/e7a4bad9-9a70-4533-819a-4689aa149fd4" />


Last task I want to run docker compose file that I copied into the server

 - For that I will use `docker_compose` module (https://docs.ansible.com/ansible/latest/collections/community/docker/docker_compose_v2_module.html#ansible-collections-community-docker-docker-compose-v2-module)

 - `project_src: <location_of_docker_compose_fle_on_the_Server>`

 - `state: Present` docker compose up . This is also the default state

```
- name: Start Docker Containers
  hosts: docker_server
  vars_prompt: ### 
    - name: docker_password
      prompt: Enter password for docker registry
  vars_file:
    - project-vars ###
  tasks:
    - name: Copy Docker Compose
      copy:
        src: <Absolute path of the Docker Compose file> 
        dest: /home/ec2-user/docker-compose.yamwl
    - name: Docker login
      docker_login:
        user_name: nguyenmanhtrinh
        password: "{{docker_password}}"
    - name: Start Containers from Compose
      community.docker.docker_compose_v2:
        project_src: <location_of_docker_compose_fle_on_the_Server>
        state: present ## docker compose up
```

#### Make the Playbook more generic

With the configured Playbook above it will not work for another Distribute Linux Install on them

Very Import to write Playbooks that can be run on many different Servers or can be run with minimum adjustments or can be run with minimum adjustment 

One of the thing that I can do in this `Playbook` to make it little general purpose and not very specicfic to EC2 Instance is instead of using the EC2 user I will create a new user and use that and use that user to basically also excute all of the tasks

I will change this Part

```
- name: Add ec2-user to Docker Group
  hosts: docker_server
  become: yes
  tasks:
    - name: Add ec2-user to Docker Group
      user:
        name: ec2-user
        groups: docker
        append: yes
    - name: Reconnect to server session
      meta: reset_connection 
```

I will create a new `play` . 

  - I will create a new Linux user using `user` module . I add this user to admin group and docker group2

  - In this case I don't have to reconnect to the server or reset the connection bcs we are not changing existing user groups, we are creating a new user with a group. This should be effective immediately

  - Then with that user I can use it to execute all the tasks by using `become: yes` and `become_user: <user_name>` on every single `play` that I want this user to execute

  - I also can parameterize a `groups : {{groups}}` like this and pass the value to a variables file

```
- name: Create new Linux User
  hosts: docker_server
  become: yes
  tasks:
    - name: Create new Linux user
      user:
        name: tim
        groups: adm, docker
```

## Ansible and Terraform

#### Intergrate Ansible Playbook execution in Terraform

Wrap up: I basically now know how to configure completely new EC2 Instance using Ansible . And for that we frist used Terraform to automatically create EC2 Instance and then we switched back to our Ansible project to execute Ansible Playbook there . 

We had to update the hosts file with the new Server's IP address and then just execute Ansible Playbook command

There is still the link between the provisioning and configuring the server that we have to do manually: 

 - We have to get that IP address from the Terraform output and we have to set that in the hosts file and then execute Ansible command 

I want to automate complete process

 - Provisioning the Server and then basically handing over the control over to Ansbile and then Ansible taking over and configuring the Server without any intervention from our side .

 - Basically I just need to execute `terraform apply`

First I will go to my Terraform project (https://github.com/ManhTrinhNguyen/Terraform-Exercise) 

In this part 

```
resource "aws_instance" "myapp" {
  ami = data.aws_ami.amazon-linux-image.id
  instance_type = var.instance_type
  subnet_id = aws_subnet.myapp-subnet.id 
  vpc_security_group_ids = [aws_security_group.myapp-sg.id]
  availability_zone = var.availability_zone

  associate_public_ip_address = true

  key_name = "terraform-exercise"


  user_data = file("entry-script.sh")

  user_data_replace_on_change = true
  tags = {
    Name = "${var.env_prefix}-myapp"
  }
}
```

I will remove a `user_data`

Inside this provision EC2 Instance block we can add a provisioner that will call Ansible command that will then execute on this instance

This time I will executing the `provisioner` with Ansible command 

Provisioner that we have available : 

 - `local-exec`

 - `remote-exec`

 - `file`
 
In this case I will use `local exec` provisioner

 - The reason why we need `local exec` provisioner bcs we are executing the Ansible command locally on this machine . On the same machine where `terraform apply` command get executed .

 - So the Ansible command that we will specify in `local exec` will execute on our laptop 

<img width="500" alt="Screenshot 2025-05-14 at 14 19 05" src="https://github.com/user-attachments/assets/8aab8dc7-222a-4d0f-aeca-bcd0302fd40b" />

I will execute `ansbile-playbook` command inside `local-exec` like this : `command: "ansible-playbook"`

 - To execute `ansible-playbook` first of all we need a playbook name . We need a path to our Ansible `playbook` : `command = ansible-playbook <path-to-ansible-playbook>`

 - But the path to Ansible playbook very long and not clean and also we don't only have the playbook, but we also don't have the configuration in that folder . We don't want to lose all that configuration when we execute the Playbook, so we want it to be the same as when we execute this playbook . For that I will change to the Ansible project directory before executing the command by using `working_dir= <Path-to-Ansible-Project>`

Another the things we have to do here if we execute this Terraform script with this local exec provisioner, so basically the server will be created with some new IP address and then this Ansible playbook will be executed from the Ansible folder, so which `hosts` will Ansbile conntect to is the `hosts` that is defined in Ansible Directory . 

 - We need someway to dynamically get the IP address instead of getting it from `hosts`

 - So instead of using this `hosts` file in our Ansible playbook command, we actually want to pass the IP address from the terraform itself .

 - And I can do that by setting a flag call `--inventory` . This `--inventory` will be a newly IP address created by Terraform which has to be available a dynamic value and we can actually get that by using `self` object .

 - Within `resource "aws_instance" ""` block the `self` refer to the AWS instance which has an attribute called public IP

 - So when the Server created the public IP address will be retrieved from the `self` object, this will be set as an IP address for the `--inventory ${self.public_ip}`. This should acutally overwrites the hosts file that we have in the Ansbile folder . So the `hosts` file will be ignored so this is not in addition to the hosts file but this is a replacement
 
 - and this `--inventory` flag take a file location as a parameter or a comma separated list of IP addresses. And if I am passing IP address just one IP address in this case, and not a file location I have to do `,` like this : `command = "ansible-playbook --inventory ${self.public_ip}, <Ansible-deploy-file>"` so Ansible knows we are passing the IP address not a file

And now on the IP address, we have to specify the Private key location and the user, just like we did in the `hosts` file 

 - And can specify those two also using flag and also parameterize the value `--private-key ${var.ssh_key_private}` .

 - In the `terraform.tfvars` I will set `ssh_key_private = /User/trinhnguyen/.ssh/id_rsa` . This will be the private key location

 - To specify user `--user ec2-user` . I can also parameterize it as well

   
```
resource "aws_instance" "myapp" {
  ami = data.aws_ami.amazon-linux-image.id
  instance_type = var.instance_type
  subnet_id = aws_subnet.myapp-subnet.id 
  vpc_security_group_ids = [aws_security_group.myapp-sg.id]
  availability_zone = var.availability_zone

  associate_public_ip_address = true

  key_name = "terraform-exercise"


  user_data = file("entry-script.sh")

  user_data_replace_on_change = true
  tags = {
    Name = "${var.env_prefix}-myapp"
  }

  provisioner "local-exec" {
    working_dir= <Path-to-Ansible-Project>
    command = "ansible-playbook --inventory ${self.public_ip}, --private-key ${var.ssh_key_private} --user ec2-user <Ansible-deploy-file>"
  }
}
```

One more thing I have to change in this `Playbook` itself . In `hosts` I am reference `docker_server`. We don't have a Docker Server reference anymore, bcs we are passing in `--inventory` which is just IP address, so we need to reference that IP address instead of reference `docker_server` . I will defind `hosts: all`

#### Wait for EC2 Server to be fully initialized 

So now we have configured executing Ansilbe `Playbook` from Terraform script there is one thing about provisioners that I want to mention which is using provisoners are not the recommended way according to Terraform bcs Terraform doesn't have control over the Provisioner, so can not managed the State 

 - So one of the problem we have with Provisioner is a timing issue , so by the time that this gets executed, the EC2 instance may not be fully created and initialized so basically when we executed a shell command here or even some other command, we might have an issue where the command already execute before the server is even able to get the request  

In Ansible we can avoid this timing issue by controlling when to connect to the server, and that is actually very helpful if I actually have this problem and also to make sure that the server is fully accessible and it is possible to ssh into the Server by the time that Ansible playbook gets executed 

To configure that in Ansible to fix the timing issue . At the beggining before we execute anything I will add the logic or basically we are gonna add a `play` that will check that the Server is already accessible on its SSH port and then we can start executing tasks on that Server 

 - `gather_facts: False` we are not trying to gather facts at the beginning, we just to check that the SSH connection is possible

 - I will use a module `wait_for` let us configure a criterion or basically some logic that tells Ansible to wait for this logic to be True before executing the next play (https://docs.ansible.com/ansible/latest/collections/ansible/builtin/wait_for_module.html#ansible-collections-ansible-builtin-wait-for-module)

 - I can configure waiting for a Port to be open on a Server and that what's I gonna use . So basically we gonna say wait for port 22, which is ssh port to be open

 - We can add the `delay` start checking ssh port is open after 10s

 - We can also add `timeout` to 100s to basically wait for that amount of time

 - `search_regex: OpenSSH` Which basically mean, so we want the port 22 to be open but also contain open SSH

 - And important part is on which host we are waiting for port to be open `host: '{{ (ansible_ssh_host|default(ansible_host))|default(inventory_hostname) }}'`

 - Also add `ansible_connection: Local` mean that we want execute this task locally, so from our local host we want to connect to the host that we get as a parameter and on that host we want to check these conditions

 - Also add `ansible_python_interpreter: /usr/bin/python3`

```
- name: Wait for SSH connection
  hosts: all
  gather_facts: False
  tasks:
    - name: Ensure SSH port open
      wait_for:
        port: 22
        host: '{{ (ansible_ssh_host|default(ansible_host))|default(inventory_hostname) }}'
        delay: 10
        timeout: 100
        search_regex: OpenSSH
      vars:
        ansible_connection: local
        ansible_python_interpreter: /usr/bin/python3
```

#### Using null resource

Another way to execute a provisioner if we want to separate it from AWS instance `resource` and have it as a separate task. 

In Terraform we can do that using a resource type called `null_resource`. 

`null_resource` is basically a Terraform provider that give us this `null_resource` type that we can use if we are not creating any specific resource on a cloud provider, or some other other platform, but we just want to execute some command either locally or remotely on the target machine 

```
resource "null_resource" "configure_server" {
  triggers = {
    trigger = aws_instance.myapp-server.public_ip
  }

  provisioner "local-exec" {
    working_dir= <Path-to-Ansible-Project>
    command = "ansible-playbook --inventory ${aws_instance.myapp-server.public_ip}, --private-key ${var.ssh_key_private} --user ec2-user <Ansible-deploy-file>"
  }
}
```

Now we have a separate task or resource for Ansible playbook execution, and I can use this for `remote exec`, `local exec` . But I have to adjust the `${self.public_ip}` bcs we don't have this self reference anymore, bcs we are outside of the aws instances resource . We can just easily reference it the `aws_instance resource` like this  `${aws_instance.myapp-server.public_ip}`

In `null_resource` we also have `triggers` attribute , Optional  but using `triggers` we can tell Terraform when to run `null_resource` . In our case we can configure Terraform to execute this `null_resource` or execute the Ansible Playbook acutally inside whenever a new server is created or basically whenever the ip address of that aws instance resource changes, so we know that a new server got created, so we need to run Ansible playbook

## Dynamic Inventory

Let say we have an infrastructure that we are managing with Anisble which is very dynamic, meaning new server get added and deleted all the time . And this is actually a very common practice when I have an infrastructure that has some auto-scaling maybe configured so whenever I have high demand for Server resources I basically scale up, I add a couple of new servers when I don't need that much resources, I scale them down 

In this case as I can imagine having a static list of inventory for Ansible doesn't make any sense, bcs IP address will change all the time .

We need some way to dynamically set the IP address or DNS names of the Servers that Ansible should manage 

Previous example we saw a case where we created a server using Terraform and then we used Terraform to hand over the control to Ansible and execute an Ansbile playbook command using that newly created Server instances, which was also an example of setting an IP address of of  the controlled node for Ansible using a Variable 

In this case we will execution of Terraform and Ansible again, so we will create 3 EC2 Instances using Terraform and then we are going to go to Ansible project to connect to those 3 Servers and configure them without hard coding the IP address of those Servers . Basically dynamically getting or fetching the information about the server IP address and using that in Ansible 

#### Terraform Create EC2 

To create 3 server I just need to replicate 3  `resources aws_instance` and give it 3 different name

For the third server I will set another `instance_type = "t2.small"`

Save it and create a Iac `terraform init` then `terraform apply`

 New server get added and removed all the time I want to fetch that information from AWS dynamically . We don't know how many Servers running on AWS account we don't know  what the IP addresses of them are, what Instance types etc . We just know that there are some servers running and we want to configure all of them using our Ansible playbooks 

 #### Inventory Plugin and Inventory Script 

For configuring a dynamic inventory for Ansible, we would need to connect to the AWS account from Ansible and fetch the information about the server instances from a specific region and get the Public IP address or public DNS name for all those Servers . 

So we need basically a functionallity in Ansible that is able to do all of that

 - Connect to AWS account and get server information

 For that we have 2 options in Ansible . (https://docs.ansible.com/ansible/latest/inventory_guide/intro_dynamic_inventory.html)

  - Dynamic inventory plugins

  - Dynamic inventory script

Ansible recommended to use Dynamic Inventory Plugins over Dynamic Iventory Script 

 - The main reason is bcs Plugins can use of Ansible Fetures like State Management

Inventory Plugis written in Yaml, Inventory Script are written in Python  

So the YAML format and the plugin functionality basically is part of the Ansible general formats and the inventory plugins also use all the Ansible Features and also the plugins use most of the recent updates of the Ansible code itself 

The script do not have that advangtages 

For both Inventory plugins and inventory scripts we have a list of them for different infrastructure providers  

 - If we connecting to an AWS account bcs we want inventory from AWS then we will need inventory plugin for AWS specifically

 - And If I am using Google or other platfor, I would need to find inventory plugin or script for that specific provider

 - In this case I will find an inventory plugin for AWS EC2  

#### AWS EC2 Plugin

(https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html)

If I want to use this plugin then I have to have Python modules, installed locally on my Laptop and the I will able to use that . I also need Boto3 and Botocore install locally

Now go back to Ansible project. First thing I need to do is in the Ansible configuration `ansible.cfg`,  we need to enbale AWS EC2 plugin

 - I will use `enable_plugins` to enable aws ec2 plugins . This could be a list of plugin 

```
enable_plugins = aws_ec2
```

#### Write plugin configuration

Now in this project I will create a new file and this is going to be our plugin configuration file `inventory_aws_ec2.yaml`

This is an important part about naming of this file . The last part of the name of this file has to be `aws_ec2`. Otherwise the plugin will not be able to recognize or identiry our plugin file  

<img width="500" alt="Screenshot 2025-05-14 at 19 08 49" src="https://github.com/user-attachments/assets/e7845723-73ed-4286-b0e4-19f61658586c" />

To write the plugin configuration we need the name of the plugin which is aws ec2

Now comes the attributes or configuration for the plugin 

 - First configuration we need is which region of AWS we want to check our inventory from . We can have multiple regions 

```
---
plugin: aws_ec2
regions:
  - us-west-1
```

Example above basically we just enables the plugin and then we configure that plugin to tell it which region to connect to . With that configuration now I can execute out plugin separately without any playbook execution and see what it returns 

So we can bascially just test it to see that we get the list of EC2 Instances . And for testing just the inventory plugin there is acutally a convenient to do that using `ansible-inventory`  command . 

<img width="500" alt="Screenshot 2025-05-14 at 19 18 37" src="https://github.com/user-attachments/assets/f5323f7a-ac67-47cb-a095-90931ba2a904" />

And to do that `ansible-inventory` command will take a parameter for inventory `ansible-inventory -i <file of inventory plugin> --list`

 - `--list` use to list a result

 - Once executed I have a huge output in the terminal. We have all the data with alot of attributes for all the Servers that this inventory plugin was able to fetch from AWS .  

Important to differenciate here we are not connecting to the Server yet . We are not ssh-ing into the Servers . We are not connecting to it bcs we haven't defined the private key or the server user here . We are just connect to AWS account and basically fetching the information about the server using the Python boto module 

If I don't want to see this huge ouput of all the information about the server, we only want the ouput of the server host names . We can do `ansible-inventory -i <plugin yaml file> --graph`

 - Then I will have a cleaner output with one group which is called AWS EC2 and the 3 Server

 - This is basically the equipvelent of the entry of the IP address in the `hosts` file

 - The Result in the terminal is a DNS name which then `Playbook` would use to connect to the Instances

 #### Assign public DNS to EC2 Instances 

This is a DNS name of the Instances which are private DNS name . Not public DNS that we can use to connect to the Server 

<img width="400" alt="Screenshot 2025-05-16 at 10 51 34" src="https://github.com/user-attachments/assets/af521e2b-c90a-4bca-a30f-b90fe6de443f" />

Important thing to know about AWS EC2 Dynamic Inventory is that if our Instances do not have Public DNS name assigned, we get the private DNS name

In order to be able to connect to the Servers using the Private DNS or Private IP address, our Ansilbe command would need to be executed from inside the VPC where the Server are running

If we want to connect to the VPC from **outside** the VPC, in this case from my Laptop I would need to use a Public DNS name or Public IP address

To fix that We will change our Terraform configuration so that when it creates these 3 Servers, all of the gets assigned a public DNS name 

 - Go back to Terraform and configure my Server to get the Public DNS automatically assigned .

 - That Configuration is acutally on the VPC level and not on the Instance level

 - I will add `enable_dns_hostnames = true` to `resources "aws_vpc" ""` . Now it will take care of that problem 

Now I have the Public DNS name and this will be use in the Playbook to connect to the Server 

#### Configure Ansible to use dynamic inventory 

 Basically now I have the `hosts` file configured in `ansible.cfg` by default as a `inventory = hosts` . 

 So instead of the `hosts` file we want our `playbook` to basically use that `inventory plugin` whenever playbook gets executed 

To do that . First in the `Playbook` we need to have a reference to the `host` that the plugin actually returns 

 - We can do either `all` that we used previously when we exected Ansible from Terraform project

 - Or we acutally have a group name that we get by default, which is `aws_ec2`
   
<img width="400" alt="Screenshot 2025-05-16 at 11 21 48" src="https://github.com/user-attachments/assets/fdb6f978-4624-4a54-9667-252faecd7c6a" />

 - We can use `aws_ec2` group name in the `hosts` instead of `all` and this will set the hosts to all the value that AWS EC2 group contains  

And second thing is whenever we are connecting to the `hosts` we need a username and the Private SSH key 

And if we want them set globally we can configure them inside the Ansible configuration file `ansible.cfg` . So we could pass those the user and private key file as parameters to Ansible Playbook command like we did in the Terraform configuration . Or if we want to spare us setting them as parameters we can just globally configure them 

So in `ansible.cfg` I will set `remote_user = ec2-user` and `private_key_fike = ~/.ssh/id_rsa`

So we have everything ready and configured to execute `Playbook` on those 3 servers . `ansible-playbook deploy-docker-new-user.yaml` . 

And we need to tell Ansible do not take `hosts` file but take the `inventory_aws_ec2.yaml` . The complete command will look like this : `ansible-playbook -i inventory_aws_ec2.yaml deploy-docker-new-user.yaml`

If we decide we alway gonna use the Inventory plugin as a Inventory source for our `Playbook` we could make `inventory_aws_ec2.yaml` as a default `hosts` file  like this in `ansible.cfg` -> `inventory = inventory_aws_ec2.yaml`

#### Target only Specific Servers

Now let say we don't want to execute the `Playbook` on all the Server we may want just target on some of the Server and not all of them 

 In terraform I will configure 2 dev server and 2 prod server as a Tag 

Imagine we may have 10 development Servers, 20 Staging Servers and 20 Production Servers . We may want to execute different `Playbooks` on different types of Servers . 

We may have a `playbook` for development server and slightly different one on production server 

To configure this dynamic plugin to give us only the `dev servers` or only the `prod servers` and not just everything from that region 

 - In the plugin configuration I can use `filters` to filter the Server I want to get as a result instead of just getting all of them by default .

 - And to know the attributes that I can use as filters I can actually reference the list (http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html#options.) right here

 - I can use any of that attribute as a filter . In our case we have tag name configure for each server . So we want to filter base on the tag name .

```
filters:
  tag:Name: dev*
```

 - The configuration above will give me a server with the tag name `dev`

 - To test that I can execute `ansible-inventory -i inventory_aws_ec2.yaml --graph`

 #### Create Dynamic Groups 

Now it could be that using that inventory plugin, we want to acutally return both Dev server and production server, but we want to differentiate them . We want to group them into separate group 

Intead of having everything in AWS EC2 groups, we want to have own group for `dev servers`, own group for `production servers`

And for that we can use another configuration which is called `keyed_group`

 - `keyed_group` actually has a list of configuration
 
   - `key` : What do we want or what attribute of the information of the instances do we want to use for grouping . In this case I want to use `tags`. `key: tags`
  
   - The different in the `keyed_groups` we use the key names, which are one of the attributes that we have . For the filters we used a different attribute lit provided by filters configuration
  
   - For `keyef_groups`. We actually look for the attributes that we get on the Instances
  
   - With this configuration we gonna get additional group that are group by `tags`. So we have 1 tag for each server which is name tag . So we gonna get 2 groups, one for developed Server and one for production Server
  
```
plugin: aws_ec2
regions:
  - us-west-1
keyed_groups:
  - key: tags
```

After executed that we still have common list where all the servers are listed so we can address all the servers with this group . And in addtion to that we have separate one for `_Name_dev_server` and `_Name_prod_server`

If we don't want to start with underscore we can add `prefix`

```
plugin: aws_ec2
regions:
  - us-west-1
keyed_groups:
  - key: tags
    prefix: "tag"
```

That mean if I want to execute this `playbook` only for the dev servers, I will going to copy the name of the group and set that as `hosts` value instead of the whole AWS EC2 lists

Now I can execute the playbook `ansible-playbook deploy-docker-new-user.yaml`

Now we know how to target server by grouping them together, and `tags` is just one of the keys that I can use to group instances . I can also use the image name and other attributes and I can acutally create multiple group . So we can leave this with the text, the grouping with text and we can add another one .

Let say in addition I want to froup the server based on their instance type 

 - I will take the attribute call `instance_type`

```
plugin: aws_ec2
regions:
  - us-west-1
keyed_groups:
  - key: tags
    prefix: "tag"
  - key: instance_type
    prefix: instance_type
```

## Deploying Application in Kubernetes

First I will create a Kubernetes Cluster on AWS using Terraform script that we created in Terraform module 

Once I have Kubernetes Cluster running we will configure Ansible to connect to that EKS Cluster and deploy a simple Deployment and service component inside the Cluster


#### Create EKS Cluster with Terraform 

This is a Terraform Project to create EKS Cluster (https://github.com/ManhTrinhNguyen/Terraform-Exercise) . 

I will switch to `eks branch` then I will create Terraform using by execute `terraform init` and `terraform apply --auto-approve`
  

#### Create a Namespace in EKS Cluster 

First I need to update `kubeconfig` file to connect to cluster  

 - This `kubeconfig` file include all the information needed to connect to our cluster like the Server Address, The hostname, certificates and so on

 - This information is what I will use to connect to the Cluster using Ansible

 - To generate `kubeconfig` file : `aws eks update-kubeconfig --region us-west-1 --name <Cluster-Name> --kubeconfig ~/terraform/kubeconfig_myapp-eks-cluster` . This command will finds the the `kubeconfig` of our Cluster after we define the region and the cluster name and create the file in the location we specify at the end here `--kubeconfig ~/terraform/kubeconfig_myapp-eks-cluster`

 - But first I need to create this file `kubeconfig_myapp-eks-cluster` in the directory and then the `kubeconfig` file will be saved in this location for me to use

Once a I have `kubeconfig` file I will switch back to Ansible project and create a file `touch deploy-to-k8s.yaml`

There is a Kubernetes module from Ansible (https://docs.ansible.com/ansible/latest/collections/kubernetes/core/k8s_module.html)

This make K8s very easy to create components to delete components, to configure stuff there . Basically instead of me running `kubectl` commands or actually even needing `kubectl` installed on my server, I can use this Kubernetes module to connect to the Kubernetes and basically do anything I want inside there 

First I will create a new Namespace 

```
---
- name: Deploy app in new namespace
  tasks:
   - name: Create a k8s namespace
     kubernetes.core.k8s:
       name: my-app
       api_version: v1
       kind: Namespace
       state: present
```

We have something very similar to Kubernetes configuration file and that's actually not a coincidence bcs as =you see both K8s and Ansible use yaml as a configuration file, so basically they kept the same attribute names  

I could write the whole Kubernetes configuration file directly in my Ansible `play` 

There is 3 more things I have to configure :

 - First is : `hosts: "localhost"` . I use `localhost` bcs I am executing Ansible `playbook` locally but I am actually connecting to a Kubernetes Cluster, so we don't need a specific worker node host or Kubernetes endpoint bcs we will define that seperately

 - Second I need to configure for Ansible acutally know which Cluster to connect to and how to connect to that Cluster . Basically it needs the address and it needs credentials which is exactly what I have in this `kubeconfig` file `kubeconfig_myapp-eks-cluster` . The way cinfigure that in Ansible is using the `kubeconfig` attribute from Ansible . This attribute points to the path of this `kubeconfig` file, by default if I don't specify that it will point to `~/.kube/config` . If my kubeconfig file is somewhere else then I have to set this path

 - Final I have the requirments basically for using this module or executing this module which are `python >= 3.9`, `kubernetes >= 24.2.0`, `PyYAML >= 3.11`, `jsonpatch` on the host where I execute the Ansible command these requirement should be fullfilled

 - If I am unsure whether I already have these module or not I can check that by using : `python3 -c "import kubernetes"` . If `PyYAML >= 3.11`  I can use `python3 -c "import YAML"` . If `jsonpatch` I can use `python3 -c "import jsonpatch"`

<img width="630" alt="Screenshot 2025-05-17 at 11 17 50" src="https://github.com/user-attachments/assets/c39b3a69-22fa-4bda-86c7-6b62dc01fabd" />

 - To install those packages or modules I will get inside Ansible project and use Python Virtual Environment .  

```
python3 -m venv ansible-env
source ansbile-env/bin/activate
python3 -m pip install <package-name>
```

 - I need to go to `ansible.cfg` and change to `inventory = hosts`. Bcs previous I set that to dynamic inventory so I will try to execute the plugin in the background

Now everything configured I can execute it `ansible-playbook deploy-to-k8s.yaml`

```
---
- name: Deploy app in new namespace
  hosts: localhost
  tasks:
   - name: Create a k8s namespace
     kubernetes.core.k8s:
       name: my-app
       api_version: v1
       kind: Namespace
       state: present
       kubeconfig: <absolute-path-to-kubeconfig>
```

Now I have a namespace created 

#### Deploy App in new Namespace

I will create another task 

This time I want to deploy or create Kubernetes component from the Kubernetes configuration file itself . 

I have a simple nginx app deployment here `Ansible/nginx-config.yaml`

To deploy from Kubernetes yaml configuration file itself I will use `src` attribute 

 - `src` point to the file that contains Kubernetes Yaml configuration

 - `state: present` to create file `state: absent` to delete file

 - I also need kubeconfig context

 - `namespace: my-app` : To deploy in this namespace

```
---
- name: Deploy app in new namespace
  hosts: localhost
  tasks:
   - name: Create a k8s namespace
     kubernetes.core.k8s:
       name: my-app
       api_version: v1
       kind: Namespace
       state: present
       kubeconfig: <absolute-path-to-kubeconfig>
   - name: Deploy Nginx App
     kubernetes.core.k8s:
       src: <absolute-path-to-yaml-file>
       state: present
       kubeconfig: <absolute-path-to-kubeconfig>
       namespace: my-app
```

Now I can execute it `ansible-playbook deploy-to-k8s.yaml`

#### Set ENV for kubeconfig

If I have a lot of task here in Kubernetes module then it would be very inconvenient to use `kubeconfig` attribute in every single task . 

I want to define a `kubeconfig` once for Ansible and then basically use that for the whole `playbook`

I can set an ENV `K8S_AUTH_KUBECONFIG` before executing Ansible that points to the `kubeconfig` and Ansible then know where to get this information and will use that for all tasks .

 - To do that : `export K8S_AUTH_KUBECONFIG=<absolute-path-to-kubeconfig>`

 - Now I can delete all the kubeconfig in all task

## Run Ansible from Jenkins Pipeline

I have a Jenkins server running on a DigitalOcean droplet 

For each tool that we wanted to use like Terraform or kubectl to execute from a pipeline basically installed it and made it available inside the Jenkins Server . So we have kubectl command or Terraform command that available on the Jenkins job 

However for Ansible we will do something a little different . Instead of installing Ansible inside Jenkins container and making it available in a Jenkins Server, we will create a dedicated Server for Ansible and Install Ansible there 

This is a common practice I can either run Ansible locally on my laptop or Jenkins server , install it locally and run it from there or as a common practice I will have a dedicated server for Ansible and install it on a different server and I can execute Ansible command from that Server where Ansible is installed 

 - I will create another Droplet and I will install Ansible on that droplet

 - Now I have separate Jenkins server and separate Jenkins Server both running on DigitalOcean

Once we have that set up we want to execute Ansible playbook from a Jenkins pipeline that configures two EC2 instances. It basically just install Docker and Docker Compose on them 

To see that in action we will create a pipeline in Jenkins, we will connect it to our already familar Java application and in Java application we will create a Jenkins file that basically calls an Ansible playbook execution on a separate server from Jenkins and that Playbook will configure 2 EC2 by installing Docker and Docker Compose on them  

#### Prepare Ansible Control Node

I will create a new Droplet called `Ansbile Server` . This will be a Server Control Node . 

The Server Control Node basically controls and configures other remote servers

Next step I will ssh into that Server and Install Ansible . `ssh root@<ip address>`

 - Update server : `apt update`

 - To Install ansible : `apt install ansible-core`

 - I also need to install Python module for AWS bcs Ansible is based on Python so whenever we want to execute certain tasks from Ansible on AWS servers for some of the tasks we will need the Python module for AWS which Ansible will then use in the background . Those Python module is `boto3` and `botocore`. To install `boto3` on Linux: `apt install python3-boto3` after installed this , this is also install `boto-core` as a dependency at the same time

 - Last thing I need AWS credentials configured on the Server .

   - We can connect to EC2 Instances using the private SSH key using Ansible . We don't need AWS credentials for that
  
   - However we will be using a `dynamic inventory` for the `Playbook` so instead of hard coding the EC2 Instance IP addresses we can basically just get the dynamically from AWS and we need AWS crednetials for that `dynamic inventory` to work . So the `aws_ec2` inventory plugin in the background will try to connect to AWS to fetch all the EC2 Instances from our AWS Account and we need to authenticate
  
   - The default location for AWS credentials is `.aws` in user home directory . Create `.aws` folder `mkdir .aws`
  
   - Inside `.aws` we will create `vim credentials` . I can copy the AWS credentiasl from my local machine
  

Wrap up : So we have Ansible installed, we have `boto3` and `boto-core` python module installed and we have AWS credentials ready and the `boto` library and credentials are there when we need to use `dynamic inventory` bcs then the plugin will need to authenticate with AWS 

#### Create 2 EC2 Instances to be managed by Ansbile

Now we will create 2 EC2 Instances the we want to configure with Ansible Playbook

I will go to AWS Dashboard and launch 2 AWS Linux Iamge Instances 

 - I need to create a new SSH key . Bcs Ansible need this key to connect to the Instances that need to configure . This is a key that I need to provide Ansible so that it can connect to and configure our EC2 Instances


## Ansible Integration in Jenkins

#### Copy files from Jenkins to Ansible Server Jenkinfile

Now we have our Ansiblr Control Node and we have our Ansible managed Nodes. Now it is time for everything to come to together and basically to call the Ansible Control Node to configure those managed nodes and do that from a Jenkins pipeline 

For that We will configure Jenkin and we will write a Jenkinsfile with all the necessary steps 

I will start with writing Jenkinsfile that copies all the necessary files to a remote Ansible Server and then executes an Ansible Playbook on the remote server 

In my Java project I will create a new branch call `git checkout -b ansible`

I will create a new Jenkinsfile from sratch 

First stage is `stage('copy files to ansible server)` 

What files we need to copy to Ansible Server so we can execute Ansible the Playbook

- When we execute Ansible Playbook we need the Playbook itself, that basically does and executes all these steps

- We also need `hosts` files or `dynamic inventory` . In this case I will use `aws_ec2` Inventory file

- We also need `ansible.cfg`

Those 3 files is what we will need so that we can execute Ansbile Playbook on a remote server which mean that we have to include these 3 files in my Java App project so then Jenkins have access to them and copy them to Ansible Server 

- In Java App create a folder `mkdir ansible` . And copy

  - `inventory_aws_ec2.yaml`
 
  - `ansible.cgf`
 
    - I need to configure `inventroy = inventory_aws_ec2.yaml`
   
    - I also have this `private_key_file = ~/.ssh/id_rsa` locally when I execute from my Laptop . This was my home folder and was my private key . However, this time we have `pem` file from AWS key pair that we created and we will copy that `pem` file to Ansible server from Jenkins and we can decide what that `pem` file is going to be called on Ansible Server let's say `~/ssh-key.pem` in the root users home directory
   
  - Third file will be `my-playbook.yaml`  . I will copy only Install Docker `play` and Install Docker-compose `play`
 
  - Those 3 files above is what I need to be available on the Ansible server

Important to understand : We are executing pipeline on a Jenkins, so Jenkins server will check out this repository and have all these files on the Jenkins Server in a build pipeline . Now Jenkins server or the pipeline will have to trigger or call Ansilbe Playbook on a remote Ansible Server and that means that this Playbook as well as inventory file and Ansible configuration have to be available on the Ansible so that Ansbile Playbook command can be executed there . So Jenkins has to copy those files from this Java projects directory to the remote Ansible Server

But in addition to those 3 Ansible files, we also want to copy the private key file to Ansible Server . Bcs When Ansible playbook get executed with this configuration, Ansible will look for `pem` file at this location `~/ssh-key.pem` and that mean this file should be available there . 

Now I will configure my Jenkinsfile : 

- To copy files generally from Jenkins to some remote Server I will use `sshAgent` plugin

- `sshagent` plugin allow me to use SSH key to connect to a remote server and basically copy all the files to that server using simple `scp` command

- I need the ssh key to connect to that remote server that I am trying to connect from Jenkins . In this case I am connecting to the Ansible Server . So I need the ssh key for that Server . On droplet I am acutally using my Private key . Now I use that Private Key to create new Credentials In Jenkins which is `SSH username with private key` . username is `root`

- Important thing to note about the ssh key is that has the start line `BENGIN OPENSSH PRIVATE KEY` and the problem is that whenever we have a key that is generated by recent version of OpenSSH, I will get a key in the new OpenSSH format . And Jenkins does not support this format so when we try to connect to the DigitalOcean droplet from a Jenkins pipelines using this newer private key, it is not going to work . There is  a way to convert it so that it is compatible for Jenkins `ssh-keygen -p -f .ssh/id_rsa -m pem -P "" -N ""`

- `ssh-keygen -p -f .ssh/id_rsa -m pem -P "" -N ""` this command will basically take my newly formatted or private key with a new OPEN SSH Format conver it to the Classic OpensSH format which will then be compatible for Jenkins so that I can use it then I will see `BENGIN RSA PRIVATE KEY`

- `sshagent([''])` syntax

  - As a parameters we are providing the credentials ID of the SSH key credentials that we created . So `sshagent` will take that private key and use that or make availalbe inside this block for all the `ssh` or `scp` command
 
  - `sh "scp -o StrictHostKeyChecking=no ansible/*" root@<remote-ip-address>:/root` this command basiscally I want to copy anything from `ansible` folder
 
  - Now I want to copy the ssh `pem` key file to Ansbile Control Node . Which mean I should have this `pem` file available in Jenkins first so Jenkins can copy that Ansible . I will create a new Credentails in Jenkins from the `.pem` file format `SSH username with private key`, username will be `ec2-user`. Then I will copy that `.pem` file to the remote server by using `withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user' )]){}`
 
    - `keyFileVariable: 'keyfile'` basically create a temporary with content of the `ec2-server-key` credentials so I have the whole file available not the content only
   
  - Now I can `sh "scp ${keyfile} root@<remote-ip-address>:/root/ssh-key.pem"` .
 
  - `root@<remote-ip-address>:/root/ssh-key.pem` I want that file to have a specific name that I configure in `ansible.cfg` -> `private_key_file = ~/ssh-key.pem`
 
  - I don't need this `-o StrictHostKeyChecking=no` bcs it happen  in `sh "scp -o StrictHostKeyChecking=no ansible/* root@<remote-ip-address>:/root"` already
 
  - This will take care of copying all the necessary files to the Ansible server so that we can execute Ansible Playbook from there 

```
stage("Copy files to Ansible Server"){
 steps {
    script {
      sshagent(['ansible-server-key']) {
        sh "scp -o StrictHostKeyChecking=no ansible/* root@<remote-ip-address>:/root"
        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user' )]){
          sh "scp ${keyfile} root@<remote-ip-address>:/root/ssh-key.pem"
        }
      }
    }
  }
}
```

No I can `git add .` and `git commit -m ""` and `git push` to the remote repository  

#### Create Jenkins pipeline 


I will create a simple Pipeline called `ansilbe-pipeline`

I will configure that pipline to work with my java Application . 

 - Put my Repository URL in there

 - Configure credentials of that Repository

 - Branch should be `/ansible` 

Then I can run the pipeline . Then `ssh` to the remote Ansible server I will see all the files that I copy will be there 
      
One thing I have to optimize is when I click inside the build I have the warning side that say we have security issue exposing the private key file 

 - Warning: A secret was passed to `sh` using Groovy String interpolation which is insecure

 - What is that mean is that right here `withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user' )]){}` I am referencing the `keyfile` here directly putting like above example is insecure and that has to do with how groovy interpreter acutally works and the problem is that the groovy interpreter will actually expose the contents of this secret on the command line so it's going to be in the command line history which is insecure so in order to fix that and avoid this from happening there is another syntax that we can use so instead of `""` I will use `''` and remove the `{}`

```
withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user' )]){
  sh 'scp $keyfile root@<remote-ip-address>:/root/ssh-key.pem'
}
```

 - This syntax will make groovy interpreter use this secret in a different way which will not expose it to a command line

####  Execute Ansible Playbook from Jenkins

Now I want to execute the Playbook from Jenkins Server by triggering it on a remote server 

I will add another stage call `stage("execute ansible playbook")`

 - I need another plugin which basically enable us to execute command line on a remote server. Jenkins will execute a command on the Ansible server on a remote server using this so basically now all the files are available on the Ansbile server Jenkins needs to trigger this Ansible Playbook command and that what's we need to do . Plugin called `SSH Pipeline Steps`

 - `sshCommand` have 2 parameters

   - `remote: remote` this is an object with multiple attributes that references the remote server so basically we are executing a command on a remote server and we have to pass in an object for the remote server including the host name and ip address, user, private key and so on . All of that will be in `remote` object that I created
  
   - Second parameters `command: ""` this is a actual command
  
   - Now I will create a `remote` object


```
stage("execute ansible playbook"){
  steps{
    script {
      echo "calling ansible playbook to configure ec2 instances"
      def remote = [:]
      remote.name = "ansible-server" # This is a name of the remote server
      remote.host = "<ip-address>" # I need to put the IP Address of  the remote server
      remote.allowAnyHosts = true # This help me not to confirm this interactively

      ## I will use withCredentials to get username and private key for the remote object .

      withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user' )]){
        sh 'scp $keyfile root@<remote-ip-address>:/root/ssh-key.pem'
      }
      
    }
  }
}
```















