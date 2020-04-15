.. title:: Files

---------------------------
Share managment and analytics
---------------------------

*The estimated time to complete this lab is 45 minutes.*


Managing SMB Shares
+++++++++++++++++++

In this exercise you will create and test a SMB share, used to support the unstructured file data needs of a cross-departmental team for the Fiesta application.

Creating the Share
..................

Configuring SMB Home Share
+++++++++++++++++++++++++++

In **Prism** > **File Server**, click **+Share/Export**. 

Fill out the following fields and click Next:
- **Name** – home
- **File Server**- POCxx-Files
- **Select Protocol** - SMB
 
 
.. image:: images/image019.png


#. Select **Enable Access Based Enumeration** and **Self Service Restore**. Select **Blocked File Types** and enter a comma separated list of extensions like .flv,.mov.

   .. figure:: images/3.png

   .. note::

      **Access Based Enumeration (ABE)** ensures that only files and folders which a given user has read access are visible to that user. This is commonly enabled for Windows file shares.

      **Self Service Restore** allows users to leverage Windows Previous Version to easily restore individual files to previous revisions based on Nutanix snapshots.

      **Blocked File Types** allow Files administrators to restrict certain types of files (such as large, personal media files) from being written to corporate shares. This can be configured on a per Server or per Share basis, with per Share settings overriding Server wide settings.

#. Click **Next**.

#. Review the **Summary** and click **Create**.

   .. figure:: images/4.png

   It is common for shares utilized by many people to leverage quotas to ensure fair use of resources. Files offers the ability to set either soft or hard quotas on a per share basis for either individual users within Active Directory, or specific Active Directory Security Groups.

#. In **Prism Element > File Server > Share/Export**, select your share and click **+ Add Quota Policy**.

#. Fill out the following fields and click **Save**:

   - Select **Group**
   - **User or Group** - SSP Developers
   - **Quota** - 10 GiB
   - **Enforcement Type** - Hard Limit

   .. figure:: images/9.png

#. Click **Save**.

Testing the Share
.................

#. Connect to your *Initials*\ **-WinTools** VM via VM console as a **non-Administrator NTNXLAB** domain account:

   .. note::

      You will not be able to connect using these accounts via RDP.

   - user01 - user25
   - devuser01 - devuser25
   - operator01 - operator25
   - **Password** nutanix/4u

   .. figure:: images/16.png

   .. note::

     The Windows Tools VM has already been joined to the **NTNXLAB.local** domain. You could use any domain joined VM to complete the following steps.

#. Open ``\\BootcampFS.ntnxlab.local\`` in **File Explorer**.

#. Open a browser within your *Initials*\ **-WinTools** desktop and download sample data to populate in your share:

   - **If using a PHX cluster** - http://10.42.194.11/workshop_staging/peer/SampleData_Small.zip
   - **If using a RTP cluster** - http://10.55.251.38/workshop_staging/peer/SampleData_Small.zip

#. Extract the contents of the zip file into your file share.

   .. figure:: images/5.png

   - The **NTNXLAB\\Administrator** user was specified as a Files Administrator during deployment of the Files cluster, giving it read/write access to all shares by default.
   - Managing access for other users is no different than any other SMB share.

..   #. From ``\\BootcampFS.ntnxlab.local\``, right-click *Initials*\ **-FiestaShare > Properties**.

   #. Select the **Security** tab and click **Advanced**.

      .. figure:: images/6.png

   #. Select **Users (BootcampFS\\Users)** and click **Remove**.

   #. Click **Add**.

   #. Click **Select a principal** and specify **Everyone** in the **Object Name** field. Click **OK**.

      .. figure:: images/7.png

   #. Fill out the following fields and click **OK**:

      - **Type** - Allow
      - **Applies to** - This folder only
      - Select **Read & execute**
      - Select **List folder contents**
      - Select **Read**
      - Select **Write**

      .. figure:: images/8.png

   #. Click **OK > OK > OK** to save the permission changes.

   All users will now be able to create folders and files within the *Initials*\ **-FiestaShare** share.

#. Open **PowerShell** and try to create a file with a blocked file type by executing the following command:

   .. code-block:: PowerShell

      New-Item \\BootcampFS\INITIALS-FiestaShare\MyFile.flv

   Observe that creation of the new file is denied.

#. Return to **Prism Element > File Server > Share/Export**, select your share. Review the **Share Details**, **Usage** and **Performance** tabs to understand the high level information available on a per share basis, including the number of files & connections, storage utilization over time, latency, throughput, and IOPS.

   .. figure:: images/11.png

   In the next exercise, you will see how Files can provide further insights into usage of each File Server and Share.

File Analytics
++++++++++++++

In this exercise you will explore the new, integrated File Analytics capabilities available in Nutanix Files, including scanning existing shares, creating anomaly alerts, and reviewing audit details. File Analytics is deployed in minutes as a standalone VM through an automated, One Click operation in Prism Element. This VM has already been deployed and enabled in your environment.

#. In **Prism Element > File Server > File Server**, select **BootcampFS** and click **File Analytics**.

   .. figure:: images/12.png

   .. note::

      File Analytics should already be enabled, but if prompted you will need to provide your Files administrator account, as Analytics will need to be able to scan all shares.

      - **Username**: NTNXLAB\\administrator
      - **Password**: nutanix/4u

      .. figure:: images/old13.png

#. As this is a shared environment, the dashboard will likely already be populated with data from shares created by other users. To scan your newly created share, click :fa:`gear` **> Scan File System**. Select your share and click **Scan**.

   .. figure:: images/14.png

   .. note::

      If your share is not shown, please give it some time to get populated...

#. Close the **Scan File System** window and refresh your browser.

#. You should see the **Data Age**, **File Distribution by Size** and **File Distribution by Type** dashboard panels update.

   .. figure:: images/15.png

   Under....

#. From your *Initials*\ **-WinTools** VM, create some audit trail activity by opening several of the files under **Sample Data**.

   .. note:: You may need to complete a short wizard for OpenOffice if using that application to open a file.

#. Refresh the **Dashboard** page in your browser to see the **Top 5 Active Users**, **Top 5 Accessed Files** and **File Operations** panels update.

   .. figure:: images/17.png

#. To access the audit trail for your user account, click on your user under **Top 5 Active Users**.

   .. figure:: images/17b.png

#. Alternatively, you can select **Audit Trails** from the toolbar and search for your user or a given file.

   .. figure:: images/18.png

   .. note::

      You can use wildcards for your search, for example **.doc**
..
   #. Next, we will create rules to detect anomalous behavior on the File Server. From the toolbar, click :fa:`gear` **> Define Anomaly Rules**.

      .. figure:: images/19.png

      .. note::

         Anomaly Rules are defined on a per File Server basis, so the below rules may have already been created by another user.

   #. Click **Define Anomaly Rules** and create a rule with the following settings:

      - **Events:** Delete
      - **Minimum Operation %:** 1
      - **Minimum Operation Count:** 10
      - **User:** All Users
      - **Type:** Hourly
      - **Interval:** 1

   #. Under **Actions**, click **Save**.

   #. Choose **+ Configure new anomaly** and create an additional rule with the following settings:

      - **Events**: Create
      - **Minimum Operation %**: 1
      - **Minimum Operation Count**: 10
      - **User**: All Users
      - **Type**: Hourly
      - **Interval**: 1

   #. Under **Actions**, click **Save**.

      .. figure:: images/20.png

   #. Click **Save** to exit the **Define Anomaly Rules** window.

   #. To test the anomaly alerts, return to your *Initials*\ **-WinTools** VM and make a second copy of the sample data (via Copy/Paste) within your *Initials*\ **-FiestaShare** share.

   #. Delete the original sample data folders.

      .. figure:: images/21.png

      While waiting for the Anomaly Alerts to populate, next we’ll create a permission denial.

      .. note:: The Anomaly engine runs every 30 minutes.  While this setting is configurable from the File Analytics VM, modifying this variable is outside the scope of this lab.

   #. Create a new directory called *Initials*\ **-MyFolder** in the *Initials*\ **-FiestaShare** share.

   #. Create a text file in the *Initials*\ **-MyFolder** directory and take out your deep seeded worldly frustrations on your for a few moments to populate the file. Save the file as *Initials*\ **-file.txt**.

      .. figure:: images/22.png

   #. Right-click *Initials*\ **-MyFolder > Properties**. Select the **Security** tab and click **Advanced**. Observe that **Users (BootcampFS\\Users)** lack the **Full Control** permission, meaning that they would be unable to delete files owned by other users.

      .. figure:: images/23.png

   #. Open a PowerShell window as another non-Administrator user account by holding **Shift** and right-clicking the **PowerShell** icon in the taskbar and selecting **Run as different user**.

      .. figure:: images/24.png

   #. Change Directories to *Initials*\ **-MyFolder** in the *Initials*\ **-FiestaShare** share.

        .. code-block:: bash

           cd \\BootcampFS.ntnxlab.local\XYZ-FiestaShare\XYZ-MyFolder

   #. Execute the following commands:

        .. code-block:: bash

           cat .\XYZ-file.txt
           rm .\XYZ-file.txt

      .. figure:: images/25.png

   #. Return to **Analytics > Dashboard** and note the **Permission Denials** and **Anomaly Alerts** widgets have updated.

      .. figure:: images/26.png

   #. Under **Permission Denials**, select your user account to view the full **Audit Trail** and observe that the specific file you tried to removed is recorded, along with IP address and timestamp.

      .. figure:: images/27.png

   #. Select **Anomalies** from the toolbar for an overview of detected anomalies.

      .. figure:: images/28.png

File Analytics puts simple, yet powerful information in the hands of storage administrators, allowing them to understand and audit both utilization and access within a Nutanix Files environment.


