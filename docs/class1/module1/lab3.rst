Install Driver/Agent
--------------------

Review online documentation

Install via Ansible

Open your Putty Window (Directions in Lab 1.1 if you closed the Window).

Change your directory by typing ``cd f5-openstack-ansible/playbooks``

.. code-block:: console

[student@openstack ~]$cd f5-openstack-ansible/playbooks/

Now run ``ansible-playbook -i hosts --extra-vars '{"remote\_user":"student"}' agent\_driver\_deploy.yaml``

|image18|

Change back to your home directory by typing ``cd``.

Now type ``. keystonerc\_admin`` and you should see a prompt that looks
like:

.. code-block:: console

    [student@openstack ~(keystone\_admin)]$

Expand the window to full screen and type. ``neutron agent-list``

|image19|

There should be a table that contains the following information.

+------------------------+---------+--------------------+----------------------+
| agent\_type            | alive   | admin\_state\_up   | binary               |
+========================+=========+====================+======================+
| Loadbalancerv2 agent   | :-)     | True               | f5-oslbaasv2-agent   |
+------------------------+---------+--------------------+----------------------+

.. |image18| image:: /_static/image20.png
.. |image19| image:: /_static/image21.png
