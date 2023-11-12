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

