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


