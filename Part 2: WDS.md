Now that we have already generated our boot media, our work using MDT/Deployment Workbench is complete for now. On this part, we will be working on WDS, where we add our newly generated boot media to our deployment server.

If you have not done so, add `Windows Deployment Services` using **Server Manager**.

## Configuring Deployment Server in Windows Deployment Services

1. Open Windows Deployment Services. On the left most column, expand `Servers` and right-click on the server you want to configure, and click `Configure Server`. I only have one listed, which is this installation of Window Server that I have been working on. Doing so should open the `Windows Deployment Services Configuration Wizard`. <br/>
<img src="https://github.com/user-attachments/assets/b65bc69d-39be-4bd9-8098-c6b6f7a256fa" Width="500" /> <br/>

2. Click `Next` after reading the `Before You Begin` page. <br/>
<img src="https://github.com/user-attachments/assets/55480150-197c-42a9-8c4d-d824185d3805" Width="500" /> <br/>

3. On the `Install Options` page, you have the option to configure the deployment server to be integrated with Active Directory, or as a standalone server. I already have Active Directory set up, so I opted to integrate. <br/>
<img src="https://github.com/user-attachments/assets/16f258f3-2b8c-438b-8c2d-94923f858962" Width="500" /> <br/>

4. On the `Remote Installation Folder Location` page, choose the file path for where you want the boot images, install images, PXE boot files, and WDS management tools will be stored. <br/>
<img src="https://github.com/user-attachments/assets/8e41ca22-f798-4675-8af3-59497c2fd662" Width="500" /> <br/>

5. On the `PXE Server Initial Settings` page, choose whether the server should respond to only known client, to not respond to any clients at all, or respond to all clients whether known or unknown. I opted to have the server to respond to all clients, but to require admin approval for unknown clients. <br/>
<img src="https://github.com/user-attachments/assets/69ae050c-83c0-4b21-bf78-81cb6b7eba4a" Width="500" /> <br/>

6. On the `Operation Complete` page, you can select to add images now. I opted to not, leaving the box unchecked and clicked on `Finish`. <br/>
<img src="https://github.com/user-attachments/assets/bc5990c4-976e-4973-83b8-0745aecd59b1" Width="500" /> <br/>

Now that the server has been configured, you should see 5 new directories like so: <br/>
<img src="https://github.com/user-attachments/assets/0e86202f-3ca1-4ec6-b3cd-78b6e9749a36" Width="500" /> <br/>

## Adding Boot Image

This step is where we will be adding a boot image to our WDS server.

1. Right click on `Boot Images` and then click on `Add Boot Image...`. This opens the `Add Image Wizard`. <br/>
<img src="https://github.com/user-attachments/assets/eb44a71d-c7b5-4afb-aa50-e9ca1089c3aa" Width="500" /> <br/>

2. On the `Image File` page, click on the `Browse...` button to select the file path for the boot image you want to add to the WDS server. This image will be the boot media that we generated earlier from the Deployment Workbench. <br/>
<img src="https://github.com/user-attachments/assets/5c0efe1a-6a86-42c7-a5d7-d7cded44ea7e" Width="500" /> <br/>

3. On the `Image Metadata` page, give your image a name and description. Since this is merely a test environment, I have left it autofilled. <br/>
<img src="https://github.com/user-attachments/assets/7a7b6773-2119-4f59-b91a-df015f673163" Width="500" /> <br/>

4. On the `Summary` page, review everything and then click `Next` to add the image/boot media to the WDS server. <br/>
<img src="https://github.com/user-attachments/assets/ce8a47a6-2576-41c8-aa5d-49c347c38468" Width="500" /> <br/>
<img src="https://github.com/user-attachments/assets/fda56ffe-5326-45b9-bf7b-cd9a32a264c4" Width="500" /> <br/>
<img src="https://github.com/user-attachments/assets/a076cbb3-3b30-4a98-9b02-be32ce35c90b" Width="500" /> <br/>

## Server Properties

With the boot image added to our deployment server, there is one more thing that needs to be done, which is to indicate that this Windows Server machine is also a PXE server.

To do so:
1. Right-click on our server on the left-hand column of the WDS homepage and click on `Properties`.

2. Go to the `DHCP` tab. Since we have set up DHCP on this Windows Server, we need to check both of the check boxes. <br/>
<img src="https://github.com/user-attachments/assets/d27f8800-fade-4718-a623-eed58298cbdf" Width="500" /> <br/>

3. Click on `Apply` and then `Ok`.

By doing so, if you open up DHCP on the Windows Server machine, and go to `Scope Options`, you will notice a new option has been added: `060 PXEClient`. <br/>
<img src="https://github.com/user-attachments/assets/e407d9d6-bdcb-4c43-9751-4289c2db10e5" Width="500" /> <br/>
