Lab Topology
============

The current Lab Environment looks like the following:

|image0|

You will be connecting via RDP to a Windows host to perform all the
steps in this lab.

Lab Components
--------------

The following table lists VLANS, IP Addresses and Credentials for all
components:

.. list-table::
    :widths: 20 40 40
    :header-rows: 1

    * - **Component**
      - **VLAN/IP Address(es)**
      - **Credentials**
    * - Windows RDP Host
      - - 10.0.10.50
      - ``student``/[Viewable in Ravello]
    * - OpenStack
      - - 10.0.10.10
      - ``student`` / [SSH Key]
    * - BIG-IP
      - - 10.0.10.20
      - ``admin`` / ``admin``

Connecting to the Lab Environment
---------------------------------


   
Please follow the instructions provided by the instructor to start your
lab and access your jump host by clicking on this "rdp" host link.

.. image:: /_static/class1/rdp-link-student.png
   :scale: 50%

.. NOTE:: All work for this lab will be performed exclusively from the Windows
   jumphost. No installation or interaction with your local system is
   required.

.. |image0| image:: /_static/class1/image2.png
   :scale: 50%
