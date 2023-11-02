<h4>Introduction to the Burp Proxy</h4>

The Burp Proxy is the most fundamental (and most important!) of the tools available in Burp Suite. It allows us to capture requests and responses between ourselves and our target. These can then be manipulated or sent to other tools for further processing before being allowed to continue to their destination.

For example, if we make a request to ```https://tryhackme.com``` through the Burp Proxy, our request will be captured and won't be allowed to continue to the TryHackMe servers until we explicitly allow it through. We can choose to do the same with the response from the server, although this isn't active by default. This ability to intercept requests ultimately means that we can take complete control over our web traffic -- an invaluable ability when it comes to testing web applications.

There are a few configurations we need to make before we can use the proxy, but let's start by looking at the interface.

Note: You do not need to follow along with this task -- just read the information and understand what the Proxy is used for.

When we first open the Proxy tab, Burp gives us a bunch of useful information and background reading. This information is well worth reading through; however, the real magic happens after we capture a request:

![burp1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/9300eeb4-c605-4863-9048-bb8fda16c6a4)

With the proxy active, a request was made to the TryHackMe website. At this point, the browser making the request will hang, and the request will appear in the Proxy tab giving us the view shown in the screenshot above. We can then choose to forward or drop the request (potentially after editing it). We can also do various other things here, such as sending the request to one of the other Burp modules, copying it as a cURL command, saving it to a file, and many others.

When we have finished working with the Proxy, we can click the "Intercept is on" button to disable the Intercept, which will allow requests to pass through the proxy without being stopped.

Burp Suite will still (by default) be logging requests made through the proxy when the intercept is off. This can be very useful for going back and analysing prior requests, even if we didn't specifically capture them when they were made.

Burp will also capture and log WebSocket communication, which, again, can be exceedingly helpful when analysing a web app.

The logs can be viewed by going to the "HTTP history" and "WebSockets history" sub-tabs:

![burp2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/ae41205f-c1aa-4b55-926a-3a452fd95924)

It is worth noting that any requests captured here can be sent to other tools in the framework by right-clicking them and choosing "Send to...". For example, we could take a previous HTTP request that has already been proxied to the target and send it to Repeater.

Finally, there are also Proxy specific options, which in the Proxy Settings, accessible by clicking on the "Proxy Settings" button.

These options give us a lot of control over how the proxy operates, so it is an excellent idea to familiarise yourself with these.

For example, the proxy will not intercept server responses by default unless we explicitly ask it to on a per-request basis. We can override the default setting by selecting the "Intercept responses based on the following rules" checkbox and picking one or more rules. The ```Or Request Was Intercepted``` rule is good for catching responses to all requests that were intercepted by the proxy:

![burp3](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/c49f57e6-2bc0-4feb-84bb-bb6196781e5f)

The ```And URL Is in target scope``` is another very good default rule; we will look at scoping later in this room.

You can make your own rules for most of the Proxy options, so this is one section where looking around and experimenting will serve you very well indeed!

Another particularly useful section of this sub-tab is the "Match and Replace" section; this allows you to perform regexes on incoming and outgoing requests. For example, you can automatically change your user agent to emulate a different web browser in outgoing requests or remove all cookies being set in incoming requests. Again, you are free to make your own rules here.

**Questions / Answers**

Which button would we choose to send an intercepted request to the target in Burp Proxy?

```Forward```

[Research] What is the default keybind for this?

Note: Assume you are using Windows or Linux (i.e. swap Cmd for Ctrl).

```Ctrl+F```

<h4>Connecting through the Proxy (FoxyProxy)</h4>

You've seen the theory; now it's time to start using the proxy for yourself.

There are two ways to proxy our traffic through Burp Suite.

We could use the embedded browser (we will cover this in a later task).
We can configure our local web browser to proxy our traffic through Burp; this is more common and so will be the focus of this task.

The Burp Proxy works by opening a web interface on ```127.0.0.1:8080``` (by default). As implied by the fact that this is a "proxy", we need to redirect all of our browser traffic through this port before we can start intercepting it with Burp. We can do this by altering our browser settings or, more commonly, by using a Firefox browser extension called FoxyProxy. FoxyProxy allows us to save proxy profiles, meaning we can quickly and easily switch to our "Burp Suite" profile in a matter of clicks, then disable the proxy just as easily.

Note: All instructions will be given with Firefox in mind, as this is the default browser for both Kali Linux and the TryHackMe AttackBox. If you are using another browser locally then you are advised to use the AttackBox, or you may otherwise need to find alternative methods to those presented in this task. If you can't get the proxy working in your local browser and do not want to use the AttackBox, then you may wish to skip ahead to the Burp Suite Browser task.

There are two versions of FoxyProxy: Basic and Standard. Both versions allow you to change your proxy settings on the fly; however, FoxyProxy Standard gives you a lot more control over what traffic gets sent through the proxy. For example, it will allow you to set pattern matching rules to determine whether a request should be proxied or not: this is more complicated than the simple proxy offered by FoxyProxy basic.

The basic edition is more than adequate for our usage. It is pre-installed and configured in the Firefox browser of the AttackBox, so if you are using the AttackBox, please feel free to skip ahead to the last section of this task.

If you are using your own machine, you can download FoxyProxy Basic here --> https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-basic/

Once installed, a button should appear at the top right of the screen which allows you to access your proxy configurations:

![foxy1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/02fd6974-ea09-407f-81e6-ad2f09aef191)

There are no default configurations, so let's click on the "Options" button to create our Burp Proxy config.

This will open a new browser tab with the FoxyProxy options page:

![foxy2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/22f86dcf-5139-40e4-b7f4-5524ede9040c)

Click on the "Add" button and fill in the following values:

```
Title: Burp (or anything else you prefer)

Proxy IP: 127.0.0.1

Port: 8080
```

![foxy3](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/b7348823-93ea-4eb2-af43-9f0010602632)

Now click "Save".

When you click on the FoxyProxy icon at the top of the screen, you will see that that there is a configuration available for Burp:

![foxy4](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/2c3cd7b3-21a2-4942-aaa4-b948eb884016)

If we click on the "Burp" config, our browser will start directing all of our traffic through ```127.0.0.1:8080```. Be warned: if Burp Suite is not running, your browser will not be able to make any requests when this config is activated!

Activate this config now -- the icon in the menu should change to indicate that we have a proxy running: ![foxy6](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/c2ce0dd8-3c41-4d90-b1ee-cf8b23f91d30)

Next, switch over to Burp Suite and make sure the Intercept is On:

![foxy7](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/4f9f2f78-243b-441a-8dc3-fb3ac1adddd2)

Now, try accessing the homepage for ```http://MACHINE_IP/``` in Firefox. Your browser should hang, and your proxy will populate with the request headers.

Congratulations, you just intercepted your first request!

From here, you can choose to forward or drop the request. Alternatively, you could send it to another tool or perform any number of other actions by right-clicking on the request and selecting an option from the right-click menu.

Remember: Whilst you are connected to the proxy and have the Proxy Intercept switched on, your browser will hang whenever you make a request. A very common mistake when you are learning to use Burp Suite (and indeed, later on!) is to accidentally leave the intercept switched on and ergo be unable to make any web requests through your browser. If your browser is hanging and you don't know why: check your proxy!

**Questions / Answers**


Read through the options in the right-click menu.

There is one particularly useful option that allows you to intercept and modify the response to your request.

What is this option?

Note: The option is in a dropdown sub-menu.

```Response to this request```

Bonus Question -- Optional Try installing FoxyProxy standard and have a look at the pattern matching features.

<h4>Proxying HTTPS</h4>

Note: The AttackBox is already configured to solve the problem posed in this task. If you are using the AttackBox and don't wish to read through the information here, you can skip to the next task.

I already have AttackBox, so I'm gonna go ahead and skip this section

<h4>The Burp Suite Browser</h4>

If the last few tasks seemed overly complex, rest assured, this topic will be a lot simpler.

In addition to giving us the option to modify our regular web browser to work with the proxy, Burp Suite also includes a built-in Chromium browser that is pre-configured to use the proxy without any of the modifications we just had to do.

Whilst this may seem ideal, it is not as commonly used as the process detailed in the previous few tasks. People tend to stick with their own browser as it gives them a lot more customisability; however, both are perfectly valid choices.

We can start the Burp Browser with the "Open Browser" button in the proxy tab:

![burp123](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a5379e43-5f4a-42f1-a798-250f4e92b098)

A Chromium window will now pop up. Any requests we make in this will go through the proxy.

Note: There are many settings to do with the Burp Browser in the Project options and User options tabs -- make sure to go look at them!

If we are running on Linux as the root user (as we are with the AttackBox), Burp Suite is unable to create a sandbox environment to start the Burp Browser in, causing it to throw an error and die:

![123burp](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/d0b45bea-f96c-423a-b4eb-966e76730c44)

There are two simple solutions to this:

- The smart option: We could create a new user and run Burp Suite under a low privilege account.

- The easy option: We could go to Project options -> Misc -> Embedded Browser and check the Allow the embedded browser to run without a sandbox option. Checking this option will allow the browser to start, but be aware that it is disabled by default for security reasons: if we get compromised using the browser, then an attacker will have access to our entire machine. On the training environment of the AttackBox this is unlikely (and isn't a huge issue even if it does happen), but keep it in mind if you try this on a local installation of Burp Suite.

Be aware that you will need to do this if using the embedded browser on the AttackBox.

---------------------------------------------------------

<h4>Scoping and Targetting</h4>

Finally, we come to one of the most important parts of using the Burp Proxy: Scoping.

It can get extremely tedious having Burp capturing all of our traffic. When it logs everything (including traffic to sites we aren't targeting), it muddies up logs we may later wish to send to clients. In short, allowing Burp to capture everything can quickly become a massive pain.

What's the solution? Scoping.

Setting a scope for the project allows us to define what gets proxied and logged. We can restrict Burp Suite to only target the web application(s) that we want to test. The easiest way to do this is by switching over to the "Target" tab, right-clicking our target from our list on the left, then choosing "Add To Scope". Burp will then ask us whether we want to stop logging anything which isn't in scope -- most of the time we want to choose "yes" here.

![burpgif](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/b8a75d95-206d-4e85-8f16-98778fa5056a)

We can now check our scope by switching to the "Scope" sub-tab (as shown in the GIF above).

The Scope Settings window allows us to control what we are targeting by either Including or Excluding domains / IPs. This is a very powerful section, so it's well worth taking the time to get accustomed to using it.

We just chose to disable logging for out of scope traffic, but the proxy will still be intercepting everything. To turn this off, we need to go into the Proxy Options sub-tab and select ```And URL Is in target scope``` from the Intercept Client Requests section:

![burpgif1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/1755f1b0-4b3a-4d0d-9bf3-8afa13fb7200)

With this option selected, the proxy will completely ignore anything that isn't in the scope, vastly cleaning up the traffic coming through Burp.

<h4>Site Map and Issue Definitions</h4>

Control of the scope may be the most useful aspect of the Target tab, but it's by no means the only use for this section of Burp.

There are three sub-tabs under Target:

- Site map allows us to map out the apps we are targeting in a tree structure. Every page that we visit will show up here, allowing us to automatically generate a site map for the target simply by browsing around the web app. Burp Pro would also allow us to spider the targets automatically (i.e. look through every page for links and use them to map out as much of the site as-is publicly accessible using the links between pages); however, with Burp Community, we can still use this to accumulate data whilst we perform our initial enumeration steps.

The Site map can be especially useful if we want to map out an API, as whenever we visit a page, any API endpoints that the page retrieves data from whilst loading will show up here.

- Scope Settings: We have already seen the Scope Settings window — it allows us to control Burp's target scope for the project.

- Issue Definitions: Whilst we don't have access to the Burp Suite vulnerability scanner in Burp Community, we do still have access to a list of all the vulnerabilities it looks for. The Issue Definitions section gives us a huge list of web vulnerabilities (complete with descriptions and references) which we can draw from should we need citations for a report or help describing a vulnerability.

**Questions / Answers**


Take a look around the site on http://MACHINE_IP/ -- we will be using this a lot throughout the module. Visit every page linked to from the homepage, then check your sitemap -- one endpoint should stand out as being very unusual!

Visit this in your browser (or use the "Response" section of the site map entry for that endpoint)

What is the flag you receive?

```THM{NmNlZTliNGE1MWU1ZTQzMzgzNmFiNWVk}```


Look through the Issue Definitions list.

What is the typical severity of a Vulnerable JavaScript dependency?

Low

<h4>Example Attack</h4>

Having looked at how to set up and configure our proxy, let's go through a simplified real-world example.



