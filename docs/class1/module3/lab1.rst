Deploy Enhanced L4-L7 via ESD
-----------------------------

In addition to supporting LBaaS v2 capabilities, the F5 OpenStack LBaaS
integration can support Enhanced Service Definitions to expose F5
specific capabilities. The following exercise will modify the TCP
profiles that we created on our first listener.

First take a look at the existing TCP configuration on the BIG-IP.
Observe that it is using the default TCP profile.

    |image34|

From your Putty window run.

.. code-block:: console

  neutron lbaas-l7policy-create --listener listener1 --name esd_demo_3 --action REJECT

You should see the following output.

    |image35|

Refresh your window on the BIG-IP and you will see that the TCP profile
has changed.

    |image36|

Now from your Putty window run.

.. code-block:: console

  cat /etc/neutron/services/f5/esd/demo.json

You will see the definition that we referenced earlier.

    |image37|

In addition to TCP profiles you can also add iRules, Local Traffic
Policies, client/server SSL profiles, and modify session persistence.

.. |image34| image:: /_static/image36.png
  :scale: 50%
.. |image35| image:: /_static/image37.png
.. |image36| image:: /_static/image38.png
.. |image37| image:: /_static/image39.png
