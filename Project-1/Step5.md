With the default **DirectoryIndex** settings on Apache, a file named **index.html** will always take precedence over an **index.php** file.

This is useful for setting up maintenance pages in PHP applications, by creating a temporary **index.html** file containing an informative message to visitors. 

Because this page will take precedence over the **index.php** page, it will then become the landing page for the application. Once maintenance is over, the **index.html** is renamed or removed from the document root, bringing back the regular application page.

In case you want to change this behavior, you’ll need to edit the **/etc/apache2/mods-enabled/dir.conf** file and change the order in which the **index.php** file is listed within the **DirectoryIndex** directive:

<div id="code-container">
  <pre><code>sudo vi /etc/apache2/mods-enabled/dir.conf</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

<div id="code-container">
  <pre><code>&lt;IfModule mod_dir.c&gt;
    #Change this:
    #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
    #To this:
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
&lt;/IfModule&gt;</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Before**

![Picture5 0](https://user-images.githubusercontent.com/130314772/231033765-3e7eb92b-ce6e-4f8b-bdc9-9ad8dfe123b2.png)

**After**

![Picture5 1](https://user-images.githubusercontent.com/130314772/231033837-3afb4225-0a82-4bf6-9410-8a5faded2b84.png)

After saving and closing the file, you will need to reload Apache so the changes take effect:

<div id="code-container">
  <pre><code>sudo systemctl reload apache2</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server.

Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.

Create a new file named **index.php** inside your custom web root folder:

<div id="code-container">
  <pre><code>sudo vi /var/www/projectlamp/index.php</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

This will open a blank file. Add the following text, which is valid PHP code, inside the file:

![Picture5 3](https://user-images.githubusercontent.com/130314772/231037526-387b469f-1810-4320-93f0-a2159e018b05.png)

![Picture5 4](https://user-images.githubusercontent.com/130314772/231037737-fa7d2801-7b95-4049-9ffc-628c4cadb3b5.png)

This page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.

If you can see this page in your browser, then your PHP installation is working as expected.

After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:

<div id="code-container">
  <pre><code>sudo rm /var/www/projectlamp/index.php</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

You can always recreate this page if you need to access the information again later.

Congratulations! You have finished your very first REAL LIFE PROJECT by deploying a LAMP stack website in AWS Cloud!

![Picture5 5](https://user-images.githubusercontent.com/130314772/231038094-282571d3-1434-4c39-bad0-c4b2f3d77cfb.png)

