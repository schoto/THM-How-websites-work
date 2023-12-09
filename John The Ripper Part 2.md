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

So far we've been using John's wordlist mode to deal with brute forcing simple., and not so simple hashes. But John also has another mode, called Single Crack mode. In this mode, John uses only the information provided in the username, to try and work out possible passwords heuristically, by slightly changing the letters and numbers contained within the username.

**Word Mangling**

The best way to show what Single Crack mode is,  and what word mangling is, is to actually go through an example:

If we take the username: Markus

Some possible passwords could be:

- Markus1, Markus2, Markus3 (etc.)
- MArkus, MARkus, MARKus (etc.)
- Markus!, Markus$, Markus* (etc.)

This technique is called word mangling. John is building it's own dictionary based on the information that it has been fed and uses a set of rules called "mangling rules" which define how it can mutate the word it started with to generate a wordlist based off of relevant factors for the target you're trying to crack. This is exploiting how poor passwords can be based off of information about the username, or the service they're logging into.

**GECOS**

John's implementation of word mangling also features compatibility with the Gecos fields of the UNIX operating system, and other UNIX-like operating systems such as Linux. So what are Gecos? Remember in the last task where we were looking at the entries of both /etc/shadow and /etc/passwd? Well if you look closely You can see that each field is seperated by a colon ":". Each one of the fields that these records are split into are called Gecos fields. John can take information stored in those records, such as full name and home directory name to add in to the wordlist it generates when cracking /etc/shadow hashes with single crack mode.

**Using Single Crack Mode**

To use single crack mode, we use roughly the same syntax that we've used to so far, for example if we wanted to crack the password of the user named "Mike", using single mode, we'd use:

```john --single --format=[format] [path to file]```

```--single```- This flag lets john know you want to use the single hash cracking mode.

Example Usage:

```john --single --format=raw-sha256 hashes.txt```

A Note on File Formats in Single Crack Mode:

If you're cracking hashes in single crack mode, you need to change the file format that you're feeding john for it to understand what data to create a wordlist from. You do this by prepending the hash with the username that the hash belongs to, so according to the above example- we would change the file hashes.txt

From:

```1efee03cdcb96d90ad48ccc7b8666033```

To

```mike:1efee03cdcb96d90ad48ccc7b8666033```

**Practical**

Now you're familiar with the Syntax for John's single crack mode, download the attached hash and crack it, assuming that the user it belongs to is called "Joker".

**Q/A**

![joker](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/df44d0ed-dfbe-4600-a720-7eae57062bba)

<h3>Custom Rules</h3>

**What are Custom Rules?**

As we journeyed through our exploration of what John can do in Single Crack Mode- you may have some ideas about what some good mangling patterns would be, or what patterns your passwords often use- that could be replicated with a certain mangling pattern. The good news is you can define your own sets of rules, which John will use to dynamically create passwords. This is especially useful when you know more information about the password structure of whatever your target is.

**Common Custom Rules**

Many organisations will require a certain level of password complexity to try and combat dictionary attacks, meaning that if you create an account somewhere, go to create a password and enter:

polopassword

You may receive a prompt telling you that passwords have to contain at least one of the following:

- Capital letter
- Number
- Symbol

This is good! However, we can exploit the fact that most users will be predictable in the location of these symbols. For the above criteria, many users will use something like the following:

Polopassword1!

A password with the capital letter first, and a number followed by a symbol at the end. This pattern of the familiar password, appended and prepended by modifiers (such as the capital letter or symbols) is a memorable pattern that people will use, and reuse when they create passwords. This pattern can let us exploit password complexity predictability.

Now this does meet the password complexity requirements, however as an attacker we can exploit the fact we know the likely position of these added elements to create dynamic passwords from our wordlists.

**How to create Custom Rules**

Custom rules are defined in the ```john.conf``` file, usually located in ```/etc/john/john.conf``` if you have installed John using a package manager or built from source with make and in ```/opt/john/john.conf``` on the TryHackMe Attackbox.

Let's go over the syntax of these custom rules, using the example above as our target pattern. Note that there is a massive level of granular control that you can define in these rules, I would suggest taking a look at the wiki here --> https://www.openwall.com/john/doc/RULES.shtml in order to get a full view of the types of modifier you can use, as well as more examples of rule implementation.

The first line:

```[List.Rules:THMRules]``` - Is used to define the name of your rule, this is what you will use to call your custom rule as a John argument.

We then use a regex style pattern match to define where in the word will be modified, again- we will only cover the basic and most common modifiers here:

- ```Az``` - Takes the word and appends it with the characters you define

- ```A0``` - Takes the word and prepends it with the characters you define

- ```c``` - Capitalises the character positionally

These can be used in combination to define where and what in the word you want to modify.

Lastly, we then need to define what characters should be appended, prepended or otherwise included, we do this by adding character sets in square brackets ```[ ]``` in the order they should be used. These directly follow the modifier patterns inside of double quotes ```" "```. Here are some common examples:

```[0-9]``` - Will include numbers 0-9

```[0]``` - Will include only the number 0

```[A-z]``` - Will include both upper and lowercase

```[A-Z]``` - Will include only uppercase letters

```[a-z]``` - Will include only lowercase letters

```[a]``` - Will include only a

```[!£$%@]``` - Will include the symbols !£$%@

Putting this all together, in order to generate a wordlist from the rules that would match the example password "Polopassword1!" (assuming the word polopassword was in our wordlist) we would create a rule entry that looks like this:

```[List.Rules:PoloPassword]```

```cAz"[0-9] [!£$%@]"```

In order to:

Capitalise the first  letter - ```c```

Append to the end of the word - ```Az```

A number in the range 0-9 - ```[0-9]```

Followed by a symbol that is one of ```[!£$%@]```

**Using Custom Rules**

We could then call this custom rule as a John argument using the  ```--rule=PoloPassword``` flag.

As a full command: ```john --wordlist=[path to wordlist] --rule=PoloPassword [path to file]```

As a note I find it helpful to talk out the patterns if you're writing a rule- as shown above, the same applies to writing RegEx patterns too.

Jumbo John already comes with a large list of custom rules, which contain modifiers for use almost all cases. If you get stuck, try looking at those rules [around line 678] if your syntax isn't working properly.

Now, time for you to have a go!

**Q/A**

![thththtm](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/8bce476e-1e37-4c61-8e52-baf888821196)

<h3>Cracking Password Protected Zip Files</h3>

Cracking a Password Protected Zip File
Yes! You read that right. We can use John to crack the password on password protected Zip files. Again, we're going to be using a separate part of the john suite of tools to convert the zip file into a format that John will understand, but for all intents and purposes, we're going to be using the syntax that you're already pretty familiar with by now.



Zip2John
Similarly to the unshadow tool that we used previously, we're going to be using the zip2john tool to convert the zip file into a hash format that John is able to understand, and hopefully crack. The basic usage is like this:

zip2john [options] [zip file] > [output file]

[options] - Allows you to pass specific checksum options to zip2john, this shouldn't often be necessary

[zip file] - The path to the zip file you wish to get the hash of

> - This is the output director, we're using this to send the output from this file to the...

[output file] - This is the file that will store the output from

Example Usage

zip2john zipfile.zip > zip_hash.txt


Cracking
We're then able to take the file we output from zip2john in our example use case called "zip_hash.txt" and, as we did with unshadow, feed it directly into John as we have made the input specifically for it.

john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt


Practical
Now have a go at cracking the attached "secure" zip file!

**Q/A**

![123](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/90657236-af06-48a8-8fd9-3f93b7cea625)

<h3>Cracking Password Protected RAR Archives</h3>

We can use a similar process to the one we used in the last task to obtain the password for rar archives. If you aren't familiar, rar archives are compressed files created by the Winrar archive manager. Just like zip files they compress a wide variety of folders and files.

**Rar2John**

Almost identical to the zip2john tool that we just used, we're going to use the rar2john tool to convert the rar file into a hash format that John is able to understand. The basic syntax is as follows:

```rar2john [rar file] > [output file]```

```rar2john``` - Invokes the rar2john tool

```[rar file]``` - The path to the rar file you wish to get the hash of

```>``` - This is the output director, we're using this to send the output from this file to the...

```[output file]``` - This is the file that will store the output from 

Example Usage

```rar2john rarfile.rar > rar_hash.txt```

**Cracking**

Once again, we're then able to take the file we output from rar2john in our example use case called "rar_hash.txt" and, as we did with zip2john we can feed it directly into John..

```john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt```

**Practical**

Now have a go at cracking the attached "secure" rar file!

**Q/A**

![answers](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/39daee84-63b8-4119-b054-ac9c93486e8b)

<h3>Cracking SSH Keys with John</h3>


