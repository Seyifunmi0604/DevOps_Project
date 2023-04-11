## STEP4 CREATING A VIRTUAL HOST FOR OUR WEBSITE USING APACHE

In this project, you will set up a domain called **projectlamp**, but you can replace this with any domain of your choice.

Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the **/var/www/html directory**.

We will leave this configuration as is and will add our own directory next to the default one.

Create the directory for **projectlamp** using **‘mkdir’** command as follows:

<div id="code-container">
  <pre><code>sudo mkdir /var/www/projectlamp</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 1](https://user-images.githubusercontent.com/130314772/231023824-b7795489-28ef-4b42-ba7c-8dc18dbc857c.png)

Next, assign ownership of the directory with your current system user which is ubuntu:

<div id="code-container">
  <pre><code>sudo chown -R $USER:$USER /var/www/projectlamp</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

<div id="code-container">
  <pre><code>ll /var/www/projectlamp</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 2](https://user-images.githubusercontent.com/130314772/231024149-38ee5987-2248-47dc-9087-546bf64ff787.png)

Then, create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. Here, we’ll be using vi or vim (They are the same by the way):

<div id="code-container">
  <pre><code>sudo vi /etc/apache2/sites-available/projectlamp.conf</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

<div id="apache-config-container">
  <pre><code>
    &lt;VirtualHost *:80&gt;
      ServerName projectlamp
      ServerAlias www.projectlamp 
      ServerAdmin webmaster@localhost
      DocumentRoot /var/www/projectlamp
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
    &lt;/VirtualHost&gt;
  </code></pre>
  <button class="btn" data-clipboard-target="#apache-config-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 9](https://user-images.githubusercontent.com/130314772/231027396-1cff2f87-5da1-40fd-8905-f014b10b74bf.png)

Confirm the content in /etc/apache2/sites-available/projectlamp.conf:

<div id="code-container">
  <pre><code>cat /etc/apache2/sites-available/projectlamp.conf</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 8](https://user-images.githubusercontent.com/130314772/231027827-bd855dd6-c424-419a-a317-6f1c9a9ce2a8.png)

You can use the ls command to show the new file in the sites-available directory

<div id="code-container">
  <pre><code>sudo ls /etc/apache2/sites-available</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

You will see something like this; **000-default.conf  default-ssl.conf  projectlamp.conf**

![Picture4 7](https://user-images.githubusercontent.com/130314772/231028228-0a422f39-bf93-406d-8b91-fd66d439551a.png)

With this VirtualHost configuration, we’re telling Apache to serve **projectlamp** using **/var/www/projectlamp** as its web root directory. If you would like to test Apache without a domain name, you can remove or comment out the options ServerName and ServerAlias by adding a # character in the beginning of each option’s lines. 

Adding the # character there will tell the program to skip processing the instructions on those lines.

You can now use a2ensite command to enable the new virtual host:

<div id="code-container">
  <pre><code>sudo a2ensite projectlamp</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

You might want to disable the default website that comes installed with Apache. This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. To disable Apache’s default website use a2dissite command , type:

<div id="code-container">
  <pre><code>sudo a2dissite 000-default</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

To make sure your configuration file doesn’t contain syntax errors, run:

<div id="code-container">
  <pre><code>sudo apache2ctl configtest</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Finally, reload Apache so these changes take effect:

<div id="code-container">
  <pre><code>sudo systemctl reload apache2</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 6](https://user-images.githubusercontent.com/130314772/231029382-ec5e6256-7448-4df1-80cd-fc9f9769c81c.png)

Your new website is now active, but the web root **/var/www/projectlamp** is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:

<div id="code-container">
  <pre><code>sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 5](https://user-images.githubusercontent.com/130314772/231029854-b8b865a1-a8b3-4c65-b6b8-97ab76a670b7.png)

Now go to your browser and try to open your website URL using IP address:

<div id="code-container">
  <pre><code>http://Public-IP-Address:80</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 4](https://user-images.githubusercontent.com/130314772/231031327-0e41a7ad-5eb5-40ca-8128-89148451581f.png)

If you see the text from ‘echo’ command you wrote to index.html file, then it means your Apache virtual host is working as expected.

In the output you will see your server’s public hostname (DNS name) and public IP address. You can also access your website in your browser by public DNS name, not only by IP – try it out, the result must be the same (port is optional)

<div id="code-container">
  <pre><code>http://Public-DNS-Names:80</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 3](https://user-images.githubusercontent.com/130314772/231031825-0d9068ce-4ee8-4f5e-b4b3-6b2571881328.png)

![Picture4 0](https://user-images.githubusercontent.com/130314772/231031989-02859f64-0b57-4312-abd5-aeb4657d7d68.png)

You can leave this file in place as a temporary landing page for your application until you set up an **index.php** file to replace it. 

Once you do that, remember to remove or rename the **index.html** file from your document root, as it would take precedence over an **index.php** file by default.



