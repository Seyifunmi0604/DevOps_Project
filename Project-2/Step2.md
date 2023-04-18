# STEP 2 — INSTALLING MYSQL

Now that you have a web server up and running, you need to install a [Database Management System (DBMS)

(https://en.wikipedia.org/wiki/Database#Database_management_system) to be able to store and manage data for your site in a 

[relational database](https://en.wikipedia.org/wiki/Database#Database_management_system). MySQL is a popular relational database management system used within PHP 

environments, so we will use it in our project.

Again, use ‘apt’ to acquire and install this software:

<div id="code-container">
  <pre><code>sudo apt install mysql-server</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

When prompted, confirm installation by typing Y, and then ENTER.

When the installation is finished, log in to the MySQL console by typing:

<div id="code-container">
  <pre><code>sudo mysql</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command. You should see output like this:

![Picture2 2](https://user-images.githubusercontent.com/130314772/232664898-8bbd80c1-301d-4268-922e-9c55a35d9d78.png)

It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to 

your database system. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining 

this user’s password as PassWord.1.

<div id="code-container">
  <pre><code>ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture2 3](https://user-images.githubusercontent.com/130314772/232665215-af8a3832-b9f1-4c0c-bd48-dd27236483e5.png)

Exit the MySQL shell with:

Start the interactive script by running:

<div id="code-container">
  <pre><code>sudo mysql_secure_installation</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.

Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It 

is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.

Answer Y for yes, or anything else to continue without enabling.

![Picture2 4](https://user-images.githubusercontent.com/130314772/232665460-5b794b1a-7c89-4a7d-8afa-256dd026f774.png)

If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when

attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words e.g 

PassWord.1.

![Picture2 5](https://user-images.githubusercontent.com/130314772/232665691-603f6b1e-d742-4d67-bab5-2ce5c1b6d254.png)

Regardless of whether you chose to set up the **VALIDATE PASSWORD PLUGIN**, your server will next ask you to select and confirm a password for the MySQL **root user**. 

This is not to be confused with the **system root**. The **database root user** is an administrative user with full privileges over the database system. Even though the default

authentication method for the MySQL root user dispenses the use of a password, **even when one is set**, you should define a strong password here as an additional safety 

measure. We’ll talk about this in a moment.If you enabled password validation, you’ll be shown the password strength for the root password you just entered and your 

server will ask if you want to continue with 

that password. If you are happy with your current password, enter Y for “yes” at the prompt:

![Picture2 6](https://user-images.githubusercontent.com/130314772/232666241-ad73af92-3b7b-47ac-a879-e5b00532ab83.png)

For the rest of the questions, press **Y** and hit the **ENTER** key at each prompt. This will prompt you to change the root password, remove some anonymous users and the test

database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.

When you’re finished, test if you’re able to log in to the MySQL console by typing:

<div id="code-container">
  <pre><code>sudo mysql -p</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture2 7](https://user-images.githubusercontent.com/130314772/232666628-bc2dd7b1-c6a0-40d3-be42-04b5e4333f04.png)

Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.

To exit the MySQL console, type:

**mysql> exit**

Notice that you need to provide a password to connect as the root user.

For increased security, it’s best to have dedicated user accounts with less expansive privileges set up for every database, especially if you plan on having multiple 

databases hosted on your server.

Your MySQL server is now installed and secured. Next, we will install PHP, the final component in the LEMP stack.


