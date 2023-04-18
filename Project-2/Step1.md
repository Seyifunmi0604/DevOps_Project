# STEP 1 – INSTALLING THE NGINX WEB SERVER

# Step 1 – Installing the Nginx Web Server

In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We’ll use the apt package manager to install this package.
Since this is our first time using apt for this session, start off by updating your server’s package index. Following that, you can use apt install to get Nginx installed:

<div id="code-container">
  <pre><code>sudo apt update && sudo apt install nginx -y</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

To verify that nginx was successfully installed and is running as a service in Ubuntu, run:

<div id="code-container">
  <pre><code>sudo systemctl status nginx</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture2 1](https://user-images.githubusercontent.com/130314772/232651544-05cad904-1e0f-44dd-b4c0-b1e22a8eeb61.png)

If it is green and running, then you did everything correctly – you have just launched your first Web Server in the Clouds!

NOTE: Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is default port that web browsers use to access web pages in the Internet.

Our server is running, and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).

First, let us try to check how we can access it locally in our Ubuntu shell, run:

<div id="code-container">
  <pre><code>curl http://localhost:80</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**or**

<div id="code-container">
  <pre><code>curl http://127.0.0.1:80</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture1 1](https://user-images.githubusercontent.com/130314772/232652112-14a37ac0-14bb-4276-88f7-958a2309d37e.png)

These 2 commands above actually do pretty much the same – they use ‘curl’ command to request our Nginx on port 80 (actually you can even try to not specify any port – it will work anyway). 

The difference is that: in the first case we try to access our server via DNS name and in the second one – by IP address (in this case IP address 127.0.0.1 corresponds

to DNS name ‘localhost’ and the process of converting a DNS name to IP address is called “resolution”). We will touch DNS in further lectures and projects.

As an output you can see some strangely formatted test, do not worry, we just made sure that our Nginx web service responds to ‘curl’ command with some payload.

Now it is time for us to test how our Nginx server can respond to requests from the Internet. Open a web browser of your choice and try to access following url

<div id="code-container">
  <pre><code>[curl http://localhost:80](http://<Public-IP-Address>:80)</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>
  
Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:
  
<div id="code-container">
  <pre><code>curl -s http://169.254.169.254/latest/meta-data/public-ipv4 </code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

or    

<div id="code-container">
  <pre><code>curl ifconfig.me</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture1 2](https://user-images.githubusercontent.com/130314772/232652604-fd930f75-7cad-46bb-931b-11c7bcc3f19b.png)
  
The URL in browser shall also work if you do not specify port number since all web browsers use port 80 by default.
  
If you see following page, then your web server is now correctly installed and accessible through your firewall.

![Picture1 3](https://user-images.githubusercontent.com/130314772/232652792-311d8818-6b1e-4df9-a3e6-3ed64510b77b.png)
  
In fact, it is the same content that you previously got by ‘curl’ command but represented in nice [HTML](https://en.wikipedia.org/wiki/HTML) formatting by your web browser.
