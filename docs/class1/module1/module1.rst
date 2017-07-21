Installing the F5 LBaaS Agent
=============================

Two pieces of software are required to use F5 BIG-IP with OpenStack LBaaS.

#. F5 LBaaS Driver
#. F5 OpenStack Agent.
  
The F5 LBaaS driver communicates with F5 OpenStack Agent that will then use F5 iControl REST to update the BIG-IP configuration.

.. image:: /_static/class1/f5-lbaas-overview.png
   :scale: 50% 

The following lab will first guide you through using both the OpenStack GUI/CLI.

You will then install the required software via an Ansible automation script.

.. toctree::
   :maxdepth: 1
   :glob:

   lab*