# Basic Setup

Hyper-V will be the base technology used for this project. I have first set up two separate virtual switches: one external virtual switch that binds this virtual machine to my host machine's physical network card for easy access to the network, and a private virtual switch (the use for this which will be explained later). I have also created a virtual machine (VM) with Windows Server 2022 installed.

This Windows Server 2022 VM (which I have given it the hostname of WinServer 2022) will become my domain controller for my domain, ARASAKA.local, with Active Directory set up.
[Screenshot of Active Directory here]

Through Server Manager, I have also installed Windows Deployment Services and DHCP (which will be set up later). Since I have this Windows Server VM connected to the external virtual switch, I went ahead and downloaded the necessary files to set up Microsoft Deployment Toolkit (MDT):
- [The MDT installation package](https://www.microsoft.com/en-us/download/details.aspx?id=54259)
- [ADK for Windows 10, version 1903](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)

# Microsoft Deployment Toolkit (MDT) Setup
