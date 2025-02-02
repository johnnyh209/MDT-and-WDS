This final part will be imaging a system using PXE Boot. Since I am using a virtual environment, I had to make sure that this system I am imaging is using the **same** private virtual switch that our Windows Server is on. This way, this VM is in the same network so that it can communicate with our server in order to PXE boot.

## Imaging Using PXE Boot
1. Upon starting up the VM, it attempts to establish connection with our server. Once that connection is established, press F12 for network boot. <br/>
<img src="https://github.com/user-attachments/assets/0624a46a-84bc-4005-864e-6855aac0b989" Width="500" /> <br/>

2. Wait for the boot files to load. You will notice that IP listed is the IP address of our server, and shows you the file path of the boot media. <br/>
<img src="https://github.com/user-attachments/assets/d5b500a8-9e8e-4e35-a575-eb4df141deed" Width="500" /> <br/>

3. Once the file is loaded, you will be presented with the `Windows Deployment Wizard`. On the `Task Sequence` page, select the Task Sequence to run. I only created one, so that is the one I selected. <br/>
<img src="https://github.com/user-attachments/assets/cdc542a9-11a0-4ac5-b8a4-8b87bd47de3e" Width="500" /> <br/>

4. On the `Computer Details` page, you can set the hostname of this machine you are imaging, and to choose to have this system join a domain or workgroup. Since during setup I already designated clients to join the active directory domain that I set up, `Join a domain` is automatically selected, and all information pertaining to the domain and OU, along with the admin credentials have already been prefilled (this is the information that we set in the Bootstrap.ini and the Customsettings.ini files from the Deployment Workbench). <br/>
<img src="https://github.com/user-attachments/assets/b28bfaac-7a3d-4554-927c-01dbf7764f7d" Width="500" /> <br/>

5. On the `Applications` page, choose the applications you want installed during the imaging process. <br/>
<img src="https://github.com/user-attachments/assets/9c5011c0-d150-426b-b5f0-b4f8c08e7509" Width="500" /> <br/>

6. Once done, the installation process begins.

7. Once installation has finished, you will be on the desktop of Windows and presented with a deplopyment summary. The entire process was successful, and thus had 0 errors or warnings presented in the summary.
![7  Getting Ready](https://github.com/user-attachments/assets/9480e521-62c6-44a1-92aa-af11e4fbe98e)
![8  Finished Installing](https://github.com/user-attachments/assets/83b53c9b-8849-41a6-9838-5a48b50112f5)

## Confirming System Is In Domain
1. Checking the network, our newly imaged system is in our domains' network, ARASAKA.local. Also, 7Zip has been installed during the process as well.
![10  Part of Domain](https://github.com/user-attachments/assets/4c171e52-dc78-416d-b17a-24ef09d55aa5)
![12  App Installed](https://github.com/user-attachments/assets/22d562a0-6506-4277-98d9-a2974ac7bc85)

2. Checking Active Directory Users and Computers, we see that the system has been added to the OU we designated in our Customesettings.ini file.
![11  Checking AD](https://github.com/user-attachments/assets/2289cc44-c070-4bdf-9d31-d9723ecba040)
