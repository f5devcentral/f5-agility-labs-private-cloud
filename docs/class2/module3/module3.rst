Remove APIC Service Graph
=========================

APIC – Remove Only Service Graph Deployment
-------------------------------------------

The easiest way to remove a service graph deployment, which is same as
removing virtual server from the BIG-IP, yet remain all the EPG and
device selection policy parameters for easy re-deployment is to
un-associate a service graph under the contract subject.

Go to the contract subject by clicking the following:

Tenants SJC -> Security Policies -> Contracts -> web2app-contract ->
Subject

Move the mouse to Service Graph and hover near the drop-down menu, you
will see “X”, click “X” and graph will be removed from contract subject:

|image91|

Click “X”, the service graph SJC/WEB will disappear:

|image92|

Click “SUBMIT”

|image93|

Notice iWorkflow: Tenant, Service and Node are empty:

|image94|

BIG-IP, the partition is removed, including all virtual servers and
network related configurations:

|image95|

APIC – Re-deploy Service Graph
------------------------------

In order to re-deploy the same graph, simply go to contract subject and
re-associate SJC/WEB under Service Graph:

|image96|

Click “SUBMIT”

|image97|

You will see the Application Service is redeployed in iWorkflow and BIG-IP

|image98|

|image99|

|image100|

Notice the tenant VID, graph ID and the RD values are different from
previous deployment.

APIC – Remove all graph associated objects
------------------------------------------

If you want to clean up all the related objects of the deployed graph
template, go to:

Tenants SJC ->L4-L7 Services -> L4-L7 Service Graph Templates, right
click on the graph template WEB, then select

“Removed Related Objects of Graph Template”

|image101|

Select:

Contract: web2app-contract

Provider EPF: App1/app

Radio button: “remove both contracts and relations to the EPGs”

Check box:

Remove related EPF parameters <- this will remove all L4-L7 parameters
of this particular contract/graph/node under EPG

Remvoe related device selection policies <- this will remove
connectivity policy of this particular contract/graph/node

Click “SUBMIT”

|image102|

Notice on APIC:

EPG app: related L4-L7 Services Parameters are removed

Related Devices Selection Policies is removed

Related contract is removed

|image103|

|image104|

|image105|

F5 iWorkflow configuration related to APIC tenant and service graph is
un-configured

|image106|

BIG-IP is also clean:

|image107|

APIC – Remove L4-L7 Devices from Tenant Common
----------------------------------------------

Remove the L4-L7 logical device cluster from common tenant.

Tenant Common->L4-L7 Services -> L4-L7 devices -> , right click on the
logical device cluster and click delete

This will also delete the device group from the BIG-IP (no device group
correcponding to the logcail device cluster present anymore)

|image108|

APIC – Remove Device Manager from Tenant Common
-----------------------------------------------

Remove the device manager from common tenant.

Tenant Common->L4-L7 Services -> L4-L7 devices -> Device Managers->
‘dcloud-device-manager, right click on the device manager and click
delete

APIC – Remove Device Manager Type from L4-L7 Services
-----------------------------------------------------

Remove the device manager type from L4-L7 services

Go to L4-L7 Services -> Inventory -> Device manager types , right click
on the device manager and click delete

*vThis conclude Scenario 3 “Remove APIC Service Graph” lab.*

.. |image91| image:: /_static/class2/image96.png
   :width: 7.68095in
   :height: 6.55589in
.. |image92| image:: /_static/class2/image97.png
   :width: 3.26406in
   :height: 1.34729in
.. |image93| image:: /_static/class2/image98.png
   :width: 8.87014in
   :height: 1.74236in
.. |image94| image:: /_static/class2/image99.png
   :width: 8.59766in
   :height: 2.19456in
.. |image95| image:: /_static/class2/image100.png
   :width: 2.09733in
   :height: 0.93060in
.. |image96| image:: /_static/class2/image101.png
   :width: 7.61150in
   :height: 6.97258in
.. |image97| image:: /_static/class2/image102.png
   :width: 8.87014in
   :height: 1.78819in
.. |image98| image:: /_static/class2/image103.png
   :width: 8.44488in
   :height: 2.18067in
.. |image99| image:: /_static/class2/image104.png
   :width: 2.93071in
   :height: 0.47225in
.. |image100| image:: /_static/class2/image105.png
   :width: 8.87014in
   :height: 1.79653in
.. |image101| image:: /_static/class2/image106.png
   :width: 5.87530in
   :height: 4.65302in
.. |image102| image:: /_static/class2/image107.png
   :width: 7.65317in
   :height: 4.12521in
.. |image103| image:: /_static/class2/image108.png
   :width: 8.87014in
   :height: 2.33889in
.. |image104| image:: /_static/class2/image109.png
   :width: 6.50033in
   :height: 2.13900in
.. |image105| image:: /_static/class2/image110.png
   :width: 7.54205in
   :height: 4.18077in
.. |image106| image:: /_static/class2/image111.png
   :width: 8.48655in
   :height: 1.84732in
.. |image107| image:: /_static/class2/image112.png
   :width: 2.04177in
   :height: 0.84727in
.. |image108| image:: /_static/class2/image115.png
   :width: 8.87014in
   :height: 3.05208in
