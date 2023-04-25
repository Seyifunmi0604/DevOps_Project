# INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER

## Step 3: Install Express and set up routes to the server

Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express in to pass book information to and from our MongoDB database.

We also will use [Mongoose](https://mongoosejs.com/) package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.

<div id="code-container">
  <pre><code>sudo npm install express mongoose</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

In ‘Books’ folder, create a folder named apps

<div id="code-container">
  <pre><code>mkdir apps && cd apps</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Create a file named routes.js

<div id="code-container">
  <pre><code>vi routes.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Copy and paste the code below into routes.js

<div id="code-container">
  <pre><code>var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**In the ‘apps’ folder, create a folder named models**

<div id="code-container">
  <pre><code>mkdir models && cd models</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Create a file named book.js

<div id="code-container">
  <pre><code>vi book.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Copy and paste the code below into ‘book.js’

<div id="code-container">
  <pre><code>var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>


