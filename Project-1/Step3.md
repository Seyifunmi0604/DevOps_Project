Apache has been installed to serve our content and MySQL installed to store and manage our data.

Apache has been installed to serve our content and MySQL installed to store and manage our data. 

PHP https://www.php.net/ is the component of our setup that will process code to display dynamic content to the end user. 

In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

To install these 3 packages at once, run:

<div id="code-container">
  <pre><code>sudo apt install php libapache2-mod-php php-mysql -y</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Once the installation is finished, you can run the following command to confirm your PHP version:

<div id="code-container">
  <pre><code>php -v</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture2 5](https://user-images.githubusercontent.com/130314772/231017807-a82ff833-eedb-403f-943f-a0b7ef79cef3.png)

At this point, your LAMP stack is completely installed and fully operational.

•	Linux (Ubuntu)

•	Apache HTTP Server

•	MySQL

•	PHP

To test our setup with a PHP script, it’s best to set up a proper Apache Virtual Host to hold your website’s files and folders. Virtual host allows you to have multiple websites located on a single machine and users of the websites will not even notice it.

![Picture2 6](https://user-images.githubusercontent.com/130314772/231022401-8d11f5a0-cfce-4cea-b495-1d9996a52394.png)

We will configure our first Virtual Host in the next step.




