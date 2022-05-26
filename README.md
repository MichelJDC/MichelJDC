## Automated ELK Stack Deployment
 
The files in this repository were used to configure the network depicted below.

These files have been tested and used to generate a live ELK deployment on Azure.
They can be used to either recreate the entire deployment pictured above.

Alternatively, select portions of the instal file may be used to install elk.yml only certain pieces of it, such as Filebeat.

 
 **install elk.yml**
 
 
This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build
 
### Description of the Topology
 
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

 
Load balancing ensures that the application will be **highly available**, in addition to restricting **access** to the network.

What aspect of security do load balancers protect? What is the advantage of a jump box?
  - Load balancers will provide high availability to the applications and access restriction to the networks other than allowed ports.
  - Jumpbox will provide access control restrictions to access the resources and allow  only authorized admins/IP’s.
 
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
 
- What does Filebeat watch for? 
  - Filebeat collects data about the file system.

- What does Metricbeat record? 
  - It will collect machines up time which will help to measure the system availability. 
  - It also collects system performance records such as RAM, CPU, Processor, Speed etc..to create metrics and reporting on the system performance.
  
The configuration details of each machine may be found below.

Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table.

| Name       	| Function     	| IP Address 	| Operating System 	|
|------------	|--------------	|------------	|------------------	|
| Jump Box   	| AccessServer 	| 10.0.0.4   	| Linux            	|
| Web -1     	| WebServer    	| 10.0.0.5   	| Linux            	|
| Web -2     	| WebServer    	| 10.0.0.6   	| Linux            	|
| Web -3     	| WebServer    	| 10.0.0.7   	| Linux            	|
| ELK-Server 	| Logging      	| 10.1.0.4   	| Linux            	|


### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- Add whitelisted IP addresses: 
  - My home public ip address for the port 5601
Machines within the network can only be accessed by Jumpbox Provisioner.

- Which machine did you allow to access your ELK VM? What was its IP address?
  - ELK Server can be accessed through SSH from any private/public ip's. Also access provison from my home public ip address for the port 5601

  - Elkserver public IP: 13.70.181.156

  - Elkserver private IP: 10.1.0.4


A summary of the access policies in place can be found in the table below.
 
 | Name         	| Publicly Accessible 	| Allowed IP Addresses            	|
|--------------	|---------------------	|---------------------------------	|
| Jump Box     	| No                  	| 10.0.0.4                        	|
| Web -1       	| No                  	| Allow Via SSH from 10.0.0.4     	|
| Web -2       	| No                  	| Allow Via SSH from 10.0.0.4     	|
| Web -3       	| No                  	| Allow Via SSH from 10.0.0.4     	|
| ELK-VM       	| No                  	| Allow Via SSH from 10.0.0.4     	|
| HTTP-Traffic 	| Yes                 	| Any ip                          	|
| ELKServer    	| Yes                 	| Home public ip address via 5601 	|


### Elk Configuration
 
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
  - Ansible allows to automate the creation, configuration and management of elk, instead of manually keeping servers updated, making configurations, moving files, etc. Ansible to automate this for groups of servers from one control machine using an orchestration tool.

The playbook implements the following tasks:

Install docker.io then install python nad install docker module, increase the VM memory and download and launch docker elk container.

Following cammands lines are used

sysadmin@Jump-Box-Provisioner:~$ sudo apt install docker.io

sysadmin@Jump-Box-Provisioner:~$ sudo systemctl status docker

sysadmin@Jump-Box-Provisioner:~$ sudo docker pull cyberxsecurity/ansible

sysadmin@Jump-Box-Provisioner:~$ sudo docker run -ti cyberxsecurity/ansible bash

root@7aa417252dc2:~# ls /

bin   dev  home  lib32  libx32  mnt  packer-files  root  sbin  sys  usr
boot  etc  lib   lib64  media   opt  proc      	run   srv   tmp  var

root@7aa417252dc2:~# exit

sysadmin@Jump-Box-Provisioner:~$ sudo docker images

REPOSITORY               TAG   	IMAGE ID   	CREATED     	SIZE
cyberxsecurity/ansible   latest	7d2d9fa20ccf   11 months ago   754MB

sysadmin@Jump-Box-Provisioner:~$ sudo docker start

sysadmin@Jump-Box-Provisioner:~$ sudo docket container list -a

CONTAINER ID   IMAGE                    COMMAND                  CREATED                        PORTS 	NAMES
4413dd57dd1b   cyberxsecurity/ansible   "/bin/sh -c /bin/bas…"   12 minutes ago  minutes ago          	condescending_curie

sysadmin@Jump-Box-Provisioner:~$ sudo docker start condescending_curie

sysadmin@Jump-Box-Provisioner:~$ sudo docker attach condescending_curie

root@4413dd57dd1b:~# ls /
bin   dev  home  lib32  libx32  mnt  packer-files  root  sbin  sys  usr
boot  etc  lib   lib64  media   opt  proc      	run   srv   tmp  var

root@4413dd57dd1b:~# cd /etc/ansible/

root@4413dd57dd1b:/etc/ansible# ls

ansible.cfg  hosts
 
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
 
![Docker](https://user-images.githubusercontent.com/105895908/169713313-d45c2f67-58de-4912-911f-ee12d6aec4b6.jpg)
 
### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- Web 1 10.0.0.5 | Web 2 10.0.0.6 |Web 3 10.0.0.7

We have installed the following Beats on these machines:

- Filebeat & Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat(monitors and collect logs)
- Metricbeat(collects metrics and statistical data) -It will collects machines up time which will helps to measure the system availbility. 
- It also collect system performance records such as RAM, CPU, Processor, Speed etc..to create metrics and reporting on the system performance)
 
### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
 
SSH into the control node and follow the steps below:

- Copy the configuration file to ansible container.
- Update the configuration file to include...
- Run the playbook, and navigate to ELK to check that the installation worked as expected.
 
Answer the following questions to fill in the blanks:

- Which file is the playbook? Where do you copy it?
 
   filebeat-playbook.yml

   /etc/ansible/files/filebeat-config.yml

   root@4413dd57dd1b:/etc/ansible# nano filebeat-playbook.yml to update the file
 
- Which file do you update to make Ansible run the playbook on a specific machine?  How do I specify which machine to install the ELK server on versus which to install Filebeat on?

   We will host file and configration file to update web ip's and elk server ip's.

   root@4413dd57dd1b:/etc/ansible# ls

   ansible.cfg  elk.yml  filebeat-config.yml  hosts  pentest.yml

   root@4413dd57dd1b:/etc/ansible# nano hosts

   root@4413dd57dd1b:/etc/ansible# nano filebeat-playbook.yml

   root@4413dd57dd1b:/etc/ansible# ansible-playbook filebeat-playbook.yml
 
- Which URL do you navigate to in order to check that the ELK server is running?

    http://13.77.56.196:5601/app/kibana
 
 
_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

SSH into Jumpbox VM (20.92.106.64)

Run these commands to connect to container

sysadmin@Jump-Box-Provisioner:~$ sudo docket container list -a

CONTAINER ID   IMAGE                    COMMAND                  CREATED                        PORTS 	NAMES
4413dd57dd1b   cyberxsecurity/ansible   "/bin/sh -c /bin/bas…"   12 minutes ago  minutes ago          	condescending_curie

sysadmin@Jump-Box-Provisioner:~$ sudo docker start condescending_curie

sysadmin@Jump-Box-Provisioner:~$ sudo docker attach condescending_curie

root@4413dd57dd1b:~# ls /

bin   dev  home  lib32  libx32  mnt  packer-files  root  sbin  sys  usr
boot  etc  lib   lib64  media   opt  proc      	run   srv   tmp  var

root@4413dd57dd1b:~# cd /etc/ansible/

root@4413dd57dd1b:/etc/ansible# ls

ansible.cfg  filebeat-playbook.yml  hosts                	pentest.yml elk.yml  	files              	metricbeat-playbook.yml

/etc/ansible/files/metricbeat-config.yml

root@4413dd57dd1b:/etc/ansible/files# ls

filebeat-config.yml  metricbeat-config.yml
 
Update the files and run.

Output:
![Kibana Output](https://user-images.githubusercontent.com/105895908/169713605-4e871003-0218-4465-8772-81927eb32b91.jpg)

**In addition some of the commands used in this project**

$ ssh-keygen

$ cat /c/Users/mages/.ssh/id_rsa.pub

ssh sysadmin@Jump-Box-Provisioner IP address

~$ sudo apt install docker.io

~$  sudo systemctl status docker

~$ sudo docker pull cyberxsecurity/ansible

~$ sudo docker run -ti cyberxsecurity/ansible bash

~$ sudo docker images

~$ sudo docker start

~$ sudo docker ps

~# ansible -m ping all

~# cd /etc/ansible/

/etc/ansible# ls

/etc/ansible# cd files

/etc/ansible/files# ls

/etc/ansible/files# nano metricbeat-configuration.yml

/etc/ansible/files# ls

/etc/ansible/files# mv metricbeat-configuration.yml /etc/ansible/files/metricbeat-config.yml

/etc/ansible# ansible-playbook metricbeat-playbook.yml
