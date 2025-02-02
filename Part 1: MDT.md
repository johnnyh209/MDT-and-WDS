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

2. Once you do so, launch the DHCP Server too. You should see something akin to the following image: <br/>
<img src="https://github.com/user-attachments/assets/797fbc52-90ea-4fc8-90e2-b853534ee600" Width="500" /> <br/>

3. Right-click on `IPv4` and select to create a new scope. This should open up the New Scope Wizard. Simply click `Next`.
![72  DHCP Scope Part 2](https://github.com/user-attachments/assets/bd1edbae-45cd-461d-b5a0-39d5b74ef61f)

4. On the `Scope Name` page, give your scope a name and description. I gave it a name of `Arasaka Scope`, but left the description blank.
![73  DHCP Scope Part 3](https://github.com/user-attachments/assets/9c5e7d3a-5bdb-41a1-8088-9d23ddfd4336)

5. On the `IP Address Range` page, define the IP Address range for this scope. These are the addresses that computers connecting to the domain will be assigned. My IP Address range for this scope will be 172.16.1.1 through 172.16.1.254 with a subnet mask of 255.255.0.0.
![Scope Option 2](https://github.com/user-attachments/assets/46cbf6cd-d908-4f07-94bd-82aed49f82e2)

6. On the `Add Exclusions and Delay` page, designate any IP Addresses that you do not want to DHCP server to assign to any computer. I have added 172.16.1.1, which is an address used for the default gateway.
![75  DHCP Scope Part 5](https://github.com/user-attachments/assets/7e9e409a-6582-4790-b768-6e11c50cf860)

7. On the `Lease Duration` page, designate how long an IP Address will be leased to a computer. I have left it at 8 days.
![76  DHCP Scope Part 6](https://github.com/user-attachments/assets/77ddd1dd-ac7f-4262-aedf-dd9da75b162e)

8. On the `Configure DHCP Options` page, select `Yes` to configure DHCP options and click `Next`.
![77  DHCP Scope Part 7](https://github.com/user-attachments/assets/2b631caf-0945-45fa-8504-8042812eba41)

9. On the `Router (Default Gateway)` page, designate the gateway address that your DHCP clients will use.
![78  DHCP Scope Part 8](https://github.com/user-attachments/assets/8d456a83-e4b1-4c70-aa24-e173d5b6ad6f)

10. On the `Domain Name and DNS Servers` page, this should have autofilled in the name of your domain and the IP address of your domain controller. If it wasn't autofill, enter in the name of your domain and the IP address of your domain controller and click `Next`.
![79  DHCP Scope Part 9](https://github.com/user-attachments/assets/f9f8cf86-6ceb-4dbc-8ffe-5b5e66f57bb0)

11. On the `WINS Server` page, you can simply click next.
![80  DHCP Scope part 10](https://github.com/user-attachments/assets/b5c75c47-2018-425d-9efe-d0c0f5750989)

12. On the `Activate Scope` page, select to activate scope and click `Next`.
![81  DHCP Scope Part 11](https://github.com/user-attachments/assets/1c91a46e-7848-4c79-8c1e-a0ebc4de1ed5)

13. Click `Finish` and your DHCP scope should be setup and active.
![82  DHCP Scope part 12](https://github.com/user-attachments/assets/a992c23e-6c53-493f-8799-30f7c1f50541)
![83  DHCP Scope Part 13](https://github.com/user-attachments/assets/31ff3fdc-ecab-44f4-a2c2-5a3638b9ab72)

**You do also want to make sure that your server has a static IP address**

## Creating a New Deployment Share
With everything setup, it is time to finally configure out Windows image. We will first be working with Deployment Workbench (MDT), so go ahead and open that on your Windows Server machine. It should look like the following: <br/>
![23  Open Deployment WorkBench](https://github.com/user-attachments/assets/99803355-d177-4af8-b27b-96cb5416d9c6)

1. On the left-most column of Deployment Workbench, right-click on `Deployment Shares` and click on `New Deployment Share`. This should open up the `New Deployment Share Wizard`.
![24  New Deployment Share](https://github.com/user-attachments/assets/94ab8c14-16b5-468b-9469-61010e8beaea)
![25  New Deployment Share Part 2](https://github.com/user-attachments/assets/e429b9ae-38fd-4343-a587-cb09ba10a21c)

2. On the `Path` page, determine where you want your deployment share to be stored in. I left it as default.
![26  New Deployment Share part 3](https://github.com/user-attachments/assets/85e648a5-b373-48b3-bb14-741c1b2f4bc4)

3. On the `Share` page, determine a name for your deployment share. Again, I left it as default.
![27  New Deployment Share Part 4](https://github.com/user-attachments/assets/f51328bd-6aea-4cfd-9546-acc6138455e9)

4. On the `Descriptive Name` page, give your deployment share a description. I also left this as default.
![28  New Deployment Share Part 5](https://github.com/user-attachments/assets/804d1ad6-e1a3-403f-809b-87210b8ade95)

5. On the `Options` page, enable or disable any of the wizard panes listed (this can be configured again later after the deployment share has been made).
![29  New Deployment Share Part 6](https://github.com/user-attachments/assets/6d5cce99-119d-44ca-88f2-0c24b96ba8a9)

6. Review the summary page, and execute the creation of this deployment share if everything looks good to you.
![30  New Deploymentshare Part 7](https://github.com/user-attachments/assets/ed797d7c-2999-40bf-835b-2b2700ab4c70)
![31  New Deployment Share Part 8](https://github.com/user-attachments/assets/11842dff-a887-40a6-96ea-61647171c520)

7. Once everything is finished, you should see new directories on the left column of Deployment Workbench like in the following screenshot.
![32  What you should see now](https://github.com/user-attachments/assets/487074a5-dac1-458e-b325-a79e5d9408ab)

## Import Operating System
Once the deployment share has been created, we will proceed to add an operating system to this deployment share. I will be using a Windows 10 iso file for this project.

1. Back on the `Deployment Workbench` homepage, on the left column, right-click on `Operating System` and then click on `Import Operating System`. This will open the `Import Operating System Wizard`.
![33  Import OS Part 1](https://github.com/user-attachments/assets/e11394a0-ae64-4969-8201-433e210b6a19)

2. On the `OS Type` page, select which type of files you will be using to import your operating system. I will be going with `Full set of source files`.
![34  Import OS Part 2](https://github.com/user-attachments/assets/2c352d55-7b1d-446d-a965-cbe7b9556b46)

3. Before we proceed, we will need to mount the iso/disc image file of Windows 10. So, locate that on our local drive, right-click it, and then click on `Mount`. This will add it as a new drive with its own drive letter that you can easily see in Windows Explorer. For me, the iso file mounted as an E: drive
![35  Import OS Part 3](https://github.com/user-attachments/assets/37d1cee3-7d67-4a3a-b0e2-15e4fb124436)
![36  Import OS Part 4](https://github.com/user-attachments/assets/65a4b1e4-e050-4c0d-8ad8-aa471080ac40)

4. Back to the OS import wizard, move to the next page, the `Source` page. Click on `Browse...` which will open up a new window for you to select the location for where the OS source file is stored. Since ours is the E: drive, I selected just that.
![37  Import OS Part 5](https://github.com/user-attachments/assets/18dfc25a-6466-47de-95b3-e19987854e0e)
![38  Import OS Part 6](https://github.com/user-attachments/assets/0b773dca-b3a3-4a42-91a6-1853eb95be4c)

5. After doing so, you will be moved to the `Destination` page. Here, you will designate a name for the directory that the OS files will be placed in. I named mine `Windows 10 x64`.
![39  Import OS Part 7](https://github.com/user-attachments/assets/29c50051-e652-4259-a723-def4c8a89120)

6. Review the summary and execute the action.
![40  Import OS Part 8](https://github.com/user-attachments/assets/d8e8bdbf-7d85-4656-9d74-7cbefd3cae97)
![41  Import OS Part 9](https://github.com/user-attachments/assets/b247e485-d70d-44a9-ac41-ecc59a4e23ba)

7. Once execution finishes, and you close the wizard, head back to the Operating Systems directory for Deployment Workbench and you will see the operating systems you have imported.
![42  Import OS Part 10](https://github.com/user-attachments/assets/8c76bbab-7ca1-4aa2-9d63-e76a9fd6f601)

## Add Application
Remember at the beginning of this documentation where I installed the .msi version of the 7-Zip installer and put it in its own folder named `7-zip`? That's becomes relevant now, as I add applications to this Windows image that will be installed during the imaging process.

1. On the homepage of Deployment Workbench, right-click on `Applications` located on the left column, and click on `New Application`. This opens the `New Application Wizard`.
![45  Application Part 3](https://github.com/user-attachments/assets/cdd4c5f0-3647-4a13-82d5-22ba1fcf9502)

2. On the `Application Type` page, select the most appropriate option that applies to you. Since I have the full source file for 7-Zip, I chose `Application with source files`.
![46  Application Part 4](https://github.com/user-attachments/assets/9df84fb3-d915-4223-b882-a17e1459aab9)

3. On the `Details` page, fill out however much you can. Every box is optional except Application Name.
![47  Application Part 5](https://github.com/user-attachments/assets/a5392712-5175-41e2-a809-1375e4624528)

4. On the `Source` page, click on `Browse...` and select the directory where you application is stored. Mine is the 7-Zip folder I created at the beginning that contains the msi installer for the program.
![48  Application Part 6](https://github.com/user-attachments/assets/2bde4ce0-65cd-4073-89a6-6623575415d9)

5. On the `Destination` page, designate a name for the directory that will be created. I named mine `7-Zip`.
![49  Application Part 7](https://github.com/user-attachments/assets/4e19a145-ec93-45a0-93f7-9c70c38469f5)

6. On the `Command Details` page, enter in the command that will be run during the imaging process to install the application. Since my 7-Zip installer is an msi file, I will be using the `msiexec.exe` command. The full command I entered is `msiexec.exe /i 7z2409-x64 /q`. The `/i` parameter indicates that it will run as a normal installation and the `/q` parameter specifies that a user intervention will not be needed. The `7z2409-x64` is the name of the installer that will be run by the msiexec command.
![50  Application Part 8](https://github.com/user-attachments/assets/74c8ed23-94a9-4140-a5c3-cadf09b7c1be)

7. Review the summary, and execute.
![51  Application Part 9](https://github.com/user-attachments/assets/b5b15b3f-3b17-4fb2-a03e-01d22534cdd5)
![52  Application Part 10](https://github.com/user-attachments/assets/6644e8a5-b919-4bfc-a9e3-25d95735d84d)

8. Once done, on the Application page of Deployment Workbench, you should see the application listed that you have just added.
![53  Application Part 11](https://github.com/user-attachments/assets/5d2ffb00-48a3-4a95-b1a3-a80897627fdf)

## Task Sequence

We will next set up the task sequence, which is a set of predetermined instructions for how Windows will be deployed (it pretty much automates the process of installing Windows).

1. On the homepage of Deployment Workbench, right-click on `Task Sequences` on the left column, and click on `New Task Sequence`. This opens up the `New Task Sequence Wizard`.
![54  Task Sequence Part 1](https://github.com/user-attachments/assets/a8c06959-755d-4b52-adfb-013d8c2d20cf)

2. On the `General Settings` page, designate a task sequence ID and task sequence name. I opted for something simple, going with Win10 and Windows 10 Installation respectively.
![55  Task Sequence Part 2](https://github.com/user-attachments/assets/ecc91615-93fa-4477-94cc-aa1246b676c5)

3. On the `Select Template` page, choose a task sequence template. I opted for the Standard Task Sequence Client template.
![56  Task Sequence Part 3](https://github.com/user-attachments/assets/bddf8de0-130e-48f5-90dc-f8198f7f9a04)

4. On the `Select OS` page, select the Windows version you want to use/install. What you will see here depends on the operating system files imported to the Deployment Workbench earlier. I chose Windows 10 Pro.
![57  Task Sequence Part 4](https://github.com/user-attachments/assets/8b1e3344-b98f-4a6f-bb8f-b990c5b38935)

5. On the `Specify Product Key` page, choose the most appropriate choice that suits your situation. As this is nothing more than a test environment, I am not specifying any product keys.
![58  Task Sequence Part 5](https://github.com/user-attachments/assets/bb35d966-fb63-4259-8b19-bc662d0a1dab)

6. On the `OS Settings` page, fill out each text box.
![59  Task Sequence Part 6](https://github.com/user-attachments/assets/f94bca3c-e3de-45a4-9258-1731dd33189d)

7. On the `Admin Password` page, choose whether to setup a local admin password.
![60  Task Sequence Part 7](https://github.com/user-attachments/assets/d1309678-cda2-4085-b8f1-ca1e2ff395df)

8. Review the summary and complete the set up.
![61  Task Sequence Part 8](https://github.com/user-attachments/assets/6843982b-dfa9-4e83-8668-24fe49189700)
![62  Task Sequence Part 9](https://github.com/user-attachments/assets/13b0d022-bacc-4f9a-808a-36b12acd86b0)

9. Once the process is complete, you should see the task sequence listed on Deployment Workbench.
![63  Task Sequence Part 10](https://github.com/user-attachments/assets/3282e8c8-6ea8-4934-a19b-a9f51606c66e)

## Deployment Share Properties

Now that we have imported our operating system, added applications to be downloaded during the imaging process, and created our task sequence, it is time to finalize our deployment share.

1. On on the homepage of Deployment Workbench, right-click on `MDT Deployment Share`, and click on `Properties`. This opens up the properties window for our deployment share.
![64  Go to Properties](https://github.com/user-attachments/assets/e40fad26-6cba-4d5b-902a-bb609feda5f0)

2. On the `General` tab, you can uncheck the x86 box. Doing so, however, means that your deployment will not work on x86 systems (which at this point in time, majority of systems should already be x64.
![65  Uncheck x86](https://github.com/user-attachments/assets/035f13f0-a6a1-4b61-a3a8-de0284914cba)

3. On the `Rules` tab, it contains a list of instructions and parameters that MDT will adhere to such as skipping Bitlocker encryption during imaging. This is commonly known as the customsettings.ini file. I set mine up as such:
![66  Customsettings ini](https://github.com/user-attachments/assets/d05647e6-3e9f-480c-9f4c-df74fef3ea84)

4. After configuring your rules, click on the `Edit Bootstrap.ini` button on the bottom right corner. Bootstrap.ini file is very similar to the customsettings.ini file, except that it is processed first when you launch into WinPE during the imaging process. I believe it is best to keep bootstrap.ini as simple as possible because anytime changes are made, you have generate a new installation media of your deployment share. Mine is the following:
![67  Bootstrap ini](https://github.com/user-attachments/assets/216dd684-2602-49ac-8189-c79e9f306313)

5. On the `Windows PE` tab, I have opted to generate an iso/installation media.
![68  Iso_WinPE tab](https://github.com/user-attachments/assets/35736ef4-7e73-4b0d-8e5c-064e69864e94)

With that, we can click `Apply` and then `Ok` to close the Properties window.

## Generating Boot Media

Now that everything is set and done, we want to create our boot media.

1. On on the homepage of Deployment Workbench, right-click on `MDT Deployment Share`, and click on `Update Deployment Share`. This opens the `Update Deployment Share` window.
![84  Update Deployment Share](https://github.com/user-attachments/assets/ea42a711-e9a1-406a-9d89-145218748de2)

2. On the `Options` page, you have 2 options: 1) to add any changes you have made to an already generated boot media, or 2) generate a completely new boot media. I chose the latter.
![85  Update Deployment Share part 2](https://github.com/user-attachments/assets/a1ac1221-16c8-4398-9ebb-732e478535a3)

3. Review the summary, and generate your boot media
![86  Update Deployment Share Part 3](https://github.com/user-attachments/assets/b5d35df5-34be-4c46-9241-02219bcf3861)
![87  Update Deployment Share Part 4](https://github.com/user-attachments/assets/7651d3f3-a8d2-42c0-b941-3ae12a659e37)
![88  Update Deployment Share part 5](https://github.com/user-attachments/assets/85170420-d102-4cea-93ce-80d5f9217c9a)
