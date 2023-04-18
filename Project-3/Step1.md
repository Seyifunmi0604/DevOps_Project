# STEP 1 – BACKEND CONFIGURATION

# Update ubuntu

<div id="code-container">
  <pre><code>sudo apt update</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Upgrade ubuntu

<div id="code-container">
  <pre><code>sudo apt upgrade -y</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

<div id="code-container">
  <pre><code>sudo hostnamectl set-hostname MERN</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Lets get the location of Node.js software from [Ubuntu repositories](https://github.com/nodesource/distributions#deb).

<div id="code-container">
  <pre><code>curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash –</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Install Node.js on the server

**Install Node.js with the command below**

<div id="code-container">
  <pre><code>sudo apt-get install -y nodejs</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Note: The command above installs both nodejs and npm. [NPM](https://www.npmjs.com/) is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.

Verify the node installation with the command below

<div id="code-container">
  <pre><code>node -v</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Verify the node installation with the command below

<div id="code-container">
  <pre><code>npm -v</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture3 3](https://user-images.githubusercontent.com/130314772/232913474-aa1a8290-32bf-4b75-99cc-4a950388b7a0.png)

Application Code Setup

Create a new directory for your To-Do project:

<div id="code-container">
  <pre><code>mkdir Todo</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Run the command below to verify that the Todo directory is created with ls command

<div id="code-container">
  <pre><code>ls</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

TIP: In order to see some more useful information about files and directories, you can use following combination of keys ls -lih – it will show you different properties and size in human readable format. You can learn more about different useful keys for ls command with ls --help.

Now change your current directory to the newly created one:

<div id="code-container">
  <pre><code>cd Todo</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.

<div id="code-container">
  <pre><code>npm init</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture3 4](https://user-images.githubusercontent.com/130314772/232914270-efe9bd4c-7238-4176-b41b-2a9e070753c5.png)

Run the command ls to confirm that you have package.json file created.
Next, we will Install ExpressJs and create the Routes directory.


## INSTALL EXPRESSJS

# Install ExpressJS

Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.
To use express, install it using npm:

<div id="code-container">
  <pre><code>npm install express</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture3 5](https://user-images.githubusercontent.com/130314772/232914722-99447617-1afb-4c64-b519-fb30813ab79f.png)

Now create a file index.js with the command below

<div id="code-container">
  <pre><code>touch index.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Run ls to confirm that your index.js file is successfully created

<div id="code-container">
  <pre><code>ls</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture3 6](https://user-images.githubusercontent.com/130314772/232915042-0463d6ae-86ec-4032-87f6-a51f608a0727.png)

**Install the dotenv module

<div id="code-container">
  <pre><code>npm install dotenv</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Open the index.js file with the command below

<div id="code-container">
  <pre><code>vi index.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Type the code below into it and save. Do not get overwhelmed by the code you see. For now, simply paste the code into the file.

<div>
  <button id="copy-button">Copy</button>
  <pre><code>const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use((req, res, next) => {
  res.send('Welcome to Express');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});</code></pre>
</div>

Notice that we have specified to use port 5000 in the code. This will be required later when we go on the browser.

Use :w to save in vim and use :qa to exit vim

Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:

<div id="code-container">
  <pre><code>node index.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture3 7](https://user-images.githubusercontent.com/130314772/232915991-aba9934f-1139-4981-9af5-92cc76fdcce4.png)

Now we need to open this port in EC2 Security Groups. Refer to Project 1 Step 1 – Installing the Nginx Web Server. There we created an inbound rule to open TCP port 80, you need to do the same for port 5000, like this:

![Picture3 8](https://user-images.githubusercontent.com/130314772/232916191-a84b4373-dbcb-461e-9d7b-52aa9959223e.png)

**http://PublicIPorPublicDNS:5000**
  
Quick reminder how to get your server’s Public IP and public DNS name:
  
1) You can find it in your AWS web console in EC2 details
  
2) Run 
 <div id="code-container">
  <pre><code>curl -s http://169.254.169.254/latest/meta-data/public-ipv4 for Public IP address </code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
 </div>
  
  or
 
<div id="code-container">
  <pre><code> curl -s http://169.254.169.254/latest/meta-data/public-hostname for Public DNS name </code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>
  
![Picture3 9](https://user-images.githubusercontent.com/130314772/232916954-8aea49f6-035a-423b-8ed2-f1fbd9edf08d.png)
  
**Routes**
  
There are three actions that our To-Do application needs to be able to do:
  
1.	Create a new task
2.	Display list of all tasks
3.	Delete a completed task
  
Each task will be associated with some particular endpoint and will use different standard [HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods): POST, GET, DELETE.
  
For each task, we need to create [routes](https://expressjs.com/en/guide/routing.html) that will define various endpoints that the To-do app will depend on. So let us create a folder routes
 
<div id="code-container">
  <pre><code>mkdir routes</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>
  
# Tip: You can open multiple shells in Putty or Linux/Mac to connect to the same EC2
  
Change directory to routes folder.

<div id="code-container">
  <pre><code>cd routes</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>
  
Now, create a file api.js with the command below

<div id="code-container">
  <pre><code>touch api.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Open the file with the command below

<div id="code-container">
  <pre><code>vi api.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>
  
Copy below code in the file. (Do not be overwhelmed with the code)

<div>
  <button id="copy-button">Copy</button>
  <pre><code>const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

});

module.exports = router;</code></pre>
</div>
  
Moving forward let create Models directory.

## MODELS

Now comes the interesting part, since the app is going to make use of [Mongodb](https://www.mongodb.com/) which is a NoSQL database, we need to create a model.

A model is at the heart of JavaScript based applications, and it is what makes it interactive.

We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document. (Seems like a lot of information, but not to worry, everything will become clear to you over time. I promise!!!)

In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database.
These are known as virtual properties

To create a Schema and a model, install [mongoose](https://mongoosejs.com/) which is a Node.js package that makes working with mongodb easier.

**Change directory back Todo folder with cd .. and install Mongoose

<div id="code-container">
  <pre><code>npm install mongoose</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Create a new folder models:

<div id="code-container">
  <pre><code>mkdir models && cd models</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Inside the models folder, create a file and name it todo.js

<div id="code-container">
  <pre><code>touch todo.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Tip: All three commands above can be defined in one line to be executed consequently with help of && operator, like this:


<div id="code-container">
  <pre><code>mkdir models && cd models && touch todo.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Open the file created with **vi todo.js** then paste the code below in the file:

<div id="code-container">
  <pre><code>const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.

<div id="code-container">
  <pre><code>cd .. && cd routes && vi api.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>


In Routes directory, open api.js with vim api.js, delete the code inside with **:%d** command and paste there code below into it then save and exit

<div id="code-container">
  <pre><code>const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

# Brief explanation of the above code do.

This code defines an Express router that handles three routes related to a "todo" application: **GET /todos, POST /todos, and DELETE /todos/:id.**
First, the code imports the Express library, sets up a router object, and imports a Todo model. The Todo model is expected to be a MongoDB model and is used to interact with a database of todo items.
The **GET /todos** route retrieves all the todo items in the database and returns only their **_id** and **action** properties as JSON to the client. The **.find()** method is used to retrieve all the documents in the **Todo** collection in the MongoDB database.
The **POST /todos** route adds a new todo item to the database. If the **req.body.action** property is present in the request body, a new todo item is created using the **Todo.create()** method and returned as JSON to the client. Otherwise, an error message is returned.
The **DELETE /todos/:id** route deletes a todo item from the database using its _id property. The **Todo.findOneAndDelete()** method is used to find the todo item with the specified _id and delete it from the database. The deleted item is returned as JSON to the client.
Finally, the router object is exported so that it can be used in other parts of the application.
Overall, this code provides a simple REST API for managing todo items in a MongoDB database using the Express framework.

## MONGODB DATABASE
# MongoDB Database

We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution [(DBaaS)](https://en.wikipedia.org/wiki/Cloud_database), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case. [Sign up here](https://www.mongodb.com/atlas-signup-from-mlab). Follow the sign up process, select AWS as the cloud provider, and choose a region near you.

Complete a get started checklist as shown on the image below

![Picture4 1](https://user-images.githubusercontent.com/130314772/232921804-13d99bf4-7add-4736-a6e8-ee42a89d5493.png)

![Picture4 0](https://user-images.githubusercontent.com/130314772/232921930-4b367e48-e3f2-465f-beec-de5abce1d647.png)

**Create User**

![Picture4 2](https://user-images.githubusercontent.com/130314772/232922030-e04baac1-391b-4cc1-ac6c-cc646e62e6c3.png)

![Picture4 3](https://user-images.githubusercontent.com/130314772/232922158-32a981ab-2e64-4652-9495-ecc82915f4dc.png)

![Picture4 4](https://user-images.githubusercontent.com/130314772/232922266-8ca1d48e-8787-4877-aee4-20feb9cec625.png)

![Picture4 5](https://user-images.githubusercontent.com/130314772/232922380-024a1bd4-f707-4fc8-8c3f-a38b9a6377f6.png)

**Click Cluster and Create MongoDB Database**

![Picture4 6](https://user-images.githubusercontent.com/130314772/232922510-e8f7ef2f-bfb7-43c9-a178-4577c49f6466.png)

![Picture4 7](https://user-images.githubusercontent.com/130314772/232922608-5e09cce2-442a-495d-b767-31fcda1864d6.png)

![Picture4 8](https://user-images.githubusercontent.com/130314772/232922790-73d0ed97-370c-4ff6-8073-44f0b98a3ab4.png)

**Here is our Database**

![Picture4 9](https://user-images.githubusercontent.com/130314772/232922930-ad11a988-1a5a-42e5-bc31-052a81a66202.png)

Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)

# IMPORTANT NOTE

In the image below, make sure you change the time of deleting the entry from 6 Hours to 1 Week

![Picture4 10](https://user-images.githubusercontent.com/130314772/232923073-d755d2d5-87ce-41ec-a58b-8e969d54eb5a.png)

In the **index.js** file, we specified **process.env** to access environment variables, but we have not yet created this file. So we need to do that now.
Create a file in your **Todo directory** and name it **.env**.

<div id="code-container">
  <pre><code>vi .env</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Add the connection string to access the database in it, just as below:

<div id="code-container">
  <pre><code>DB = 'mongodb+srv://username:password@network-address/dbname?retryWrites=true&w=majority'
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Ensure to update <username>, <password>, <network-address> and <database> according to your setup**
  
Here is how to get your connection string

![Picture4 11](https://user-images.githubusercontent.com/130314772/232923859-faf4d39e-d8cb-4b17-9e41-69b04fcd8320.png)

![Picture4 12](https://user-images.githubusercontent.com/130314772/232923962-394457f8-a7c3-4e75-b461-9df57645cd02.png)

![Picture4 13](https://user-images.githubusercontent.com/130314772/232924075-61be2b73-cfee-4491-b1fe-298a98a810b4.png)

![Picture4 14](https://user-images.githubusercontent.com/130314772/232924160-58ddce58-0c8e-44c4-94f0-1b4853656b17.png)

**Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.**
  
Simply delete existing content in the file, and update it with the entire code below.
  
To do that using vim, follow below steps
  
1.	Open the file with vim index.js
2.	Press esc
3.	Type :
4.	Type %d
5.	Hit ‘Enter’
The entire content will be deleted, then,
  
6.	Press i to enter the insert mode in vim
7.	Now, paste the entire code below in the file.
  
<div id="code-container">
  <pre><code>const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.
  
**Start your server using the command:**

<div id="code-container">
  <pre><code>node index.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>
  
![Picture4 15](https://user-images.githubusercontent.com/130314772/232925391-ee44b250-1cdf-4e5e-ae80-bbff02955ee9.png)

# You shall see a message ‘Database connected successfully’, if so – we have our backend configured. Now we are going to test it.
  
Testing Backend Code without Frontend using RESTful API
  
So far we have written backend part of our **To-Do application**, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfull API. Therefore, we will need to make use of some API development client to test our code.
  
In this project, we will use [Postman](https://www.postman.com/) to test our API.
Click [Install Postman](https://www.postman.com/downloads/) to download and install postman on your machine.
  
Click [HERE](https://www.youtube.com/watch?v=FjgYtQK_zLE) to learn how perform [CRUD operations](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)  on Postman
  
You should test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.
  
Now open your Postman, create a POST request to the API **http://PublicIPorPublicDNS:5000/api/todos**. This request sends a new task to our To-Do list so the application could store it in the database.
  
Note: make sure your set header key **Content-Type** as **application/json**

**Create New Create**

![Picture4 16](https://user-images.githubusercontent.com/130314772/232926513-4c2ac2e8-6e22-4703-973f-813f3baa8dcd.png)

![Picture4 17](https://user-images.githubusercontent.com/130314772/232926604-361014c6-46fb-4333-a803-e77be2112cf4.png)
  
**Navigate on Body menu>> Raw Tab**

![Picture4 18](https://user-images.githubusercontent.com/130314772/232926765-e8a927a1-53f9-4637-96a9-e0c0dc37e540.png)

# POST operation task completed.

#  Let’s Create GET operation
  
Create a GET request to your API on **http://PublicIPorPublicDNS:5000/api/todos**. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request).
  
![Picture4 19](https://user-images.githubusercontent.com/130314772/232926989-e1e518f4-2e14-4d39-8179-0bb3db47aca2.png)
 
**Optional task**: Try to figure out how to send a **DELETE** request to delete a task from out To-Do list.
  
**Hint**: To delete a task – you need to send its ID as a part of DELETE request.
  
•	**Click your POST REQUEST BUTTON few time for GET REQUEST to have multiple request registered**:

 ![Picture4 20](https://user-images.githubusercontent.com/130314772/232927174-c7d6dae0-96e3-489b-936a-6463f04df57c.png)
  
 Navigate to DEL tab and paste behind our url or our header: **http://44.204.72.91:5000/api/todos/643b0e17b2509b50d70d3737** and click send, navigate to GET tab you will see is gone!

**If the status of DELETE shows like Status Ok then delete**
  
![Picture4 21](https://user-images.githubusercontent.com/130314772/232927371-de17763e-fda3-4560-82dd-0c090ec0663a.png)

**“id” “643b0e17b2509b50d70d3737” is deleted as shown below**
  
![Picture4 22](https://user-images.githubusercontent.com/130314772/232927521-34cae88e-b874-4e5f-892e-76b0cc0799de.png)

By now you have tested backend part of our To-Do application and have made sure that it supports all three operations we wanted:
•	 Display a list of tasks – HTTP GET request
•	 Add a new task to the list – HTTP POST request
•	 Delete an existing task from the list – HTTP DELETE request
  
# We have successfully created our Backend, now let go create the Frontend.

  
  
  
  
  
  
  
