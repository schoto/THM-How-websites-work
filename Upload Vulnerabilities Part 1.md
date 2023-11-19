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

