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

