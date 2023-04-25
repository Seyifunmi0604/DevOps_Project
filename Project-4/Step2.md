## Step 2: Install MongoDB

MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.
mages/WebConsole.gif

<div id="code-container">
  <pre><code>sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

<div id="code-container">
  <pre><code>echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

# Install MongoDB

<div id="code-container">
  <pre><code>sudo apt install -y mongodb</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

# Start The server

<div id="code-container">
  <pre><code>sudo service mongodb start</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Verify that the service is up and running**

<div id="code-container">
  <pre><code>sudo systemctl status mongodb</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 2](https://user-images.githubusercontent.com/130314772/234302603-1827e0b2-7351-441e-abb0-55aa667c4a44.png)

**Install npm – Node package manager.**

<div id="code-container">
  <pre><code>sudo apt install -y npm</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Install [body-parser](https://www.npmjs.com/package/body-parser) package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

<div id="code-container">
  <pre><code>sudo npm install body-parser</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Create a folder named ‘Books’**

<div id="code-container">
  <pre><code>mkdir Books && cd Books</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**In the Books directory, Initialize npm project**

<div id="code-container">
  <pre><code>npm init</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 3](https://user-images.githubusercontent.com/130314772/234304310-43c7ce9d-3897-4302-870b-b59044c752de.png)

**Add a file to it named server.js**

<div id="code-container">
  <pre><code>vi server.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Copy and paste the web server code below into the server.js file.

<div id="code-container">
  <pre><code>var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>


