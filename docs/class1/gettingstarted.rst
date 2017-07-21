Getting Started
---------------

During this lab you will learn how to:

* Install the F5 LBaaS Agent using Ansible
* Deploy Basic L4-L7 services using LBaaS
* Deploy enhanced L4-L7 services using ESD.

About OpenStack
~~~~~~~~~~~~~~~

OpenStack provides an Open Source Infrastructure As-A Service (IaaS)
solution that provides a framework for provisioning Network, Compute,
and Storage in an automated and repeatable manner.

About LBaaS
~~~~~~~~~~~

Load Balancing As-A Service (LBaaS) is a community standard around
providing Load Balancing as a standardized service within OpenStack. The
current version, LBaaS v2, provides basic L4-L7 capabilities.

About F5 & OpenStack
~~~~~~~~~~~~~~~~~~~~

F5 can be deployed in two ways in an OpenStack environment. The two
methods are a **undercloud** or **overcloud** deployment. It is possible to use
one or both of these methodologies when deploying F5 & OpenStack.

Undercloud: LBaaS
^^^^^^^^^^^^^^^^^

Undercloud commonly refers to a deployment where the BIG-IP device
(physical or virtual) is outside of the OpenStack environment. Typically
this is done with physical hardware to provide a multi-tenant
environment and used with LBaaS.

Overcloud: HEAT
^^^^^^^^^^^^^^^

Overcloud refers to a deployment where a BIG-IP Virtual Edition (VE) is
provisioned within a tenant network as a virtual machine within
OpenStack Nova. In this scenario the BIG-IP is in a similar topology to
other tenant virtual machines. When deploying in overcloud OpenStack
HEAT templates (automation templates) are commonly used to deploy the
BIG-IP device. A customer can manage the BIG-IP device through
traditional methods, HEAT templates, and/or other automation templates.

It is also possible to deploy a BIG-IP VE in an overcloud deployment and
use LBaaS. In this deployment you are limited by the number of
interfaces currently supported on BIG-IP VE can use (9 data & 1 mgmt).

Under or Over?
~~~~~~~~~~~~~~

The decision to use one method or both will depend on customer
requirements. An undercloud deployment using LBaaS is well suited to
providing basic services that can be provided in a multi-tenant manner.
Overcloud is well suited to providing access to features and functions
that may not be exposed via LBaaS or provide per-tenant services.
