.. _verify_openstack:

Login to OpenStack CLI
----------------------



Verify OpenStack environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The first exercise is to use the OpenStack CLI to verify the environment.

First Launch Putty from the Desktop.

    |image1|

Click on “OpenStack All-In-One”, select load, then click “Open”.

    |image2|

If you fail to connect on the first try, try again. When you connect you
should see.

    |image3|

Type ``source keystonerc_demo``. The prompt should
change from:

.. code-block:: console

    [student@openstack ~]$

To:

.. code-block:: console

    [student@openstack ~(keystone_demo)]$

Run ``neutron subnet-list`` and you should see

    |image4|

Please ask for assistance if you do not see the correct output. Leave
this window open, it will be used throughout the lab.

.. |image1| image:: /_static/image3.png
  :scale: 50%
.. |image2| image:: /_static/image4.png
  :scale: 50%
.. |image3| image:: /_static/image5.png
  :scale: 50%
.. |image4| image:: /_static/image6.png
  :scale: 50%
