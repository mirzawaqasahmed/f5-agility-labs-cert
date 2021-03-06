Review Setup and Packet Processing Lab
======================================

BIG-IP VE System Configuration 
------------------------------

Access your BIG-IP and verify it is configured properly.

Open a new Web browser and access https://10.1.1.245. Log into the BIG-IP VE
system using the following credentials:

.. code-block:: bash

   Username: admin
   Password: admin

Check the upper left-hand corner and ensure you are on the active device
the status should be **ONLINE (ACTIVE)**. Most deployments are
active-standby and either device could be the active device.

On the **System > Resource Provisioning** page ensure **Local Traffic
(LTM) and Application Visibility and Reporting (AVR)** modules are
provisioned\ **.**

Go to **Local Traffic > Virtual Servers** and verify your virtual
server states. They should match the image below.

.. image:: /_static/201L/201ex211t1-virtuals.png

.. NOTE::
   This BIG-IP has been pre-configured and the **purple\_vs**
   virtual server is down on purpose.

Open BIG-IP TMSH and TCPDump session
------------------------------------

In this task, you will open two SSH sessions to the BIG-IP. One for TMSH
commands and the other for a tcpdump of the client-side network.

Open a terminal window (window1) from the shortcut bar at the
bottom of the jumpbox.

.. code-block:: bash

   ssh root@10.1.1.245
   password: default

Use tcpdump to monitor traffic from the client (10.1.10.51) destined to
**ftp\_vs** (10.1.10.100)

.. code-block:: bash

   tcpdump -nni client_vlan host 10.1.10.51 and 10.1.10.100

Open another terminal window (window2) and use **tmsh** to display the
connection table.

.. code-block:: bash

   ssh root@10.1.1.245
   password: default

   tmsh

At the TMOS prompt **(tmos)#**

.. code-block:: bash

   show sys connection

Do you see any connections from the jumpbox 10.1.1.51 to 10.1.1.245:22?

*Q1. Why are the ssh management sessions not displayed in connection
table?*

Establish ftp connection
------------------------

In this task you will open a third terminal window and establish an FTP
session through the **ftp\_vs** virtual server. With the connection
remaining open you will view the results in window1 (tcpdump) and
window2 (tmsh).

Open a third command/terminal window (window3).

.. code-block:: bash

   ftp 10.1.10.100

It may take 15 to 20 seconds for the logon on prompt, just leave it at
prompt to hold the connection open.

In window1 you should see something similar to the tcpdump captured
below.

.. image:: /_static/201L/201ex211t2a-tcpdump.png

*Q1. In the tcpdump above, what is client IP address and port and the
server IP address port?*

In window2 (tmsh) run the **show sys conn** again, but strain out the
noise of other connections (mirrored and selfIP) by just looking at
connections from your jumpbox.

.. code-block:: bash

   sho sys conn cs-client-addr 10.1.10.51

The connection table on window2 will show the client-side and
server-side connection similar to below:

.. image:: /_static/201L/201ex211t2b-shsysconn.png

*Q2. What is source ip and port as seen by ftp server in the example
above?*

*Q3. What happened to the original client IP address and where did
10.1.20.249 come from?*

.. HINT::
   You will have to review the configuration of **ftp\_vs** to  determine the answer to question 3.

