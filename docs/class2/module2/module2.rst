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

Contract Name: ``SJC/web2app-contract``

Graph Name: ``SJC/WEB``

Node Name: ``ADC``

Then click “All Parameters”

|image86|

Expand ``WEB`` folder, double click on ``pool__port``, change the value
from ``80`` to ``8080``, then “UPDATE”

|image87|

Then “SUBMIT”

|image88|

Notice on iWorkflow, under Services, the port value is updated to 8080

|image89|

BIG-IP virtual server reflects the same configuration update

|image90|

*This concludes Scenario 2 “Modify L4 – L7 deployed graph parameters”
lab.*

.. |image85| image:: /_static/class2/image90.png
   :width: 8.87014in
   :height: 3.03750in
.. |image86| image:: /_static/class2/image91.png
   :width: 8.87014in
   :height: 3.95833in
.. |image87| image:: /_static/class2/image92.png
   :width: 7.43094in
   :height: 3.36128in
.. |image88| image:: /_static/class2/image93.png
   :width: 6.78481in
   :height: 4.76578in
.. |image89| image:: /_static/class2/image94.png
   :width: 8.87014in
   :height: 4.21111in
.. |image90| image:: /_static/class2/image95.png
   :width: 8.87014in
   :height: 3.41597in