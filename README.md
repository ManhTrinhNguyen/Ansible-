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

