.. _file_analytics_deploy:

----------------------
File Analytics: Deploy
----------------------

Overview
++++++++

*The estimated time to compleate this lab is 10 minutes.*

In this exercise you will deploy the File Analytics VM and scan the existing shares to build out the dashboard.  You will also create anomaly alerts and view the audit details for your file server instance.

Deploy File Analytics
+++++++++++++++++++++

#. In **Prism** > **File Server** > click **Deploy File Analytics**

   .. figure:: images/31.png

#. Select **Deploy**

   For the purpose of saving time, the File Analytics 2.1.0 package has already been uploaded to your cluster. Files binaries can be downloaded from the Nutanix Portal and uploaded manually.

#. After the upload completes select **Install**

#. Fill out the details

   - **Name** - FA
   - **Storage Container** – Will automatically select the container used by your file server instance
   - **Network List** – Secondary - UnManaged (We choose Secondary subnetwork because FA has to be available to Windows Client VM) 
   - **Subnet Mask** – 255.255.255.128
   - **Defult Gateway IP** – 10.42.xx.129
   - **IP Address** – 10.42.xx.171

#. Select **Show Advanced Settings**

#. Ensure **DNS Resolver IP** is set to your Active Directory, ntnxlab.local, domain controller/DNS IP address 10.42.xx.yz and **ONLY** that address.

#. Choose **Deploy**

#. You can monitor the deployment from the **Tasks** page.  The Analytics VM deployment should take ~5 minutes.

#. In **Prism** > **File Server** > click **File Analytics**

   .. figure:: images/33.png

#. On the Enable File Analytics page enter your domain administrator which is also your file server administrator.

   - **Username**: administrator
   - **Password**: default

   .. figure:: images/34.png

#. Select **Enable**
