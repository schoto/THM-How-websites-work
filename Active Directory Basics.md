Microsoft's Active Directory is the backbone of the corporate world. It simplifies the management of devices and users within a corporate environment. In this room, we'll take a deep dive into the essential components of Active Directory.

Room Objectives

In this room, we will learn about Active Directory and will become familiar with the following topics

- What Active Directory is
- What an Active Directory Domain is
- What components go into an Active Directory Domain
- Forests and Domain Trust

And much more!

<h3>Windows Domains</h3>

Picture yourself administering a small business network with only five computers and five employees. In such a tiny network, you will probably be able to configure each computer separately without a problem. You will manually log into each computer, create users for whoever will use them, and make specific configurations for each employee's accounts. If a user's computer stops working, you will probably go to their place and fix the computer on-site.

While this sounds like a very relaxed lifestyle, let's suppose your business suddenly grows and now has 157 computers and 320 different users located across four different offices. Would you still be able to manage each computer as a separate entity, manually configure policies for each of the users across the network and provide on-site support for everyone? The answer is most likely no.

To overcome these limitations, we can use a Windows domain. Simply put, a Windows domain is a group of users and computers under the administration of a given business. The main idea behind a domain is to centralise the administration of common components of a Windows computer network in a single repository called Active Directory (AD). The server that runs the Active Directory services is known as a Domain Controller (DC).

![file server](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/5ce0aa1b-b3dd-4f1f-892f-14230ac28fcb)

The main advantages of having a configured Windows domain are:

- Centralised identity management: All users across the network can be configured from Active Directory with minimum effort.
- Managing security policies: You can configure security policies directly from Active Directory and apply them to users and computers across the network as needed.

A Real-World Example

If this sounds a bit confusing, chances are that you have already interacted with a Windows domain at some point in your school, university or work.

In school/university networks, you will often be provided with a username and password that you can use on any of the computers available on campus. Your credentials are valid for all machines because whenever you input them on a machine, it will forward the authentication process back to the Active Directory, where your credentials will be checked. Thanks to Active Directory, your credentials don't need to exist in each machine and are available throughout the network.

Active Directory is also the component that allows your school/university to restrict you from accessing the control panel on your school/university machines. Policies will usually be deployed throughout the network so that you don't have administrative privileges over those computers.

**Q/A**

![active d](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/916de905-6c7d-4b77-93f4-a65c6cc69993)

<h3>Active Directory</h3>

The core of any Windows Domain is the Active Directory Domain Service (AD DS). This service acts as a catalogue that holds the information of all of the "objects" that exist on your network. Amongst the many objects supported by AD, we have users, groups, machines, printers, shares and many others. Let's look at some of them:

Users

Users are one of the most common object types in Active Directory. Users are one of the objects known as security principals, meaning that they can be authenticated by the domain and can be assigned privileges over resources like files or printers. You could say that a security principal is an object that can act upon resources in the network.

Users can be used to represent two types of entities:

- People: users will generally represent persons in your organisation that need to access the network, like employees.
- Services: you can also define users to be used by services like IIS or MSSQL. Every single service requires a user to run, but service users are different from regular users as they will only have the privileges needed to run their specific service.

Machines

Machines are another type of object within Active Directory; for every computer that joins the Active Directory domain, a machine object will be created. Machines are also considered "security principals" and are assigned an account just as any regular user. This account has somewhat limited rights within the domain itself.

The machine accounts themselves are local administrators on the assigned computer, they are generally not supposed to be accessed by anyone except the computer itself, but as with any other account, if you have the password, you can use it to log in.

Note: Machine Account passwords are automatically rotated out and are generally comprised of 120 random characters.

Identifying machine accounts is relatively easy. They follow a specific naming scheme. The machine account name is the computer's name followed by a dollar sign. For example, a machine named DC01 will have a machine account called DC01$.

Security Groups

If you are familiar with Windows, you probably know that you can define user groups to assign access rights to files or other resources to entire groups instead of single users. This allows for better manageability as you can add users to an existing group, and they will automatically inherit all of the group's privileges. Security groups are also considered security principals and, therefore, can have privileges over resources on the network.

Groups can have both users and machines as members. If needed, groups can include other groups as well.

Several groups are created by default in a domain that can be used to grant specific privileges to users. As an example, here are some of the most important groups in a domain:

|Security Group	  | Description |
|---|---|
| Domain Admins	 | Users of this group have administrative privileges over the entire domain. By default, they can administer any computer on the domain, including the DCs. |
|Server Operators	  | Users in this group can administer Domain Controllers. They cannot change any administrative group memberships. |
| Backup Operators	 | Users in this group are allowed to access any file, ignoring their permissions. They are used to perform backups of data on computers. |
| Account Operators	 | Users in this group can create or modify other accounts in the domain. |
| Domain Users	 | Includes all existing user accounts in the domain. |
| Domain Computers	 | Includes all existing computers in the domain. |
| Domain Controllers	 | Includes all existing DCs on the domain. |

You can obtain the complete list of default security groups from the Microsoft documentation.

Active Directory Users and Computers

To configure users, groups or machines in Active Directory, we need to log in to the Domain Controller and run "Active Directory Users and Computers" from the start menu:

![best match](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/c0a90bbb-c202-45f6-96d4-abb588846a2d)

This will open up a window where you can see the hierarchy of users, computers and groups that exist in the domain. These objects are organised in Organizational Units (OUs) which are container objects that allow you to classify users and machines. OUs are mainly used to define sets of users with similar policing requirements. The people in the Sales department of your organisation are likely to have a different set of policies applied than the people in IT, for example. Keep in mind that a user can only be a part of a single OU at a time.

Checking our machine, we can see that there is already an OU called THM with four child OUs for the IT, Management, Marketing and Sales departments. It is very typical to see the OUs mimic the business' structure, as it allows for efficiently deploying baseline policies that apply to entire departments. Remember that while this would be the expected model most of the time, you can define OUs arbitrarily. Feel free to right-click the THM OU and create a new OU under it called Students just for the fun of it.

![mgmt](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/4bac765e-2b81-4236-9fed-78638c663309)

If you open any OUs, you can see the users they contain and perform simple tasks like creating, deleting or modifying them as needed. You can also reset passwords if needed (pretty useful for the helpdesk):

![view](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/fbe6ba10-e44b-419c-8da3-3877a1359b43)

You probably noticed already that there are other default containers apart from the THM OU. These containers are created by Windows automatically and contain the following:

- Builtin: Contains default groups available to any Windows host.
- Computers: Any machine joining the network will be put here by default. You can move them if needed.
- Domain Controllers: Default OU that contains the DCs in your network.
- Users: Default users and groups that apply to a domain-wide context.

Managed Service Accounts: Holds accounts used by services in your Windows domain.

Security Groups vs OUs

You are probably wondering why we have both groups and OUs. While both are used to classify users and computers, their purposes are entirely different:

- OUs are handy for applying policies to users and computers, which include specific configurations that pertain to sets of users depending on their particular role in the enterprise. Remember, a user can only be a member of a single OU at a time, as it wouldn't make sense to try to apply two different sets of policies to a single user.
- Security Groups, on the other hand, are used to grant permissions over resources. For example, you will use groups if you want to allow some users to access a shared folder or network printer. A user can be a part of many groups, which is needed to grant access to multiple resources.

**Q/A**

![answers](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/7cc90714-0756-4078-a154-68c99d97850f)

<h3>Managing Users in AD</h3>

Your first task as the new domain administrator is to check the existing AD OUs and users, as some recent changes have happened to the business. You have been given the following organisational chart and are expected to make changes to the AD to match it:

![daniel](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/b8ae5e13-e7c3-4e0e-900f-cd95455b19d6)

Deleting extra OUs and users

The first thing you should notice is that there is an additional department OU in your current AD configuration that doesn't appear in the chart. We've been told it was closed due to budget cuts and should be removed from the domain. If you try to right-click and delete the OU, you will get the following error:

![active](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/05c06c32-1a95-4fb8-bb9b-d82ff8cb0315)

By default, OUs are protected against accidental deletion. To delete the OU, we need to enable the Advanced Features in the View menu:

![dvanced](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/598a8d52-5f53-4a21-bcf5-28ab93567185)

This will show you some additional containers and enable you to disable the accidental deletion protection. To do so, right-click the OU and go to Properties. You will find a checkbox in the Object tab to disable the protection:

![object](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/6adc2e2d-1562-439d-b74d-bfaa6b9edae6)

Be sure to uncheck the box and try deleting the OU again. You will be prompted to confirm that you want to delete the OU, and as a result, any users, groups or OUs under it will also be deleted.

After deleting the extra OU, you should notice that for some of the departments, the users in the AD don't match the ones in our organisational chart. Create and delete users as needed to match them.

Delegation

One of the nice things you can do in AD is to give specific users some control over some OUs. This process is known as delegation and allows you to grant users specific privileges to perform advanced tasks on OUs without needing a Domain Administrator to step in.

One of the most common use cases for this is granting IT support the privileges to reset other low-privilege users' passwords. According to our organisational chart, Phillip is in charge of IT support, so we'd probably want to delegate the control of resetting passwords over the Sales, Marketing and Management OUs to him.

For this example, we will delegate control over the Sales OU to Phillip. To delegate control over an OU, you can right-click it and select Delegate Control:

![delegate](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/5e99cb76-c905-42c6-ab98-eda808695318)

This should open a new window where you will first be asked for the users to whom you want to delegate control:

Note: To avoid mistyping the user's name, write "phillip" and click the Check Names button. Windows will autocomplete the user for you.

![users groups](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/2c3978a8-08c8-41e2-aba0-95e8e924bc25)

Click OK, and on the next step, select the following option:

![tasks](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/d9b3f198-6b93-4851-b46e-45c66fbefe1f)

Click next a couple of times, and now Phillip should be able to reset passwords for any user in the sales department. While you'd probably want to repeat these steps to delegate the password resets of the Marketing and Management departments, we'll leave it here for this task. You are free to continue to configure the rest of the OUs if you so desire.

Now let's use Phillip's account to try and reset Sophie's password. Here are Phillip's credentials for you to log in via RDP:

Username : *******
Password : *******

Note: When connecting via RDP, use THM\phillip as the username to specify you want to log in using the user phillip on the THM domain.

While you may be tempted to go to Active Directory Users and Computers to try and test Phillip's new powers, he doesn't really have the privileges to open it, so you'll have to use other methods to do password resets. In this case, we will be using Powershell to do so:

```
PS C:\Users\phillip> Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose

New Password: *********

VERBOSE: Performing the operation "Set-ADAccountPassword" on target "CN=Sophie,OU=Sales,OU=THM,DC=thm,DC=local".
```

Since we wouldn't want Sophie to keep on using a password we know, we can also force a password reset at the next logon with the following command:

```
PS C:\Users\phillip> Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose

VERBOSE: Performing the operation "Set" on target "CN=Sophie,OU=Sales,OU=THM,DC=thm,DC=local".
```

**Log into Sophie's account with your new password and retrieve a flag from Sophie's desktop.**

Note: When connecting via RDP, use THM\sophie as the username to specify you want to log in using the user sophie on the THM domain.

**Q/A**

![answers thm](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/013c1083-a047-42ed-9f98-ae7293381a1c)

<h3>Managing Computers in AD</h3>

By default, all the machines that join a domain (except for the DCs) will be put in the container called "Computers". If we check our DC, we will see that some devices are already there:

