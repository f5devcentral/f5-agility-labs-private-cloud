Lab 1.5: Deploy L4-L7 via CLI
-----------------------------

In addition to using the Horizon GUI panel you can also provision LBaaS
via the command-line. From your Putty window run the following commands:

``neutron lbaas-loadbalancer-create --name lb2 internal-subnet``

``neutron lbaas-listener-create --name listener2 --loadbalancer lb2 --protocol TCP --protocol-port 22``

``neutron lbaas-pool-create --name pool2 --lb-algorithm ROUND\_ROBIN --listener listener2 --protocol TCP``

``neutron lbaas-member-create --subnet internal-subnet --address 10.1.100.101 --protocol-port 22 pool2``

``neutron lbaas-member-create --subnet internal-subnet --address 10.1.100.102 --protocol-port 22 pool2``

``neutron lbaas-healthmonitor-create --delay 3 --type TCP --max-retries 3 --timeout 3 --pool pool2``

Verify on the BIG-IP that you see the new Virtual Server deployed.

