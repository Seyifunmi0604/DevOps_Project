# STEP2 INSTALLING MYSQL

Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

Again, use ‘apt’ to acquire and install this software:

<div id="code-container">
  <pre><code>sudo apt install mysql-server -y</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Once the installation is finished, log in to the MySQL console by typing:

<div id="code-container">
  <pre><code>sudo mysql</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command. You should see output like below:

![Picture2 1](https://user-images.githubusercontent.com/130314772/231011090-22032f2a-1045-43fb-b48c-382ec5e8f7e6.png)

It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.1.

<div id="code-container">
  <pre><code>ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture2 2](https://user-images.githubusercontent.com/130314772/231014448-b1283767-82e6-46eb-b252-016f70ea26ca.png)

Start the interactive script by running:

<div id="code-container">
  <pre><code>sudo mysql_secure_installation</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.

Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.

Answer Y for yes, or anything else to continue without enabling.

![Picture2 3](https://user-images.githubusercontent.com/130314772/231014755-7adebd80-8869-4d12-81f2-5fb0c763f59c.png)

When you’re finished, test if you’re able to log in to the MySQL console by typing:

<div id="code-container">
  <pre><code>sudo mysql -p</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Then exit after sign in successfully.

![Picture2 4](https://user-images.githubusercontent.com/130314772/231015211-bbe1a926-9e0a-4c98-8291-e89911fab28b.png)

Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.

Notice that you need to provide a password to connect as the root user.

For increased security, it’s best to have dedicated user accounts with less expansive privileges set up for every database, especially if you plan on having multiple databases hosted on your server.





