# Basic Setup

Hyper-V will be the base technology used for this project. I have first set up two separate virtual switches: one external virtual switch that binds this virtual machine to my host machine's physical network card for easy access to the network, and a private virtual switch (the use for this which will be explained later). I have also created a virtual machine (VM) with Windows Server 2022 installed.

This Windows Server 2022 VM (which I have given it the hostname of WinServer 2022) will become my Domain Controller for my domain, ARASAKA.local, with Active Directory set up.<br/>
<img src="https://github.com/user-attachments/assets/bbcbda96-9529-43ea-b465-e8d4356d9784" Width="700" /> <br/>

Through Server Manager, I have also installed Windows Deployment Services and DHCP (which will be set up later). Since I have this Windows Server VM connected to the external virtual switch, I went ahead and downloaded the necessary files to set up Microsoft Deployment Toolkit (MDT):
- [The MDT installation package](https://www.microsoft.com/en-us/download/details.aspx?id=54259)
- [ADK for Windows 10, version 1903](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)
- 7-Zip (which will be added to the Windows installation image that we will generate using MDT so that it can be installed during imaging)
  - Note that I also created a folder and placed the installer in it. This folder/directory will be used to add to our Windows image later on.

# Microsoft Deployment Toolkit (MDT) Setup

Let's quickly install MDT.<br/>

1. Run the MDT installer. You should see the following: <br/>
<img src="https://github.com/user-attachments/assets/28d1b0fb-2e5f-4067-9ea1-599baf167d77" Width="500" /> <br/>
Click `Next`.

2. Read the End-User License Agreement, check the `I accept the terms in the License Agreement` box if you agree, and click `Next`. <br/>
<img src="https://github.com/user-attachments/assets/106ed0ad-4927-449b-abc8-7322b500b8f3" Width="435" /> <br/>

3. In `Custom Setup` page, make any changes you need for the way features will be installed then click `Next`. <br/>
<img src="https://github.com/user-attachments/assets/d0352ea0-6895-45a2-9fc8-540c9a66c521" Width="512" /> <br/>

4. In `Customer Experience Improvement Program` page, click `Yes` or `No` and then click `Next`. <br/>
<img src="https://github.com/user-attachments/assets/78ee8d8f-97d7-4bed-bf7e-7c7c0e102182" Width="440" /> <br/>

5. In `Ready to install Microsoft Deployment Toolkit` page, click on `Install`. <br/>
<img src="https://github.com/user-attachments/assets/40973e53-15ed-4bb0-8024-6f93a2232a5e" Width="597" /> <br/>

6. After installation is completed, click on `Finish`. <br/>
<img src="https://github.com/user-attachments/assets/27281e72-3051-436d-a4a8-4e4ca2cde415" Width="447" /> <br/>

# Installing Assessment Deployment Kit

With MDT installed, let's install the 2 ADK files we downloaded earlier.

Installing Windows Assessment and Deployment Kit (ADK):
1. Run the ADK installer. <br/>
<img src="https://github.com/user-attachments/assets/f8902c91-446e-42e0-936e-77d40056e0e1" Width="500" /> <br/>

2. Specify install path and click `Next`. <br/>
<img src="https://github.com/user-attachments/assets/eb89be22-b12e-4e38-bbd9-bcfee78cc2ce" Width="500" /> <br/>

3. Agree or decline sending usage data to Microsoft and click `Next`. <br/>
<img src="https://github.com/user-attachments/assets/95bd01b1-e8c6-431f-91b5-3882f49926c0" Width="500" /> <br/>

4. Read the license agreement. If you agree, click on `Accept`. <br/>
<img src="https://github.com/user-attachments/assets/228f2bbf-9480-4efe-ab1e-a32ff255bb18" Width="500" /> <br/>

5. Select or unselect the features you would like installed and click on `Install`. I left it as is for this project. <br/>
<img src="https://github.com/user-attachments/assets/8f3aa3b6-7b56-4340-aba0-bb6879ca762e" Width="500" /> <br/>

6. Close the installer once installation has finished. <br/>
<img src="https://github.com/user-attachments/assets/abbd5dc0-89ab-4339-8c75-c7b15397d1b6" Width="500" /> <br/>

Installing Windows Assessment and Deployment Kit Windows Preinstallation Environment Add-On (ADK WinPE):
1. Run the ADK WinPE installer. <br/>
<img src="https://github.com/user-attachments/assets/ab5e60ff-fc0d-4b48-a3a4-0063ac11fed7" Width="500" /> <br/>

2. Specify install path and click `Next`. <br/>
<img src="https://github.com/user-attachments/assets/57b9708f-5493-4d39-b5b4-72781a6172d2" Width="500" /> <br/>

3. Agree or decline sending usage data to Microsoft and click `Next`. This should already be selected for you based on what you selected when installing ADK previously. <br/>
<img src="https://github.com/user-attachments/assets/4c9284cd-e22c-4d82-8e09-0f97728aea25" Width="500" /> <br/>

4. Agree or decline sending usage data to Microsoft and click `Next`. <br/>
<img src="https://github.com/user-attachments/assets/a38b5b58-88f5-4b39-86de-5bd9a726e1e2" Width="500" /> <br/>

5. Simply click `Install`. There is only one thing to install here, which is WinPE. <br/>
<img src="https://github.com/user-attachments/assets/6bebbcbc-a16c-4457-8d40-4295bdd033c9" Width="500" /> <br/>

6. Close the installer once installation has finished. <br/>
<img src="https://github.com/user-attachments/assets/a7539422-1de2-4fee-a0ef-671b0f09b3ba" Width="500" /> <br/>

# DHCP Configuration on Windows Server/Domain Controller

Now that MDT has been set up, the next thing to work on is the network for PXE boot. First, in the settings for the Windows Server VM, I changed the **external** virtual switch to a **private** virtual switch. The virtual machine/computer that we will be imaging will also be using this same private virtual switch. By having our virtual machines connected to this private virtual switch, we are creating an isolated network from our external physical network. <br/>
<img src="https://github.com/user-attachments/assets/29267b3d-d499-4d66-8f11-42091cd83894" Width="500" /> <br/>
Doing so will allow a computer performing PXE boot to pick up an IP address from our Windows Server machine which will act as a DHCP server, AND allow the PC to communicate with the Domain Controller **directly**. Otherwise, using the external virtual switch means that our virtual machines will be receiving/communicating through the DHCP server in our real-world physical network instead, which causes issues with machines not being able to join the domain during the imaging process. <br/>

Before I can setup a DHCP server on our Windows Server machine, I need to assign a static IP address to the Windows Server machine first. For this project, I have decided on a Class B IP address. My IP configuration for the Windows Server is below: <br/>
<img src="https://github.com/user-attachments/assets/9b768913-a9a7-4174-ba78-f64e25ac65c3" Width="500" /> <br/>
Note that since I am  only using one Domain Controller for this project, I have set the DNS to 127.0.0.1, a loopback address. This allows the Domain Controller to resolve its own address/hostname, and at the same time, keeping address resolutions local.

With a static address assigned to the Windows Server/Domain Controller machine, we can finally set up the DHCP sever on this machine. Because this Windows Server machine's subnet is 255.255.0.0, our DHCP scope should contain 172.16.x.x in the first two octets. 
To set up DHCP:
1. Install the DHCP Server role through Server Manager if you have yet to do so
2. Once you do so, launch the DHCP Server too. You should see something akin to the following image:
3. Right-click on `IPv4` and select to create a new scope. This should open up the New Scope Wizard. Simply click `Next`.
4. On the `Scope Name` page, give your scope a name and description. I gave it a name of `Arasaka Scope`, but left the description blank.
5. On the `IP Address Range` page, define the IP Address range for this scope. These are the addresses that computers connecting to the domain will be assigned. My IP Address range for this scope will be 172.16.1.1 through 172.16.1.254 with a subnet mask of 255.255.0.0.
6. On the `Add Exclusions and Delay` page, designate any IP Addresses that you do not want to DHCP server to assign to any computer. I have added 172.16.1.1, which is an address used for the default gateway.
7. On the `Lease Duration` page, designate how long an IP Address will be leased to a computer. I have left it at 8 days.
8. On the `Configure DHCP Options` page, select `Yes` to configure DHCP options and click `Next`.
9. On the `Router (Default Gateway)` page, designate the gateway address that your DHCP clients will use.
10. On the `Domain Name and DNS Servers` page, this should have autofilled in the name of your domain and the IP address of your domain controller. If it wasn't autofill, enter in the name of your domain and the IP address of your domain controller and click `Next`.
11. On the `WINS Server` page, you can simply click next.
12. On the `Activate Scope` page, select to activate scope and click `Next`.
13. Click `Finish` and your DHCP scope should be setup and active.

# Deployment Workbench

## Creating a New Deployment Share
With everything setup, it is time to finally configure out Windows image. We will first be working with Deployment Workbench (MDT), so go ahead and open that on your Windows Server machine. It should look like the following: <br/>
[insert image] <br/>

1. On the left-most column of Deployment Workbench, right-click on `Deployment Shares` and click on `New Deployment Share`. This should open up the `New Deployment Share Wizard`.
2. On the `Path` page, determine where you want your deployment share to be stored in. I left it as default.
3. On the `Share` page, determine a name for your deployment share. Again, I left it as default.
4. On the `Descriptive Name` page, give your deployment share a description. I also left this as default.
5. On the `Options` page, enable or disable any of the wizard panes listed (this can be configured again later after the deployment share has been made).
6. Review the summary page, and execute the creation of this deployment share if everything looks good to you.
7. Once everything is finished, you should see new directories on the left column of Deployment Workbench like in the following screenshot.

## Import Operating System
Once the deployment share has been created, we will proceed to add an operating system to this deployment share. I will be using a Windows 10 iso file for this project.

1. Back on the `Deployment Workbench` homepage, on the left column, right-click on `Operating System` and then click on `Import Operating System`. This will open the `Import Operating System Wizard`.
2. On the `OS Type` page, select which type of files you will be using to import your operating system. I will be going with `Full set of source files`.
3. Before we proceed, we will need to mount the iso/disc image file of Windows 10. So, locate that on our local drive, right-click it, and then click on `Mount`. This will add it as a new drive with its own drive letter that you can easily see in Windows Explorer. For me, the iso file mounted as an E: drive
4. Back to the OS import wizard, move to the next page, the `Source` page. Click on `Browse...` which will open up a new window for you to select the location for where the OS source file is stored. Since ours is the E: drive, I selected just that.
5. After doing so, you will be moved to the `Destination` page. Here, you will designate a name for the directory that the OS files will be placed in. I named mine `Windows 10 x64`.
6. Review the summary and execute the action.
7. Once execution finishes, and you close the wizard, head back to the Operating Systems directory for Deployment Workbench and you will see the operating systems you have imported.

## Add Application
Remember at the beginning of this documentation where I installed the .msi version of the 7-Zip installer and put it in its own folder named `7-zip`? That's becomes relevant now, as I add applications to this Windows image that will be installed during the imaging process.

1. On the homepage of Deployment Workbench, right-click on `Applications` located on the left column, and click on `New Application`. This opens the `New Application Wizard`.
2. On the `Application Type` page, select the most appropriate option that applies to you. Since I have the full source file for 7-Zip, I chose `Application with source files`.
3. On the `Details` page, fill out however much you can. Every box is optional except Application Name.
4. On the `Source` page, click on `Browse...` and select the directory where you application is stored. Mine is the 7-Zip folder I created at the beginning that contains the msi installer for the program.
5. On the `Destination` page, designate a name for the directory that will be created. I named mine `7-Zip`.
6. On the `Command Details` page, enter in the command that will be run during the imaging process to install the application. Since my 7-Zip installer is an msi file, I will be using the `msiexec.exe` command. The full command I entered is `msiexec.exe /i 7z2409-x64 /q`. The `/i` parameter indicates that it will run as a normal installation and the `/q` parameter specifies that a user intervention will not be needed. The `7z2409-x64` is the name of the installer that will be run by the msiexec command.
7. Review the summary, and execute.
8. Once done, on the Application page of Deployment Workbench, you should see the application listed that you have just added.

## Task Sequence

We will next set up the task sequence, which is a set of predetermined instructions for how Windows will be deployed (it pretty much automates the process of installing Windows).

1. On the homepage of Deployment Workbench, right-click on `Task Sequences` on the left column, and click on `New Task Sequence`. This opens up the `New Task Sequence Wizard`.
2. On the `General Settings` page, designate a task sequence ID and task sequence name. I opted for something simple, going with Win10 and Windows 10 Installation respectively.
3. On the `Select Template` page, choose a task sequence template. I opted for the Standard Task Sequence Client template.
4. On the `Select OS` page, select the Windows version you want to use/install. What you will see here depends on the operating system files imported to the Deployment Workbench earlier. I chose Windows 10 Pro.
5. On the `Specify Product Key` page, choose the most appropriate choice that suits your situation. As this is nothing more than a test environment, I am not specifying any product keys.
6. On the `OS Settings` page, fill out each text box.
7. On the `Admin Password` page, choose whether to setup a local admin password.
8. Review the summary and complete the set up.
9. Once the process is complete, you should see the task sequence listed on Deployment Workbench.

## Deployment Share Properties

Now that we have imported our operating system, added applications to be downloaded during the imaging process, and created our task sequence, it is time to finalize our deployment share.

1. On on the homepage of Deployment Workbench, right-click on `MDT Deployment Share`, and click on `Properties`. This opens up the properties window for our deployment share.
2. On the `General` tab, you can uncheck the x86 box. Doing so, however, means that your deployment will not work on x86 systems (which at this point in time, majority of systems should already be x64.
3. On the `Rules` tab, it contains a list of instructions and parameters that MDT will adhere to such as skipping Bitlocker encryption during imaging. This is commonly known as the customsettings.ini file. I set mine up as such:
4. After configuring your rules, click on the `Edit Bootstrap.ini` button on the bottom right corner. Bootstrap.ini file is very similar to the customsettings.ini file, except that it is processed first when you launch into WinPE during the imaging process. I believe it is best to keep bootstrap.ini as simple as possible because anytime changes are made, you have generate a new installation media of your deployment share. Mine is the following:
5. On the `Windows PE` tab, I have opted to generate an iso/installation media.

With that, we can click `Apply` and then `Ok` to close the Properties window.

## Generating Boot Media

Now that everything is set and done, we want to create our boot media.

1. On on the homepage of Deployment Workbench, right-click on `MDT Deployment Share`, and click on `Update Deployment Share`. This opens the `Update Deployment Share` window.
2. On the `Options` page, you have 2 options: 1) to add any changes you have made to an already generated boot media, or 2) generate a completely new boot media. I chose the latter.
3. Review the summary, and generate your boot media
