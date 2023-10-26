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

