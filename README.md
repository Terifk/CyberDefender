
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](Diagrams/Cloud_Security_Network_ELK_Stack.png)
These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above.
  Alternatively, select portions of the .yml files may be used to install only certain pieces such as Filebeat.
  
  - ansible.cfg
  - ansible-playbook
  - elk.yml
  - elk vm with docker.yml
  - filebeat-config.yml
  - filebeat-playbook.yml
  - hosts
  - install-elk.yml
  - metricbeat-config.yml
  - metricbeat-playbook.yml
  - web vm with docker.yml

This document contains the following details:

- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Using load balancing ensures that the application will be highly available and reliable, in addition to restricting traffic to the network.  Load Balancers provide security functions such as to help prevent DoS and DDoS attacks and overload of servers by, also, using a "health probe" to detect faulty servers and redistributes to those functioning properly.


Using a Jump Box has an advantage of increasing security. Before connecting to other servers it provides the only gateway for access ("fanning in") to network infrastructure reducing size of the potential attack surface.

The Jump Box is a "hardened," secure server to be used only for system administrator's tasks thus minimizing threat penetrations into the walls of a private network to withstand malicious attacks or threats.

Creates a "bridge" between users and private network tightening access to resources, instances, and gateways.  

Allows users to access these instances in private subnets via ssh or rdp. Provides a dedicated ssh access point for aggregated audit log of all the ssh connections. 

Integrating an ELK server with Beats allows users to easily monitor the vulnerable VMs for changes to the data metrics such as CPU usuage and system logs.  Filebeat watches for log files and Metricbeat records metrics and statistics from the operating system. 

The configuration details of each machine may be found below:

|           				  				Name 			         |         				  				Function 			       |  				  				Public/Private  				  				 IP Address 			 |    				  				Operating System 			   |
|:-------------------------:|:-------------------------:|:---------------------------------:|:------------------------:|
|  				 Jump-Box-Provisioner 			   |  				 Gateway 			                |  				 20.120.87.181/10.0.0.4 			         |  				 Linux 				Ubuntu (64 bit) 			 |
|  				 Web-1 			                  |  				 Web 				Server for DVWA 			    |  				 13.82.220.149/10.0.0.7 			         |  				 Linux 				Ubuntu (64 bit) 			 |
|  				 Web-2 			                  |  				 Web 				Server for DVWA 			    |  				 52.188.210.98/10.0.0.8 			         |  				 Linux 				Ubuntu (64 bit) 			 |
|  				 ELK 			                    |  				 Monitoring 				Web Servers 			 |  				 20.109.173.55/10.1.0.4 				 				 			       |  				 Linux 				Ubuntu (64 bit) 			 |
|  				 Load 				Balancer 			          |  				 Balancing 				Server Load 			  |  				      				         /Static 				Internal 			 |  				 Linux 				Ubuntu (64 bit) 			 |
|  				 Local 				Host-Workstation 			 |  				 Access 				Control 			         |  				 73.15.233.190/ 			                 |  				 Linux 				Ubuntu (64 bit) 			 |







             

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: Local Host-Workstation 73.15.233.190.

Machines within the network can only be accessed by ELK VM or Jump-Box-Provisioner.
ELK VM can be accessed by Local Host-Workstation.
      Its IP address: 73.15.233.190

A summary of the access policies in place can be found in the table below.

|      				  				Name 			     |  				  				Publicly 				Accessible 			 |       				  				Allowed IP 				Addresses 			      |
|:----------------:|:-----------------------:|:----------------------------------:|
|  				 Jump 				Box 			      |  				  				Yes 			                 |  				 Local 				Host Public IP on SSH 22 			  |
|  				 Web-1 			         |  				  				No 			                  |  				 10.0.0.4 				on SSH 22 			              |
|  				 Web-2 			         |  				  				No 			                  |  				 10.0.0.4 				on SSH 22 			              |
|  				 ELK 				Server 			    |  				  				Yes 			                 |  				 Local 				Host Public IP TCP 5601 			   |
|  				 Load 				balancer 			 |  				  				No 			                  |  				 Local 				Host Public IP on HTTP 80 			 |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine.  No configuration was performed manually, which is advantageous because the configuration can automatically be replicated for additional machines.
The main advantage of automating configuration with Ansible is it easily facilitates maintenance and the many monthly updates for ELK by using one file to update changes to all machines in the network.
 
The playbook implements the following tasks:

 - Install docker.io

 - Install python3-pip

 - Install Python Docker Module

 - Increase virtual memory

 - Download and launch docker web container: cyberxsecurity/dvwa

 - Enable docker service

The following screenshot displays the result successfully configuring the ELK instance with all machines, Network Security Groups, and Load Balancer. 

![image](https://user-images.githubusercontent.com/95733311/151053447-ee28f4e0-e6e4-456e-b76f-a219ff37d01f.png)





### Target Machines & Beats
This ELK server is configured to monitor the following machines:

|  				  				Name 			 |         				  				Function 			       |  				  				Public/Private  				  				 IP Address 			 |
|:--------:|:-------------------------:|:---------------------------------:|
|  				 ELK 			   |  				 Monitoring 				Web Servers 			 |  				 20.109.173.55/10.1.0.4 				 				 			       |
|  				 Web-1 			 |  				 Web 				Server for DVWA 			    |  				 13.82.220.149/10.0.0.7 			         |
|  				 Web-2 			 |  				 Web 				Server for DVWA 			    |  				 52.188.210.98/10.0.0.8 			         |


The following Beats have been installed on the ELK server:  Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:

Filebeat: watches for log files, tailing them, and ships the data to either Logstash for more advanced processing or directly into Elasticsearch for indexing. 

Metricbeat: records metrics and statistics from the operating system and from services running on the server then ships them to output of Elasticsearch or Logstash. 









### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured.

Assuming you have such a control node provisioned:

 - SSH into the control node and follow the steps below:

 - Copy roles files to /etc/ansible/roles. 

 - Update hosts file to include webserver IP's and ELKServer IP 

 - Check the installation works as intended.

 - Run the playbook navigating to:

 - HTTP://<ELKServer_Public_IP>:5601

 - Copy roles folder (linked above) and files to /etc/ansible/roles 

 - Update /etc/ansible/hosts file to include the webservers IP's and ELKServer IP 

 - Update configuration files with ELKServer IP 

 - Kibana: replace localhost with local IP for ELK Server 

 - Elasticsearch: replace localhost with local IP for ELK Server

             curl -XGET elasticsearch_ip_or_hostname:9200/
             
![image](https://user-images.githubusercontent.com/95733311/151057610-50c7e919-8b73-4259-a699-b6dc6b6f5e8d.png)

![Screenshot (263)](https://user-images.githubusercontent.com/95733311/151084226-8f8d64de-6594-42d0-be2c-357cc9d2c6c0.png)



|**LINUX COMMANDS                                  |USED TO**                                |
|-------------------------------------------------|-------------------------------------------|
| sudo apt-get update                             | Update Packages                           |
| sudo apt install docker.io                      | Install Docker Application                |
| sudo service docker start                       | Start Docker Application                  |
| systemctl status docker                         | Give Status Docker Application            |
| sudo docker pull cyberxsecurity/ansible         | Pull-Download Docker File                 |
| sudo docker run -ti cyberxsecurity/ansible bash | Run Docker Container                      |
| sudo docker start                               | Start Docker Container                    |
| sudo docker container list -a                   | List All Docker Containers                |
| sudo docker ps -a                               | List Process Status All Docker Containers |
| sudo docker attach                              | View or Control Docker Container          |
| ssh-keygen                                      | Generate Key-Client/Server Authentication |
| ansible -m ping all                             | Check Connected-Able Run Containers       |

 

**Click my name to see LinkedIn CV:**
<div class="badge-base LI-profile-badge" data-locale="en_US" data-size="medium" data-theme="dark" data-type="VERTICAL" data-vanity="allan-anthony-91235924" data-version="v1"><a class="badge-base__link LI-simple-link" href="https://www.linkedin.com/in/allan-anthony-91235924?trk=profile-badge">Allan Anthony</a></div>
              

