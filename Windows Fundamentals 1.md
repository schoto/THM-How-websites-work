<h3>Introduction to Windows</h3>

The Windows operating system (OS) is a complex product with many system files, utilities, settings, features, etc. 

This module will attempt to provide a general overview of just a handful of what makes up the Windows OS, navigate the user interface, make changes to the system, etc. The content is aimed at those who wish to understand and use the Windows OS on a more comfortable level. 

Launch the attached virtual machine. The virtual machine should open within your web browser.

<h3>Windows Editions</h3>

The Windows operating system has a long history dating back to 1985, and currently, it is the dominant operating system in both home use and corporate networks. Because of this, Windows has always been targeted by hackers & malware writers.

Windows XP was a popular version of Windows and had a long-running. Microsoft announced Windows Vista, which was a complete overhaul of the Windows operating system. There were many issues with Windows Vista. It wasn't received well by Windows users, and it was quickly phased out.

When Microsoft announced the end-of-life date for Windows XP, many customers panicked. Corporations, hospitals, etc., scrambled and tested the next viable Windows version, which was Windows 7, against many other hardware and devices. Vendors had to work against the clock to ensure their products worked with Windows 7 for their customers. If they couldn't, their customers had to break their agreement and find another vendor that upgraded their products to work with Windows 7. It was a nightmare for many, and Microsoft took note of it.

Windows 7, as quickly as it was released soon after, was marked with an end of support date. Windows 8.x came and left and it was short-lived, like Vista.

Then arrived Windows 10, which is the current Windows operating system version for desktop computers.

Windows 10 comes in 2 flavors, Home and Pro. You can read the difference between the Home and Pro here. 

Even though we didn't talk about servers, the current version of the Windows operating system for servers is Windows Server 2019.

Many critics like to bash on Microsoft, but they have made long strides to improve the usability and security with each new version of Windows.

Note: The Windows edition for the attached VM is Windows Server 2019 Standard, as seen in System Information.

Update: As of June 2021, Microsoft announced the retirement dates for Windows 10 here. 

"Microsoft will continue to support at least one Windows 10 Semi-Annual Channel until October 14, 2025".

As of October 5th, 2021 - Windows 11 now is the current Windows operating system for end-users. Read more about Windows 11 here.  

**Q/A**

What encryption can you enable on Pro that you can't enable in Home?

```Bitlocker```

<h3>The Desktop (GUI)</h3>

The Windows Desktop, aka the graphical user interface or GUI in short, is the screen that welcomes you once you log into a Windows 10 machine.

Traditionally, you need to pass the login screen first. The login screen is where you need to enter valid account credentials; usually, a username & password of a preexisting Windows account on that particular system or in the Active Directory environment (if it's a domain-joined machine)

![win-desktop2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a87af73f-bb81-4ab2-8010-de44f39158e0)

The above screenshot is an example of a typical Windows Desktop. Each component that makes up the GUI is explained briefly below.

1. The Desktop
2. Start Menu
3. Search Box (Cortana)
4. Task View
5. Taskbar
6. Toolbars
7. Notification Area

The Desktop

The desktop is where you will have shortcuts to programs, folders, files, etc. These icons will either be well organized in folders sorted alphabetically or scattered randomly with no specific organization on the desktop. In either case, these items are typically placed on the desktop for quick access.

The look and feel of the desktop can be changed to suit your liking. By right-clicking anywhere on the desktop, a context menu will appear. This menu will allow you to change the sizes of the desktop icons, specify how you want to arrange them, copy/paste items to the desktop, and create new items, such as a folder, shortcut, or text document.

Under Display settings, you can make changes to the screen's resolution and orientation. In case you have multiple computer screens, you can make configurations to the multi-screen setup here. 

Note: In a Remote Desktop session, some of the display settings will be disabled. 

The Start Menu

In previous versions of Windows, the word Start was visible at the bottom left corner of the desktop GUI. In modern versions of Windows, such as Windows 10, the word 'Start' doesn't appear anymore, but rather a Windows Logo is shown instead. Even though the look of the Start Menu has changed, its overall purpose is the same. 

The Start Menu provides access to all the apps/programs, files, utility tools, etc., that are most useful. 

Clicking on the Windows logo, the Start Menu will open. The Start Menu is broken up into sections.

This section of the Start Menu provides quick shortcuts to actions that you can perform with your account or login session, such as making changes to your user account, lock your screen, or signing out of your account. Other shortcuts specific to your account are your Documents (document icon) folder and Pictures folder (pictures icon). Lastly, the gear/cog icon will take you to the Settings screen, and the power icon will allow you to Disconnect from a Remote Desktop session, shut down the computer, or restart the computer.

This section will show all Recently added apps/programs at the top and all the installed apps/programs (that are configured to appear in the Start Menu). In this section, you'll also see the apps/programs will be listed in alphabetical order. Each letter will have its own section. 

The right side of the Start Menu is where you will find icons for specific apps/programs or utilities. These icons are known as tiles. Some tiles are added to this section by default. If you right-click any of the tiles, you guessed it; a menu will appear to allow you to perform more actions on the selected tile; such as resizing the tile, unpinning from Start Menu, view its Properties, etc.

The Taskbar

Some of the components are enabled and visible by default. The Toolbar (6), for example, was enabled for demonstration purposes.  

If you're like me and want to disable some of these components, you can right-click on Taskbar to bring up a context menu that will allow you to make changes.

Hovering over the icon will provide a preview thumbnail, along with a tooltip. This  tooltip is handy if you have many apps/programs open, such as Google Chrome, and you wish to find which instance of Google Chrome is the one you need to bring in to focus. 

When you close any of these items, they will disappear from the taskbar (unless you explicitly pinned it to the taskbar). 

The Notification Area

The Notification Area, which is typically located at the bottom right of the Windows screen, is where the date and time are displayed. Other icons possibly visible in this area is the volume icon, network/wireless icon, to name a few. Icons can be either added or removed from the Notification Area in Taskbar settings. 

**Q/A**

![TryHackMe-Windows-Fundamentals-1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/cff2a517-7836-41f1-949c-5b79d34fcccc)

<h3>The File System</h3>

The file system used in modern versions of Windows is the New Technology File System or simply NTFS.

Before NTFS, there was FAT16/FAT32 (File Allocation Table) and HPFS (High Performance File System). 

You still see FAT partitions in use today. For example, you typically see FAT partitions in USB devices, MicroSD cards, etc. but traditionally not on personal Windows computers/laptops or Windows servers.

NTFS is known as a journaling file system. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.   

NTFS addresses many of the limitations of the previous file systems; such as: 

- Supports files larger than 4GB
- Set specific permissions on folders and files
- Folder and file compression
- Encryption (Encryption File System or EFS)

If you're running Windows, what is the file system your Windows installation is using? You can check the Properties (right-click) of the drive your operating system is installed on, typically the C drive (C:\).

![gif](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/f13c6750-f82a-4e1e-908b-52d7567d8275)

You can read Microsoft's official documentation on FAT, HPFS, and NTFS here. 

Let's speak briefly on some features that are specific to NTFS. 

On NTFS volumes, you can set permissions that grant or deny access to files and folders.

The permissions are:

- Full control
- Modify
- Read & Execute
- List folder contents
- Read
- Write

The below image lists the meaning of each permission on how it applies to a file and a folder. (credit Microsoft)

![permits](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/d34d4469-927b-437c-8b0d-eae90980de35)

How can you view the permissions for a file or folder?

- Right-click the file or folder you want to check for permissions.
- From the context menu, select Properties.
- Within Properties, click on the Security tab.
- In the Group or user names list, select the user, computer, or group whose permissions you want to view.

In the below image, you can see the permissions for the Users group for the Windows folder. 

![user](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/70feafd9-406e-4d5d-8511-959b3ca52454)

Refer to the Microsoft documentation to get a better understanding of the NTFS permissions for Special Permissions.

Another feature of NTFS is Alternate Data Streams (ADS).

Alternate Data Streams (ADS) is a file attribute specific to Windows NTFS (New Technology File System).

Every file has at least one data stream ($DATA), and ADS allows files to contain more than one stream of data. Natively Window Explorer doesn't display ADS to the user. There are 3rd party executables that can be used to view this data, but Powershell gives you the ability to view ADS for files.

From a security perspective, malware writers have used ADS to hide data.

Not all its uses are malicious. For example, when you download a file from the Internet, there are identifiers written to ADS to identify that the file was downloaded from the Internet.

To learn more about ADS, refer to the following link from MalwareBytes here. 

Bonus: If you wish to interact hands-on with ADS, I suggest exploring Day 21 of Advent of Cyber 2 (already done)

![qa](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/5348b625-83cf-4310-83f4-97f072032f97)

<h3>The Windows\System32 Folders</h3>

The Windows folder (C:\Windows) is traditionally known as the folder which contains the Windows operating system. 

The folder doesn't have to reside in the C drive necessarily. It can reside in any other drive and technically can reside in a different folder.

This is where environment variables, more specifically system environment variables, come into play. Even though not discussed yet, the system  environment variable for the Windows directory is %windir%.

Per Microsoft, "Environment variables store information about the operating system environment. This information includes details such as the operating system path, the number of processors used by the operating system, and the location of temporary folders".

There are many folders within the 'Windows' folder. See below.

![boot](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/0779ce07-ab26-43ca-8ef8-2826b490dc95)

One of the many folders is System32. 

![system32](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/ea6c9f42-163c-4aa8-8e0c-5fb9436ed8fb)

The System32 folder holds the important files that are critical for the operating system.

You should proceed with extreme caution when interacting with this folder. Accidentally deleting any files or folders within System32 can render the Windows OS inoperational. Read more about this action here --> https://www.howtogeek.com/346997/what-is-the-system32-directory-and-why-you-shouldnt-delete-it/

Note: Many of the tools that will be covered in the Windows Fundamentals series reside within the System32 folder. 

**Q/A**

![qa1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/d6c6318f-4d73-42a9-82e6-85424b1d99a7)

<h3>User Accounts, Profiles, and Permissions</h3>


