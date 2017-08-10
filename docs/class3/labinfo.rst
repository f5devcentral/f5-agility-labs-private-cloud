Lab Topology
------------

Install Ansible
~~~~~~~~~~~~~~~~~
-  On dCloud once logged into RDP, open Putty and go to server ‘Tools’ (root/C1sco12345). Run the following commands to install Ansible

   -	``pip install --upgrade pip``
   -	``yum install openssl-devel``
   -	``yum install python-devel``
   -	``yum install gcc``
   -	``pip install cryptography``
   -	``pip install ansible``

-  Once ansible is installed successfully, run following command from /root directory

   -  ``export ANSIBLE_LIBRARY=/root/library``

Environment setup
~~~~~~~~~~~~~~~~~

-  Download ``ansible_automation_files.tar`` from ``https://tinyurl.com/y9zvj6nl`` to desktop

-  Open WinSCP, click on with windows startup button and then click
   WinSCP

   |image1|

-  On WinSCP

   -  Hostname: ``tools.dcloud.cisco.com``

   -  Port: ``22``

   -  Click on the EDIT button to change username and password

      -  Username: ``root``

      -  Password: ``C1sco12345``

   -  Click Save

   -  Click login

   -  In the right hand pane click on the ``/home/user01/Scripts`` tab,
      change it to ``/root``

      |image3|

   -  Click OK

   -  Similarly change the left hand pane from ``C:\Scripts`` to
      ``C:\Users\demouser\Desktop``

   -  Copy the download tar file from the desktop to the root directory
      on the ansible host

-  SSH to the ‘Tools’ host using Putty

   -  Username: ``root``

   -  Password: ``C1sco12345``

   -  Untar the ``ansible_automation_files.tar`` file using command:

      ``tar xvf ansible_automation_files.tar``

Directory structure
~~~~~~~~~~~~~~~~~~~

All the files and folders are under /root directory itself. Let’s take a
look at the files and directories. This is for reading and familiarizing
yourself with the playbooks and files we are going to use. No task to be
performed in this section

|image4|

-  File ``ansible.cfg``

   -  Ansible configuration file where you can set ansible environment
      variables, for more information refer to link
      http://docs.ansible.com/ansible/intro_configuration.html

-  File ``host_file``

   -  This file is the ansible inventory file, which stored information
      about the host(s) that we want to run the playbook against, and
      variable information pertaining to those hosts. For more
      information about the inventory file refer to link
      http://docs.ansible.com/ansible/intro_inventory.html#inventory

   -  The host file is specific to your environment

   -  Sample ``host_file`` for the dCloud environment

      .. code-block:: ini

         [iworkflow]
         198.18.128.135

         [iworkflow:vars]
         username=admin
         password=C1sco12345

         [apic]
         198.18.133.200

         [apic:vars]
         username=admin
         password=C1sco12345

-  Directory ``playbooks`` – This directory contains

   -  All the playbooks we are going to run in this lab

      -  ``iworkflow_setup.yaml`` – Configure setting on iWorkflow

      -  ``aci_tenant_setup.yaml`` – Create a tenant and related
         parameters on APIC

      -  ``logical_device_cluster.yaml`` – Create a logical device cluster
         on APIC (this enabled communication of APIC with BIG-IP)

      -  ``service_insertion.yaml`` - Configure service insertion on APIC

      -  ``aci_delete_service.yaml`` – Clean up of the configuration done
         on APIC

   -  The variable file which we are going to edit to customize it to
      our needs

      -  This is a sample input to the variable file, you can modify it
         to fit your environment

         +------------------------------------+----------------------------------------+
         | bigip\_ip                          | ``198.18.128.130``                     |
         +------------------------------------+----------------------------------------+
         | bigip\_username                    | ``admin``                              |
         +------------------------------------+----------------------------------------+
         | bigip\_password                    | ``C1sco12345``                         |
         +------------------------------------+----------------------------------------+
         | bigip\_hostname                    | ``bigip1.dcloud.cisco.com``            |
         +------------------------------------+----------------------------------------+
         |                                    |                                        |
         +------------------------------------+----------------------------------------+
         | iworkflow\_ip                      | ``198.18.128.135``                     |
         +------------------------------------+----------------------------------------+
         | iworkflow\_username                | ``admin``                              |
         +------------------------------------+----------------------------------------+
         | iworkflow\_password                | ``C1sco12345``                         |
         +------------------------------------+----------------------------------------+
         |                                    |                                        |
         +------------------------------------+----------------------------------------+
         | tenant\_name                       | ``Demo``                               |
         +------------------------------------+----------------------------------------+
         | context\_name                      | ``{{tenant_name}}_ctx1``               |
         +------------------------------------+----------------------------------------+
         | app\_profile\_name                 | ``App_profile``                        |
         +------------------------------------+----------------------------------------+
         | provider\_bd\_name                 | ``{{tenant_name}}_BDApp``              |
         +------------------------------------+----------------------------------------+
         | provider\_ip                       | ``192.168.10.220``                     |
         +------------------------------------+----------------------------------------+
         | provider\_mask                     | ``24``                                 |
         +------------------------------------+----------------------------------------+
         | provider\_epg\_name                | ``prov_EPG_app``                       |
         +------------------------------------+----------------------------------------+
         | consumer\_bd\_name                 | ``{{tenant_name}}_BDWeb``              |
         +------------------------------------+----------------------------------------+
         | consumer\_ip                       | ``10.10.10.220``                       |
         +------------------------------------+----------------------------------------+
         | consumer\_mask                     | ``24``                                 |
         +------------------------------------+----------------------------------------+
         | consumer\_epg\_name                | ``cons_EPG_web``                       |
         +------------------------------------+----------------------------------------+
         |                                    |                                        |
         +------------------------------------+----------------------------------------+
         | contract\_name                     | ``web2app-demo-contract``              |
         +------------------------------------+----------------------------------------+
         | filter\_name                       | ``{{contract_name}}_filter``           |
         +------------------------------------+----------------------------------------+
         | subject\_name1                     | ``http``                               |
         +------------------------------------+----------------------------------------+
         | subject\_name2                     | ``https``                              |
         +------------------------------------+----------------------------------------+
         |                                    |                                        |
         +------------------------------------+----------------------------------------+
         | iworkflow\_servicetemplate\_name   | ``SimpleHTTP``                         |
         +------------------------------------+----------------------------------------+
         | devicePackage\_name                | ``dCloudConnector``                    |
         +------------------------------------+----------------------------------------+
         | downloaded\_devicePackage\_name    | ``F5DevicePackageSimple``              |
         +------------------------------------+----------------------------------------+
         | logicalDeviceCluster\_name         | ``StandaloneBIGIP``                    |
         +------------------------------------+----------------------------------------+
         | SGtemplate\_name                   | ``SimpleHTTP_ServiceGraphTemplate``    |
         +------------------------------------+----------------------------------------+
         |                                    |                                        |
         +------------------------------------+----------------------------------------+
         | external\_selfip                   | ``10.10.10.120``                       |
         +------------------------------------+----------------------------------------+
         | external\_netmask                  | ``255.255.255.0``                      |
         +------------------------------------+----------------------------------------+
         | internal\_selfip                   | ``192.168.10.120``                     |
         +------------------------------------+----------------------------------------+
         | internal\_netmask                  | ``255.255.255.0``                      |
         +------------------------------------+----------------------------------------+
         | vip\_ip                            | ``10.10.10.100``                       |
         +------------------------------------+----------------------------------------+
         | vip\_port                          | ``80``                                 |
         +------------------------------------+----------------------------------------+
         | poolMember\_ip                     | ``192.168.10.140``                     |
         +------------------------------------+----------------------------------------+
         | lb\_method                         | ``round-robin``                        |
         +------------------------------------+----------------------------------------+

-  Directory ``aci_posts``

   -  This directory has all the aci posts we are going to execute on
      the APIC

   -  Each post is a j2 (jinja2) template file. This template file
      contains variables which are going to be substituted at run time
      from information present in the variable file. The XML file then
      created after the substitution will be then run on the APIC

-  JSON blob for creating a service template on iWorkflow

-  Directory ``library``

   -  This contains the python files which are responsible for running
      code for modules. For this lab we have the one aci module
      ``aci_rest.py`` which will be used to run the posts on the APIC

.. |image1| image:: /_static/class3/image1.png
   :scale: 50%
.. |image3| image:: /_static/class3/image3.png
   :scale: 50%
.. |image4| image:: /_static/class3/image4.png
   :scale: 50%

