# BLOCKER 1

# Troubleshooting/Blocker on project3: 

**Ran: node index.js and got error below**

**MongoServerError: bad auth : Authentication failed**

![Picture1 1](https://user-images.githubusercontent.com/130314772/233025569-b9c12ade-7c4b-4827-bcff-84ee3a48ee88.png)

•	This error is an indication that MongoServerError wouldn’t connect because of authentication issue

•	Double check .env file to make sure the appropriate Database connection string is passed.

•	Check if user existed on your MongoDB server, especially if you have initially stopped your EC2 instance, plus the 6hrs duration has expired. Create user with password or auto generate password if not existed.

•	Grab your connection string and update your .env file.

![Picture1 2](https://user-images.githubusercontent.com/130314772/233026022-8e6ab5eb-f52d-48b2-b2b5-dc2830995ca8.png)

![Picture1 6](https://user-images.githubusercontent.com/130314772/233029971-d13579ff-271a-4eaa-9314-2d02d61bed1d.png)

To navigate to grab your connection string

![Picture1 7](https://user-images.githubusercontent.com/130314772/233031710-04c0bd26-c774-402c-a5ec-ddf1148e56f6.png)

Try and reconnect: **node index.js**

# Blocker2

**Error: listen EADDRINUSE: address already in use :::5000 after running npm run dev a command to start our development server for our application.

![Picture1 3](https://user-images.githubusercontent.com/130314772/233026644-e954c125-35eb-429c-8d84-4d8e12640e08.png)

This clearly indicates that port 5000 is already in use by another application or process.

•	Run: lsof -I tcp:5000 to know the exact process that is running on port 5000

![Picture1 4](https://user-images.githubusercontent.com/130314772/233026928-8e6a5713-c0f9-4395-9c6b-fc8169e54ffa.png)

•	Focus here is to kill the process which can be done: kill <PID> in my case PID=1678 i.e kill 1678
  
•	Try and rerun the command: **npm run dev**
  
![Picture1 5](https://user-images.githubusercontent.com/130314772/233027262-a75165f6-f64a-48a8-95fd-b16d349c8556.png)
  
**Note: a different port number can also be used.**


