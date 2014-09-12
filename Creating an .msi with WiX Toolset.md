Creating an .msi with WiX Toolset
=========

This guide will walk you through creating a basic msi package using the WixToolset.

Prerequisite
--------------
Note: These MUST be installed before you continue with the guide.
  - [WiX Toolset]
  - [Visual Studio Professional 2013]
  - Add the WixToolset bin directory to your `PATH` environment variables. This is where `candle.exe` and `light.exe` reside.
    - This is usually **C:\Program Files (x86)\WiX Toolset v3.8\bin**

Quick Primer on WiX
--------------
1. `candle` is the WiX compiler, it generates an object file with the **.wixobj** extension.
2. `light` is the WiX Linker, it generates the final installer, or **.msi** file.

Getting Started - Creating your project.
--------------
1. Open Visual Studio Professional 2013, Click **File** > **New** > **Project**
2. In the New Project window, select **Windows Installer XML** in the Templates Pane.
3. In the middle pane, select **Setup Project**, 
4. Make sure to give your project a **Name**. I chose to use **SimpleApp**. Click **OK**.

Make it yours!
-------------
1. On **line 3** you'll find something like the following: `<Product ...  Manufacturer="" ... ">`
2. We need to define the `Manufacturer=""`!
3. Put **your name** or **company name** in the quotes. ex: `Manufacturer="Aperture Labs"`
4. We can also change **line 6** `[ProductName]` to something like **SimpleApp**.
5. Let's **remove** the `<MediaTemplate />` line and replace it with the following: `<Media Id="33" Cabinet="product.cab" EmbedCab="yes" />`
6. Replace this `<Component />` section with `<Component Id="ApplicationFiles" Guid="12345678-1234-1234-1234-222222222222" />` 
7. Save the file. 

Create the object file.
--------------
We will be using [Windows PowerShell] to navigate around. 
1. Open **PowerShell** as **Administrator**.
2. Navigate  to the Visual Studio Projects Directory you created. 
 * `cd $env:HOME\Documents\Visual Studio 2013\Projects\SimpleApp\SimpleApp` 
 * This is usually **C:\Users\Your_Username\Documents\Visual Studio 2013\Projects\Project_Name**
3. If you run the `dir` command, you should see your **Project.wxs** and **.wixproj** files. If you don't see those files, you're in the wrong directory. 
4. Run `candle Product.wxs`
5. You should see something similar: 

    ```Powershell
    Windows Installer XML Toolset Compiler version 3.8.1128.0
    Copyright (c) Outercurve Foundation. All rights reserved.

    Product.wxs
    ```
6. If you run the `dir` command again, you should see your newly created **Product.wixobj** file. Wooohoo! Now we're cookin' with hot oil!

Creating the .msi file.
--------------
1. In the PowerShell window run the following command: `light Product.wixobj`
 * Note: You may see a warning about the media table not having entries, you can ignore that for the moment.
2. Verify your .msi file was created by using `dir` again.
3. Viola! You've done it! 
4. Try installing the msi by navigating to the location in Windows Explorer and **double-clicking** the **Product.msi**.
5. Verify it installed by checking **Program and Features** on your computer.


**Free Software, Hell Yeah!**
[Visual Studio Professional 2013]:http://www.visualstudio.com/downloads/download-visual-studio-vs
[WiX Toolset]:http://wixtoolset.org/
[Windows PowerShell]:http://technet.microsoft.com/en-us/scriptcenter/powershell.aspx
