## Step 6 — Retrieving Data from MySQL Database with PHP

In this step you will create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.

At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.

We will create a database named example_database and a user named example_user, but you can replace these names with different values.

First, connect to the MySQL console using the root account:

<div id="code-container">
  <pre><code>sudo mysql</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

To create a new database, run the following command from your MySQL console:

<div id="code-container">
  <pre><code>CREATE DATABASE `example_database`;</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Now you can create a new user and grant him full privileges on the database you have just created.

The following command creates a new user named example_user, using mysql_native_password as default authentication method. We’re defining this user’s password as password, but you should replace this value with a secure password of your own choosing.

<div id="code-container">
  <pre><code>CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Now we need to give this user permission over the example_database database:

<div id="code-container">
  <pre><code>GRANT ALL ON example_database.* TO 'example_user'@'%';</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.

![Picture5 1](https://user-images.githubusercontent.com/130314772/232849913-6199dcdb-7296-4bf6-aa6f-5aacd92ac541.png)

Now exit the MySQL shell with:

You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

<div id="code-container">
  <pre><code>mysql -u example_user -p</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Notice the -p flag in this command, which will prompt you for the password used when creating the example_user user. After logging in to the MySQL console, confirm that you have access to the example_database database:

<div id="code-container">
  <pre><code>SHOW DATABASES;</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture5 2](https://user-images.githubusercontent.com/130314772/232852666-24139674-c425-4593-b71d-f91665a9992e.png)

<div>
  <button id="copy-button">Copy</button>
  <pre><code>CREATE TABLE example_database.todo_list (
    item_id INT AUTO_INCREMENT,
    content VARCHAR(255),
    PRIMARY KEY(item_id)
  );</code></pre>
</div>

Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:
<div>
  <button id="copy-button">Copy</button>
  <pre><code>INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
INSERT INTO example_database.todo_list (content) VALUES ("My second important item");
INSERT INTO example_database.todo_list (content) VALUES ("My third important item");
INSERT INTO example_database.todo_list (content) VALUES ("My forth important item");</code></pre>
</div>

To confirm that the data was successfully saved to your table, run:

<div id="code-container">
  <pre><code>SELECT * FROM example_database.todo_list;</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

You’ll see the following output:

![Picture5 3](https://user-images.githubusercontent.com/130314772/232854851-a94da2bf-db47-4e43-9f75-b81e02041299.png)

![Picture5 4](https://user-images.githubusercontent.com/130314772/232855071-605cde8f-a15d-4bbf-8bbd-c2cfaf0667c2.png)

After confirming that you have valid data in your test table, you can exit the MySQL console:

**Exit**

Now you can create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that:

<div id="code-container">
  <pre><code>sudo vi /var/www/projectLEMP/todo_list.php</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

The following PHP script connects to the MySQL database and queries for the content of the **todo_list table**, displays the results in a list. If there is a problem with the database connection, it will throw an exception.
Copy this content into your **todo_list.php** script:

<div>
  <button id="copy-button">Copy</button>
  <pre><code>&lt;?php
  $user = "example_user";
  $password = "password";
  $database = "example_database";
  $table = "todo_list";

  try {
    $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
    echo "&lt;h2>TODO&lt;/h2>&lt;ol>";
    foreach($db->query("SELECT content FROM $table") as $row) {
      echo "&lt;li>" . $row['content'] . "&lt;/li>";
    }
    echo "&lt;/ol>";
  } catch (PDOException $e) {
      print "Error!: " . $e->getMessage() . "&lt;br/>";
      die();
  }
</code></pre>
</div>

Save and close the file when you are done editing.

You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by **/todo_list.php:

![Picture5 5](https://user-images.githubusercontent.com/130314772/232857296-6de2da70-4063-4b07-8768-1f9d6aad24f8.png)

That means your PHP environment is ready to connect and interact with your MySQL server.

Congratulations!

In this guide, we have built a flexible foundation for serving PHP websites and applications to your visitors, using Nginx as web server and MySQL as database management system.

![Picture5 6](https://user-images.githubusercontent.com/130314772/232857895-336ad063-98d4-40ce-b197-5d777a5f0837.png)

