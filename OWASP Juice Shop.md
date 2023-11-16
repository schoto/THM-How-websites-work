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


