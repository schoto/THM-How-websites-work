Within this room, we will look at OWASP's TOP 10 vulnerabilities in web applications. You will find these in all types of web applications. But for today we will be looking at OWASP's own creation, Juice Shop!

Juice Shop is a large application so we will not be covering every topic from the top 10.

We will, however, cover the following topics which we recommend you take a look at as you progress through this room.

<------------------------------------------------->

Injection

Broken Authentication

Sensitive Data Exposure

Broken Access Control

Cross-Site Scripting XSS

<------------------------------------------------->

Troubleshooting

The web app takes about 2-5 minutes to load, so please be patient!

Temporarily disable burp in your proxy settings for the current browser. Refresh the page and the flag will be shown. 

(This is not an issue with the application but an issue with burp stopping the flag from being shown. )

If you are doing the XSS Tasks and they are not working. Clear your cookies and site data, as this can sometimes be an issue. 

If you are sure that you have completed the task but it's still not working. Go to [Task 8], as this will allow you to check its completion.



Credits to OWASP and Bjorn Kimminich

<h3>Let's go on an adventure!</h3>

![juice1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/d281d969-b5a5-4730-aaf1-bdafd73f2700)

Before we get into the actual hacking part, it's good to have a look around. In Burp, set the Intercept mode to off and then browse around the site. This allows Burp to log different requests from the server that may be helpful later. 

This is called walking through the application, which is also a form of reconnaissance!

Question #1: What's the Administrator's email address?

![adminmail](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a9c71dff-7f0f-4959-b36a-8ce56b5d29e6)

```admin@juice-sh.op```

Question #2: What parameter is used for searching? 

![search1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/dc2355a0-c32f-4b55-98d8-e1fc0256521b)

Click on the magnifying glass in the top right of the application will pop out a search bar.

![a](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/7b43ff7a-9ea6-40e8-885d-ff2a856a1af3)

We can then input some text and by pressing Enter will search for the text which was just inputted.

Now pay attention to the URL which will now update with the text we just entered.

![10](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/543171f3-16ca-433a-b6e7-46e26b3da146)

We can now see the search parameter after the /#/search? the letter q

```q```

Question #3: What show does Jim reference in his review? 

Jim did a review on the Green Smoothie product. We can see that he mentions a replicator. 

![jim](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a3f4324c-9599-4d47-9720-4386fa1719ba)

If we google "replicator" we will get the results indicating that it is from a TV show called Star Trek

![replicator](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/2959848e-f7e7-41c5-aa86-5709984938d0)

```star trek```

<h3>Inject the juice</h3>

![login](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/f349d5ce-d16a-4cc8-a4c6-461766a171f0)

This task will be focusing on injection vulnerabilities. Injection vulnerabilities are quite dangerous to a company as they can potentially cause downtime and/or loss of data. Identifying injection points within a web application is usually quite simple, as most of them will return an error. There are many types of injection attacks, some of them are:

- SQL Injection is when an attacker enters a malicious or malformed query to either retrieve or tamper data from a database. And in some cases, log into accounts.
- Command Injection is when web applications take input or user-controlled data and run them as system commands. An attacker may tamper with this data to execute their own system commands. This can be seen in applications that perform misconfigured ping tests.
- Email injection is a security vulnerability that allows malicious users to send email messages without prior authorization by the email server. These occur when the attacker adds extra data to fields, which are not interpreted by the server correctly. 

But in our case, we will be using SQL Injection.

Question #1: Log into the administrator account!

After we navigate to the login page, enter some data into the email and password fields.

Before clicking submit, make sure Intercept mode is on.

This will allow us to see the data been sent to the server!

![intercept](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a0a06a7f-d5a8-4ab0-944b-79480e0bdb8c)

We will now change the "a" next to the email to: ' or 1=1-- and forward it to the server.

![sqlblah](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/d446bdb9-b37d-432a-863a-c7536be746b1)

Why does this work?

The character ```'``` will close the brackets in the SQL query

```'OR'``` in a SQL statement will return true if either side of it is true. As ```1=1``` is always true, the whole statement is true. Thus it will tell the server that the email is valid, and log us into user id 0, which happens to be the administrator account.

The ```--``` character is used in SQL to comment out data, any restrictions on the login will no longer work as they are interpreted as a comment. This is like the ```#``` and ```//``` comment in Python and Javascript respectively.

![shop](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/210b77a8-d57c-4ee3-a834-fa344480a4ef)

```— 32a5e0f21372bcc1000a6088b93b458e41f0e02a```

Question #2: Log into the Bender account!

Similar to what we did in Question #1, we will now log into Bender's account! Capture the login request again, but this time we will put: bender@juice-sh.op'-- as the email. 

Now, forward that to the server!

But why don't we put the 1=1?

Well, as the email address is valid (which will return true), we do not need to force it to be true. Thus we are able to use '-- to bypass the login system. Note the 1=1 can be used when the email or username is not known or invalid.

```— fb364762a3c102b2db932069c0e6b78e738d4066```
