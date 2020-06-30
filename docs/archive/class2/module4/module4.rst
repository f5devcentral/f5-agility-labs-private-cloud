Using POSTMAN REST client to deploy service graph
=================================================

Launch POSTMAN from desktop

Import the POSTMAN collection

The JSON collection if saved on your desktop -
‘dCloud-F5-iWorkflow-App-iApps-Final.postman\_collection.json’

Click on Collection->Import

Click on the ‘Choose Files’ button and browse to the json collection and
import it

|image109|

The POSTMAN collection will be loaded in your POSTMAN window:

|image110|

To view what each API call executed, click on the POST requests

Click on the Body to view the payload being passed

Click the Send button to execute the request

Check the status at the bottom of the window to see if the request got
executed successfully (200 OK)

|image111|

.. NOTE:: Device package install, device manager configuration has
   already been done, POSTS are from the point of when a graph is to be
   created

Run each postman POST and then see the corresponding object created on
the APIC

#.  *Login Token to APIC* – Used for authentication to the APIC. The
    response to the POST operation will contain an authentication token.
    Subsequent operations on the REST API will use this token value to
    authenticate future requests.

#.  *CreateDeviceManagerType* – Used to create a device manger type
    under L4-L7 services->Inventory

#.  *CreateDeviceManager-Common* – Will create a device manager which
    has iWorkflow credentials under tenant common

#.  *Create-Ldev-Common*– Creates a logical device cluster on the APIC
    in tenant common

#.  *Export from Common to SJC tenant* – Exports the LDev from common
    tenant to SJC tenant

#.  *Scope Network under AP* – This will scope the network parameters
    like self IP/route under the application profiles

#.  *Create contract* – Creates a contract to be used in tenant SJC

#.  Assign contract to web EPG

#.  Assign contract to app EPG

#.  *Create service graph template* – Creates the service graph template
    to be used

#.  *Apply service graph template* – Specifies the parameters (virtual
    server/pool. Pool members etc.) to be configured for this particular
    graph

#.  *Create device selection policy* – Creates a device selection policy
    (This construct gets created automatically when using the UI, this
    is an extra step needed when using automation)

#.  *Apply graph to contact* – Attach the graph to the contract

*This conclude Scenario 4 “POSTMAN REST client” lab.*

A. Reset APIC Simulator

APIC Fabric Members are created by default, so that the demonstration
can begin with the creation of the APIC objects.

If you want to demonstrate the fabric discovery, reboot the **apic-fcs**
via Guest OS Control as follows:

#. From the Demo Dashboard, click **Servers**.

#. Servers Tab

   |image112|

#. From the **Servers** list, click the |image113|\ next to
   **apic-fcs**.

   |image114|

#. Click the **Reboot** button in **Guest OS Control** to restart the
   server.

   |image115|

.. NOTE:: It will take up to 5 minutes before you can login and rebuild
   the Fabric using one of the Fabric Discovery methods in `Appendix
   B <#Fabric_Discovery>`__.

A. Fabric Discovery

If they are not configured, use one of the three methods below to
configure:

+----------------------------+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+
|     **Method**             |     **Automation Level**   |     **Explanation**                                                                                                                                    |     **Completion Time**                                     |
+============================+============================+========================================================================================================================================================+=============================================================+
|     Script Configuration   |     High                   |     Skip the configuration steps and discover the APIC Fabric automatically, as shown in `Configure APIC Fabric Using Script <#Fabric_Script>`__\ s.   |     | 1 minute, followed by                                 |
|                            |                            |                                                                                                                                                        |     | 15 minutes to build the fabric                        |
+----------------------------+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+
|     Wizard Configuration   |     Medium                 |     Set up the APIC Fabric using the Postman–REST client, as shown in `Configure APIC Fabric Using Postman–REST Client <#Fabric_Wizards>`__.           |     5 minutes, followed by 15 minutes to build the fabric   |
+----------------------------+----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------+

.. NOTE:: The full fabric discovery can take up to 15 minutes. The apic3
   controller will be discovered after all the devices are discovered. You
   can check monitor the progress by selecting **Topology** from the
   **Inventory** pane in the APIC GUI. While the discovery is taking place,
   you can complete `Scenario 1 <#System_Health>`__, which ends in the APIC
   Topology window showing the discovered elements.

Demonstration Steps

Configure APIC Fabric Using Scripts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. From the demonstration workstation, click the **Build ACI Fabric**
   icon. |image116|

#. Type **Y <Enter>** at the **Do you want to continue (Y/N)?** prompt.
   The script will begin building the fabric, which will take about 15
   minutes.

#. Build ACI Fabric Script

   |image117|

#. Type **Y <Enter>** at the **Do you want to continue (Y/N)?** prompt.
   The script will begin building the F5, which will complete before the
   ACI fabric is set up.

Configure APIC Fabric Using Postman–REST Client
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. From the demonstration workstation, launch **‘APIC Login’**, and then
   log in to the **Application Policy Infrastructure Controller** with
   the following credentials: Username: **admin**, Password:
   **C1sco12345**.

#. From the menu bar, click **Fabric**.

#. From the sub-menu bar, click **Inventory**.

#. In the left-pane, choose **Fabric Membership**.

#. Review the current members of the Fabric.

#. Fabric Membership

   |image118|

#. Launch the **Postman – REST Client** [|image119|] from the taskbar.
   You are automatically be logged in. This is where you will register
   the switches for the APIC.

   .. NOTE:: If you get a status of **403 Forbidden** while performing
      the activity in this scenario, review the text below for more
      information on the error. If you see **Token was invalid (Error: Token
      timeout)**, this means that your session has timed out. You will need to
      launch the **APIC Login** POST [|image120|] and then proceed with the
      next POST.

   |image121|

#. In the left-pane, click the arrow [|image122|] next to **dCloud APIC
   Demo**, and then click the arrow next to **Create Fabric** and
   **dCloud APIC Connectivity**.

#. dCloud APIC Demo

   |image123|

#. Go to **dCloud APIC Connectivity** and then choose **APIC Login**.
   Click **Send** to connect to the APIC.

#. APIC Login and Send

   |image124|

#. Review the **Status** of the submission. A result of **200 OK** means
   the submission was successful.

#. Status

   |image125|

#. Go to **Create Fabric**.

#. Choose the **Add Spine1 to Fabric** post. Click **Send** to configure
   the first spine,a and then it will discover the others.

#. Review the status of the submission.

#. In the APIC application window, you can see Spine1 is now part of the
   Fabric Membership.

#. Fabric Membership

   |image126|

#. Go to the **Postman – REST Client** window.

#. Under **Create Fabric**, choose the **Add Spine2 to Fabric** post and
   then click **Send** to configure the second spine.

#. Review the status of the submission.

#. In the APIC window, you can see Spine2 is now part of the Fabric
   Membership.

#. Fabric Membership

   |image127|

#. Go to the **Postman – REST** **Client** window.

#. Under **Create Fabric**, choose the **Add Leaf2 to Fabric** post.

#. Review the command for this post and you can see that it:

   -  Looks for the serial number (TEP-1-102)

   -  Sets up the serial number for node 102

   -  Names Leaf2

#. Add Leaf2 to Fabric

   |image128|

#. Click **Send**.

#. Review the status of the submission.

#. In the **APIC window**, you can see **Leaf2** is now part of the
   **Fabric Membership**.

#. Fabric Membership

   |image129|

#. Go to the **Postman – REST** Client window.

#. Under **Create Fabric**, choose the **Configure Leaf 1 to Fabric**
   post, which will update the first member of the Fabric.

#. Click **Send**.

#. Review the status of the submission.

#. In the **APIC window**, you can see that **Node ID** and **Node
   Name** have been set for serial number TEP-1-101.

#. As it discovers Leaf1, an IP address is allocated.

#. The discovery will continue until it finds all of the links to the
   other members and populates the IP Addresses.

#. Fabric Membership

   |image130|

#. Wait for discovery to finish. In the APIC window, select **Fabric >
   Inventory** from the main menu. Click **Topology** and demonstrate
   that the entire fabric has been discovered and is included in the
   topology.

#. Fabric Discovery Topology

   |image131|

   |image132|

.. |image109| image:: /_static/class2/image122.png
   :width: 8.69792in
   :height: 5.35417in
.. |image110| image:: /_static/class2/image123.png
   :width: 5.46875in
   :height: 1.33333in
.. |image111| image:: /_static/class2/image124.png
   :width: 5.50000in
   :height: 4.28125in
.. |image112| image:: /_static/class2/image125.png
   :width: 7.23333in
   :height: 1.11667in
.. |image113| image:: /_static/class2/image126.png
   :width: 0.24167in
   :height: 0.25833in
.. |image114| image:: /_static/class2/image127.png
   :width: 7.25000in
   :height: 1.75000in
.. |image115| image:: /_static/class2/image128.png
   :width: 6.75000in
   :height: 1.44167in
.. |image116| image:: /_static/class2/image129.png
   :width: 0.26667in
   :height: 0.36667in
.. |image117| image:: /_static/class2/image130.png
   :width: 5.87500in
   :height: 0.99167in
.. |image118| image:: /_static/class2/image131.png
   :width: 5.85000in
   :height: 1.08333in
.. |image119| image:: /_static/class2/image132.png
   :width: 0.24167in
   :height: 0.20833in
.. |image120| image:: /_static/class2/image133.png
   :width: 0.52500in
   :height: 0.11667in
.. |image121| image:: /_static/class2/image134.png
   :width: 2.97500in
   :height: 1.58333in
.. |image122| image:: /_static/class2/image135.png
   :width: 0.12500in
   :height: 0.15833in
.. |image123| image:: /_static/class2/image136.png
   :width: 2.10000in
   :height: 2.41667in
.. |image124| image:: /_static/class2/image137.png
   :width: 6.12500in
   :height: 3.23333in
.. |image125| image:: /_static/class2/image138.png
   :width: 5.73333in
   :height: 3.10000in
.. |image126| image:: /_static/class2/image139.png
   :width: 7.24167in
   :height: 1.55000in
.. |image127| image:: /_static/class2/image140.png
   :width: 6.60000in
   :height: 1.62500in
.. |image128| image:: /_static/class2/image141.png
   :width: 5.80833in
   :height: 2.54167in
.. |image129| image:: /_static/class2/image142.png
   :width: 6.33333in
   :height: 1.80000in
.. |image130| image:: /_static/class2/image143.png
   :width: 5.93333in
   :height: 1.67500in
.. |image131| image:: /_static/class2/image144.png
   :width: 5.83333in
   :height: 3.92500in
.. |image132| image:: /_static/class2/image76.png
   :width: 7.24167in
   :height: 1.66667in

