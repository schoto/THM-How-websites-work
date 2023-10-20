When you visit a website, your browser (like Safari or Google Chrome) makes a request to a web server asking for information about the page you're visiting. 
It will respond with data that your browser uses to show you the page; a web server is just a dedicated computer somewhere else in the world that handles your requests.

There are two major components that make up a website:

Front End (Client-Side) - the way your browser renders a website.
Back End (Server-Side) - a server that processes your request and returns a response.
There are many other processes involved in your browser making a request to a web server, but for now, you just need to understand that you make a request to a server, and it responds with data your browser uses to render information to you.

What term best describes the component of a web application rendered by your browser?

```front end```

<h3>HTML</h3>

Websites are primarily created using:

- HTML, to build websites and define their structure

- CSS, to make websites look pretty by adding styling options

- JavaScript, implement complex features on pages using interactivity

HyperText Markup Language (HTML) is the language websites are written in. Elements (also known as tags) are the building blocks of HTML pages and tells the browser how to display content. 
The code snippet below shows a simple HTML document, the structure of which is the same for every website:

![example_html](https://github.com/schoto/THM-How-websites-work/assets/69323411/a07b035f-4e55-4f4f-ab55-6dfa7fa52300)

The HTML structure (as shown in the screenshot) has the following components:

- The ```<!DOCTYPE html>``` defines that the page is a HTML5 document. This helps with standardisation across different browsers and tells the browser to use HTML5 to interpret the page.

- The ```<html>``` element is the root element of the HTML page - all other elements come after this element.

- The ```<head>``` element contains information about the page (such as the page title)

- The ```<body>``` element defines the HTML document's body; only content inside of the body is shown in the browser.

- The ```<h1>``` element defines a large heading

- The ```<p>``` element defines a paragraph

- There are many other elements (tags) used for different purposes. For example, there are tags for buttons (```<button>```), images (```<img>```), lists, and much more.

Tags can contain attributes such as the class attribute which can be used to style an element (e.g. make the tag a different color) ```<p class="bold-text">```, or the src attribute which is used on images to specify the location of an image: ```<img src="img/cat.jpg">```.An element can have multiple attributes each with its own unique purpose, e.g., ```<p attribute1="value1" attribute2="value2">```.

Elements can also have an id attribute (```<p id="example">```), which is unique to the element. Unlike the class attribute, where multiple elements can use the same class, an element must have different id's to identify them uniquely. Element id's are used for styling and to identify it by JavaScript.

You can view the HTML of any website by right-clicking and selecting "View Page Source" (Chrome) / "Show Page Source" (Safari).

**Questions / Answers**

Let's play with some HTML! On the right-hand side, you should see a box that renders HTML - If you enter some HTML into the box and click the green "Render HTML Code" button, it will render your HTML on the page; you should see an image of some cats.

One of the images on the cat website is broken - fix it, and the image will reveal the hidden text answer!

```htmlhero```

Add a dog image to the page by adding another img tag (```<img>```) on line 11. The dog image location is ```img/dog-1.png```. What is the text in the dog image?

```doghtml```

<h3>Javascript</h3>

JavaScript (JS) is one of the most popular coding languages in the world and allows pages to become interactive. HTML is used to create the website structure and content, while JavaScript is used to control the functionality of web pages - without JavaScript, a page would not have interactive elements and would always be static. JS can dynamically update the page in real-time, giving functionality to change the style of a button when a particular event on the page occurs (such as when a user clicks a button) or to display moving animations.

JavaScript is added within the page source code and can be either loaded within ```<script>``` tags or can be included remotely with the src attribute: ```<script src="/location/of/javascript_file.js"></script>```

The following JavaScript code finds a HTML element on the page with the id of "demo" and changes the element's contents to "Hack the Planet" : ```document.getElementById("demo").innerHTML = "Hack the Planet";```

HTML elements can also have events, such as "onclick" or "onhover" that execute JavaScript when the event occurs. The following code changes the text of the element with the demo ID to Button Clicked: ```<button onclick='document.getElementById("demo").innerHTML = "Button Clicked";'>Click Me!</button>``` - onclick events can also be defined inside the JavaScript script tags, and not on elements directly.

Click the "View Site" button on this task. On the right-hand side, add JavaScript that changes the demo element's content to "Hack the Planet"

```jsisfun```

Add the button HTML from this task that changes the element's text to "Button Clicked" on the editor on the right, update the code by clicking the "Render HTML+JS Code" button and then click the button.

Done

**Sensitive Data Exposure**



