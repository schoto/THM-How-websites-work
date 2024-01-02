We will continue our journey exploring the Windows operating system. 

In Windows Fundamentals 1, we covered the desktop, the file system, user account control, the control panel, settings, and the task manager. 

This module will attempt to provide an overview of some other utilities available within the Windows operating system and different methods to access these utilities.

Launch the attached virtual machine. If you wish to access the virtual machine via Remote Desktop, use the credentials below. 

Machine IP: MACHINE_IP

User: administrator

Password: ********

<h3>System Configuration</h3>

The System Configuration utility (MSConfig) is for advanced troubleshooting, and its main purpose is to help diagnose startup issues. 

Reference the following document here for more information on the System Configuration utility. 

There are several methods to launch System Configuration. One method is from the Start Menu.

![msconfig](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/37daf3fc-6c23-4d18-ae6f-efc9abbf167d)

Note: You need local administrator rights to open this utility. 

The utility has five tabs across the top. Below are the names for each tab. We will briefly cover each tab in this task. 

- General
- Boot
- Services
- Startup
- Tools

![system](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/5fe96e6f-bdbd-47e2-9218-3563e04a1236)

In the General tab, we can select what devices and services for Windows to load upon boot. The options are: Normal, Diagnostic, or Selective. 

In the Boot tab, we can define various boot options for the Operating System. 

![general](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/26e47e81-6e10-423f-b796-3c60354af959)

The Services tab lists all services configured for the system regardless of their state (running or stopped). A service is a special type of application that runs in the background.  

![stoppped](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/7c2157df-f278-4f61-ba81-17f1ac4bd9df)

In the Startup tab, you won't see anything interesting in the attached VM.  Below is a screenshot of the Startup tab for MSConfig from my local machine. 

![task](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/87838ead-79cd-43c2-bdaa-9b59240a88af)


As you can see, Microsoft advises using Task Manager (taskmgr) to manage (enable/disable) startup items. The System Configuration utility is NOT a startup management program. 

Note: If you open Task Manager for the attached VM, you will notice that Task Manager doesn't show a Startup tab. 

There is a list of various utilities (tools) in the Tools tab that we can run to configure the operating system further. There is a brief description of each tool to provide some insight into what the tool is for. 

![display](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/4091c408-86dc-419e-b5d6-04b1ecd17b66)

Notice the Selected command section. The information in this textbox will change per tool.

To run a tool, we can use the command to launch the tool via the run prompt, command prompt, or by clicking the Launch button. 

**Q/A**

![answers](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/1579dd51-77c6-421c-bd07-08d0b24b09d1)

<h3>Change UAC Settings</h3>

We're continuing with Tools that are available through the System Configuration panel.

User Account Control (UAC) was covered in great detail in Windows Fundamentals 1. 

The UAC settings can be changed or even turned off entirely (not recommended).

You can move the slider to see how the setting will change the UAC settings and Microsoft's stance on the setting.

![choose](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/37f2c73c-f0a8-4a56-9ace-55cb888e2898)

**Q/A**

What is the command to open User Account Control Settings? (The answer is the name of the .exe file, not the full path)

```UserAccountControlSettings.exe```

<h3>Computer Management</h3>

We're continuing with Tools that are available through the System Configuration panel.

The Computer Management (compmgmt) utility has three primary sections: System Tools, Storage, and Services and Applications.

![mgmt1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/72a9e93d-a359-4fa1-912e-0135feacafd2)

System Tools

Let's start with Task Scheduler. Per Microsoft, with Task Scheduler, we can create and manage common tasks that our computer will carry out automatically at the times we specify.

A task can run an application, a script, etc., and tasks can be configured to run at any point. A task can run at log in or at log off. Tasks can also be configured to run on a specific schedule, for example, every five mins.

To create a basic task, click on Create Basic Task under Actions (right pane).

![actions](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/33a0da9c-28a0-47f6-9a71-3103bc172fae)

Next is Event Viewer.

Event Viewer allows us to view events that have occurred on the computer. These records of events can be seen as an audit trail that can be used to understand the activity of the computer system. This information is often used to diagnose problems and investigate actions executed on the system. 

![overview](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/8ebc8df5-f6fb-4b60-9161-5eb8989cd89c)

Event Viewer has three panes.

- The pane on the left provides a hierarchical tree listing of the event log providers. (as shown in the image above)
- The pane in the middle will display a general overview and summary of the events specific to a selected provider.
- The pane on the right is the actions pane.

There are five types of events that can be logged. Below is a table from docs.microsoft.com providing a brief description for each.

![table](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/387339a7-fbd5-4e16-9077-fec6544d3c64)

The standard logs are visible under Windows Logs. Below is a table from docs.microsoft.com providing a brief description for each.

![table1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/51057528-1ff8-4214-8771-05112967a13c)

For more information about Event Viewer and Event Logs, please refer to the Windows Event Log room. 

Shared Folders is where you will see a complete list of shares and folders shared that others can connect to. 

![client](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/47abea3f-5a20-4f91-bcab-bd54e8dc93c5)

In the above image, under Shares, are the default share of Windows, C$, and default remote administration shares created by Windows, such as ADMIN$. 

As with any object in Windows, you can right-click on a folder to view its properties, such as Permissions (who can access the shared resource). 

Under Sessions, you will see a list of users who are currently connected to the shares. In this VM, you won't see anybody connected to the shares.

All the folders and/or files that the connected users access will list under Open Files.

The Local Users and Groups section you should be familiar with from Windows Fundamentals 1 because it's lusrmgr.msc.

In Performance, you'll see a utility called Performance Monitor (perfmon).

Perfmon is used to view performance data either in real-time or from a log file. This utility is useful for troubleshooting performance issues on a computer system, whether local or remote. 

![thm](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/f0d7e2dc-6386-447c-832b-383ee66f8c4d)

Device Manager allows us to view and configure the hardware, such as disabling any hardware attached to the computer.

![thm2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/ae2e7d96-5808-43a7-89cf-4ab6b87b586b)

Storage  

Under Storage is Windows Server Backup and Disk Management. We'll only look at Disk Management in this room.

Note: Since the virtual machine is a Windows Server operating system, there are utilities available that you will typically not see in Windows 10.  

![basic](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/6b6064b5-671c-462d-b5e8-0eaeb3793618)

Disk Management is a system utility in Windows that enables you to perform advanced storage tasks.  Some tasks are:

- Set up a new drive
- Extend a partition
- Shrink a partition
- Assign or change a drive letter (ex. E:) 

Services and Applications

![routing](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/ac594684-f102-4f2e-9a9a-250a78846b41)

Recall from the previous task; a service is a special type of application that runs in the background. Here you can do more than enable and disable a service, such as view the Properties for the service. 

![svga](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/05fe1cf2-c005-45b2-ac95-dbc695d5c522)

WMI Control configures and controls the Windows Management Instrumentation (WMI) service.

Per Wikipedia, "WMI allows scripting languages (such as VBScript or Windows PowerShell) to manage Microsoft Windows personal computers and servers, both locally and remotely. Microsoft also provides a command-line interface to WMI called Windows Management Instrumentation Command-line (WMIC)."

Note: The WMIC tool is deprecated in Windows 10, version 21H1. Windows PowerShell supersedes this tool for WMI. 

**Q/A**

![TryHackMe-Windows-Fundamentals-2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/798c14c5-720a-4991-b189-5a511f707b12)

<h3>System Information</h3>

We're continuing with Tools that are available through the System Configuration panel.

What is the System Information (msinfo32) tool?

Per Microsoft, "Windows includes a tool called Microsoft System Information (Msinfo32.exe).  This tool gathers information about your computer and displays a comprehensive view of your hardware, system components, and software environment, which you can use to diagnose computer issues."

The  information in System Summary is divided into three sections:

- Hardware Resources
- Components
- Software Environment

System Summary will display general technical specifications for the computer, such as processor brand and model.

![summary](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/b9e1b052-dbb0-4474-93df-15f756631262)

The information displayed in Hardware Resources is not for the average computer user. If you want to learn more about this section, refer to the official Microsoft page.

![hardware](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a1e88fcd-1709-4473-a983-ba1b75466d8c)

Under Components, you can see specific information about the hardware devices installed on the computer. Some sections don't show any information, but some sections do, such as Display and Input.

![display](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/5e1f9d90-a95f-4950-b441-6c177acb59fd)

In the Software Environment section, you can see information about software baked into the operating system and software you have installed. Other details are visible in this section as well, such as the Environment Variables and Network Connections. 

![components](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/dbe91f24-08a9-45b4-8f5d-247fdaff3ec6)

Recall from the Windows Fundamentals 1 room (The Windows\System32 Folder task) where Environment Variables was briefly touched on. 

Per Microsoft, "Environment variables store information about the operating system environment. This information includes details such as the operating system path, the number of processors used by the operating system, and the location of temporary folders.

The environment variables store data that is used by the operating system and other programs. For example, the WINDIR environment variable contains the location of the Windows installation directory. Programs can query the value of this variable to determine where Windows operating system files are located".

Click on Environment Variables to see the assigned values for the virtual machine.

![drivers](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/4c02c6b6-d448-403b-a0ef-cb5be6da7f13)

Another method to view environment variables is ```Control Panel > System and Security > System > Advanced system settings > Environment Variables``` OR ```Settings > System > About > system info > Advanced system settings > Environment Variables```.

![ok](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/fe2c369c-5c7e-4eb9-8013-5e7a0366662c)

The detour is over. Let's redirect our attention back to msinfo32 and pick up where we left off.

Towards the very bottom of this utility, there is a search bar. Please give it a go. Select Components and search for IP address.

![search](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/dfb4eece-1d8d-4df3-acd2-da356dbbf975)

**Q/A**

![TryHackMe-Windows-Fundamentals-2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/72ed9f83-c41a-4ccd-9a19-58bfe2ab5d3f)

<h3>Resource Monitor</h3>

We're continuing with Tools that are available through the System Configuration panel.

What is Resource Monitor (resmon)?

Per Microsoft, "Resource Monitor displays per-process and aggregate CPU, memory, disk, and network usage information, in addition to providing details about which processes are using individual file handles and modules. Advanced filtering allows users to isolate the data related to one or more processes (either applications or services), start, stop, pause, and resume services, and close unresponsive applications from the user interface. It also includes a process analysis feature that can help identify deadlocked processes and file locking conflicts so that the user can attempt to resolve the conflict instead of closing an application and potentially losing data."

As some of the other tools mentioned in this room, this utility is geared primarily to advanced users who need to perform advanced troubleshooting on the computer system.

In the Overview tab, Resmon has four sections:

- CPU
- Disk
- Network
- Memory

![file](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a76fcf19-2050-49ae-bba7-8c5a3299bbfd)

The same four sections have corresponding tabs across the top. See below.

![net](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/2db6d50c-b052-4f11-8dbf-8940d59061cc)

Note that each tab has additional information for each. An image is shown below for each tab. 

CPU

![disk](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/eff16d51-0d9e-4a35-91eb-01e2f7641b02)

Memory

![memm](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/3e065097-5573-4308-8abf-4185bfc31130)

Disk

![resmon-disk](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/0fe8da48-bc52-47db-b4e3-75f889359e00)

Network

![resmon-network](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/00859d0a-946e-4fc2-a1fc-c5fa57295b15)

Although not captured in any of the images above, Resource Monitor has a pane at the far right. This pane shows a graphical view in real-time for each section. 

Note: The information displayed in Resource Monitor will be different for you compared to the images above.

**Q/A**

What is the command to open Resource Monitor? (The answer is the name of the .exe file, not the full path)

```resmon.exe```

<h3>Command Prompt</h3>

We're continuing with Tools that are available through the System Configuration panel.

The command prompt (cmd) can seem daunting at first, but it's really not that bad once you understand how to interact with it. 

In early operating systems, the command line was the sole way to interact with the operating system.

When the GUI (graphical user interface) was introduced, it allowed users to perform complex tasks with a few clicks of a button instead of entering commands in the command prompt. 

Even though the GUI is the primary way to interact with the operating system, a computer user can still interact via the command prompt. 

In this task, we'll only cover a few commands that a computer user can run in the command prompt to obtain information about the computer system.

Let's start with a few simple commands, such as hostname and whoami.

The command hostname will output the computer name.

![thm1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/5e4598d3-6896-49cd-954a-fedade9ba7b5)

The command whoami will output the name of the logged-in user.

![thm2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/1f2a7d0c-3a49-4652-9651-4f7dbae64326)

Next, let's look at some commands that are useful when troubleshooting.

A command used often is ipconfig. This command will show the network address settings for the computer.

![thm3](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/c20c53f2-c7b3-489e-abd2-98a51e3df8b6)

Each command will have a help manual to explain the expected syntax to execute the command properly, along with any additional parameters that can be added to the command to expand its execution.

A  command to retrieve the help manual for a command is /?.

For example, to see the help manual for ipconfig, you can use the following command: ipconfig /?

![thm4](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/3e1bc664-4bb1-4b50-b702-a2fff577d613)

Note: To clear the command prompt screen, the command is cls. 

The next command is netstat. Per the help manual, this command will display protocol statistics and current TCP/IP network connections. 

![thm5](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a9bd2dc3-f9f5-4843-859b-2110952cf6e3)

In the above image, the line within the red box shows us an example syntax for the command. 

The structure tells us the netstat command can be run alone or with parameters, such as -a,  -b,  -e, etc. 

When any of the parameters are appended to the root command, netstat in this case, the output changes. Play with a few to see for yourself. 

The net command is primarily used to manage network resources. This command supports sub-commands.

If you type net without a sub-command, the output will show the syntax for the root command showing a few of the sub-commands you can use.

![thm6](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/af41065d-c16b-4a49-a66a-9a0bdff6f9f8)

For the net command, to display the help manual /? will not work. In this case, you need to use different syntax, which is net help.

![thm7](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/ada6bcb1-3b67-474d-a28c-6cab9fc788ea)

So, if you wish to see the help information for net user , the command is net help user. 

![thm8](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/2cfa9c73-71d6-4956-a1b1-a9c684b2ff73)

You can use the same command to view the help information for other useful net sub-commands, such as localgroup, use, share, and session. 

**Q/A**

![qa](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/0d930f17-8ff6-4347-9f41-cfb37be011ab)

<h3>Registry Editor</h3>

We're continuing with Tools that are available through the System Configuration panel.

The Windows Registry (per Microsoft) is a central hierarchical database used to store information necessary to configure the system for one or more users, applications, and hardware devices.

The registry contains information that Windows continually references during operation, such as:

- Profiles for each user
- Applications installed on the computer and the types of documents that each can create
- Property sheet settings for folders and application icons
- What hardware exists on the system
- The ports that are being used.

Warning: The registry is for advanced computer users. Making changes to the registry can affect normal computer operations. 

There are various ways to view/edit the registry. One way is to use the Registry Editor (regedit).

![regedit](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/aaab9d48-8149-4975-a658-ab795aa73e73)

**Q/A**

What is the command to open the Registry Editor? (The answer is the name of  the .exe file, not the full path)

```regedt32.exe```
