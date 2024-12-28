# Basic Setup

Hyper-V will be the base technology used for this project. I have first set up two separate virtual switches: one external virtual switch that binds this virtual machine to my host machine's physical network card for easy access to the network, and a private virtual switch (the use for this which will be explained later). I have also created a virtual machine (VM) with Windows Server 2022 installed.

This Windows Server 2022 VM (which I have given it the hostname of WinServer 2022) will become my Domain Controller for my domain, ARASAKA.local, with Active Directory set up.<br/>
<img src="https://github.com/user-attachments/assets/bbcbda96-9529-43ea-b465-e8d4356d9784" Width="700" /> <br/>

Through Server Manager, I have also installed Windows Deployment Services and DHCP (which will be set up later). Since I have this Windows Server VM connected to the external virtual switch, I went ahead and downloaded the necessary files to set up Microsoft Deployment Toolkit (MDT):
- [The MDT installation package](https://www.microsoft.com/en-us/download/details.aspx?id=54259)
- [ADK for Windows 10, version 1903](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)
- 7-Zip (which will be added to the Windows installation image that we will generate using MDT so that it can be installed during imaging)

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

Now that MDT has been set up, the next thing to work on is the network for PXE boot. First, in the settings for the Windows Server VM, I changed the virtual switch to a private virtual switch. The virtual machine/computer that we will be imaging will also be using this same private virtual switch. By having our virtual machines connected to this private virtual switch, we are creating an isolated network from our external physical network. <br/>
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
4. In the `Scope Name` page, give your scope a name and description. I gave it a name of `Arasaka Scope`, but left the description blank.
5. In the `IP Address Range` page, define the IP Address range for this scope. These are the addresses that computers connecting to the domain will be assigned. My IP Address range for this scope will be 172.16.1.1 through 172.16.1.254 with a subnet mask of 255.255.0.0.
6. In the `Add Exclusions and Delay` page, designate any IP Addresses that you do not want to DHCP server to assign to any computer. I have added 172.16.1.1, which is an address used for the default gateway.
7. In the `Lease Duration` page, designate how long an IP Address will be leased to a computer. I have left it at 8 days.
8. In the `Configure DHCP Options` page, select `Yes` to configure DHCP options and click `Next`.
9. In the `Router (Default Gateway)` page, designate the gateway address that your DHCP clients will use.
10. In the `Domain Name and DNS Servers` page, this should have autofilled in the name of your domain and the IP address of your domain controller. If it wasn't autofill, enter in the name of your domain and the IP address of your domain controller and click `Next`.
11. In the `WINS Server` page, you can simply click next.
12. In the `Activate Scope` page, select to activate scope and click `Next`.
13. Click `Finish` and your DHCP scope should be setup and active.
