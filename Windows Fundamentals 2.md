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



