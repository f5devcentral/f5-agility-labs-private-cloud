Install Driver/Agent
--------------------

Complete directions for installing the Driver/Agent can be found at: http://f5-openstack-lbaasv2-driver.readthedocs.io/en/mitaka/map_quick-start-guide.html

During this lab we will be using Ansible (a Systems/Network automation tool) to automate the installation.  The Ansible module that is being used in this lab can be found at: https://github.com/f5devcentral/f5-openstack-ansible

Install via Ansible
~~~~~~~~~~~~~~~~~~~

Open your Putty Window (Directions in :ref:`verify_openstack` if you closed the Window).

Change your directory by typing ``cd f5-openstack-ansible/playbooks``

.. code-block:: console

  [student@openstack ~]$cd f5-openstack-ansible/playbooks/

Now run 

.. code-block:: console

  ansible-playbook -i hosts --extra-vars '{"remote_user":"student"}' agent_driver_deploy.yaml

You should see.

|image18|

Change back to your home directory by typing ``cd``.

Now type ``source keystonerc_admin`` and you should see a prompt that looks
like:

.. code-block:: console

    [student@openstack ~(keystone_admin)]$

Expand the window to full screen and type. ``neutron agent-list``

|image19|

There should be a table that contains the following information.

+------------------------+---------+--------------------+----------------------+
| agent\_type            | alive   | admin\_state\_up   | binary               |
+========================+=========+====================+======================+
| Loadbalancerv2 agent   | :-)     | True               | f5-oslbaasv2-agent   |
+------------------------+---------+--------------------+----------------------+

Now type ``source keystonerc_demo`` to restore your prompt to the demo user.

.. code-block:: console

    [student@openstack ~(keystone_demo)]$


.. |image18| image:: /_static/image20.png
  :scale: 50%
.. |image19| image:: /_static/image21.png
  :scale: 50%
