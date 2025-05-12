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
















