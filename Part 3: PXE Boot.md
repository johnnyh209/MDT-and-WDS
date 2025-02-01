This final part will be imaging a system using PXE Boot. Since I am using a virtual environment, I had to make sure that this system I am imaging is using the **same** private virtual switch that our Windows Server is on. This way, this VM is in the same network so that it can communicate with our server in order to PXE boot.

## Imaging Using PXE Boot
1. Upon starting up the VM, it attempts to establish connection with our server. Once that connection is established, press F12 for network boot.
2. Wait for the boot files to load. You will notice that IP listed is the IP address of our server, and shows you the file path of the boot media.
3. Once the file is loaded, you will be presented with the `Windows Deployment Wizard`. On the `Task Sequence` page, select the Task Sequence to run. I only created one, so that is the one I selected.
4. On the `Computer Details` page, you can set the hostname of this machine you are imaging, and to choose to have this system join a domain or workgroup. Since during setup I already designated clients to join the active directory domain that I set up, `Join a domain` is automatically selected, and all information pertaining to the domain and OU, along with the admin credentials have already been prefilled (this is the information that we set in the Bootstrap.ini and the Customsettings.ini files from the Deployment Workbench).
5. On the `Applications` page, choose the applications you want installed during the imaging process.
6. Once done, the installation process begins.
7. Once installation has finished, you will be on the desktop of Windows and presented with a deplopyment summary. The entire process was successful, and thus had 0 errors or warnings presented in the summary.

## Confirming System Is In Domain
1. Checking the network, our newly imaged system is in our domains' network, ARASAKA.local.
2. Checking Active Directory Users and Computers, we see that the system has been added to the OU we designated in our Customesettings.ini file.
