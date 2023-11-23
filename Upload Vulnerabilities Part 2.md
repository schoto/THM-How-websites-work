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

