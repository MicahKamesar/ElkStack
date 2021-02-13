## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

README/Images/Elk__Depoloyment_Azure.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the /etc/ansible file may be used to install only certain pieces of it, such as Filebeat.

  /etc/ansible/hosts
  /etc/ansible/ansible.cfg
  /etc/ansible/filebeat.cfg
  /etc/ansible/install-elk.yml
  /etc/ansible/metricbeat-config.yml
  /etc/ansible/roles/playbook.yml
  /etc/ansible/roles/filebeat-playbook.yml
  /etc/ansible/roles/metricbeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
Load balancers relate to the "availability" portion of the CIA triad. By having a load balancer, we can ensure we have high availability by having the balancer distribute traffic to more than 1 VM.  This also creates redundancy and a fail-safe should there be too much traffic, or issues, with 1 VM.
Jump boxes give us an environment to setup our network, along with a place to setup our containers. They provide an extra layer of security since they are isolated from the public internet and the internal network, and we only perform admin tasks on them.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.
- _TODO: What does Filebeat watch for? data about the file system, specifically what files have changed and when
- _TODO: What does Metricbeat record? machine metrics

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name    | Function   | IP Address | Operating System |
|---------|------------|------------|------------------|
| JumpBox | Gateway    | 10.0.0.4   | Linux            |
| Web1    | Web Server | 10.0.0.5   | Linux            |
| Web2    | Web Server | 10.0.0.7   | Linux            |
| ElkVM   | Elk Server | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
71.127.219.67

Machines within the network can only be accessed by the JumpBox machine.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address? The host machine only, IP address = 71.127.219.67 

A summary of the access policies in place can be found in the table below.

| Name    | Publicly Accessible | Allowed IP Addresses |
|---------|---------------------|----------------------|
| JumpBox | No                  | 71.127.219.67        |
| Web1    | No                  | 10.0.0.4             |
| Web2    | No                  | 10.0.0.4             |
| ElkVM   | No                  | 71.127.219.67        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
It is not prone to manual mistakes and is much more efficient.

The playbook implements the following tasks:
- Installs docker
- Installs Python3
- Increases the memory count
- Downloads and launches a docker elk container, image: sebp/elk:761
- Enables Docker service

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

README/Images/Screenshot_Docker_PS.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
Web1 & Web2

We have installed the following Beats on these machines:
Filebeat & Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat collects logs files from systems. One example would be a system log of when the server was accessed.
Metricbeat collects system level usage info. One example would be seeing how much memory is being used on a VM.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml to the destination /etc/ansible folder.
- Update the install-elk.yml to include...
- Run the playbook, and navigate to http://20.83.56.184:5601/app/kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it? The playbook is located in the container in /etc/ansible/.  You can copy it to any machine that you want via .yml files and setting up the machines in the hosts file.
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? You update the /etc/ansible/hosts file to run it on a specific machine.  Currently "[webservers]" has both Web1 & Web2 configured, and "[elk]" has the ElkVM configured. To specify which machine to install ELK or firebeat, in the .yml playbook of the specific program, specify the name in the "hosts" field.   This name is based on what is the the /etc/ansible/hosts file. 
- _Which URL do you navigate to in order to check that the ELK server is running? http://20.83.56.184:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
To download the playbook -
ssh azadmin@20.84.88.30
sudo docker start affectionate_matsumoto
sudo docker attach affectionate_matsumoto
cd /etc/ansible
ansible-playbook playbook.yml

To update the playbook files-
ssh azadmin@20.84.88.30
sudo docker start affectionate_matsumoto
sudo docker attach affectionate_matsumoto
cd /etc/ansible
nano playbook.yml (if updating the playbook - the same can be done with the filebeat and metricbeat playbooks)