# ABB Motion Program Exec Manual Robot Setup

ABB Motion Program Exec requires software to be installed on the robot. This software can be
installed manually by copying files to the robot controller and importing configuration files,
or by using a RobotWare Add-In. The RobotWare Add-In is cleaner and probably more reliable,
but is also less flexible and requires using the Installation Manager which can be confusing.
This documents covers manually installing the software on a virtual controller in Robot Studio. See
robot_setup_robotware_addin.md for manual setup instructions.

## Step 1: Install RobotWare 6.13

**This section only needs to be completed once on RobotWare installation.**

Begin by installing and starting RobotStudio. See 
http://new.abb.com/products/robotics/robotstudio/downloads for downloads.

Once installed, open RobotStudio and click on the "Add-Ins" tab on the top of the window.

![](figures/robotstudio_addin_tab.png)

First, install the newest version of RobotWare 6.13 for IRC5 controllers. Versions greater 
than RobotWare 6.13 may work, but they have not been tested. In the "RobotApps" -> "Gallery" window,
select RobotWare for IRC5. On the right, select "Version" to be the greatest version of 6.13.
In this example, version 6.13.01 is the greatest version available. Click "Add", accept the next
window, and wait for the installation to finish.

![](figures/robotstudio_addin_robotware_irc5.png)

RobotStudio is now ready to create a solution.

## 2. Create Solution and Install Robot

Click on the "File" tab, and then "Solution with Station and Virtual Controller". Select the 
RobotWare version installed in Step 1, in this case "6.13.01.00". For this example, the 
"IRB 1200 5kg 0.9m" robot is used. Any 6 or 7 axis single-arm robot can be used instead. Change
the "Solution Name" if desired. Click "Create" when ready.

![](figures/robotstudio_new_solution.png)

If a window "Select library for '1200.09_5_TypeB (ROB_1)" appears, select "IRB1200_5_90_STD_03" 
and click "OK".

![](figures/robotstudio_irb1200_select.png)

Check which model the "real" robot is if using a robot other than IRB1200. Normally the "STD"
version is fine for simulation. **The variant in terms of reach and payload is very important.** The
other options often don't affect simulation.

Click on the "Controller" tab on the top. Then right click on the virtual control listed in
the left tree view under "Current Station". In this example, the controller is "IRB1200_5_90", but 
it will vary depending on the project configuration. Click "Change Options" on the right-click
popup menu.

![](figures/robotstudio_change_options.png)

In the "Change Options" window, select "Communication" -> "616-1 PC Interface" and 
"Engineering Tools" -> "623-1 Multitasking".

![](figures/robotstudio_change_options2.png)

![](figures/robotstudio_change_options3.png)

Click OK. Click "Yes" to restart the controller. Click
"OK" to confirm I-start.

Next, copy over the RAPID files to the controller "HOME" directory. For the virtual controller,
right click on "HOME" in the controller tree. (This is the same controller that options were
changed on previously.) Click "Open Folder".

![](figures/robotstudio_open_home.png)

Copy `error_reporter.mod`, `motion_program_exec.mod`, `motion_program_logger.mod`, and
`mp.sys` from `<repo_root>/robot/HOME` to the folder opened by clicking "Open Folder". The four
files should now be visible under "HOME" in the tree view in RobotStudio.

![](figures/robotstudio_rapid_copied.png)

Right click on "Configuration" in the controller tree, and click on "Load Parameters". Browse to 
`<repo_root>/robot/config_params` and select `SYS.cfg`. Click OK to confirm loading parameters,
and OK to acknowledge restart is required. Repeat for `EIO.cfg` in the same directory. Make sure
"Load parameters and replace duplicates" is selected in the file browser window for both. Click
"Restart" to restart the controller.

![](figures/robotstudio_restart.png)

The installation should now be complete. The following should match, and can be checked against 
your system to verify installation:

![](figures/robotstudio_addin_installed1.png)

![](figures/robotstudio_addin_installed2.png)

![](figures/robotstudio_addin_installed3.png)

![](figures/robotstudio_addin_installed4.png)

![](figures/robotstudio_addin_installed7.png)

![](figures/robotstudio_addin_installed6.png)

![](figures/robotstudio_addin_installed5.png)

## 3. Run Programs

The robot is now ready to run programs! The `abb_motion_program_exec_client.py` can either
be imported to be used as a python module, or can be run directly to run an example
motion sequence.

```
python abb_motion_program_exec_client.py
```

On Linux, it may be necessary to run `python3`

```
python3 abb_motion_program_exec_client.py
```

By default, the virtual controller listens on `http://localhost:80` for requests. The Python
module uses ABB WebServices for communication.

## Updating RAPID modules

To update RAPID modules, copy the updated files to `HOME`, and click Controller -> 
Restart (dropdown)-> Reset RAPID (P-start).
