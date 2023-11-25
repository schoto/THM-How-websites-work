<h3>Bypassing Client-Side Filtering</h3>

We'll begin with the first (and weakest) line of defence: Client-Side Filtering.

As mentioned previously, client-side filtering tends to be extremely easy to bypass, as it occurs entirely on a machine that you control. When you have access to the code, it's very easy to alter it.

There are four easy ways to bypass your average client-side file upload filter:

1. Turn off Javascript in your browser -- this will work provided the site doesn't require Javascript in order to provide basic functionality. If turning off Javascript completely will prevent the site from working at all then one of the other methods would be more desirable; otherwise, this can be an effective way of completely bypassing the client-side filter.
2. Intercept and modify the incoming page. Using Burpsuite, we can intercept the incoming web page and strip out the Javascript filter before it has a chance to run. The process for this will be covered below.
3. Intercept and modify the file upload. Where the previous method works before the webpage is loaded, this method allows the web page to load as normal, but intercepts the file upload after it's already passed (and been accepted by the filter). Again, we will cover the process for using this method in the course of the task.
4. Send the file directly to the upload point. Why use the webpage with the filter, when you can send the file directly using a tool like ```curl```? Posting the data directly to the page which contains the code for handling the file upload is another effective method for completely bypassing a client side filter. We will not be covering this method in any real depth in this tutorial, however, the syntax for such a command would look something like this: 
```
curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site> 
```
To use this method you would first aim to intercept a successful upload (using Burpsuite or the browser console) to see the parameters being used in the upload, which can then be slotted into the above command.
We will be covering methods two and three in depth below.

Let's assume that, once again, we have found an upload page on a website:

![bypass](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/aa41111a-e49d-40a0-8215-8ffb756ee094)

As always, we'll take a look at the source code. Here we see a basic Javascript function checking for the MIME type of uploaded files:

![jssss](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/86003c95-d626-4faa-8f74-7ddcc9dd68d4)

In this instance we can see that the filter is using a whitelist to exclude any MIME type that isn't ```image/jpeg```.

Our next step is to attempt a file upload -- as expected, if we choose a JPEG, the function accepts it. Anything else and the upload is rejected.

Having established this, let's start Burpsuite and reload the page. We will see our own request to the site, but what we really want to see is the server's response, so right click on the intercepted data, scroll down to "Do Intercept", then select "Response to this request":

![burp](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/9f3b59e3-a4c0-4aeb-a886-9d5f21141a5f)

When we click the "Forward" button at the top of the window, we will then see the server's response to our request. Here we can delete, comment out, or otherwise break the Javascript function before it has a chance to load:

![burp1](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/648bda5a-64c9-49f5-8e57-b52c8cc8eb2b)

Having deleted the function, we once again click "Forward" until the site has finished loading, and are now free to upload any kind of file to the website:

![shelll](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/3503ae5b-1e59-4b0e-8d77-456b55502a89)

It's worth noting here that Burpsuite will not, by default, intercept any external Javascript files that the web page is loading. If you need to edit a script which is not inside the main page being loaded, you'll need to go to the "Options" tab at the top of the Burpsuite window, then under the "Intercept Client Requests" section, edit the condition of the first line to remove ```^js$|```:

![intercept](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/50af390f-e2d8-4481-8338-bdb9656252e3)

We've already bypassed this filter by intercepting and removing it prior to the page being loaded, but let's try doing it by uploading a file with a legitimate extension and MIME type, then intercepting and correcting the upload with Burpsuite.

Having reloaded the webpage to put the filter back in place, let's take the reverse shell that we used before and rename it to be called "shell.jpg". As the MIME type (based on the file extension) automatically checks out, the Client-Side filter lets our payload through without complaining:

![bypass11](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/62f1d53b-7c4c-48e1-827e-b579eb6b7ec9)

Once again we'll activate our Burpsuite intercept, then click "Upload" and catch the request:

![bypaSSS](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/e5c8252e-de83-4da3-a7a7-4e5257386246)

Observe that the MIME type of our PHP shell is currently ```image/jpeg```. We'll change this to ```text/x-php```, and the file extension from ```.jpg``` to ```.php```, then forward the request to the server:

![TEXTXT](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/1cfd8f84-768a-4173-85f3-aa517d71d02f)

Now, when we navigate to ```http://demo.uploadvulns.thm/uploads/shell.php``` having set up a netcat listener, we receive a connection from the shell!

![meoww](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/531aea5e-219c-4881-aa0e-e7d319071e51)


We've covered in detail two ways to bypass a Client-Side file upload filter. Now it's time for you to give it a shot for yourself! Navigate to ```java.uploadvulns.thm``` and bypass the filter to get a reverse shell. Remember that not all client-side scripts are inline! As mentioned previously, Gobuster would be a very good place to start here -- the upload directory name will be changing with every new challenge.

**Q/A**

What is the flag in /var/www/?

```THM{NDllZDQxNjJjOTE0YWNhZGY3YjljNmE2}```

<h3>Bypassing Server-Side Filtering: File Extensions</h3>

Time to turn things up another notch!
Client-side filters are easy to bypass -- you can see the code for them, even if it's been obfuscated and needs processed before you can read it; but what happens when you can't see or manipulate the code? Well, that's a server-side filter. In short, we have to perform a lot of testing to build up an idea of what is or is not allowed through the filter, then gradually put together a payload which conforms to the restrictions.

For the first part of this task we'll take a look at a website that's using a blacklist for file extensions as a server side filter. There are a variety of different ways that this could be coded, and the bypass we use is dependent on that. In the real world we wouldn't be able to see the code for this, but for this example, it will be included here:

```
<?php
    //Get the extension
    $extension = pathinfo($_FILES["fileToUpload"]["name"])["extension"];
    //Check the extension against the blacklist -- .php and .phtml
    switch($extension){
        case "php":
        case "phtml":
        case NULL:
            $uploadFail = True;
            break;
        default:
            $uploadFail = False;
    }
?>
```

In this instance, the code is looking for the last period (```.```) in the file name and uses that to confirm the extension, so that is what we'll be trying to bypass here. Other ways the code could be working include: searching for the first period in the file name, or splitting the file name at each period and checking to see if any blacklisted extensions show up. We'll cover this latter case later on, but in the meantime, let's focus on the code we've got here.

We can see that the code is filtering out the ```.php``` and ```.phtml``` extensions, so if we want to upload a PHP script we're going to have to find another extension. The wikipedia page for PHP gives us a few common extensions that we can try; however, there are actually a variety of other more rarely used extensions available that webservers may nonetheless still recognise. These include: ```.php3```, .```php4```, ```.php5```, ```.php7```, ```.phps```, ```.php-s```, ```.pht``` and ```.phar```. Many of these bypass the filter (which only blocks.php and .phtml), but it appears that the server is configured not to recognise them as PHP files, as in the below example:

![array](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/eeb2048f-73a0-461e-aa37-e537ac2f6360)

This is actually the default for Apache2 servers, at the time of writing; however, the sysadmin may have changed the default configuration (or the server may be out of date), so it's well worth trying.

Eventually we find that the ```.phar``` extension bypasses the filter -- and works -- thus giving us our shell:

![extension filtering](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/6b9c64e4-7b89-4617-955f-cf7b4432395b)

Let's have a look at another example, with a different filter. This time we'll do it completely black-box: i.e. without the source code.

Once again, we have our upload form:

![filtering meow](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/8e97a1ea-240d-443d-8c92-41af6c3e3103)

Ok, we'll start by scoping this out with a completely legitimate upload. Let's try uploading the ```spaniel.jpg``` image from before:

![filterrrrrr](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/d1f35f9d-750b-48b2-ac7e-db8de8d7a314)

Well, that tells us that JPEGS are accepted at least. Let's go for one that we can be pretty sure will be rejected (```shell.php```):

![kali](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/048b2503-91e6-40a0-a870-73bd83a1197e)

Can't say that was unexpected.

From here we enumerate further, trying the techniques from above and just generally trying to get an idea of what the filter will accept or reject.

In this case we find that there are no shell extensions that both execute, and are not filtered, so it's back to the drawing board.

In the previous example we saw that the code was using the ```pathinfo()``` PHP function to get the last few characters after the ```.```, but what happens if it filters the input slightly differently?

Let's try uploading a file called ```shell.jpg.php```. We already know that JPEG files are accepted, so what if the filter is just checking to see if the ```.jpg``` file extension is somewhere within the input?

Pseudocode for this kind of filter may look something like this:

```
ACCEPT FILE FROM THE USER -- SAVE FILENAME IN VARIABLE userInput
IF STRING ".jpg" IS IN VARIABLE userInput:
    SAVE THE FILE
ELSE:
    RETURN ERROR MESSAGE
```

When we try to upload our file we get a success message. Navigating to the ```/uploads``` directory confirms that the payload was successfully uploaded:

![index](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/249ea7cb-b700-4917-8ca6-98d75bb9eebf)

Activating it, we receive our shell:

![upload](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/974d5275-3ff4-4b7d-b43f-4ef2727bae03)

This is by no means an exhaustive list of upload vulnerabilities related to file extensions. As with everything in hacking, we are looking to exploit flaws in code that others have written; this code may very well be uniquely written for the task at hand. This is the really important point to take away from this task: there are a million different ways to implement the same feature when it comes to programming -- your exploitation must be tailored to the filter at hand. The key to bypassing any kind of server side filter is to enumerate and see what is allowed, as well as what is blocked; then try to craft a payload which can pass the criteria the filter is looking for.

Now your turn. You know the drill by now -- figure out and bypass the filter to upload and activate a shell. Your flag is in ```/var/www/```. The site you're accessing is ```annex.uploadvulns.thm```.

Be aware that this task has also implemented a randomised naming scheme for the first time. For now you shouldn't have any trouble finding your shell, but be aware that directories will not always be indexable...

**Q/A**

What is the flag in /var/www/?

```THM{MGEyYzJiYmI3ODIyM2FlNTNkNjZjYjFl}```

<h3>Bypassing Server-Side Filtering: Magic Numbers</h3>

We've already had a look at server-side extension filtering, but let's also take the opportunity to see how magic number checking could be implemented as a server-side filter.

As mentioned previously, magic numbers are used as a more accurate identifier of files. The magic number of a file is a string of hex digits, and is always the very first thing in a file. Knowing this, it's possible to use magic numbers to validate file uploads, simply by reading those first few bytes and comparing them against either a whitelist or a blacklist. Bear in mind that this technique can be very effective against a PHP based webserver; however, it can sometimes fail against other types of webserver (hint hint).

Let's take a look at an example. As per usual, we have an upload page:

![magic](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/be682444-c958-4abd-a6a9-d2c163c32d56)

As expected, if we upload our standard shell.php file, we get an error; however, if we upload a JPEG, the website is fine with it. All running as per expected so far.

From the previous attempt at an upload, we know that JPEG files are accepted, so let's try adding the JPEG magic number to the top of our ```shell.php``` file. A quick look at the list of file signatures on Wikipedia shows us that there are several possible magic numbers of JPEG files (https://en.wikipedia.org/wiki/List_of_file_signatures)

 It shouldn't matter which we use here, so let's just pick one (```FF D8 FF DB```). We could add the ASCII representation of these digits (ÿØÿÛ) directly to the top of the file but it's often easier to work directly with the hexadecimal representation, so let's cover that method.

 Before we get started, let's use the Linux ```file``` command to check the file type of our shell:

![shell ;eow](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/cd2d3bad-a4f0-4207-a754-1e4ba72a7917)

As expected, the command tells us that the filetype is PHP. Keep this in mind as we proceed with the explanation.

We can see that the magic number we've chosen is four bytes long, so let's open up the reverse shell script and add four random characters on the first line. These characters do not matter, so for this example we'll just use four "A"s:

![AAAA](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/0adcc54a-d299-4c54-b1bb-c08b887009e5)

Save the file and exit. Next we're going to reopen the file in hexeditor (which comes by default on Kali), or any other tool which allows you to see and edit the shell as hex. In ```hexeditor``` the file looks like this:

![asCCii](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/e9a17cdb-c681-4d75-9209-fca02cdcaeb4)

Note the four bytes in the red box: they are all ```41```, which is the hex code for a capital "A" -- exactly what we added at the top of the file previously.

Change this to the magic number we found earlier for JPEG files: ```FF D8 FF DB```

![offset](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/21c86744-4923-44af-85bb-b1bd19ea9357)

Now if we save and exit the file (Ctrl + x), we can use ```file``` once again, and see that we have successfully spoofed the filetype of our shell:

![data](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/f6ce14a2-96fb-4880-8892-113192201796)

Perfect. Now let's try uploading the modified shell and see if it bypasses the filter!

![magic numberes](https://github.com/schoto/THM-Web-Hacking-Fundamentals/assets/69323411/57f73a8f-aad8-4094-8725-c99bc73b96b1)

There we have it -- we bypassed the server-side magic number filter and received a reverse shell.

Head to ```magic.uploadvulns.thm``` -- it's time for the last mini-challenge.

This will be the final example website you have to hack before the challenge in task eleven; as such, we are once again stepping up the level of basic security. The website in the last task implemented an altered naming scheme, prepending the date and time of upload to the file name. This task will not do so to keep it relatively easy; however, directory indexing has been turned off, so you will not be able to navigate to the directory containing the uploads. Instead you will need to access the shell directly using its URI.

Bypass the magic number filter to upload a shell. Find the location of the uploaded shell and activate it. Your flag is in ```/var/www/```.

**Q/A**

Grab the flag from /var/www/

```THM{MWY5ZGU4NzE0ZDlhNjE1NGM4ZThjZDJh}```

<h3>Example Methodology</h3>

We've seen various different types of filter now -- both client side and server side -- as well as the general methodology for file upload attacks. In the next task you're going to be given a black-box file upload challenge to complete, so let's take the opportunity to discuss an example methodology for approaching this kind of challenge in a little more depth. You may develop your own alternative to this method, however, if you're new to this kind of attack, you may find the following information useful.

We'll look at this as a step-by-step process. Let's say that we've been given a website to perform a security audit on.

1. The first thing we would do is take a look at the website as a whole. Using browser extensions such as the aforementioned Wappalyzer (or by hand) we would look for indicators of what languages and frameworks the web application might have been built with. Be aware that Wappalyzer is not always 100% accurate. A good start to enumerating this manually would be by making a request to the website and intercepting the response with Burpsuite. Headers such as ```server``` or ```x-powered-by``` can be used to gain information about the server. We would also be looking for vectors of attack, like, for example, an upload page.

2. Having found an upload page, we would then aim to inspect it further. Looking at the source code for client-side scripts to determine if there are any client-side filters to bypass would be a good thing to start with, as this is completely in our control.

3. We would then attempt a completely innocent file upload. From here we would look to see how our file is accessed. In other words, can we access it directly in an uploads folder? Is it embedded in a page somewhere? What's the naming scheme of the website? This is where tools such as Gobuster might come in if the location is not immediately obvious. This step is extremely important as it not only improves our knowledge of the virtual landscape we're attacking, it also gives us a baseline "accepted" file which we can base further testing on.

- An important Gobuster switch here is the ```-x``` switch, which can be used to look for files with specific extensions. For example, if you added ```-x php,txt,html``` to your Gobuster command, the tool would append ```.php```, ```.txt```, and ```.html``` to each word in the selected wordlist, one at a time. This can be very useful if you've managed to upload a payload and the server is changing the name of uploaded files.

4. Having ascertained how and where our uploaded files can be accessed, we would then attempt a malicious file upload, bypassing any client-side filters we found in step two. We would expect our upload to be stopped by a server side filter, but the error message that it gives us can be extremely useful in determining our next steps.

Assuming that our malicious file upload has been stopped by the server, here are some ways to ascertain what kind of server-side filter may be in place:

- If you can successfully upload a file with a totally invalid file extension (e.g. ```testingimage.invalidfileextension```) then the chances are that the server is using an extension blacklist to filter out executable files. If this upload fails then any extension filter will be operating on a whitelist.

- Try re-uploading your originally accepted innocent file, but this time change the magic number of the file to be something that you would expect to be filtered. If the upload fails then you know that the server is using a magic number based filter.

- As with the previous point, try to upload your innocent file, but intercept the request with Burpsuite and change the MIME type of the upload to something that you would expect to be filtered. If the upload fails then you know that the server is filtering based on MIME types.

- Enumerating file length filters is a case of uploading a small file, then uploading progressively bigger files until you hit the filter. At that point you'll know what the acceptable limit is. If you're very lucky then the error message of original upload may outright tell you what the size limit is. Be aware that a small file length limit may prevent you from uploading the reverse shell we've been using so far.

You should now be well equipped to take on the challenge in task eleven.

<h3>Challenge</h3>

It's challenge time!

Head over to ```jewel.uploadvulns.thm```.

Take what you've learned in this room and use it to get a shell on this machine. As per usual, your flag is in ```/var/www/```. Bear in mind that this challenge will be an accumulation of everything you've learnt so far, so there may be multiple filters to bypass. The attached wordlist might help. Also remember that not all webservers have a PHP backend...

```THM{NzRlYTUwNTIzODMwMWZhMzBiY2JlZWU2}```
