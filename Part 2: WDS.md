Now that we have already generated our boot media, our work using MDT/Deployment Workbench is complete for now. On this part, we will be working on WDS, where we add our newly generated boot media to our deployment server.

If you have not done so, add `Windows Deployment Services` using **Server Manager**.

## Configuring Deployment Server in Windows Deployment Services

1. Open Windows Deployment Services. On the left most column, expand `Servers` and right-click on the server you want to configure, and click `Configure Server`. I only have one listed, which is this installation of Window Server that I have been working on. Doing so should open the `Windows Deployment Services Configuration Wizard`.
2. Click `Next` after reading the `Before You Begin` page.
3. On the `Install Options` page, you have the option to configure the deployment server to be integrated with Active Directory, or as a standalone server. I already have Active Directory set up, so I opted to integrate.
4. On the `Remote Installation Folder Location` page, choose the file path for where you want the boot images, install images, PXE boot files, and WDS management tools will be stored.
5. On the `PXE Server Initial Settings` page, choose whether the server should respond to only known client, to not respond to any clients at all, or respond to all clients whether known or unknown. I opted to have the server to respond to all clients, but to require admin approval for unknown clients.
6. On the `Operation Complete` page, you can select to add images now. I opted to not, leaving the box unchecked and clicked on `Finish`.

Now that the server has been configured, you should see 5 new directories like so:

## Adding Boot Image

This step is where we will be adding a boot image to our WDS server.

1. Right click on `Boot Images` and then click on `Add Boot Image...`. This opens the `Add Image Wizard`.
2. On the `Image File` page, click on the `Browse...` button to select the file path for the boot image you want to add to the WDS server. This image will be the boot media that we generated earlier from the Deployment Workbench.
3. On the `Image Metadata` page, give your image a name and description. Since this is merely a test environment, I have left it autofilled.
4. On the `Summary` page, review everything and then click `Next` to add the image/boot media to the WDS server.
