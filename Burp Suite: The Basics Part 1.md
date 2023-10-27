Welcome to Burp Suite Basics!

This room will cover the foundations of using the Burp Suite web application framework.
Specifically, we will be looking at:

What Burp Suite is
An overview of the available tools in the framework
Installing Burp Suite for yourself
Navigating and configuring Burp Suite.

We will also be introducing the core of the Burp Suite framework: the Burp Proxy. This room is primarily designed to provide a foundational knowledge of Burp Suite which can then be built upon further in the other rooms of the Burp module; as such, it will be a lot heavier in theory than subsequent rooms, which take more of a practical approach. You are advised to read the information here and follow along yourself with a copy of the tool if you haven't used Burp Suite before. Experimentation is key: use this information in tandem with playing around with the app for yourself to build a foundation for using the framework, which can then be built upon in later rooms.

Let's begin!

<h4>What is the Burp Suite</h4>

Put simply: Burp Suite is a framework written in Java that aims to provide a one-stop-shop for web application penetration testing. In many ways, this goal is achieved as Burp is very much the industry standard tool for hands-on web app security assessments. Burp Suite is also very commonly used when assessing mobile applications, as the same features which make it so attractive for web app testing translate almost perfectly into testing the APIs (Application Programming Interfaces) powering most mobile apps.

At the simplest level, Burp can capture and manipulate all of the traffic between an attacker and a webserver: this is the core of the framework. After capturing requests, we can choose to send them to various other parts of the Burp Suite framework -- we will be covering some of these tools in upcoming rooms. This ability to intercept, view, and modify web requests prior to them being sent to the target server (or, in some cases, the responses before they are received by our browser), makes Burp Suite perfect for any kind of manual web app testing.

There are various different editions of Burp Suite available. We will be working with the Burp Suite Community edition, as this is free to use for any (legal) non-commercial use. The Burp Suite Professional and Enterprise editions both require expensive licenses but come with powerful extra features:

Burp Suite Professional is an unrestricted version of Burp Suite Community. It comes with features such as:

- An automated vulnerability scanner

- A fuzzer/bruteforcer that isn't rate limited

- Saving projects for future use; report generation

- A built-in API to allow integration with other tools

- Unrestricted access to add new extensions for greater functionality

- Access to the Burp Suite Collaborator (effectively providing a unique request catcher self-hosted or running on a Portswigger owned server)

In short, Burp Pro is an extremely powerful tool -- which is why it comes with a £319/$399 price tag per user for a one-year subscription. For this reason, Burp Pro is usually only used by professionals (with licenses often being provided by employers).
Burp Suite Enterprise is slightly different. 

Unlike the community and professional editions, Burp Enterprise is used for continuous scanning. It provides an automated scanner that can periodically scan webapps for vulnerabilities in much the same way as software like Nessus performs  automated infrastructure scanning. Unlike the other editions of Burp Suite which allow you to perform manual attacks from your own computer, Enterprise sits on a server and constantly scans target web apps for vulnerabilities.
Due to the prohibitive costs involved with either of these editions of Burp Suite, we will stick to the core feature-set provided by Burp Suite Community.

Note: Burp Suite for Windows is featured in the screenshots for many demonstrations; however, there are no differences between this and the copy of Burp Suite installed on the AttackBox.

**Questions / Answers**

Which edition of Burp Suite will we be using in this module?

Burp Suite Community

Which edition of Burp Suite runs on a server and provides constant scanning for target web apps?

Burp Suite Enterprise

Burp Suite is frequently used when attacking web applications and ______ applications.

Mobile

<h4>Features of Burp Community</h4>

Whilst Burp Community has a relatively limited feature-set compared to the Professional edition, it still has many superb tools available. These include:

-Proxy: The most well-known aspect of Burp Suite, the Burp Proxy allows us to intercept and modify requests/responses when interacting with web applications.

-Repeater: The second most well-known Burp feature -- Repeater -- allows us to capture, modify, then resend the same request numerous times. This feature can be absolutely invaluable, especially when we need to craft a payload through trial and error (e.g. in an SQLi -- Structured Query Language Injection) or when testing the functionality of an endpoint for flaws.

-Intruder: Although harshly rate-limited in Burp Community, Intruder allows us to spray an endpoint with requests. This is often used for bruteforce attacks or to fuzz endpoints.

-Decoder: Though less-used than the previously mentioned features, Decoder still provides a valuable service when transforming data -- either in terms of decoding captured information, or encoding a payload prior to sending it to the target. Whilst there are other services available to do the same job, doing this directly within Burp Suite can be very efficient.

-Comparer: As the name suggests, Comparer allows us to compare two pieces of data at either word or byte level. Again, this is not something that is unique to Burp Suite, but being able to send (potentially very large) pieces of data directly into a comparison tool with a single keyboard shortcut can speed things up considerably.

-Sequencer: We usually use Sequencer when assessing the randomness of tokens such as session cookie values or other supposedly random generated data. If the algorithm is not generating secure random values, then this could open up some devastating avenues for attack.

In addition to the myriad of in-built features, the Java codebase also makes it very easy to write extensions to add to the functionality of the Burp framework. These can be written in Java, Python (using the Java Jython interpreter), or Ruby (using the Java JRuby interpreter). The Burp Suite Extender module can quickly and easily load extensions into the framework, as well as providing a marketplace to download third-party modules (referred to as the "BApp Store"). Whilst many of these extensions require a professional license to download and add in, there are still a fair number that can be integrated with Burp Community. For example, we may wish to extend the inbuilt logging functionality of Burp Suite with the Logger++ module.

**Questions / Answers**

Which Burp Suite feature allows us to intercept requests between ourselves and the target?

Proxy

Which Burp tool would we use if we wanted to bruteforce a login form?

Intruder

<h4>Installation</h4>

I don't really need to install Burp Suite, because it's already installed on AttackBox on TryHackMe. Just gonna go ahead and skip it

<h4>Dashboard</h4>

When we open Burp Suite and have accepted the terms and conditions, we are met with a window asking us to select the project type.

This window doesn't give us many options in Burp Community. Burp Pro would allow us to save our work to the disk or load a previously saved project at this point. All we can do here is click "Next", however.

The next window allows us to choose a configuration for Burp Suite. Leaving this at the default is perfect for most situations:

Click "Start Burp", and the main Burp Suite interface will open!

The first time you open Burp Suite, you may be presented with a screen of training options. These are well worth reading through if you get the time.

If not (and in any subsequent sessions regardless), you will be presented with the slightly daunting Burp Dashboard:

![burp1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/f8ea06a5-ae23-4fb3-8973-8cf80234aa5e)

Don't be alarmed if this doesn't make too much sense just yet -- it soon will!

In short, the Dashboard interface is split into four quadrants:

![burp2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/e694dbad-35f0-4c99-bd23-f7e49727d9f0)

1. The Tasks menu allows us to define background tasks that Burp Suite will run whilst we use the application. The Pro version would also allow us to create on-demand scans. The default "Live Passive Crawl" (which automatically logs the pages we visit) will be more than suitable for our uses in this module.

2. The Event log tells us what Burp Suite is doing (e.g. starting the Proxy), as well as information about any connections that we are making through Burp.

3. The Issue Activity section is exclusive to Burp Pro. It won't give us anything using Burp Community, but in Burp Professional it would list all of the vulnerabilities found by the automated scanner. These would be ranked by severity and filterable by how sure Burp is that the component is vulnerable.

4. The Advisory section gives more information about the vulnerabilities found, as well as references and suggested remediations. These could then be exported into a report.
Clicking on one of the example vulnerabilities in the Issue Activity section gives us an idea of what this looks like:

![burp3](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/7c88b7fa-e84b-4df3-af18-29ac5fbe21a4)

Throughout the various tabs and windows of Burp Suite, you will find little help icons: a question mark within a circle. ![burp4](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/22b4cf2d-b4cc-48e1-a514-4a40e31ed00c)

Clicking on these will open a new window containing help for the section, for example:

![burp5](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/b298a986-bfc3-4f58-b0a5-fab7aea8c0aa)

These are extremely useful if you're ever stuck and don't know what a feature does, so make good use of them!

<h4>Navigation</h4>

Navigating around the Burp Suite GUI by default is done entirely using the top menu bars:

![burp6](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/18e887ef-9045-4275-ad9d-7046652ecbf0)

These allow you to switch between modules (along the top row of the attached image). If the selected module has more than one sub-tab, then these can be selected using a second menu bar which appears directly below the original bar (the bottom row of the image above). It is common for module-specific settings to be provided in these sub-tabs (as is the case with the Proxy Options above).

Tabs can also be popped out into separate windows should you prefer to view multiple tabs separately. This can be done by clicking "Window" in the application menu at the top of the screen, then choosing to "Detach" tabs:

![burp7](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/d463610d-77c5-46ad-8a60-a34eac74e405)

These can be reattached in the same way.

In addition to the menu bar, Burp Suite also has keyboard shortcuts that allow quick navigation to key tabs. By default, these are:

Shortcut ||||||||||| Does

```Ctrl + Shift + D``` Switch to the Dashboard

```Ctrl + Shift + T``` Switch to the Target tab

```Ctrl + Shift + P``` Switch to the Proxy tab

```Ctrl + Shift + I``` Switch to the Intruder tab

```Ctrl + Shift + R``` Switch to the Repeater tab

<h4>Options</h4>

Before we start learning about the Burp Proxy, let's take a look at the options available for configuring Burp Suite.

There are two types of settings: Global Settings (also called User Settings), and Project Settings.

User settings affect the Burp Suite installation as a whole and will be applied every time we start the application. By contrast, Project Settings only apply to the current project. Given that we can't save projects in Burp Suite Community Edition, this effectively means that any project options we set will be lost every time we close Burp.

Many options are provided as both global settings (which are used to set a baseline) and project settings (which can be used to override the equivalent global setting).

Note: The settings system in Burp Suite was recently completely revamped. This task has been updated to use the new version. Please ensure that you are using the latest version of Burp Suite.

The Settings window can be accessed by clicking on the "Settings" button in the top navigation bar:

![settingsburp](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/3427c204-1980-4dd9-b3d0-9973f53e8bc7)

On the left hand side we have a menu containing options to change the scope between all settings, user settings, and project settings, as well as search for specific settings, or select them by category.

It should be noted that many of the tools in Burp Suite offer shortcuts to specific categories of settings. For example, the Proxy tool includes a "Proxy settings" button which will open the Settings window directly to the section relevant to the proxy.

![set burp2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/8940bfd0-ad63-47f6-bb36-e638efc2725c)

The Search feature of the settings page is a relatively new addition; however, it is absolutely invaluable, allowing us to search for settings using keywords.

Familiarise yourself with the range of configurable options in Burp Suite, then complete the following exercises.

**Questions / Answers**

Change the Burp Suite theme to dark mode

Done

In which category can you find reference to a "Cookie jar"?

Sessions

In which base category can you find the "Updates" sub-category, which controls the Burp Suite update behaviour?

Suite

What is the name of the sub-category which allows you to change the keybindings for shortcuts in Burp Suite ?

Hotkeys

If we have uploaded Client-Side TLS certificates, can we override these on a per-project basis (Aye/Nay)?

Aye


There are many more configuration options available. Take the time to read through them.

In the next section, we will cover the Burp Proxy -- a much more hands-on aspect of the room.

End of the first part ;)




