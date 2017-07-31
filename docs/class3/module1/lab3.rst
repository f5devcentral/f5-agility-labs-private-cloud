Lab 3: Making modifications to the service graph
------------------------------------------------

Playbook ``modify_parameters.yaml`` - this playbook will perform the
following task

-  Changes the load balancing method to the desired load balancing
   method (input taken from the variable file)

#. Open the variable file placed under ``/root/playbooks/variable_file.yaml``
   and change the ``lb_method`` parameter from ``round-robin`` to ``fastest-node``

   Before modification:

   |image21|

   |image22|

#. To execute the playbook run command

   ``ansible-playbook --step playbooks/ modify_parameters.yaml``

   After running the playbook for modification:

   |image23|

   |image24|

.. |image21| image:: /_static/class2/image21.png
.. |image22| image:: /_static/class2/image22.png
.. |image23| image:: /_static/class2/image23.png
.. |image24| image:: /_static/class2/image24.png