.. _windows_tools_vm:

----------------
Windows Tools VM
----------------

Overview
+++++++++

Deploy this Windows 10 VM on your assigned cluster if directed to do so as part of **Lab Setup**.

.. raw:: html

  <strong><font color="red">Only deploy the VM once, it does not need to be cleaned up as part of any lab completion.</font></strong>

Deploying Tools VM
++++++++++++++++++

Using an SSH client, connect to the Node A CVM IP <10.42.xx.29> in your assigned block using the following credentials:

Username - nutanix

Password - default

Execute the following commands to upload AD image:

.. code-block:: bash

  acli image.create AutoDC container=Images image_type=kDiskImage source_url=https://s3.amazonaws.com/get-ahv-images/Windows10-1709.qcow2


In **Prism Central** > select :fa:`bars` **> Virtual Infrastructure > VMs**, and click **Create VM**.

Fill out the following fields:

- **Name** - *Initials*-Windows-ToolsVM
- **Description** - (Optional) Description for your VM.
- **vCPU(s)** - 1
- **Number of Cores per vCPU** - 2
- **Memory** - 4 GiB

- Select **+ Add New Disk**
    - **Type** - DISK
    - **Operation** - Clone from Image Service
    - **Image** - Windows10-1709.qcow2
    - Select **Add**

- Select **Add New NIC**
    - **VLAN Name** - Secondary
    - Select **Add**

Click **Save** to create the VM.

Power On the VM.

Login to the VM via RDP or Console session, using the following credentials:

- **Username** - NTNXLAB\\Administrator
- **password** - nutanix/4u
