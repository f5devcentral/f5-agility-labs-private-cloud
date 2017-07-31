Lab 4: Deleting the service
---------------------------

Playbook ``aci_delete_service.yaml`` - this playbook will perform the
following tasks

-  Detach the service graph from the contract

   -  This will delete the partition created on the BIG-IP (thus
      deleting all the objects that belong to that partition)

-  Delete the device selection policy

-  Delete the BIG-IP parameters which are present under the provider End
   Point Group (EPG). Remove the provided as well as consumed contracts
   from the EPG’s

-  Delete the service graph template

-  Delete the contract

-  Delete the logical device cluster

   -  This will delete the device group that is created on the BIG-IP

-  Delete the device manager from tenant common

-  Delete the device manager type under L4-L7 Services

#. To execute the playbook run command

   ``ansible-playbook --step playbooks/ aci_delete_service.yaml``

   After execution of this playbook, the BIG-IP will be in a clean state.
   There will be no partition on the BIG-IP pertaining to the service graph
   and there will be no device group pertaining to the logical device
   cluster

   |image25|

   |image26|

.. |image25| image:: /_static/class2/image25.png   
.. |image26| image:: /_static/class2/image26.png
