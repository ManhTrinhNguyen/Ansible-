- [Introduction](#Introduction)

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

 - I can do that In my terminal `ssh-copy-id root@<ip-address>`. This will take the public key in the default location which is `cat .ssh/authorized_key` and copy that to the droplet root users in this location `cat .ssh/authorized_key`

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




