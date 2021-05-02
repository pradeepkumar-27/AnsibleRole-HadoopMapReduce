HadoopMapReduce
===============

Ansible role to configure Hadoop MapReduce cluster

Requirements
------------

I have created this role to configure my MapReduce cluster on top of Amazom Web Services (AWS), hence I have created my custom Elastic Cloud Compute (EC2) Amazon Machine Image (AMI) on top of Amazon Linux 2 with JDK 8u171 and Hadoop 1.2.1 pre-installed. You can find my custom AMI ID "ami-01bb2347b233b5110" on AWS.

Role Variables
--------------

This role has a default variable "mr_port" which represents the port number on which the MapReduce program is run.


ansibele.cfg
------------

Ansible configuration file to run this role

    [defaults]
    interpreter_python=auto_silent
    inventory      = ./hosts
    roles_path    = ./yourRolesPath (i.e the path where you have downloaded this role)
    host_key_checking = False
    remote_user = ec2-user
    private_key_file = ./yourKey.pem
    
    [privilege_escalation]
    become=True
    become_method=sudo
    become_user=root
    become_ask_pass=False

hosts
------------

Ansible inventory file where you have to put the IP of the servers. Since I'm setting up the MapReduce cluster, I have previously configured my JobTracker with HDFS cluster for connectivity. Here I'm configuring the client to be able to perform jobs using the MapReduce cluster


    [JobTracker]
    jobtracker
    
    [TaskTrackers]
    tasktracker1
    tasktracker2
    tasktracker3
    
    [Client]
    client
    
    [MR:children]
    Client
    JobTracker
    TaskTrackers
    

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: MR
      roles:
         - HadoopMapReduce
           vars:
             mr_port: 7002
