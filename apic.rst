Cisco Application Policy Infrastructure Controller 2.1(1h) with F5
iWorkflow and BIG-IP Integration

Last Updated: *20-JULY-2017*

About This Cisco Solution
=========================

The **Cisco Application Policy Infrastructure Controller** (Cisco APIC)
is the unifying point of automation and management for the **Cisco
Application Centric Infrastructure** (Cisco ACI™) fabric. The Cisco APIC
provides centralized access to all fabric information, optimizes the
application lifecycle for scale and performance, supporting flexible
application provisioning across physical and virtual resources.

For additional information, visit
`www.cisco.com/go/apic <http://www.cisco.com/go/apic>`__.

About This Demonstration
========================

This preconfigured demonstration includes:

-  Scenario 1: Deploy Service Graph using F5 iApps in Cisco ACI with F5
   iWorkflow

-  Scenario 2: Modify L4 – L7 deployed graph parameters

-  Scenario 3: Remove APIC Service Graph

-  Scenario 4: Using POSTMAN REST client to deploy service graph

There are two options to complete each lab task

(1) Using iWorkflow and APIC UI – Scenario 1

(2) Using POSTMAN REST client (APIC Only) – Scenario 4

The goal of ACI is to accelerate application deployment by building
L4-L7 policy into Cisco ACI model. We recommend using the REST client
model as the most effective way to execute the APIC portion of the lab;
for BIG-IP and iWorkflow, please continue to use the UI. You are
encouraged to use the UI screen shots as a reference to the tasks
executed by POSTMAN.

Demonstration Requirements

1. Demonstration Requirements

+--------------------+-----------------------+
|     **Required**   |     **Optional**      |
+====================+=======================+
| -  Laptop          | -  Cisco AnyConnect   |
+--------------------+-----------------------+

Demonstration Configuration

This demonstration contains preconfigured users and components to
illustrate the scripted scenarios and features of this Cisco solution.
All access information needed to complete the demonstration scenario, is
located in the **Topology** and **Servers** menus of your **active
demonstration**, and throughout this script.

-  **Topology Menu**. Click on any server in the topology to display the
   available server options and credentials.

-  **Servers Menu**. Click on |image0| or |image1|\ next to any server
   name to display the available server options and credentials.

Demonstration Topology

The following is the virtual demonstration topology, which consists of
the following virtual machines:

-  APIC Simulator – version 2.1(1h)

   -  APIC1, APIC2, APIC3

   -  Leaf1 and Leaf2

   -  Spine1 and Spine2

-  VMware Virtual Center Server 5.5 Appliance

-  F5 iWorkflow – release 2.0.2

-  F5 BIG-IP – release 12.0.0 HF4

-  VMware ESXi 5.5 Host 1

-  VMware ESXi 5.5 Host 2

-  Workstation – Windows 8

-  NetApp EDGE Storage Appliance – ONTAP 8.2

-  Linux Tools Repository (Ubuntu 12.04)

1. Demonstration Topology

|image2|

This demonstration contains preconfigured users and components to
illustrate the scripted scenarios and features. All access information
needed to complete the scripted scenarios is located in the **Topology**
and **Servers** menus of your **active demonstration**, and throughout
this script.

Demonstration Preparation

Follow the steps below to schedule and configure your environment.

+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **BEFORE DEMONSTRATING**                                                                                                                                                                                                |
|                                                                                                                                                                                                                         |
| We strongly recommend that you go through this process at least once, before presenting in front of a live audience. This will allow you to become familiar with the structure of the document and the demonstration.   |
|                                                                                                                                                                                                                         |
| **PREPARATION IS KEY TO A SUCCESSFUL CUSTOMER PRESENTATION.**                                                                                                                                                           |
+=========================================================================================================================================================================================================================+
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

1. Browse to `dcloud.cisco.com <http://dcloud.cisco.com>`__, choose the
   location closest to you, and then login with your Cisco.com
   credentials.

2. Schedule a session. [`Show Me
   How <http://dcloud.cisco.com/dCloud/help/sched_demo.html>`__].

3. Test your bandwidth from the demonstration location before performing
   any scenario. [`Show Me
   How <http://dcloud.cisco.com/dCloud/help/connect_test.html>`__]

4. Verify your session has a status of **Active** under **My
   Demonstrations** on the **My Dashboard** page in the Cisco dCloud UI.

-  It may take up to **15 minutes for your demo to become active**.

1. Access the workstation named **wkst1** located at **198.18.133.36**
   and login using the following credentials: Username:
   **dcloud\\demouser**, Password: **C1sco12345**.

-  **Option 1**: **(Preferred)** Use **Cisco AnyConnect** [`Show Me
   How <http://dcloud.cisco.com/dCloud/help/install_anyconnect_pc_mac.html>`__]
   and the **local RDP client** on your laptop [`Show Me
   How <http://dcloud.cisco.com/dCloud/help/local_rdp_mac_windows.html>`__].

   -  Accept any certificates or warnings.

   -  From the **Start** menu, click **Desktop**.

-  **Option 2**: Use the **Cisco dCloud Remote Desktop client with
   HTML5**. [`Show Me
   How <http://dcloud.cisco.com/dCloud/help/access_demo_wkstn.html>`__]

   -  Accept any certificates or warnings.

   -  From the **Start** menu, click **Desktop**.

1. Start Menu

|image3|

1. The fabric discovery is automatically started at demo setup.
   Double-click the **APIC Login** icon |image4| and login
   (**admin**/**C1sco12345**).

2. Select **Fabric** from the top menu.

3. Select **Inventory** from the top sub-menu.

4. In the left menu, click **Fabric Membership** and check that you have
   the 4 devices populated as shown in Figure 3. (IP addresses may
   vary.)

**NOTE:** The fabric discovery can take up to 15 minutes to complete. If
you login before 15 minutes have passed, all devices may not be fully
discovered.

1. Completed Fabric Membership

|image5|

**NOTE:** To demonstrate Fabric Discovery, reset the APIC Simulator (see
`Appendix A <#Reset_APIC_Simulator>`__.) If only TEP-1-101 is present at
login, see `Appendix B <#Fabric_Script>`__ to discover the Fabric.

1. Double-click the **VI Login** icon |image6| and login with the
   following credentials: Username: **demouser**, Password:
   **C1sco12345**. (If password is grayed out, click **Login**.)

2. Check that the **F5 iWorkflow and BIG-IP** virtual machine is present
   and running as below.

1. Virtual Center Inventory

|image7|

**NOTE:** If the F5 BIG-IP and iWorkflow VMs are not present in the
L4-L7 Services Resource Pool, `add it manually <#Add_BIGIP>`__.

\ **
**

Deploy Service Graph using F5 iApps in Cisco ACI with F5 iWorkflow
==================================================================

Overview
========

Cisco Application Centric Infrastructure (ACI) technology provides the
capability to insert Layer 4 through Layer 7 (L4-L7) functions using an
approach called a service graph. One of Cisco ACI’s changes to the
operation model with the service graph function is that a configuration
now includes not only the network connectivity consisting of VLANs, IP
addresses, etc., but also the configuration of access control lists,
load-balancing rules, etc., on service appliances, such as the firewalls
and load balancers. This approach differs from the traditional operation
model of service insertion. Prior to Cisco ACI, the fabric configuration
would have consisted only of connectivity for firewalls and load
balancers. With Cisco ACI, the service graph configuration includes the
ability to push configuration of firewalls and load balancers from ACI.

APIC GUI Layout
===============

|image8|

The top of the GUI screen is the Menu bar tab, the middle of the GUI is
the Submenu bar tab, the bottom left of the GUI screen is the Navigation
Pane, and the middle-right of the GUI is the Work Pane.

F5 iWorkflow and Cisco ACI Lab
==============================

The goal of this lab is to demonstrate a WEB application deployment that
has L4-L7 ADC requirements in ACI environment. Using F5 iWorkflow
service catalog model, the WEB application ADC requirements are defined
in iWorkflow service catalog template using F5 iApps technology. Thru F5
dynamic device package, this service catalog is imported into ACI. In
Cisco ACI, when deploy application WEB, administrator can now pick WEB
template to apply ADC functionality to application WEB.

To achieve this scenario, you will configure ACI L4-L7 service insertion
in managed mode with device manager using F5 BIG-IP VE Virtual ADC and
F5 iWorkflow orchestration + automation platform using User Interface.

F5 iWorkflow and Cisco ACI Lab Flow Chart
=========================================

|image9|

BIG-IP – Verify the F5 BIG-IP iApps
===================================

F5 iApps is a user-customized framework for deploying application,
providing a flexible way to automate tasks and templatize F5 virtual
server configurations.

The iApps must be imported into F5 BIG-IP in order to allow F5 iWorkflow
to create an application template based on this iApps. In this step, we
will verify the iApps is already exist in F5 BIG-IP.

Log into the F5 BIG-IP with the following username and password from the
web browser:

BIG-IP: https://198.18.128.130 (https://198.18.128.130)

Username: admin

Password: C1sco12345

After you have logged into the F5 BIG-IP GUI. In the Navigation pane,
click the iApps -> Templates. You should see the iApps template
**appsvcs\_integration\_v1.0\_001** pre-loaded into the F5 BIG-IP:

|image10|

**NOTE:** Up to iWorkflow release 2.0.2, iApps to be used by iWorkflow /
APIC integration must be exist in BIG-IP in order for iWorkflow to be
discovered. Beginning iWorkflow release 2.1.0, user import iApps into
iWorkflow and iWorkflow will push the iApps to BIG-IP

iWorkflow – Set up the F5 iWorkflow Clouds and Services
=======================================================

F5 iApps template is **ALREADY** added in iWorkflow:

|image11|

F5 iWorkflow Clouds and Services allows administrator to create a cloud
connector to Cisco APIC by generating a customized device package that
contains the service catalog. It is also where administrator can manage
service catalog life cycle.

In this step, we will configure F5 iWorkflow prior to Cisco ACI
integration.

Log into the F5 iWorkflow 198.18.128.135 with the following username and
password from the web browser:

iWorkflow: https:// 198.18.128.135 (https:// 198.18.128.135)

Username: admin

Password: C1sco123

After you have logged into the F5 iWorkflow GUI. Click on “Clouds and
Services”, select “+” Devices

|image12|

Register F5 BIG-IP by selecting “Discover Device”

|image13|

Register the F5 BIG-IP by using the BIG-IP’s IP address and credential
as the following:

IP Address: 198.18.128.130

Username: admin

Password: C1sco12345

Click Save to register the BIG-IP device:

|image14|

You can now double click the registered BIG-IP and verify its status. It
should say “Available” when the BIG-IP is communicating with the
iWorkflow:

|image15|

iWorkflow – Create WEB application template in iWorkflow Catalog
================================================================

After BIG-IP is successfully discovered by iWorkflow, the iApps reside
on BIG-IP are now exposed to iWorkflow.

In this step, we will create a WEB application template based on iApps
in iWorkflow Cloud Catalog. We can specify the WEB application F5
virtual server requirements here and build it into a template.

Move your mouse to the left or right side of the screen and the Cloud
Catalog menu should appear, click “+” to add a template

|image16|

A New Template screen will appear. Enter and select the following in the
New Template:

Name: **WEB**

Input Parameters: **All Options**

Cloud: **All Clouds**

Application Type: **appsvcs\_integration\_v1.0\_001**

|image17|

**NOTE:** Only field that mark “Tenant Editable” will be visible in
Cisco APIC

|image18|

You can now edit all the available options that need to be included with
this template.

Expand the Virtual Server Listener & Pool Configuration by clicking the
>. Scroll down and CHECK the following to make them Tenant Editable.
What this does is allow the parameters expose to Cisco APIC thru F5
device package. Administrator has total control over what is exposed via
a custom device package (this reduces the complexity). It is highly
recommended to expose only what is needed to APIC:

pool\_\_addr: this is the VIP

pool\_\_port: this is the VIP listening port

Notice: by default, this iApps allow VIP as tenant editable field. When
you check VIP listening port as tenant editable, iWorkflow will
highlight it.

|image19|

Click “Tenant Preview” to review the parameters will be visible in Cisco
APIC:

|image20|

You should only see 3 parameters:

Virtual Server: Address

Virtual Server: Port

Pool: Members

|image21|

Click |image22| to go back, then “Save”

|image23|

Notice a new application template now under iWorkflow Cloud Catalog. The
"Save" operation will also update the F5 iWorkflow Cloud APIC device
package with the updated service catalog.

This service catalog is ready to be consumed by Cisco APIC.

|image24|

iWorkflow – Create F5 iWorkflow APIC device package
===================================================

The next step is to create the iWorkflow Cloud APIC Connectors which
will generate a custom device package that contains iWorkflow service
catalog. The template we created in the previous step will appear in
APIC as a service function.

Move your mouse to the left / right side of the screen to make the
Clouds menu to appear.

To create a new Connectors, move the mouse to the Clouds menu and the +
should appear.

|image25|

Click “+” to create a new Cloud Connector:

Name: dcloud

Connector Type: Cisco APIC

Click “Save” to finish

|image26|

Double Click the dcloud connector, you can download this customized
device package that contains iWorkflow Catalog to your desktop.

|image27|

|image28|

We now complete the configuration steps on iWorkflow necessary prior to
F5 ACI integration.

APIC – Import the Custom Device Package
=======================================

Starting here, you will use Cisco APIC to perform the workflow in
deploying the WEB application, with the integration of F5 iWorkflow and
BIG-IP, user can apply WEB application L4-L7 requirements within APIC
policy model, reducing significant amount of operation complexity.

In this step, you will import the customized device package generated by
F5 iWorkflow into Cisco APIC. This will allow the iWorkflow service
catalog available in Cisco APIC. The device package serves as a conduit
to facilitate communications between F5 iWorkflow and BIG-IP.

Switch to your APIC GUI and click the following to import the device
package:

L4-L7 Services -> Packages -> L4-L7 Service Device Type

Click the ACTIONS button at the Work pane and choose IMPORT DEVICE
PACKAGE

|image29|

A new pop-up should appear to allow you to choose the device package to
be installed, click “Browse”:

|image30|

Go to Desktop and select F5DevicePackage.zip

|image31|

Click “Submit”

|image32|

Now F5 device package is imported into APIC

|image33|

Expand the Device Package, notice Service Function “WEB” is equivalent
to iWorkflow Catalog template “WEB”. Under Operational, parameters
visible in APIC are the “Tenant Editable” parameters in iWorflow:

|image34|

Under Function Profiles, you can see if there is any default value
assigned to the parameters:

|image35|

APIC – Create APIC L4-L7 Device Manager under L4-L7 Services
============================================================

In order to integrate F5 iWorkflow cluster into Cisco APIC L4-L7
devices, we use Cisco APIC device manager feature to define and specify
F5 iWorkflow.

From APIC perspective, F5 iWorkflow is a "device manager" managing the
F5 BIG-IP ADC (both physical and virtual form factors).

We will first define the device manager type. In the APIC GUI, click the
following to configure the Device Manager Type:

L4-L7 Services -> Inventory -> Device Manager Type

Click the ACTIONS button at the Work pane and choose Create Device
Manager Type

|image36|

A new pop-up should appear to allow you to enter the device manager
information. Enter the following information:

Vendor: F5

Model: iWorkflow

Version: 2.0-dcloud

L4-L7 Service Device Type: F5-iWorkflow-2.0-dcloud

Device Manager: Leave this field empty

**NOTE:** It is extremely import to match the Version number with the
major version of the device package

|image37|

Click SUBMIT to accept the configuration.

|image38|

The Device Manager Type is now configured and we can now associate this
device manager type with a device manager.

APIC – Create Device Manager under Tenant Common
================================================

To create a device manager, navigate to your tenant common to create a
new L4-L7 Device Manager by clicking the following:

Tenants Common -> L4-L7 Services -> Device Managers

In the Work pane, click: ACTIONS -> Create Device Manager

|image39|

A new pop-up should appear to allow you to Create Device Manager in your
tenant. You will specify F5 iWorkflow management IP here and associate
it with the device manager type created in the previous step. Enter the
following information:

Device Manager Name: dcloud-device-manager

Management EPG: Leave this field empty since we use OOB to communicate

Device Manager Type: F5-iWorkflow-2.0-dcloud

Click the + to enter the iWorkflow management IP for device manager
Management connectivity:

Host: 198.18.128.135

Port: 443

Click UPDATE to accept.

Enter the Device Manager's login credential:

Username: admin

Password: C1sco12345

Confirm Password: C1sco12345

Click SUBMIT to accept the configuration.

|image40|

This complete the steps to create APIC L4-L7 device manager. We will use
this device manager in the next step when creating APIC L4-L7 device.

APIC – Create the L4-L7 Device
==============================

In this step, we will create an APIC L4-L7 device, this is the logical
construct that contains F5 BIG-IP and iWorkflow information. You will
see in the later steps on how to build an APIC service graph using this
L4-L7 device.

Navigate to your tenant to create a new L4-L7 Device by clicking the
following:

Tenants Common -> L4-L7 Services -> L4-L7 Devices

In the Work pane, click:

ACTIONS -> Create L4-L7 Devices

|image41|

A new window should appear for you to create the L4-L7 Devices.

|image42|

In the Create L4-L7 Devices window, enter the following:

Managed: CHECK

Name: F5-BIG-IP

Service Type: ADC

Device Type: Virtual

VMM Domain, click the down arrow to select: My-vCenter

Mode: Single Node

Device Package: F5-iWorkflow-2.0–dcloud

Model: Unknown (Manual)

Context Aware: Single

APIC to Device Management Connectivity: Out-Of-Band

Username: admin

Password: C1sco12345

Confirm Password: C1sco12345

After completion, it should look like:

|image43|

***What did I configure?***

Managed: this means this L4-L7 device will be managed by Cisco APIC to
be used in L4-L7 service insertion

Name: User defined name of the L4-L7 device

Service Type: Firewall or ADC, F5 BIG-IP is considered an ADC device

Device Type: Physical or Virtual, we use BIG-IP Virtual Edition in this
lab

VMM Domain: If device type is virtual, select the VMM domain for this
L4-L7 device, the VMM domain contains BIG-IP VE virtual machine

Mode: Single or HA, in this lab, only one BIG-IP VE, so select Single
Node

Device Package: Drop down menu, pick the device package dcloud

Model: Choose Unknown(Manual) giving you flexibility to enter any F5
BIG-IP interface convention

Context Aware: Single Context device can be used by only 1 tenant; where
Multi Context device can be shared among multiple tenants. In the case
of virtual, we will select single context

APIC to Device Management Connectivity: All management connections are
out-of-band in this lab Credentials: F5 BIG-IP admin credentials

On the right-hand side of the wizard, in the Device 1, enter the
following:

Management IP Address: 198.18.128.130

VM: Click the down arrow and select dcloud-DC/F5-BIG-IP

Management Port: https

Click the + to add a Device Interface:

Name: 1\_1

VNIC: Network adapter 2

Click UPDATE to accept the Device Interface configuration.

Click the + to add 2\ :sup:`nd` Device Interface:

Name: 1\_2

VNIC: Network adapter 3

Click UPDATE to accept the Device Interface configuration.

|image44|

***What did I configure?***

Under Device 1, enter the BIG-IP VE management IP and management port of
https (443)

Since this is a BIG-IP VE cluster, the VM field is visible and based on
the VMM domain specified earlier, pick the VM for this L4-L7 device.

Device Interfaces: specify the BIG-IP VE interface to be used in data
plane. We are configuring physical 2-arm in this lab, two BIG-IP
interfaces are specified in this cluster. Notice the interface naming is
1\_1, which is equivalent to interface 1.1 of BIG-IP. "\_" is used
instead of "." is because APIC does not allow "." as parameter value.

Next part of the configuration is L4-L7 device cluster information.

By default, APIC will populate Device 1's management IP as the Cluster
Management IP. In this lab, since we are going to use the iWorkflow to
manage BIG-IP, the Cluster IP will be changed to the iWorkflow’s IP. The
device will eventually ignore this setting and it will use the Device
Manager information configured earlier to establish communication.

Management IP Address: 198.18.128.135

Management Port: https

Device Manager: common/dcloud-device-manager

Click the + to add the 1\ :sup:`st` Logical Interface:

Type: consumer

Name: External

Concrete Interface: Device1/1\_1

Click UPDATE to accept the consumer interface configuration.

Click the + to add the 2\ :sup:`nd` Logical Interface:

Type: provider

Name: Internal

Concrete Interface: Device1/1\_2

Click UPDATE to accept the consumer interface configuration.

|image45|

Make sure all L4-L7 Devices parameters are entered correctly, click
“NEXT”

|image46|

STEP2, Device Configuration. We would like to set up some basic
information on the BIG-IP by choosing the All Parameters tab.

Click > to expand the field Device Host Configuration and enter the
following parameters and click UPDATE to save the change:

Host Name: bigip1.dcloud.cisco.com

Click “FINISH”

|image47|

Navigate to the newly created L4-L7 Device to verify its Configuration
State is stable:

Tenants common ->L4-L7 Services -> L4-L7 Devices -> F5-BIG-IP

In the Work pane, ensure the Configuration State is stable, if the
device is not stable, click the Faults tab and ensure no faults or all
the faults are in clearing state.

|image48|

We now complete the configuration of the ACI L4-L7 device, and we will
use this device when creating L4-L7 Service Graph Template in the next
step.

APIC – Export L4-L7 Device to Tenant
====================================

Export F5-BIG-IP L4-L7 device as a resource to another tenant where
application profile is configured.

Right click on F5-BIG-IP, and select “Export L4-L7 Device”

|image49|

Drop down and select tenant “SJC”, the “SUBMIT”

|image50|

|image51|

APIC – Create L4-L7 Service Graph Templates
===========================================

An APIC L4-L7 Service Graph Template is an abstract object allowing
L4-L7 configuration build into ACI policy model. In this step, you will
create a service graph template and add L4-L7 device you created in the
previous step, then select the WEB service function for this graph.

Go to Tenant SJC by typing “SJC” in the Tenant search box

|image52|

To create a new Service Graph Template, click the following in the
navigation pane:

Tenants SJC -> L4-L7 Services -> L4-L7 Service Graph Template

In the Work pane:

ACTIONS -> Create L4-L7 Service Graph Template

|image53|

In the new window, enter the following:

Graph Name: WEB

Graph Type: Create a New One (should be the default)

Now, drag the Device Clusters to the right side of the window into the
graph. You should be able to place the Node “SJC/F5-BIG-IP (Imported
Managed)” between the Consumer EPG and the Provider EPG.

When this graph template is deployed, the traffic will be redirected to
the F5 BIG-IP of this device cluster automatically by Cisco ACI.

Double click the word N1 under the Node to change the name to ADC.

Under F5-BIG-IP Information, click the Two-Arm option for this graph.

Select the Profile: F5-iWorkflow-2.0–dcloud/WEB <- this coming from the
F5 device package

This is WEB application template that we created earlier.

Click “SUBMIT”

|image54|

APIC – Deploy the Service Graph (EPG and Contract selection)
============================================================

The new ADC L4-L7 Service Graph Template is now created and we are ready
to deploy the BIG-IP with the pre-created web and app EPG.

In this step, we are deploying WEB graph, connecting between the web
tier and the app tier. Inside contract between the web and app EPG, we
will assign the service graph template created in the previous step,
this will provide F5 BIG-IP ADC functionality to APP tier.

To deploy the service graph, click the following in the Navigation pane
of your tenant:

Tenants SJC -> L4-L7 Services -> L4-L7 Service Graph Template

Select the Service Graph Template you just created from the Work pane.
Right click and choose the option to

Apply L4-L7 Service Graph Template

|image55|

In the new window, you will have the ability to choose which EPGs the
Service Graph will be inserted in between.

Select the following for the EPG information:

Consumer EPG / External Network: SJC/App1/epg-web

Provider EPG / External Network: SJC/App1/epg-app

Under Contract Information, use the option to create a new Contract:

Create a New Contract: SELECTED

Contract Name: web2app-contract

No Filter (Allow All Traffic): CHECKED

|image56|

Click NEXT to continue to the next screen.

APIC – Deploy the Service Graph (Connectivity to Fabric)
========================================================

A new window to apply the service graph template will now appear. This
window will show the Service Graph Template that you created earlier.

In addition to the Service Graph Template, there are some options that
need to be selected to deploy the BIG-IP with a Service Graph. Under the
SJC/WEB Information, you need to choose the appropriate connector
information:

Under the Connector, choose the following:

Type: General

BD: SJC/SJCBDWeb

Cluster Interface: External

We use the External interface for the communication between the BIG-IP
and the Web servers. The Web servers belong to Web EPG, which tied to
the SJCBDWeb Bridge Domain.

Type: General

BD: SJC/SJCBDApp

Cluster Interface: internal

We use the Internal interface for the communication between the BIG-IP
and the App servers. The App servers belong to App EPG, which tied to
the SJCBDApp Bridge Domain.

Click NEXT to continue to the next screen.

|image57|

APIC – Deploy the Service Graph (BIG-IP Parameters)
===================================================

A new window for the BIG-IP parameters will now appear. In this window,
you will have the ability to modify the parameters to be deployed to the
BIG-IP. Let us modify some parameters to push the Service Graph into the
BIG-IP.

Under Feature, it should be selected All. Parameters should be All
Parameters.

|image58|

Once you click the All Parameters tab, the folder and parameters will
appear. To edit the parameter, you need to expand the parameter by
clicking the > and double the field to change the parameter’s name and
value. Let us edit the following parameters:

Under Device Config

Press > to expand the Network configuration folder

Press > to expand the folder ExternalSelfIP

Double click the parameter Enable Floating? and select No as the value

Click UPDATE to apply

Double click the parameter External Self IP Address and enter
10.10.10.130 as the value

Click UPDATE to apply

Double click the parameter External Self IP Netmask and enter
255.255.255.0 as the value

Click UPDATE to apply

Double click the parameter Port Lockdown and select Default as the value

Click UPDATE to apply

Press > to expand the folder InternalSelfIP

Double click the parameter Enable Floating? and select No as the value

Click UPDATE to apply

Double click the parameter Internal Self IP Address and enter
192.168.10.130 as the value

Click UPDATE to apply

Double click the parameter Internal Self IP Netmask and enter
255.255.255.0 as the value

Click UPDATE to apply

Double click the parameter Port Lockdown and select Default as the value

Click UPDATE to apply

|image59|

Device config is BIG-IP device level configuration, like self-IP and
default route. Resource configured in the device config will be used by
Function Config

Assign Device Config “Network” to Function Config “NetworkRelation”

**NOTE:** It is extremely important to assign Network to
NetworkRelation, fail to perform this step will result in graph
deployment failure, as there will not be any network resource associated
with the graph

|image60|

The above step associates the network information under device config to
the BIG-IP virtual server.

Apply at deployment WEB service graph configuration under Function
Config

Press > to expand the WEB configuration folder

Double click on the name and delete Default

Click UPDATE to apply

Press > to expand the Pool Members folder

Press > to expand the Member folder

Double click to enter value into the IPAddress field: 192.168.10.150

Click UPDATE to apply

Back to the WEB configuration folder

Double click to enter value into the Address field (pool\_\_addr):
10.10.10.100

Click UPDATE to apply

Double click the parameter Port field (pool\_\_port): 80

Click UPDATE to apply

|image61|

Function config is BIG-IP virtual server level configuration. We define
the WEB service catalog parameters here, as well as associating the
device level network config to this virtual server.

Make sure both the device config and function config are correct

Device Config

|image62|

Function Config

|image63|

Click “FINISH” to deploy the graph

|image64|

APIC – Verifying WEB application deployment
===========================================

APIC: Verifying the service graph deployment

You can now verify if APIC has deployed the service graph correctly.
First, navigate the following:

Tenant SJC -> L4-L7 Services -> Deployed Graph Instances

You should be able to see a screen similar to the following. The State
should say “applied”

|image65|

Tenant SJC -> L4-L7 Services -> Deployed Devices

You should be able to see a screen similar to the following. The State
should say “allocated”

|image66|

Make sure there is no faults to the deployment:

|image67|

iWorkflow – Verifying the template deployment
=============================================

Once the service graph is deployed in Cisco APIC, administrator can also
view application status in F5 iWorkflow.

Log into the F5 iWorkflow 198.18.128.135 with the following username and
password from the web browser (if the previous session has timed out):

iWorkflow: https://198.18.128.135 (https://198.18.128.135)

Username: admin

Password: C1sco12345

Under the iWorkflow Cloud and Services. In the Work pane, under:

Services: graph deployment status

Tenant: APIC tenant information

Nodes: pool members information

Notice the graph is “unhealthy” because no servers are available to the
BIG-IP virtual server. This is expected because dCloud only validate
control plane, as a result, BIG-IP data plane validation to the servers
failed.

|image68|

Tenants:

|image69|

Services

Notices “Customize Application Template” contains the fields visible in
APIC. User input the values from APIC.

|image70|

In case Customize Application Template is empty, please check back in a
few minutes until the resource is refresh

|image71|\ Nodes:

This the member IP entered through APIC.

|image72|

BIG-IP – Verifying Application Services (Virtual Server) deployment
===================================================================

Log into the F5 BIG-IP 198.18.128.130 with the following username and
password from the web browser (if the previous session has timed out):

BIG-IP: https://198.18.128.130 (https://198.18.128.130)

Username: admin

Password: C1sco12345

On the Main menu, click Local Traffic -> Network Map. Then on the top
right corner, next to the Log out button, click the drop down to select
the newly created Partition (please note that this reflects the APIC
Virtual Device ID):

|image73|

Once you are in the partition, click Local Traffic -> Network Map. You
should be able to see the virtual server is configured along with its
pool and pool members.

|image74|

On the right Navigation menu, click the Local Traffic -> Virtual Servers
and you should be able to see the brief Virtual IP information. You can
see that the VIP is currently listening on HTTP port 80.

The number (in this example, 2848) after the % mark represents the route
domain (RD) number. There will be a RD number assign to each APIC
partition, which equivalent to an ACI L3 VRF. This allows BIG-IP to
provide multi-tenancy support in ACI environment.

|image75|

In the Virtual Server List, click the Name in the hyperlink and you will
see the Property of the Virtual Server with more detailed information.
The configured the parameters will appear here.

|image76|

Click on “Resource”, notice the pool name being used

|image77|

Click Local Traffic -> Pools and you should see the brief information of
the real server pool information:

|image78|

Go back to the Navigation pane and click the iApps -> Application
Services. Notice the name of the Application Services is same as the
Services name in iWorkflow.

Template is the iApps template that associated with this application
service

Partition/Path is the APIC created partition and the name of the
application service

|image79|

F5 iWorkflow service name

|image80|

Click the application service name will direct to the Application
Services Components. By using iApps template, you can configure a full
features virtual server by specifying customized parameters exposed to
APIC. Only the highlighted ones are entered by APIC, the rest of the
virtual servers features are built inside the iApps template.

|image81|

Network -> Self IP configuration from APIC

|image82|

VLAN information imported from APIC:

|image83|

Same VLAN tags are being assigned in APIC

|image84|

*This conclude Scenario 1 “Deploy Service Graphs in Cisco ACI using F5
iWorkflow” lab.*

Modify L4 – L7 deployed graph parameters
========================================

User can modify deployed graph parameters, only parameters mark “Tenant
Editable” in iWorkflow can be changed in APIC. Once a graph is deployed,
user need to go under Application Profiles / EPG level in order to make
changes to deployed graph parameters. The deployed graph parameters
reside under the provider EPG, in this case, it is the app EPG.

Go to APIC Tenant SJC -> Application Profiles -> App1 -> Application
EPGs -> EPG app -> L4-L7 Service Parameters, click the pen button:

|image85|

Select the following:

Contract Name: SJC/web2app-contract

Graph Name: SJC/WEB

Node Name: ADC

Then click “All Parameters”

|image86|

Expand “WEB” folder, double click on “pool\_\_port”, change the value
from 80 to 8080, then “UPDATE”

|image87|

Then “SUBMIT”

|image88|

Notice on iWorkflow, under Services, the port value is updated to 8080

|image89|

BIG-IP virtual server reflects the same configuration update

|image90|

*This conclude Scenario 2 “Modify L4 – L7 deployed graph parameters”
lab.*

Remove APIC Service Graph
=========================

APIC – Remove Only Service Graph Deployment
===========================================

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
==============================

In order to re-deploy the same graph, simply go to contract subject and
re-associate SJC/WEB under Service Graph:

|image96|

Click “SUBMIT”

|image97|\ You will see the Application Service is redeployed in
iWorkflow and BIG-IP

|image98|

|image99|

|image100|

Notice the tenant VID, graph ID and the RD values are different from
previous deployment.

APIC – Remove all graph associated objects
==========================================

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
==============================================

Remove the L4-L7 logical device cluster from common tenant.

Tenant Common->L4-L7 Services -> L4-L7 devices -> , right click on the
logical device cluster and click delete

This will also delete the device group from the BIG-IP (no device group
correcponding to the logcail device cluster present anymore)

|image108|

APIC – Remove Device Manager from Tenant Common
===============================================

Remove the device manager from common tenant.

Tenant Common->L4-L7 Services -> L4-L7 devices -> Device Managers->
‘dcloud-device-manager, right click on the device manager and click
delete

APIC – Remove Device Manager Type from L4-L7 Services
=====================================================

Remove the device manager type from L4-L7 services

Go to L4-L7 Services -> Inventory -> Device manager types , right click
on the device manager and click delete

*vThis conclude Scenario 3 “Remove APIC Service Graph” lab.*

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

*ASSUMPTION –* Device package install, device manager configuration has
already been done, POSTS are from the point of when a graph is to be
created

Run each postman POST and then see the corresponding object created on
the APIC

1.  *Login Token to APIC* – Used for authentication to the APIC. The
    response to the POST operation will contain an authentication token.
    Subsequent operations on the REST API will use this token value to
    authenticate future requests.

2.  *CreateDeviceManagerType* – Used to create a device manger type
    under L4-L7 services->Inventory

3.  *CreateDeviceManager-Common* – Will create a device manager which
    has iWorkflow credentials under tenant common

4.  *Create-Ldev-Common*– Creates a logical device cluster on the APIC
    in tenant common

5.  *Export from Common to SJC tenant* – Exports the LDev from common
    tenant to SJC tenant

6.  *Scope Network under AP* – This will scope the network parameters
    like self IP/route under the application profiles

7.  *Create contract* – Creates a contract to be used in tenant SJC

8.  Assign contract to web EPG

9.  Assign contract to app EPG

10. *Create service graph template* – Creates the service graph template
    to be used

11. *Apply service graph template* – Specifies the parameters (virtual
    server/pool. Pool members etc.) to be configured for this particular
    graph

12. *Create device selection policy* – Creates a device selection policy
    (This construct gets created automatically when using the UI, this
    is an extra step needed when using automation)

13. *Apply graph to contact* – Attach the graph to the contract

*This conclude Scenario 4 “POSTMAN REST client” lab.*

A. Reset APIC Simulator

APIC Fabric Members are created by default, so that the demonstration
can begin with the creation of the APIC objects.

If you want to demonstrate the fabric discovery, reboot the **apic-fcs**
via Guest OS Control as follows:

1. From the Demo Dashboard, click **Servers**.

1. Servers Tab

|image112|

1. From the **Servers** list, click the |image113|\ next to
   **apic-fcs**.

|image114|

1. Click the **Reboot** button in **Guest OS Control** to restart the
   server.

|image115|

**NOTE:** It will take up to 5 minutes before you can login and rebuild
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

**NOTE:** The full fabric discovery can take up to 15 minutes. The apic3
controller will be discovered after all the devices are discovered. You
can check monitor the progress by selecting **Topology** from the
**Inventory** pane in the APIC GUI. While the discovery is taking place,
you can complete `Scenario 1 <#System_Health>`__, which ends in the APIC
Topology window showing the discovered elements.

Demonstration Steps

Configure APIC Fabric Using Scripts

1. From the demonstration workstation, click the **Build ACI Fabric**
   icon. |image116|

2. Type **Y <Enter>** at the **Do you want to continue (Y/N)?** prompt.
   The script will begin building the fabric, which will take about 15
   minutes.

1. Build ACI Fabric Script

|image117|

1. Type **Y <Enter>** at the **Do you want to continue (Y/N)?** prompt.
   The script will begin building the F5, which will complete before the
   ACI fabric is set up.

Configure APIC Fabric Using Postman–REST Client

1. From the demonstration workstation, launch **‘APIC Login’**, and then
   log in to the **Application Policy Infrastructure Controller** with
   the following credentials: Username: **admin**, Password:
   **C1sco12345**.

2. From the menu bar, click **Fabric**.

3. From the sub-menu bar, click **Inventory**.

4. In the left-pane, choose **Fabric Membership**.

5. Review the current members of the Fabric.

1. Fabric Membership

|image118|

1. Launch the **Postman – REST Client** [|image119|] from the taskbar.
   You are automatically be logged in. This is where you will register
   the switches for the APIC.

**Important**: If you get a status of **403 Forbidden** while performing
the activity in this scenario, review the text below for more
information on the error. If you see **Token was invalid (Error: Token
timeout)**, this means that your session has timed out. You will need to
launch the **APIC Login** POST [|image120|] and then proceed with the
next POST.

|image121|

1. In the left-pane, click the arrow [|image122|] next to **dCloud APIC
   Demo**, and then click the arrow next to **Create Fabric** and
   **dCloud APIC Connectivity**.

1. dCloud APIC Demo

|image123|

1. Go to **dCloud APIC Connectivity** and then choose **APIC Login**.
   Click **Send** to connect to the APIC.

1. APIC Login and Send

|image124|

1. Review the **Status** of the submission. A result of **200 OK** means
   the submission was successful.

1. Status

|image125|

1. Go to **Create Fabric**.

2. Choose the **Add Spine1 to Fabric** post. Click **Send** to configure
   the first spine,a and then it will discover the others.

3. Review the status of the submission.

4. In the APIC application window, you can see Spine1 is now part of the
   Fabric Membership.

1. Fabric Membership

|image126|

1. Go to the **Postman – REST Client** window.

2. Under **Create Fabric**, choose the **Add Spine2 to Fabric** post and
   then click **Send** to configure the second spine.

3. Review the status of the submission.

4. In the APIC window, you can see Spine2 is now part of the Fabric
   Membership.

1. Fabric Membership

|image127|

1. Go to the **Postman – REST** **Client** window.

2. Under **Create Fabric**, choose the **Add Leaf2 to Fabric** post.

3. Review the command for this post and you can see that it:

-  Looks for the serial number (TEP-1-102)

-  Sets up the serial number for node 102

-  Names Leaf2

1. Add Leaf2 to Fabric

|image128|

1. Click **Send**.

2. Review the status of the submission.

3. In the **APIC window**, you can see **Leaf2** is now part of the
   **Fabric Membership**.

1. Fabric Membership

|image129|

1. Go to the **Postman – REST** Client window.

2. Under **Create Fabric**, choose the **Configure Leaf 1 to Fabric**
   post, which will update the first member of the Fabric.

3. Click **Send**.

4. Review the status of the submission.

5. In the **APIC window**, you can see that **Node ID** and **Node
   Name** have been set for serial number TEP-1-101.

6. As it discovers Leaf1, an IP address is allocated.

7. The discovery will continue until it finds all of the links to the
   other members and populates the IP Addresses.

1. Fabric Membership

|image130|

1. Wait for discovery to finish. In the APIC window, select **Fabric >
   Inventory** from the main menu. Click **Topology** and demonstrate
   that the entire fabric has been discovered and is included in the
   topology.

1. Fabric Discovery Topology

|image131|

|image132|

.. |image0| image:: media/image5.png
   :width: 0.10000in
   :height: 0.11667in
.. |image1| image:: media/image6.png
   :width: 0.12500in
   :height: 0.13333in
.. |image2| image:: media/image7.png
   :width: 5.14211in
   :height: 3.40030in
.. |image3| image:: media/image8.png
   :width: 6.50000in
   :height: 1.71389in
.. |image4| image:: media/image9.png
   :width: 0.49167in
   :height: 0.55000in
.. |image5| image:: media/image10.png
   :width: 6.34167in
   :height: 1.34167in
.. |image6| image:: media/image11.png
   :width: 0.40833in
   :height: 0.55000in
.. |image7| image:: media/image12.png
   :width: 4.21637in
   :height: 3.08065in
.. |image8| image:: media/image13.png
   :width: 7.24028in
   :height: 1.73333in
.. |image9| image:: media/image14.png
   :width: 8.87014in
   :height: 4.42361in
.. |image10| image:: media/image15.png
   :width: 8.87014in
   :height: 1.94167in
.. |image11| image:: media/image16.png
   :width: 8.87014in
   :height: 1.80903in
.. |image12| image:: media/image17.png
   :width: 8.87014in
   :height: 1.92222in
.. |image13| image:: media/image18.png
   :width: 2.56958in
   :height: 1.43063in
.. |image14| image:: media/image19.png
   :width: 8.87014in
   :height: 1.56042in
.. |image15| image:: media/image20.png
   :width: 8.87014in
   :height: 3.99236in
.. |image16| image:: media/image21.png
   :width: 8.87014in
   :height: 1.65764in
.. |image17| image:: media/image22.png
   :width: 8.87014in
   :height: 2.82222in
.. |image18| image:: media/image23.png
   :width: 1.38896in
   :height: 1.06950in
.. |image19| image:: media/image24.png
   :width: 7.41705in
   :height: 5.90308in
.. |image20| image:: media/image25.png
   :width: 7.72262in
   :height: 0.50003in
.. |image21| image:: media/image26.png
   :width: 7.73651in
   :height: 2.56958in
.. |image22| image:: media/image27.png
   :width: 0.22956in
   :height: 0.24869in
.. |image23| image:: media/image28.png
   :width: 7.69484in
   :height: 0.54169in
.. |image24| image:: media/image29.png
   :width: 8.87014in
   :height: 5.10208in
.. |image25| image:: media/image30.png
   :width: 2.38901in
   :height: 1.13895in
.. |image26| image:: media/image31.png
   :width: 7.72262in
   :height: 2.11122in
.. |image27| image:: media/image32.png
   :width: 8.87014in
   :height: 2.98542in
.. |image28| image:: media/image33.png
   :width: 5.05660in
   :height: 4.61766in
.. |image29| image:: media/image34.png
   :width: 8.87014in
   :height: 1.86319in
.. |image30| image:: media/image35.png
   :width: 3.06604in
   :height: 1.90281in
.. |image31| image:: media/image36.png
   :width: 4.25922in
   :height: 3.87736in
.. |image32| image:: media/image37.png
   :width: 3.67006in
   :height: 2.28302in
.. |image33| image:: media/image38.png
   :width: 8.87014in
   :height: 1.81736in
.. |image34| image:: media/image39.png
   :width: 8.87014in
   :height: 2.60208in
.. |image35| image:: media/image40.png
   :width: 8.87014in
   :height: 3.80347in
.. |image36| image:: media/image41.png
   :width: 8.87014in
   :height: 1.90417in
.. |image37| image:: media/image42.png
   :width: 6.33019in
   :height: 1.97097in
.. |image38| image:: media/image43.png
   :width: 4.41234in
   :height: 3.50939in
.. |image39| image:: media/image44.png
   :width: 8.87014in
   :height: 4.18889in
.. |image40| image:: media/image45.png
   :width: 5.51153in
   :height: 5.03774in
.. |image41| image:: media/image46.png
   :width: 8.87014in
   :height: 3.01875in
.. |image42| image:: media/image47.png
   :width: 8.87014in
   :height: 5.95278in
.. |image43| image:: media/image48.png
   :width: 4.23633in
   :height: 6.63923in
.. |image44| image:: media/image49.png
   :width: 7.44483in
   :height: 2.48624in
.. |image45| image:: media/image50.png
   :width: 7.40316in
   :height: 2.45846in
.. |image46| image:: media/image51.png
   :width: 8.87014in
   :height: 6.02778in
.. |image47| image:: media/image52.png
   :width: 8.87014in
   :height: 6.00833in
.. |image48| image:: media/image53.png
   :width: 8.87014in
   :height: 4.12222in
.. |image49| image:: media/image54.png
   :width: 4.86136in
   :height: 3.33351in
.. |image50| image:: media/image55.png
   :width: 5.26416in
   :height: 3.18072in
.. |image51| image:: media/image56.png
   :width: 8.87014in
   :height: 2.22847in
.. |image52| image:: media/image57.png
   :width: 1.76398in
   :height: 1.29173in
.. |image53| image:: media/image58.png
   :width: 8.87014in
   :height: 1.69861in
.. |image54| image:: media/image59.png
   :width: 8.87014in
   :height: 3.91806in
.. |image55| image:: media/image60.png
   :width: 6.06976in
   :height: 4.59746in
.. |image56| image:: media/image61.png
   :width: 8.87014in
   :height: 2.45903in
.. |image57| image:: media/image62.png
   :width: 8.87014in
   :height: 5.40417in
.. |image58| image:: media/image63.png
   :width: 6.46283in
   :height: 3.51266in
.. |image59| image:: media/image64.png
   :width: 7.05592in
   :height: 3.66685in
.. |image60| image:: media/image65.png
   :width: 6.09754in
   :height: 0.73615in
.. |image61| image:: media/image66.png
   :width: 7.36149in
   :height: 1.52786in
.. |image62| image:: media/image67.png
   :width: 7.58372in
   :height: 3.52796in
.. |image63| image:: media/image68.png
   :width: 7.51427in
   :height: 2.25012in
.. |image64| image:: media/image69.png
   :width: 8.87014in
   :height: 5.47292in
.. |image65| image:: media/image70.png
   :width: 8.87014in
   :height: 3.94444in
.. |image66| image:: media/image71.png
   :width: 8.87014in
   :height: 3.07569in
.. |image67| image:: media/image72.png
   :width: 8.87014in
   :height: 3.05000in
.. |image68| image:: media/image73.png
   :width: 8.87014in
   :height: 2.84375in
.. |image69| image:: media/image74.png
   :width: 8.87014in
   :height: 3.68750in
.. |image70| image:: media/image75.png
   :width: 8.87014in
   :height: 4.18403in
.. |image71| image:: media/image76.png
   :width: 7.24167in
   :height: 1.66667in
.. |image72| image:: media/image77.png
   :width: 8.87014in
   :height: 2.94583in
.. |image73| image:: media/image78.png
   :width: 8.87014in
   :height: 0.74583in
.. |image74| image:: media/image79.png
   :width: 8.87014in
   :height: 2.47986in
.. |image75| image:: media/image80.png
   :width: 8.87014in
   :height: 2.30417in
.. |image76| image:: media/image81.png
   :width: 8.87014in
   :height: 4.70347in
.. |image77| image:: media/image82.png
   :width: 6.75035in
   :height: 3.08349in
.. |image78| image:: media/image83.png
   :width: 8.87014in
   :height: 4.27569in
.. |image79| image:: media/image84.png
   :width: 8.87014in
   :height: 1.85000in
.. |image80| image:: media/image85.png
   :width: 8.65322in
   :height: 2.04177in
.. |image81| image:: media/image86.png
   :width: 8.87014in
   :height: 5.43542in
.. |image82| image:: media/image87.png
   :width: 8.87014in
   :height: 3.02361in
.. |image83| image:: media/image88.png
   :width: 8.87014in
   :height: 3.96458in
.. |image84| image:: media/image89.png
   :width: 8.87014in
   :height: 2.65000in
.. |image85| image:: media/image90.png
   :width: 8.87014in
   :height: 3.03750in
.. |image86| image:: media/image91.png
   :width: 8.87014in
   :height: 3.95833in
.. |image87| image:: media/image92.png
   :width: 7.43094in
   :height: 3.36128in
.. |image88| image:: media/image93.png
   :width: 6.78481in
   :height: 4.76578in
.. |image89| image:: media/image94.png
   :width: 8.87014in
   :height: 4.21111in
.. |image90| image:: media/image95.png
   :width: 8.87014in
   :height: 3.41597in
.. |image91| image:: media/image96.png
   :width: 7.68095in
   :height: 6.55589in
.. |image92| image:: media/image97.png
   :width: 3.26406in
   :height: 1.34729in
.. |image93| image:: media/image98.png
   :width: 8.87014in
   :height: 1.74236in
.. |image94| image:: media/image99.png
   :width: 8.59766in
   :height: 2.19456in
.. |image95| image:: media/image100.png
   :width: 2.09733in
   :height: 0.93060in
.. |image96| image:: media/image101.png
   :width: 7.61150in
   :height: 6.97258in
.. |image97| image:: media/image102.png
   :width: 8.87014in
   :height: 1.78819in
.. |image98| image:: media/image103.png
   :width: 8.44488in
   :height: 2.18067in
.. |image99| image:: media/image104.png
   :width: 2.93071in
   :height: 0.47225in
.. |image100| image:: media/image105.png
   :width: 8.87014in
   :height: 1.79653in
.. |image101| image:: media/image106.png
   :width: 5.87530in
   :height: 4.65302in
.. |image102| image:: media/image107.png
   :width: 7.65317in
   :height: 4.12521in
.. |image103| image:: media/image108.png
   :width: 8.87014in
   :height: 2.33889in
.. |image104| image:: media/image109.png
   :width: 6.50033in
   :height: 2.13900in
.. |image105| image:: media/image110.png
   :width: 7.54205in
   :height: 4.18077in
.. |image106| image:: media/image111.png
   :width: 8.48655in
   :height: 1.84732in
.. |image107| image:: media/image112.png
   :width: 2.04177in
   :height: 0.84727in
.. |image108| image:: media/image115.png
   :width: 8.87014in
   :height: 3.05208in
.. |image109| image:: media/image122.png
   :width: 8.69792in
   :height: 5.35417in
.. |image110| image:: media/image123.png
   :width: 5.46875in
   :height: 1.33333in
.. |image111| image:: media/image124.png
   :width: 5.50000in
   :height: 4.28125in
.. |image112| image:: media/image125.png
   :width: 7.23333in
   :height: 1.11667in
.. |image113| image:: media/image126.png
   :width: 0.24167in
   :height: 0.25833in
.. |image114| image:: media/image127.png
   :width: 7.25000in
   :height: 1.75000in
.. |image115| image:: media/image128.png
   :width: 6.75000in
   :height: 1.44167in
.. |image116| image:: media/image129.png
   :width: 0.26667in
   :height: 0.36667in
.. |image117| image:: media/image130.png
   :width: 5.87500in
   :height: 0.99167in
.. |image118| image:: media/image131.png
   :width: 5.85000in
   :height: 1.08333in
.. |image119| image:: media/image132.png
   :width: 0.24167in
   :height: 0.20833in
.. |image120| image:: media/image133.png
   :width: 0.52500in
   :height: 0.11667in
.. |image121| image:: media/image134.png
   :width: 2.97500in
   :height: 1.58333in
.. |image122| image:: media/image135.png
   :width: 0.12500in
   :height: 0.15833in
.. |image123| image:: media/image136.png
   :width: 2.10000in
   :height: 2.41667in
.. |image124| image:: media/image137.png
   :width: 6.12500in
   :height: 3.23333in
.. |image125| image:: media/image138.png
   :width: 5.73333in
   :height: 3.10000in
.. |image126| image:: media/image139.png
   :width: 7.24167in
   :height: 1.55000in
.. |image127| image:: media/image140.png
   :width: 6.60000in
   :height: 1.62500in
.. |image128| image:: media/image141.png
   :width: 5.80833in
   :height: 2.54167in
.. |image129| image:: media/image142.png
   :width: 6.33333in
   :height: 1.80000in
.. |image130| image:: media/image143.png
   :width: 5.93333in
   :height: 1.67500in
.. |image131| image:: media/image144.png
   :width: 5.83333in
   :height: 3.92500in
.. |image132| image:: media/image76.png
   :width: 7.24167in
   :height: 1.66667in
