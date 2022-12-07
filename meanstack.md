## A simple MEAN Stack Project on Ubuntu
MEAN stands for MongoDB, Express.js, AngularJS, and Node.js. MEAN is an end-to-end JavaScript stack largely used for cloud-ready applications. MEAN is an open source web stack that is mainly used to create cloud-hosted applications. 
1. **MongoDB** (Document database) – Stores and allows retrieval of data.
1. **Express** (Back-end application framework) – Makes requests to Database for Reads and Writes.
1. **Angular** (Front-end application framework) – Handles Client and Server Requests
1. **Node.js** (JavaScript runtime environment) – Accepts requests and displays results to end user

>In this tutorial we're going to implement a simple Book Register web form using MEAN stack.

 ## Preparing prerequisites
In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS, 20.04 LTS (HVM) image.

## Step 1: Install NodeJs

Update ubuntu

`$ sudo apt update`

Upgrade ubuntu

`$ sudo apt upgrade`

>In this tutorial, we will explore three different ways of installing Node.js and npm on Ubuntu 20.04:

- From the **"standard Ubuntu repositories"**. This is the easiest way to install Node.js and npm on Ubuntu and should be sufficient for most use cases. The version included in the Ubuntu repositories is 10.19.0.
- From the **"NodeSource repository."** Use this repository if you want to install a different Node.js version than the one provided in the Ubuntu repositories. Currently, NodeSource supports Node.js v14.x, v13.x, v12.x, and v10.x.
- Using **"nvm (Node Version Manager)"**. This tool allows you to have multiple Node.js versions installed on the same machine. If you are Node.js developer, then this is the preferred way of installing Node.js.
  
### Install Node.js and npm from the Ubuntu repository
At the time of writing, the Node.js version included in the Ubuntu 20.04 repositories is 10.19.0 which is the previous TLS version.

The installation is pretty straightforward. Run the following commands to update the package index and install Node.js and npm:

`$ sudo apt install nodejs npm`

The command above will install a number of packages, including the tools necessary to compile and install native addons from npm.

Once done, verify the installation by running:

`$ nodejs --version`

```
Output

v10.19.0
```
### Installing Node.js and npm from NodeSource
NodeSource is a company focused on providing enterprise-grade Node support. It maintains an APT repository containing multiple Node.js versions. Use this repository if your application requires a specific version of Node.js.

>At the time of writing, NodeSource repository provides the following versions:

- v14.x - The latest stable version.
- v13.x
- v12.x - The latest LTS version.
- v10.x - The previous LTS version.

>We’ll install Node.js version 14.x:

Run the following command as a user with sudo privileges to download and execute the NodeSource installation script:

`$ sudo curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -`

The script will add the NodeSource signing key to your system, create an apt repository file, install all necessary packages, and refresh the apt cache.

>If you need another Node.js version, for example 12.x, change the setup_14.x with setup_12.x.

Once the NodeSource repository is enabled, install Node.js and npm:

`$ sudo apt install nodejs`

>The nodejs package contains both the node and npm binaries.

Verify that the Node.js and npm were successfully installed by printing their versions:

`$ node --version`
```
Output

v14.2.0
```
`$ npm --version`
```
Output

6.14.4
```
To be able to compile native addons from npm you’ll need to install the development tools:

`$ sudo apt install build-essential`

### Installing Node.js and npm using NVM
NVM (Node Version Manager) is a bash script that allows you to manage multiple Node.js versions on a per-user basis. With NVM you can install and uninstall any Node.js version that you want to use or test.

>Visit the <https://github.com/nvm-sh/nvm#installing-and-updating> copy either the curl or wget command to download and install the nvm script:

`$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`

**Do not use sudo as it will enable nvm for the root user**

The script will clone the project’s repository from Github to the ~/.nvm directory:

> Close and reopen your terminal to start using nvm

Once the script is in your PATH, verify that nvm was properly installed by typing:

`$ nvm --version`
```
Output

0.35.3
```

To get a list of all Node.js versions that can be installed with nvm, run:

`$ nvm list-remote`

>The command will print a huge list of all available Node.js versions.

To install the latest available version of Node.js, run:

`$ nvm install node`

The output should look something like this:

![](assets/8.png)

Verify it by printing the Node.js version:

`$ node --version`
```
Output

v19.2.0
```

**Let’s install two more versions, the latest LTS version and version 10.9.0:**

`$ nvm install --lts`
`$ nvm install 10.9.0`

You can list the installed Node.js versions by typing:

`$ nvm ls`

The output should look something like this:

![](assets/9.png)

The entry with an arrow on the right (> v10.9.0) is the Node.js version used in the current shell session and the default version is set to v19.2.0. 

>Default version is the version that will be active when opening new shells.

If you want to change the currently active version enter:

`$ nvm use 12.16.3`
```
Output

Now using node v12.16.3 (npm v6.14.4)
```
To change the default Node.js version, run the following command:

`$ nvm alias default 12.16.3`

___

## Step 2: Install MongoDB
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.
```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```
Install MongoDB

`$ sudo apt install -y mongodb`

Start The server

`$ sudo service mongodb start`

Verify that the service is up and running

`$ sudo systemctl status mongodb`

You should see something like this:

![](assets/1.png)

Install body-parser package

`$sudo npm install body-parser`

>We need *‘body-parser’* package to help us process JSON files passed in requests to the server.

Create a folder named **‘Books’**

`$ mkdir Books && cd Books`

In the Books directory, Initialize npm project

`$npm init`

You should see something like this:

![](assets/2.png)

 Add a file to it named server.js

`vi server.js`

Copy and paste the web server code below into the *"server.js"* file.
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
---

## Step 3: Install Express and set up routes to the server
Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. **We will use Express to pass book information to and from our MongoDB database**.

We also will use Mongoose package which provides a straightforward, *schema-based solution* to model your application data. We will use Mongoose to establish a *schema for the database to store data of our **book register***.

`$ sudo npm install express mongoose`

In ‘Books’ folder, create a folder named apps

`mkdir apps && cd apps`

Create a file named routes.js

`vi routes.js`
 
Copy and paste the code below into *"routes.js"*
```
var Book = require('./models/book');
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
```

In the ‘apps’ folder, create a folder named models

`mkdir models && cd models`

Create a file named book.js

`vi book.js`

Copy and paste the code below into **'book.js'**
```
var mongoose = require('mongoose');
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
```
___

## Step 4 – Access the routes with AngularJS
AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial,**we use AngularJS to connect our web page with Express** and perform actions on our book register.

Change the directory back to **"Books"**

`cd ../..`

Create a folder named public

``mkdir public && cd public``

Add a file named script.js

`vi script.js`
 ```
 Copy and paste the Code below (controller configuration defined) into the script.js file.
 var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});
```

In the **public** folder, create a file named **index.html**;

`vi index.html`

Copy and paste the code below into **"index.html"** file.
```
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>
 
        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>
 
          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
```
Change the directory back up to **"Books"**
`cd ..`

Start the server by running this command:

`node server.js`

You should see something like this:

![](assets/3.png)


The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what the curl command returns locally.

`curl -s http://localhost:3300`

It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.

>For this – you need to open TCP port 3300 in your AWS Web Console for our EC2 Instance.

![](assets/4.png)

**Now we can access our Book Register web application from the Internet with a browser using a Public IP address or Public DNS name.**

This is how your Web Book Register Application will look like in browser:

![](assets/5.png)

Now let's go ahead to input some data

![](assets/6.png)

>Remember to refresh on each addition or deletion of entries to see the changes

If you observe, you'd see Mongoose is working in the terminal, reflecting, updating, fetching each actions you take on the browser, concerning deletion and addition.

![](assets/7.png)

**Congratulations! You've just set up your fully functional MEAN Stack on Ubuntu.**✨Yippee!✨






