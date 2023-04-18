## STEP 5 – TESTING PHP WITH NGINX

Your LEMP stack should now be completely set up. At this point, your LAMP stack is completely installed and fully operational.
You can test it to validate that Nginx can correctly hand .php files off to your PHP processor.

You can do this by creating a test PHP file in your document root. Open a new file called info.php within your document root in your text editor:

<div id="code-container">
  <pre><code>sudo vi /var/www/projectLEMP/info.php</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:

**<?php
phpinfo();**

You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by **/info.php:**
You will see a web page containing detailed information about your server:

You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:
You will see a web page containing detailed information about your server:

![Picture4 9](https://user-images.githubusercontent.com/130314772/232815432-98abd6c8-1596-4436-8c13-b1a90879a021.png)

After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:

<div id="code-container">
  <pre><code>sudo rm /var/www/your_domain/info.php</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

You can always regenerate this file if you need it later.
