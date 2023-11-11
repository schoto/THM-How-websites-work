<h3>Injection</h3>

Injection flaws are very common in applications today. These flaws occur because the application interprets user-controlled input as commands or parameters. Injection attacks depend on what technologies are used and how these technologies interpret the input. Some common examples include:

SQL Injection: This occurs when user-controlled input is passed to SQL queries. As a result, an attacker can pass in SQL queries to manipulate the outcome of such queries. This could potentially allow the attacker to access, modify and delete information in a database when this input is passed into database queries. This would mean an attacker could steal sensitive information such as personal details and credentials.
Command Injection: This occurs when user input is passed to system commands. As a result, an attacker can execute arbitrary system commands on application servers, potentially allowing them to access users' systems.

The main defence for preventing injection attacks is ensuring that user-controlled input is not interpreted as queries or commands. There are different ways of doing this:

Using an allow list: when input is sent to the server, this input is compared to a list of safe inputs or characters. If the input is marked as safe, then it is processed. Otherwise, it is rejected, and the application throws an error.
Stripping input: If the input contains dangerous characters, these are removed before processing.
Dangerous characters or input is classified as any input that can change how the underlying data is processed. Instead of manually constructing allow lists or stripping input, various libraries exist that can perform these actions for you.

<h3>Command Injection</h3>

Command Injection occurs when server-side code (like PHP) in a web application makes a call to a function that interacts with the server's console directly. An injection web vulnerability allows an attacker to take advantage of that call to execute operating system commands arbitrarily on the server. The possibilities for the attacker from here are endless: they could list files, read their contents, run some basic commands to do some recon on the server or whatever they wanted, just as if they were sitting in front of the server and issuing commands directly into the command line. 

Once the attacker has a foothold on the web server, they can start the usual enumeration of your systems and look for ways to pivot around.

**Code Example**

Let's consider a scenario: MooCorp has started developing a web-based application for cow ASCII art with customisable text. While searching for ways to implement their app, they've come across the ```cowsay``` command in Linux, which does just that! Instead of coding a whole web application and the logic required to make cows talk in ASCII, they decide to write some simple code that calls the cowsay command from the operating system's console and sends back its contents to the website.

Let's look at the code they used for their app.  See if you can determine why their implementation is vulnerable to command injection.  We'll go over it below.

```
<?php
    if (isset($_GET["mooing"])) {
        $mooing = $_GET["mooing"];
        $cow = 'default';

        if(isset($_GET["cow"]))
            $cow = $_GET["cow"];
        
        passthru("perl /usr/bin/cowsay -f $cow $mooing");
    }
?>
```

In simple terms, the above snippet does the following:

- Checking if the parameter "mooing" is set. If it is, the variable ```$mooing``` gets what was passed into the input field

- Checking if the parameter "cow" is set. If it is, the variable ```$cow``` gets what was passed through the parameter.

- The program then executes the function ```passthru("perl /usr/bin/cowsay -f $cow $mooing");```. The passthru function simply executes a command in the operating system's console and sends the output back to the user's browser. You can see that our command is formed by concatenating the $cow and $mooing variables at the end of it. Since we can manipulate those variables, we can try injecting additional commands by using simple tricks. If you want to, you can read the docs on ```passthru()``` on PHP's website for more information on the function itself.

![php1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a169d894-26ed-4d2a-ae03-ed275b867e22)

**Exploiting Command Injection**

Now that we know how the application works behind the curtains, we will take advantage of a bash feature called "inline commands" to abuse the cowsay server and execute any arbitrary command we want. Bash allows you to run commands within commands. This is useful for many reasons, but in our case, it will be used to inject a command within the cowsay server to get it executed.

To execute inline commands, you only need to enclose them in the following format ```$(your_command_here)```. If the console detects an inline command, it will execute it first and then use the result as the parameter for the outer command. Look at the following example, which runs ```whoami``` as an inline command inside an ```echo``` command:

![php2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/95937144-29a8-4972-92f2-e6378e813491)

So coming back to the cowsay server, here's what would happen if we send an inline command to the web application:

![php3](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/9e5c0153-9956-49aa-a72e-c1ca8ea4b952)

Since the application accepts any input from us, we can inject an inline command which will get executed and used as a parameter for cowsay. This will make our cow say whatever the command returns! In case you are not that familiar with Linux, here are some other commands you may want to try:

```
whoami
id
ifconfig/ip addr
uname -a
ps -ef
```
**Questions / Answers**

To complete the questions below, navigate to http://MACHINE_IP:82/ and exploit the cowsay server.

What strange text file is in the website's root directory?

![TryHackMe-OWASP-Top-10-2021](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/405bc735-6e0a-4a69-91e1-42878f938f04)

```drpepper.txt```

How many non-root/non-service/non-daemon users are there?

0

What user is this app running as?

```apache```

What is the user's shell set as?

```sbin/nologin```

What version of Alpine Linux is running?

```3.16.0```

<h3>Insecure design</h3>

Insecure design refers to vulnerabilities which are inherent to the application's architecture. They are not vulnerabilities regarding bad implementations or configurations, but the idea behind the whole application (or a part of it) is flawed from the start. Most of the time, these vulnerabilities occur when an improper threat modelling is made during the planning phases of the application and propagate all the way up to your final app. Some other times, insecure design vulnerabilities may also be introduced by developers while adding some "shortcuts" around the code to make their testing easier. A developer could, for example, disable the OTP validation in the development phases to quickly test the rest of the app without manually inputting a code at each login but forget to re-enable it when sending the application to production.

**Insecure Password Resets**

A good example of such vulnerabilities occurred on Instagram a while ago. Instagram allowed users to reset their forgotten passwords by sending them a 6-digit code to their mobile number via SMS for validation. If an attacker wanted to access a victim's account, he could try to brute-force the 6-digit code. As expected, this was not directly possible as Instagram had rate-limiting implemented so that after 2
50 attempts, the user would be blocked from trying further.

![insec1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/2367ee39-d142-44ee-9bf6-8e91382d779d)

However, it was found that the rate-limiting only applied to code attempts made from the same IP. If an attacker had several different IP addresses from where to send requests, he could now try 250 codes per IP. For a 6-digit code, you have a million possible codes, so an attacker would need 1000000/250 = 4000 IPs to cover all possible codes. This may sound like an insane amount of IPs to have, but cloud services make it easy to get them at a relatively small cost, making this attack feasible.

![insec2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/6990d83f-b8dd-4027-98d4-b881146f9c37)

Notice how the vulnerability is related to the idea that no user would be capable of using thousands of IP addresses to make concurrent requests to try and brute-force a numeric code. The problem is in the design rather than the implementation of the application in itself.

Since insecure design vulnerabilities are introduced at such an early stage in the development process, resolving them often requires rebuilding the vulnerable part of the application from the ground up and is usually harder to do than any other simple code-related vulnerability. The best approach to avoid such vulnerabilities is to perform threat modelling at the early stages of the development lifecycle.

**Practical Example**

Navigate to http://machine_ip:85 and get into joseph's account. This application also has a design flaw in its password reset mechanism. Can you figure out the weakness in the proposed design and how to abuse it?

Try to reset joseph's password. Keep in mind the method used by the site to validate if you are indeed joseph.

What is the value of the flag in joseph's account?

![thm1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/6353e38a-a492-4a84-90bd-8eb084441841)

```THM{Not_3ven_c4tz_c0uld_sav3_U!}```

<h3>Security Misconfiguration</h3>

Security Misconfigurations are distinct from the other Top 10 vulnerabilities because they occur when security could have been appropriately configured but was not. Even if you download the latest up-to-date software, poor configurations could make your installation vulnerable.

Security misconfigurations include:

- Poorly configured permissions on cloud services, like S3 buckets.
- Having unnecessary features enabled, like services, pages, accounts or privileges.
- Default accounts with unchanged passwords.
- Error messages that are overly detailed and allow attackers to find out more about the system.
- Not using HTTP security headers.

This vulnerability can often lead to more vulnerabilities, such as default credentials giving you access to sensitive data, XML External Entities (XXE) or command injection on admin pages.

For more info, look at the OWASP top 10 entry for Security Misconfiguration --> https://owasp.org/Top10/A05_2021-Security_Misconfiguration/

**Debugging Interfaces**

A common security misconfiguration concerns the exposure of debugging features in production software. Debugging features are often available in programming frameworks to allow the developers to access advanced functionality that is useful for debugging an application while it's being developed. Attackers could abuse some of those debug functionalities if somehow, the developers forgot to disable them before publishing their applications.

One example of such a vulnerability was allegedly used when Patreon got hacked in 2015. Five days before Patreon was hacked, a security researcher reported to Patreon that he had found an open debug interface for a Werkzeug console. Werkzeug is a vital component in Python-based web applications as it provides an interface for web servers to execute the Python code. Werkzeug includes a debug console that can be accessed either via URL on ```/console```, or it will also be presented to the user if an exception is raised by the application. In both cases, the console provides a Python console that will run any code you send to it. For an attacker, this means he can execute commands arbitrarily.

![thm1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/9368f9bf-09d9-4411-a34f-514f1049dabd)

**Practical example**

This VM showcases a ```Security Misconfiguration``` as part of the OWASP Top 10 Vulnerabilities list.

Navigate to ```http://machine_ip:86``` and try to exploit the security misconfiguration to read the application's source code.

**Questions / Answers**

Navigate to http://10.10.20.47:86/console to access the Werkzeug console.

![console1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/8b389395-1d8b-4518-8082-96b8d3b6dd89)


Use the Werkzeug console to run the following Python code to execute the ```ls -l``` command on the server:

```
import os; print(os.popen("ls -l").read())
```

![console2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/f4e639f6-5e7e-4e55-8a06-e48c4f4c2e6d)

What is the database file name (the one with the .db extension) in the current directory?

```todo.db```

Modify the code to read the contents of the ```app.py``` file, which contains the application's source code. What is the value of the ```secret_flag``` variable in the source code?

```THM{Just_a_tiny_misconfiguration}```

<h3>Vulnerable and Outdated Components</h3>

Occasionally, you may find that the company/entity you're pen-testing is using a program with a well-known vulnerability.

For example, let's say that a company hasn't updated their version of WordPress for a few years, and using a tool such as WPScan, you find that it's version 4.6. Some quick research will reveal that WordPress 4.6 is vulnerable to an unauthenticated remote code execution(RCE) exploit, and even better, you can find an exploit already made on Exploit-DB.

As you can see, this would be quite devastating because it requires very little work on the attacker's part. Since the vulnerability is already well known, someone else has likely made an exploit for the vulnerability already. The situation worsens when you realise that it's really easy for this to happen. If a company misses a single update for a program they use, it could be vulnerable to any number of attacks.

<h3>Vulnerable and Outdated Components - Exploit</h3>

Recall that since this is about known vulnerabilities, most of the work has already been done for us. Our main job is to find out the information of the software and research it until we can find an exploit. Let's go through that with an example web application.

