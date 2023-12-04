<h3>Cracking /etc/shadow Hashes</h3>

**Cracking Hashes from /etc/shadow**

The /etc/shadow file is the file on Linux machines where password hashes are stored. It also stores other information, such as the date of last password change and password expiration information. It contains one entry per line for each user or user account of the system. This file is usually only accessible by the root user- so in order to get your hands on the hashes you must have sufficient privileges, but if you do- there is a chance that you will be able to crack some of the hashes.

**Unshadowing**

John can be very particular about the formats it needs data in to be able to work with it, for this reason- in order to crack /etc/shadow passwords, you must combine it with the /etc/passwd file in order for John to understand the data it's being given. To do this, we use a tool built into the John suite of tools called unshadow. The basic syntax of unshadow is as follows:

```
unshadow [path to passwd] [path to shadow]
```

```unshadow``` - Invokes the unshadow tool

```[path to passwd]``` - The file that contains the copy of the /etc/passwd file you've taken from the target machine

```[path to shadow]``` - The file that contains the copy of the /etc/shadow file you've taken from the target machine

Example Usage:

```unshadow local_passwd local_shadow > unshadowed.txt```

Note on the files

When using unshadow, you can either use the entire /etc/passwd and /etc/shadow file- if you have them available, or you can use the relevant line from each, for example:

FILE 1 - local_passwd

Contains the /etc/passwd line for the root user:

```root:x:0:0::/root:/bin/bash```

FILE 2 - local_shadow

Contains the ```/etc/shadow``` line for the root user:

```root:$6$2nwjN454g.dv4HN/$m9Z/r2xVfweYVkrr.v5Ft8Ws3/YYksfNwq96UL1FX0OJjY1L6l.DS3KEVsZ9rOVLB/ldTeEL/OIhJZ4GMFMGA0:18576::::::```

**Cracking**

We're then able to feed the output from unshadow, in our example use case called "unshadowed.txt" directly into John. We should not need to specify a mode here as we have made the input specifically for John, however in some cases you will need to specify the format as we have done previously using: 

```--format=sha512crypt```

```john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt```

**Practical**

Now, see if you can follow the process to crack the password hash of the root user that is provided in the "etchashes.txt" file. Good luck!

**Q/A**

![passwd](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/606075b7-871e-452d-b2ae-82af857a2507)

<h3> Single Crack Mode</h3>


