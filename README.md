# ELK-Stack
ELK Stack Project 1

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/1D3P75fcXxavbW0YBVUgoqaLCAdFAws4P/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ELK Stack file may be used to install only certain pieces of it, such as Filebeat.

  - filebeat-playbook.yml: 
  ---
   - name: installing and launching filebeat
     hosts: webservers
     become: yes
     tasks:

   - name: download filebeat .deb file
     command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

   - name: install filebeat .deb
     command: dpkg -i filebeat-7.6.1-amd64.deb
     args:
       warn: false

   - name: drop in filebeat.yml
     copy:
       src: /etc/ansible/files/filebeat-config.yml
       dest: /etc/filebeat/filebeat.yml

   - name: enable and configure system module
     command: filebeat modules enable system

   - name: setup filebeat
     command: filebeat setup

   - name: start filebeat service
     command: service filebeat start

   - name: enable service filebeat on boot
     systemd:
       name: filebeat
       enabled: yes


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- Load balancers act intelligently by distributing the incoming web traffic from clients across multiple servers but for this project, the load balancers distributed the traffic across multiple virtual machines. The advantage of having a jump box is that it is an origination point for the virtual machines connected to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- Filebeat watches for log files/locations and collects log events.
- Metricbeat collects the metrics from the operating system and from services running on the server.

The configuration details of each machine may be found below.

| Name       |  Function   |  IP Address   | Operating System |
|------------|-------------|---------------|------------------|
| Jump Box   |  Gateway    |104.45.138.174 |   Linux          |
| Web-1      |  Webserver  |  10.0.0.5     |   Linux          |
| Web-2      |  Webserver  |  10.0.0.6     |   Linux          |
| Web-3      |  Webserver  |  10.0.0.7     |   Linux          |
| ELK        |  Monitoring |52.165.144.163 |   Linux          |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 5601 Kibana

Machines within the network can only be accessed by jump box provisioner.
- The only IP address allowed to access the ELK virtual machine is from my IP address, which is 70.127.99.159.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |       Yes           |   104.45.138.174     |
| Web-1    |       No            |      10.0.0.5        |
| Web-2    |       No            |      10.0.0.6        |
| Web-3    |       No            |      10.0.0.7        |
| ELK      |       No            |      10.1.0.4        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Simple to set up and use
- Flexible, powerful, and efficient

The playbook implements the following tasks:
- Install docker.io: Installs the latest version of docker.io when the playbook is run
- Install Docker Python module: module allows Docker engine to have Python library and run Docker commands through Python apps
- Download and launch a docker: this task downloaded and launched the installed docker container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/twebbv89/ELK-Stack/blob/main/Images/sudo_docker_ps.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines: Private IP's of Web-1, Web-2, Web-3, and ELK VM's.
| Name     | IP Addresses |
|----------|--------------|
| Web-1    |    10.0.0.5  |
| Web-2    |    10.0.0.6  |
| Web-3    |    10.0.0.7  |  
| ELK      |    10.1.0.4  |

We have installed the following Beats on these machines:
- Microbeats

These Beats allow us to collect the following information from each machine:
- Filebeat: Collects data about the file system
- Metricbeat: Collects machine metrics, such as uptime

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to Ansible Control Node
- Navigate to the ansible directory using cd /etc/ansible
- Update the hosts file to include webservers and elk
- Run the playbooks and navigate to Kibana (http://http://52.165.144.163:5601/app/kibana) to check that the installation worked as expected.

https://docs.google.com/document/d/1LxCYJENzy2-s0IbgmAI77qsDUFCSBFgxI8foVCyFbzA/edit?usp=sharing
https://docs.google.com/document/d/1LP3hNtcGFj_oztXdBb8v9XQ985jKDMyTaHW4qK-1u44/edit?usp=sharing

