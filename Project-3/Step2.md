## Step 2 – Frontend creation

Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.

In the same root directory as your backend code, which is the Todo directory, run:

<div id="code-container">
  <pre><code>npx create-react-app client -y</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

This will create a new folder in your Todo directory called client, where you will add all the react code.

Running a React App

Before testing the react app, there are some dependencies that need to be installed.

1.	Install concurrently. It is used to run more than one command simultaneously from the same terminal window.

<div id="code-container">
  <pre><code>npm install concurrently --save-dev</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

2.	Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

<div id="code-container">
  <pre><code>npm install nodemon --save-dev</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>
3.	In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.

<div id="code-container">
  <pre><code>"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 23](https://user-images.githubusercontent.com/130314772/232929367-52adc8b3-235e-4fa9-ae36-71ae34b1745e.png)

![Picture4 24](https://user-images.githubusercontent.com/130314772/232929498-d75f199a-040b-448c-b6a1-6f9cefceb023.png)

## Configure Proxy in package.json

1.	Change directory to ‘client’

<div id="code-container">
  <pre><code>cd client</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

2.	Open the package.json file

![Picture4 25](https://user-images.githubusercontent.com/130314772/232929716-e4f17608-faa8-4701-99d8-dd2d8ba8df4d.png)

<div id="code-container">
  <pre><code>vi package.json</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

3.	Add the key value pair in the package.json file "proxy": "http://localhost:5000".

![Picture4 26](https://user-images.githubusercontent.com/130314772/232929866-42d1e80c-e4df-484b-999d-5c54e02774f2.png)

The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos

Now, ensure you are inside the Todo directory, and simply do:

<div id="code-container">
  <pre><code>cd .. && npm run dev</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Your app should open and start running on localhost:3000

![Picture4 27](https://user-images.githubusercontent.com/130314772/232930052-51c5cef2-d5f2-4943-98db-a9f2fd372de8.png)

Important note: In order to be able to access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule. You already know how to do it.

# Creating your React Components

One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.
From your Todo directory run:

<div id="code-container">
  <pre><code>cd client</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**move to the src directory**

<div id="code-container">
  <pre><code>cd src</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Inside your src folder create another folder called components

<div id="code-container">
  <pre><code>mkdir components</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Move into the components directory with

<div id="code-container">
  <pre><code>cd components</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.

<div id="code-container">
  <pre><code>touch Input.js ListTodo.js Todo.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Open Input.js file

<div id="code-container">
  <pre><code>vi Input.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Copy and paste the following

<div id="code-container">
  <pre><code>import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

To make use of [Axios](https://github.com/axios/axios), which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Move to the src folder
<div id="code-container">
  <pre><code>cd ..</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Move to clients folder

<div id="code-container">
  <pre><code>cd ..</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Install Axios**

<div id="code-container">
  <pre><code>npm install axios</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Sip a coffee, click on the next button and let finish this up.

Go to ‘components’ directory

<div id="code-container">
  <pre><code>cd src/components</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

After that open your ListTodo.js

<div id="code-container">
  <pre><code>vi ListTodo.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

in the ListTodo.js copy and paste the following code

<div id="code-container">
  <pre><code>import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Then in your Todo.js file you write the following code

<div id="code-container">
  <pre><code>import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this.
Move to the src folder

<div id="code-container">
  <pre><code>cd ..</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Make sure that you are in the src folder and run**

<div id="code-container">
  <pre><code>vi App.js</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

<div id="code-container">
  <pre><code>import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

After pasting, exit the editor.

In the src directory open the **App.css

<div id="code-container">
  <pre><code>vi App.css</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Then paste the following code into App.css:

<div id="code-container">
  <pre><code>.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**Exit**

In the src directory open the index.css

<div id="code-container">
  <pre><code>vi index.css</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Copy and paste the code below:

<div id="code-container">
  <pre><code>body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

Go to the Todo directory

<div id="code-container">
  <pre><code>cd ../..</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

**When you are in the Todo directory run:

<div id="code-container">
  <pre><code>npm run dev</code></pre>
  <button class="btn" data-clipboard-target="#code-container"><i class="fa fa-copy"></i> Copy</button>
</div>

![Picture4 31](https://user-images.githubusercontent.com/130314772/232936461-aee78c3d-0400-438c-b391-13a2e763df30.png)

Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all your tasks.

![Picture4 28](https://user-images.githubusercontent.com/130314772/232935648-aeaecb47-2719-4e46-8c5b-4c2bcbd22cf3.png)

**Let’s add todo

![Picture4 29](https://user-images.githubusercontent.com/130314772/232935813-e65c40c9-72ef-4e1a-a81b-867f283a46ea.png)

Let’s try and delete todo #4 by clicking on it

![Picture4 30](https://user-images.githubusercontent.com/130314772/232935947-fae2f2d7-1649-4694-be52-f4740a9a8192.png)

**Congratulations**

In this Project #3 you have created a simple To-Do and deployed it to MERN stack. You wrote a frontend application using React.js that communicates with a backend application written using Expressjs. You also created a Mongodb backend for storing tasks in a database.


