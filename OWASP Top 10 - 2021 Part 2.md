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

A good example of such vulnerabilities occurred on Instagram a while ago. Instagram allowed users to reset their forgotten passwords by sending them a 6-digit code to their mobile number via SMS for validation. If an attacker wanted to access a victim's account, he could try to brute-force the 6-digit code. As expected, this was not directly possible as Instagram had rate-limiting implemented so that after 250 attempts, the user would be blocked from trying further.

![insec1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/2367ee39-d142-44ee-9bf6-8e91382d779d)



