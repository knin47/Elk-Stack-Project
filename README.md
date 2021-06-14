# Project-1
Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/knin47/Project-1/blob/main/Diagrams/Network-Diagram-RedGroup.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

Ansible/filebeat-playbook.yml
Ansible/install-elk.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, in addition to restricting traffic to the network.

What aspect of security do load balancers protect?<br>
It helps maximize performance by delegating traffic so the servers are not overloaded which can also prevent DDOS attacks.

What is the advantage of a jump box?<br>
Advantages of a jump box would be that it is highly secure and mainly used for admin tasks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.

What does Filebeat watch for?<br>
It watches the log files that you specify along with any changes to them and forwards them to Elasticsearch or Logstash for indexing.

What does Metricbeat record?<br>
It records the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

<!DOCTYPE html>
<html>
   </head>

   <body>
      <img src="https://github.com/knin47/Project-1/blob/main/Images/Configuration%20Detail.png?raw=true" alt="Simply Easy Learning" width="80000" height="300">
   </body>
</html>


Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:<br>
My Home Public IP Address

Machines within the network can only be accessed by Jump-Box-Provisioner.<br>
Which machine did you allow to access your ELK VM?<br>
Jump-Box-Provisioner

What was its IP address?<br>
10.0.0.9

A summary of the access policies in place can be found in the table below.

<!DOCTYPE html>
<html>
   </head>

   <body>
      <img src="https://github.com/knin47/Project-1/blob/main/Images/Access%20Policies.png?raw=true" alt="Simply Easy Learning" width="80000" height="300">
   </body>
</html>

Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...<br>
What is the main advantage of automating configuration with Ansible?<br>
The playbooks can be deployed to any or all VM’s without having to do each 1 by 1.

The playbook implements the following tasks:<br>
In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.<br>
Install docker.io (This installs docker.io)<br>
Install python3-pip (This installs python3-pip)<br>
Install Docker module (This installs docker)<br>
Download and launch a docker elk container<br>
Enable docker service

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

<!DOCTYPE html>
<html>
   </head>

   <body>
      <img src="https://github.com/knin47/Project-1/blob/main/Images/docker%20ps%20for%20elk.png?raw=true" alt="Simply Easy Learning" width="80000" height="300">
   </body>
</html>

Target Machines & Beats<br>
This ELK server is configured to monitor the following machines:<br>
Web-1 (10.0.0.7)<br>
Web-2 (10.0.0.8)

We have installed the following Beats on these machines:<br>
Filebeat <br>
Metricbeat

These Beats allow us to collect the following information from each machine:<br>
Filebeat collects log files of changes from specified machines and forwards them to Elasticsearch or Logstash for indexing.<br>
Metricbeat collects metrics and statistics from specified machines and forwards them to Elasticsearch or Logstash.

Using the Playbook<br>
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:<br>
Copy the install-elk.yml file to /etc/ansible/<br>
Update the hosts file to include

[elk]<br>
10.1.0.4 ansible_python_interpreter=/usr/bin/python3

Run the playbook, and navigate to http://20.98.215.246:5601/app/kibana#/home to check that the installation worked as expected.<br>

Answer the following questions to fill in the blanks:<br>
Which file is the playbook?<br>
install-elk.yml

Where do you copy it?<br>
/etc/ansible/

Which file do you update to make Ansible run the playbook on a specific machine?<br>
/etc/ansible/hosts

How do I specify which machine to install the ELK server on versus which to install Filebeat on?<br>
In the /etc/ansible/host file, there is a section where you can put your VM’s with a specific group.<br>
Under [webservers] will list Web-1 and Web-2 along with their IP Address and under [elk] will list ELK-1 along with its IP Address.<br>
In the playbook you just need to specify webservers for Filebeat or elk for ELK server.

Which URL do you navigate to in order to check that the ELK server is running?<br>
http://20.98.215.246:5601/app/kibana#/home

As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

touch /etc/ansible/install-elk.yml

nano /etc/ansible/install-elk.yml

---<br>
- name: Configure Elk VM with Docker<br>
  hosts: elk<br>
  remote_user: RedAdmin<br>
  become: true<br>
  tasks:<br>
  - name: Install docker.io<br>
    apt:<br>
      force_apt_get: yes<br>
      update_cache: yes<br>
      name: docker.io<br>
      state: present

  - name: Install python3-pip<br>
    apt:<br>
      force_apt_get: yes<br>
      name: python3-pip<br>
      state: present
 
  - name: Install Docker module<br>
    pip:<br>
      name: docker<br>
      state: present
 
  - name: Increase virtual memory<br>
    command: sysctl -w vm.max_map_count=262144

  - name: use more memory<br>
    sysctl:<br>
      name: vm.max_map_count<br>
      value: '262144'<br>
      state: present<br>
      reload: yes

  - name: download and launch a docker elk container<br>
    docker_container:<br>
      name: elk<br>
      image: sebp/elk:761<br>
      state: started<br>
      restart_policy: always<br>
      published_ports:<br>
        - 5601:5601<br>
        - 9200:9200<br>
        - 5044:5044
 
  - name: Enable docker service<br>
    systemd:<br>
      name: docker<br>
      enabled: yes

Ctrl + x  to save and exit

Ansible-playbook /etc/ansible/install-elk.yml

