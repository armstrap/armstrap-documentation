Getting Started with C/C++ Development Tools for Armstrap Boards, Eclipse Edition
=================================================================================

Overview
--------

This guide outlines how to create C/C++ projects using the C/C++ Development Tools for Armstrap boards using Eclipse, build an ELF executable from your project source code, run and debug the executable on your Armstrap target.

While this specific document shows screenshots for Apple Mac OSX, the steps are verified to work on both Microsoft Windows and Ubuntu Linux machines as well.

Download and Installation
-------------------------

1. Install Java (Java SE 6 or greater is recommended), which you can download at http://www.java.com/getjava.
2. Download "Eclipse IDE for C/C++ Developers" from http://www.eclipse.org/downloads
3. Download "GNU Tools for ARM Embedded Processors" from https://launchpad.net/gcc-arm-embedded
4. Download "Armstrap blinky examples" from https://s3.amazonaws.com/armstrap-public/examples/armstrap_blinkyexamples_1.0.0.zip

Consolidate all Downloaded Parts into a Single Folder
-----------------------------------------------------

1. Create a folder on your Desktop called *armstrap*
2. Extract your downloaded "Eclipse IDE for C/C++ Developers" into *<user>/Desktop/armstrap/eclipse*
3. Extract your downloaded "GNU Tools for ARM Embedded Processors" into *<user>/Desktop/armstrap/gcc-arm*
4. Extract your downloaded "Armstrap blinky examples" into *<user>/Desktop/armstrap/workspace*

Configuring C/C++ Development Tools for Armstrap boards, Eclipse Edition, for First Use
---------------------------------------------------------------------------------------

Complete the following steps to configure C/C++ Development Tools for Armstrap boards, Eclipse Edition, for first use:

1. Launch Eclipse by clicking on the *<user>/Desktop/armstrap/eclipse/eclipse* executable
2. When prompted, select the *<user>/Desktop/armstrap/workspace* folder in which to store Eclipse projects and click **OK**.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Workspace.png
       :align: center

    .. Tip:: Enable **Use this as the default and do not ask again** to save a project folder as your default workspace.

3. In the Eclipse welcome screen, select the **Workbench** icon on the far right to open the workbench view.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Workbench-Icon.png
       :align: center

4. Eclipse highlights the active perspective on the perspectives bar, as shown in the following image. The first time you use Eclipse the workbench view opens in the C/C++ perspective.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Perpective.png
       :align: center

5. Debugging Armstrap requires the "C/C++ GDB Hardware Debugging" plugin.  To install the plugin, select the **Help>>Install New Software...** menu item

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Install-New-Software.png
       :align: center

6. Configure the plugin installation

   * In the **Work with:** drop-down, select the version of Eclipse you downloaded (for example "Kepler").  As seen in label mark 1 in picture.
   * In the search field, type *Hardware* as seen in label mark 2 in picture.
   * Click the check-box to select the **C/C++ GDB Hardware Debugging** plugin as seen in label mark 3 in picture.
   * Click the **Next** button, as seen in label mark 4 in picture and accept the licensing agreement to complete the installation.
   * You will need to restart Eclipse when the plugin installation is complete.
   
    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Install-New-Software-Screen.png
       :align: center

Creating a C/C++ Project
------------------------

Complete the following steps to create a C or C++ project in C/C++ Development Tools for Armstrap boards

1. Switch to the C/C++ perspective.
2. Select **File>>New>>C Project** to open the New Project Wizard.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-New-Project-Menu.png
       :align: center

3. Configure the C Project

   * Give the project the name *blinky*, as seen in label mark 1 in picture.
   * Under **Project Type**, select the **Empty Project** option, as seen in label mark 2 in picture.
   * Under **Toolchains**, select **Cross GCC** option, as seen in label mark 3 in picture.
   
    .. image:: images/getting-started-eclipse-development-tools/Eclipse-New-Project-Wizard1.png
       :align: center

4. Click **Next** to open the **Select Configuration** page.
5. Enable **Debug** to configure the project to allow debugging your executable, and/or enable **Release** to configure the project to allow building a smaller, faster executable optimized for release.  Note: For purposes of this tutorial, ensure you enable **Debug**.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-New-Project-Wizard2.png
       :align: center

6. Click **Next** to open the **Cross GCC Command** page.
7. In the **Cross compiler prefix** text box, enter *arm-none-eabi-*, including the hyphen (-) at the end, to specify the correct compiler for Armstrap targets.
8. In the Cross compiler path text box, browse to the location of the *<user>/Desktop/armstrap/gcc-arm/bin* directory to specify the location of the compiler.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-New-Project-Wizard3.png
       :align: center

9. Click **Finish** to create your project and return to the workbench view.
10. Verify your project source code appears in the Project Explorer.  If it does not, you may have to hit the **F5** key to refresh.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-New-Project-Wizard4.png
       :align: center

In the next section of this tutorial, you create an executable build of your project to enable it to run.

Creating a Build of a C/C++ Project
-----------------------------------

Before you can run your project, you need to test that your source code compiles by creating an executable build of your project. Complete the following steps to create an executable build of a C/C++ project:

1. Switch to the C/C++ perspective.
2. Right-click (or Ctrl-click on a Mac) your project in the **Project Explorer** tab and select **Properties**.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Properties.png
       :align: center

3. Select **C/C++ Build>>Settings** in the left pane of the **Properties** dialog box.  Verify that **Cross Settings>>Tool Settings>>Prefix** is set to *arm-none-eabi-* and **Cross Settings>>Tool Settings>>Path** is set to the bin path to your compiler toolchain *<user>/Desktop/armstrap/gcc-arm/bin*

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings1.png
       :align: center

4. Under **Cross GCC Compiler>>Symbols>>Defined symbols**, enter

   ::

     STM32F4
     ARM_MATH_CM4
     USE_STDPERIPH_DRIVER
	  
   .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings2.png
      :align: center

5. Under **Cross GCC Compiler>>Includes>>Include paths**, enter

   ::

     "${workspace_loc:/${ProjName}/includes/CMSIS}"
     "${workspace_loc:/${ProjName}/includes/STM32F4xx}"
     "${workspace_loc:/${ProjName}/includes/STM32F4xx_StdPeriph_Driver/inc}"

   .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings3.png
      :align: center

6. Under **Cross GCC Compiler>>Miscellaneous>>Other flags**, enter

   ::
   
     -c -fno-common -mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 -MD

   .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings4.png
      :align: center

7. Under **Cross GCC Linker>>Libraries>>Library search path**, enter

   ::

     "${workspace_loc:/${ProjName}/scripts}"

   .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings5.png
      :align: center

8. Under **Cross GCC Linker>>Miscellaneous>>Linker flags**, enter

   ::

     -Tstm32_flash.ld -nostartfiles -Wl,--gc-sections -mthumb -mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16

   .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings6.png
      :align: center

9. Under **Cross GCC Assembler>General>>Assembler flags**, enter

   ::

     -mcpu=cortex-m4 -mthumb

   .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings7.png
      :align: center

10. In the **Build Artifacts** tab, under **Artifact extension**, enter

   ::

     elf

   .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings8.png
      :align: center

11. Select **C/C++ Build>>Tool Chain Editor** in the left pane of the **Properties** dialog box.  Set **Current builder** to *CDT Internal Builder*

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings9.png
       :align: center

12. Click **Apply** and then **OK** to close the **Properties** dialog box.
13. Click the build icon in the toolbar or select **Project>>Build Project** in the workbench view to create an elf executable of your project.  Verify your project builds successfully.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Project-Settings10.png
       :align: center

14. The Console tab displays Build Finished if the build completes successfully, as shown in the following image.

In the next section of this tutorial, you prepare to run and debug the ELF executable on your Armstrap target.

Downloading and Debugging Code
------------------------------

Before you can run the ELF executable you created in the previous section on your Armstrap target, you need to create a Debug Configuration.

1. In the C/C++ perspective, select the **Debug Configurations...** in the debug drop-down

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-1.png
       :align: center

2. Double-click the **GDB Hardware Debugging** to create a new debug configuration.  The debug configuration should be populated with settings from the current project.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-2.png
       :align: center

3. Change the debug configuration name to *blinky (Flash and Debug)*, as seen in label mark 1 in picture.  Click **Enable auto build** in the **Build configuration** section to enable builds to automatically happen (if needed) when the debug button is pressed, as seen in label mark 2.  Click the **Select other...** link, as seen in label mark 3, to configure the GDB Hardware Debugging Launcher.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-3.png
       :align: center

4. Check **User configuration specific settings** option as seen in label mark 1 in picture.  Select **Standard GDB Hardware Debugging Launcher** in the list of Launchers. Click the **OK** button to complete the GDB launcher configuration.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-4.png
       :align: center

5. In the **Debugger** tab, 
    * Under, **GDB Setup>>GDB Command**, enter the full path to the location of *arm-none-eabi-gdb* that was downloaded with GNU Tools for ARM Embedded Processors.  This should be *<user>/Desktop/armstrap/gcc-arm/bin/arm-none-eabi-gdb*, as seen in label mark 1 in picture
    * Under **Remote Target**, uncheck **Use remote target**, as seen in label mark 2.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-5.png
       :align: center

6. In the **Startup** tab, under the Initialization Commands:

   * Uncheck **Reset and Delay (seconds)** option
   * Check **Halt** option
   * For Apple Mac OSX machines, enter the following start-up script

     ::

        target extended /dev/tty.usbmodem7B4078B1
        monitor swdp_scan
        attach 1
        monitor vector_catch disable hard
        set mem inaccessible-by-default off
        set print pretty
        
   * For Ubuntu Linux machines, enter the following start-up script

     ::
        
        target extended-remote /dev/ttyACM0
        mon swdp_scan
        attach 1
        monitor vector_catch disable hard
        set mem inaccessible-by-default off
        set print pretty

   * For Microsoft Windows machines, enter the following start-up script

     ::
        
        target extended-remote \\.\COM2
        mon swdp_scan
        attach 1
        monitor vector_catch disable hard
        set mem inaccessible-by-default off
        set print pretty

   * Check **Load image** option and **Use project binary**
   * Check **Load symbols** option and **User project binary**
   
    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-6.png
       :align: center

7. Click the **Apply** button and the **Close** button to return to the C/++ perspective.
8. Open *main.c* from in the project find the first line inside the main() function.  In this project, the first line is a call to init().  Right-click (or Ctrl-click on a Mac) on the margin to open a menu-item and select the **Toggle Breakpoint** menu option to set a breakpoint.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-7.png
       :align: center

9. Verify the breakpoint is set by visually inspecting a blue dot in the margin.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-8.png
       :align: center

10. Click the debug toolbar and select your debug configuration to start flashing and debugging your Armstrap board.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-10.png
       :align: center

11. If this is the first time you are debugging, you may be presented with a confirmation dialog to confirm the perspective switch.  Check the **Remember my decision** option and click the **OK** button.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-11.png
       :align: center

12. By default, Eclipse will halt on the first line of code, usually the *Reset_Handler*.  Click on the **F8** key or the green **Play** button to continue.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-12.png
       :align: center

13. The execution should stop at your breakpoint (as seen below) and you should be able to debug your target.

    .. image:: images/getting-started-eclipse-development-tools/Eclipse-Debugging-13.png
       :align: center

    .. image:: images/getting-started-eclipse-development-tools/Exploring-the-Eclipse-debugging-commands.png
       :align: center
