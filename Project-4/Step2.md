## Step 2: Install MongoDB

MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.
mages/WebConsole.gif

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```
## ADDING MONGODB REPO BELOW
```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```
# Install MongoDB
```
sudo apt install -y mongodb
```

# Start The server
```
sudo service mongodb start
```
**Verify that the service is up and running**
```
sudo systemctl status mongodb
```

![Picture4 2](https://user-images.githubusercontent.com/130314772/234302603-1827e0b2-7351-441e-abb0-55aa667c4a44.png)

**Install npm – Node package manager.**
```
sudo apt install -y npm
```

Install [body-parser](https://www.npmjs.com/package/body-parser) package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.
```
sudo npm install body-parser
```

**Create a folder named ‘Books’**
```
mkdir Books && cd Books
```
**In the Books directory, Initialize npm project**
```
npm init
```

![Picture4 3](https://user-images.githubusercontent.com/130314772/234304310-43c7ce9d-3897-4302-870b-b59044c752de.png)

**Add a file to it named server.js**
```
vi server.js
```

Copy and paste the web server code below into the server.js file.
```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});

```


