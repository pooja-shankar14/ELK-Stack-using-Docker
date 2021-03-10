## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

****Note**: The following image link needs to be updated. Replace `diagram_filename.png` with the name of your diagram image file.**  

[/Image/ELK diagram-Page-1.png](https://drive.google.com/file/d/12LEy61Ep5UTWs7x6MLt-mDWvjz5aVeW7/view?usp=sharing)
These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

 **Enter the playbook file.**
In my scenario, I have three playbook files; one for ELK server, one for filebeat and one for metricbeat. I can use all three for the recreation of the entire deployment shown above. 


This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting public access to the network.

 **What aspect of security do load balancers protect? What is the advantage of a jump box?**

With the use of load balancers, the organisation can be protected against DDoS attacks. DDoS attacks occur when a malicious entity aim to compromise the availability of an organisation. Hence, they actively bombard the organisation's servers with a flood of internet traffic. By doing this they aim to overwhelm the targeted server and crash the availability of the organisation. Load balancers are used in this scenario to handle such traffic and "balance" the incoming traffic load. With the use of load balancing rules and specified algorithms, the incoming traffic can be distributed equally and efficiently to the required servers. In addition to the configuration of Load Balancer rulesets, the VNET level protection can be enabled for the LoadBalancer network to protect DDOS options enabled to protect from any availability issues.

The use of jumphost in this architecture is to provide a gateway between the external public systems connecting to the internal trusted networks where VMs in the virtual network "poojalabnet" are running, and the incoming traffic from  the Internet can be restricted to specific systems.  It also protects the internal VMs from not exposing to the internet world and allow secure managed access from the Jumphost for IT management. This way, the virtual machines are not exposed to the public network and only known IP addresses can connect to the virtual machines via secure connection such as SSH. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the system logs through filebeat and system metrics using metricbeat.

**What does Filebeat watch for?**

Filebeat is one of the 8 beats that ELK supports which is used as a collection tool to collect data about the file system about a given machine. Once the filebeat collection suite is deployed onto a machine periodically, it can actively monitor specific log files, collecting log events across the machines send the file system data to the ELK server. This can then be displayed using the Kibana Dashboard for further analysis. 

**What does Metricbeat record?**

Metricbeat is a similar lightweight tool to Filebeat. Once deployed, Metricbeat simultaneously gathers the "metric" aspects of the system. These include CPU usage, Uptime,Memory usage as well as Inbound/Outbound traffic information. This information again can all be collected and virtually sent across to the ELK server, where Kibana is used to visualise and query the information using a GUI. 

**The configuration details of each machine may be found below.**

_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name               | Function                         | IP Address | Operating System |  
|--------------------|----------------------------------|------------|------------------|
| JumpBoxProvisioner | Gateway                          | 10.0.0.4   | Linux            |   
| Web-1              | Web server                       | 10.0.0.8   | Linux            |   
| Web-2              | Web Server                       | 10.0.0.9   | Linux            |   
| Web-3              | Web Server (redundancy purposes) | 10.0.0.7   | Linux            |   
| ELK-server         | ELK server                       | 10.1.0.4   | Linux            |   

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses.

**Add whitelisted IP addresses_**
203.x.y.z (My PC)

This whitelisted address will connect to the Jumphost machine via SSH connection on port 22. 

Machines within the network can only be accessed by the jumphost server.

**Which machine did you allow to access your ELK VM? What was its IP address?**

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible                                     | Allowed IP Addresses     |
|------------|---------------------------------------------------------|--------------------------|
| Jump Box   | Yes but only allows my local workstation IP via port 22 | 203.x.y.x          |
| Web-1      | No                                                      | 10.0.0.4                 |
| Web-2      | No                                                      | 10.0.0.4                 |
| Web-3      | No                                                      | 10.0.0.4                 |
| ELK-server | Yes but only from local workstation IP via port 5601    | 203.x.y.z (my PC)|

### Elk Configuration

**What is the main advantage of automating configuration with Ansible?**
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because of its ability to periodically and simultaneously across multiple different machines all at once. This way, the user can use Ansible software which is deployed across all these machines to automatically deploy Filebeat and Metricbeat shippers using the Ansible playbooks. These playbooks provide an efficient and fast way to gain system log and metric information from the different machines without having to download the required software (Filebeat / metricbeat) manually for each individual machine. This free and powerful program enables users to reduce time and simultaneously complete required tasks. 
The playbook implements the following tasks:

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

**Note**: The following image link needs to be updated. Replace `docker_ps_output.png` with the name of your screenshot image file.  

**Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)**

![docker_ps_output.png](https://drive.google.com/file/d/1eRteXq-owvbjsellfXQNOTmMvFKW6lvv/view?usp=sharing)



### Target Machines & Beats
This ELK server is configured to monitor the following machines:

 **List the IP addresses of the machines you are monitoring**
Web-1 10.0.0.8
Web-2 10.0.0.9
Web-3 10.0.0.7
These are the IP addresses of the machines that are linked / monitored by the ELK server

We have installed the following Beats on these machines:
**Specify which Beats you successfully installed**
Filebeat 
Metric Meat

These Beats allow us to collect the following information from each machine:
 ****In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.****

File Beat: 
- System Log files  (/var/log)
- Log events


Metric Beat: 

- CPU usage
- RAM usage
- Uptime of system 
- Inbound and outbound traffic information 


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to filebeat.yml.
- Update the filebeat.yml file to include the IP address of the ELK server for the Elastic search and Kibana arguments.  
- Run the playbook, and navigate to the installed web servers to check that the installation worked as expected.


**Which file is the playbook? Where do you copy it?**
The elk.yml file is the playbook. Within the file, there is an action to copy the config yml file (filebeat-config.yml) from control node /etc/ansible/ folder to the web server /etc/filebeat/filebeat.yml in the playbook configurations. 

**Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?**

/etc/ansible/hosts  file is what updates with the IP address of the machines 

**Which URL do you navigate to in order to check that the ELK server is running?**

http://elkserveripaddress:5601
In my example
http://13.77.181.213:5601

