# Basic Setup

Hyper-V will be the base technology used for this project. I have first set up two separate virtual switches: one external virtual switch that binds this virtual machine to my host machine's physical network card for easy access to the network, and a private virtual switch (the use for this which will be explained later). I have also created a virtual machine (VM) with Windows Server 2022 installed.

This Windows Server 2022 VM (which I have given it the hostname of WinServer 2022) will become my domain controller for my domain, ARASAKA.local, with Active Directory set up.<br/>
<img src="https://github.com/user-attachments/assets/bbcbda96-9529-43ea-b465-e8d4356d9784" Width="700" /> <br/>

Through Server Manager, I have also installed Windows Deployment Services and DHCP (which will be set up later). Since I have this Windows Server VM connected to the external virtual switch, I went ahead and downloaded the necessary files to set up Microsoft Deployment Toolkit (MDT):
- [The MDT installation package](https://www.microsoft.com/en-us/download/details.aspx?id=54259)
- [ADK for Windows 10, version 1903](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)

# Microsoft Deployment Toolkit (MDT) Setup

Let's quickly install MDT.<br/>

1. Run the MDT installer. You should see the following: <br/>
<img src="https://github.com/user-attachments/assets/28d1b0fb-2e5f-4067-9ea1-599baf167d77" Width="700" /> <br/>
Click `Next`.

2. Read the End-User License Agreement, check the `I accept the terms in the License Agreement` box if you agree, and click `Next`. <br/>
![6  MDT Setup Part 2](https://github.com/user-attachments/assets/106ed0ad-4927-449b-abc8-7322b500b8f3)

3. In `Custom Setup` page, make any changes you need for the way features will be installed then click `Next`.
4. In `Customer Experience Improvement Program` page, click `Yes` or `No` and then click `Next`.
5. In `Ready to install Microsoft Deployment Toolkit` page, click on `Install`.
6. After installation is completed, click on `Finish`.

With MDT installed, let's install the 2 ADK files we downloaded earlier.

