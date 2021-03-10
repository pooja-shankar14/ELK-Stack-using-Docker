ELK Stack Project Process

In the network diagram, it depicts the current ELK stack deployment that I performed for the bootcamp. Firstly, the approach as per the network topology  is to allow my local workstation to Azure cloud with two setup methods ie; one being for secure management of azure cloud instances and other for allowing my workstation to connect to Elastic search via the Internet. Here are the following steps I have taken to successfully deploy ELK Stack: 

	•	To start with, I have created my personal Azure account with personal credit card to join the Azure subscription.  

	•	Created Azure resource group 

	•	Created two Vnets; one for ELK network and one for the Jumphost + Web servers. 

	•	Created Virtual machines (Linux) for Jumphost, plus three Web servers using Ubuntu 18.x version, with public IP for jumphost 	 only. 

	•	Created the above virtual machines instances to allow authentication only for public key and private key method. Two of the 	web servers (Web-1 and Web-2) being in one availability set and another web server (web-3) placed in a different availability 	  set for redundancy purposes. 

	•	After this, created an Azure Load Balancer with a Health probe, Load balancer rule and placed two servers in the backend pool  	   from the two different availability sets

	•	Created a Network Security Group for subnet plus also virtual machine level network security groups. 

	•	Started the Jumphost instance plus all the web servers in the Cloud, and ensured the connectivity from my local workstation to 	   the jumphost via SSH public private key authentication. 

	•	After successful connection, I have installed docker.io container within the Jumphost and downloaded Ansible Container for further software package rollouts. 

	•	Once Ansible container has been installed, started the Docker services and attached to the local Ansible container. Using the Ansible configuration file and host file, I added the IP address of the web servers in these files. 

	•	Once the connection was successful to all three web servers after test: (docker -m ping all) from Ansible container, a playbook yml file was created with actions to deploy packages such as;  python, pip3 and DVWA web server image. 

	•	The ansible playbook command executed from the Ansible container (Jumphost) to setup the necessary packages across three web servers using : ansible-playbook pentest.yml . This installed the necessary packages, successfully deployed and started web services. 

	•	Configured Network security group to allow port 80 from my local internet public IP to the load balancer public IP on port 80. Also created rules from load balancer to the web servers virtual machines on port 80. Tested from my workstation and was able to access DVWA web server pages.

	•	For the ELK, I created a new virtual machine with 2 CPU and 8 GB RAM and default storage settings. This machine was in a different region to the Web servers (West US 2) and seperate virtual network (ELK-net). Also a network security group was created for the ELK server to address ingress and egress traffic flow. 

	•	The ELK VNet and the management VNet (poojalabnet) was peered using the VNet peering to establish bidirectional access. Created network security rule on each VNet network security group to allow port 22 and port 80. 

	•	Then the connection was established to the Jumphost Ansible container and added ELK server IP address in the Ansible configuration and host files. From here, did a Ansible ping test to check whether the connectivity from the Ansible container to the ELK server is successful. 

	•	Once this was established, connected to the ELK server and downloaded the Docker and ELK Server stack package including pip3, python and other libraries. 

	•	After successful deployment of the Ansible playbook to the ELK server with all the necessary packages installed, connected to the ELK server to verify these. 

	•	Configured firewall rule on the ELK VNET security group to allow access from my workstation public IP to destination ELK server on port TCP 5601. 

	•	Created a new elk.yml playbook within the Ansible container to deploy the services; filebeat and metric beat. Copied the pre-configured Ansible configuration file for both filebeat and metric beat setup requirements. 

	•	Executed the elk.yml playbook to install filebeat and metric beat for three web servers with pre-configured yml setup to allow traffic to the ELK server. 

	•	Connected to the ELK server using ELK server public IP using the port 5601. Verified the events with the system log and metric beat log validation check. System Logs and docker metric information were successfully received from all three web servers

All public and private IP information can be seen from the network diagram
