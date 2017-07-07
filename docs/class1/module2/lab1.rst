Lab 1.4: Deploy L4-L7 via Horizon
---------------------------------

The F5 LBaaS integration will configure the Networking on the BIG-IP to
connect to the OpenStack network. From Chrome click on the “BIG-IP”
bookmark and login with the credentials “admin / admin”. Observe that
there is only a single partition “Common”.

Also note only one self-ip in Route Domain 0.

    |image20|

From Chrome do a forced refresh of the Horizon page (Shift+[Reload]).

You should now see a new menu item under “Network”.

    |image21|

If you do not see “Load Balancers” verify that the Loadbalancer Agent is
running from the previous lab. Click on the “Load Balancers” menu item,
then click on “+Create Load Balancer”.

    |image22|

Complete the following information.

Load Balancer Details

+----------+-------------------+
| name     | value             |
+==========+===================+
| Name     | lb1               |
+----------+-------------------+
| Subnet   | internal-subnet   |
+----------+-------------------+
|          |                   |
+----------+-------------------+

Listener Details

+------------+-------------+
| name       | value       |
+============+=============+
| Name       | listener1   |
+------------+-------------+
| Protocol   | HTTP        |
+------------+-------------+
| Port       | 80          |
+------------+-------------+

Pool Details

+----------+----------------+
| name     | value          |
+==========+================+
| Name     | pool1          |
+----------+----------------+
| Method   | ROUND\_ROBIN   |
+----------+----------------+
|          |                |
+----------+----------------+

Pool Members

+------------+--------+
| name       | port   |
+============+========+
| server-1   | 80     |
+------------+--------+
| server-2   | 80     |
+------------+--------+
|            |        |
+------------+--------+

Monitor type

+----------------+---------+
| name           | value   |
+================+=========+
| Monitor type   | HTTP    |
+----------------+---------+

Then click on “Create Load Balancer”

    |image23|

On the BIG-IP take a look at the Partition. You should see that a new
partition was created.

    |image24|

Change to that partition and inspect the Self IPs items under Network.
You should see a VXLAN tunnel that was created to connect to the tenant
network.

    |image25|

Under Network Map you will see the entries that were created by LBaaS
via the Horizon Panel.

    |image26|

Observe that the Pool name uses the OpenStack Pool ID (yours will differ
in value from the example).

    |image27|

To test this configuration we will need to add a Floating IP to be able
to access the Tenant Subnet externally. On the main “Load Balancers”
page, click on the downward arrow next to “Edit” and select “Associate
Floating IP”

    |image28|

Specify the “public” pool.

    |image29|

And click “Associate”. Click on “lb1” and you will see the Floating IP
Address.

    |image30|

Enter this value into the Chrome URL and you should see (colors may
vary, there’s a chance they may be the same).

    |image31|

Adding “/simple.shtml” you can see the Server IP and see the service
being load balanced.

+-------------+-------------+
| |image32|   | |image33|   |
+=============+=============+
+-------------+-------------+

.. |image20| image:: /_static/image22.png
.. |image21| image:: /_static/image23.png
.. |image22| image:: /_static/image24.png
.. |image23| image:: /_static/image25.png
.. |image24| image:: /_static/image26.png
.. |image25| image:: /_static/image27.png
.. |image26| image:: /_static/image28.png
.. |image27| image:: /_static/image29.png
.. |image28| image:: /_static/image30.png
.. |image29| image:: /_static/image31.png
.. |image30| image:: /_static/image32.png
.. |image31| image:: /_static/image33.png
.. |image32| image:: /_static/image34.png
.. |image33| image:: /_static/image35.png
