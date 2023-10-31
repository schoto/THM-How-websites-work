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

