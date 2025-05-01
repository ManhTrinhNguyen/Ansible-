- [Introduction](#Introduction)

  - [Modules | Ansible architecture](#Modules)
  
  - [Ansible uses YAML](#Ansible-uses-YAML)
  
  - [Ansible Playbooks](#Ansible-Playbooks)
  
  - [Ansible Inventory List](#Ansible-Inventory-List)
  
  - [Ansible for Docker](#Ansible-for-Docker)
 
  - [Ansible Tower](#Ansible-Tower)

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














