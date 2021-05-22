

MEAN STACK Deployment on Linux

It is a full, [free and open-source](https://en.wikipedia.org/wiki/Free_and_open-source_software)  [JavaScript](https://en.wikipedia.org/wiki/JavaScript)  [software stack](https://en.wikipedia.org/wiki/Software_stack) for building [dynamic web sites](https://en.wikipedia.org/wiki/Dynamic_web_page) and [web applications](https://en.wikipedia.org/wiki/Web_application).Because all components of the MEAN stack support programs that are written in JavaScript, MEAN applications can be written in one language for both [server-side](https://en.wikipedia.org/wiki/Server-side) and [client-side](https://en.wikipedia.org/wiki/Client-side) execution environments.MEAN is generally used to create browser-based web applications because Angular (client-side) and Express (server-side) are both frameworks for web apps.

The MEAN stack enables a perfect harmony of JavaScript Object Notation (JSON) development: MongoDB stores data in a JSON-like format, Express and Node.js facilitate easy JSON query creation, and Angular allows the client to seamlessly send and receive JSON documents.

The components of MEAN stack includes the below:

-   MongoDB express is a schemaless NoSQL database system. It is a document-oriented NoSQL database, that stores data in a JSON-like format and allows the user to perform SQL-like queries against it.

-   Express JS is a framework used to build web applications in Node. It is a web framework for NodeJS, commonly used as a backend for web apps in the stack.

-   AngularJS is a JavaScript framework developed by Google. It is the client side web framework.

-   Node.js is a server-side JavaScript execution environment. It powers express and will be the layer where our server will run.

Steps 1:  Install Node.js

1.  Update Ubuntu

sudo apt-get update

1.  Upgrade Ubuntu

sudo apt-get upgrade

1.  Identify the location of node.js software from ubuntu repositories over the internet.

curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

1.  Install Node.js

sudo apt-get install -y node.js 

This installs both NPM and Nodejs. 

1.  Verify the node installation (nodejs)

node -v

1.  Verify the node installation (npm)

npm -v

Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.

$ sudo apt-get install -y nodejs

![](https://lh4.googleusercontent.com/kclQJ2cN1tYiBGrMeGRXnfDsdgnTURh3qIKTBtfkG66uQjwtY9t81GyhlucuU7MHmKstAVTI7C1LhqZ2IorCvr7ZUGSwjYH2Hd3dPH3qrVRzOoXw9UFw5LPWIVg4hQGNcG6Z3WY7)

  Install npm(as suggested in screenshot above)

![](https://lh4.googleusercontent.com/dgDR0fRhy81tHtCAMRnFFlvx5SoMZdJt9ohrFHV7vLiy80BMISlWmbe_3_0EPdOMNuqUCDjLWe5bYfxtEOfzAua0TUw9FqZuMPnA3VMT_n1T8K-j-4X6LhD-3TlCv-YTX2tvsqN7)

2\. Install MongoDB and update Package Manager

    MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from   

    document to document and data structure can be changed over time. For our example application,    

    we are adding book records to MongoDB that contain book name, isbn number, author, and 

    number of pages.

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

Update Ubuntu: sudo apt-get update

Install MongoDB: sudo apt-get install -y mongodb

Start the server: sudo service mongodb start

![](https://lh3.googleusercontent.com/7Igv0W4aC143HVfnrdzog81rbEfdZm97cZTPZDFoxPqxzzw76VzvDEgWdfTIVszRkYTGoHqWWjueSAHhPGgdjShFu1N-dgCzKHIz8C5mXl9q0VD9cgyt-L6Z2oww9lWfKcmi1aEI)

![](https://lh5.googleusercontent.com/4H1nRbH_qKLnEk9O1hYZ-d0J4UG8olLrN3Un7imu1uAQqa5QYouV8eNESB0-Bf8IF8pOJ6dtmVeXn3Dp1M7-RJssPc-wVxjQftzppFwHFyhbv4n1PYWQRxVMvdTIe71znzoxUmmO)

Install the body-parser package to help process the JSON passed in requests to the server.

Install the npm package manager. sudo apt-get install npm

Install Body-Parser

$ sudo npm install body-parser

![](https://lh4.googleusercontent.com/ZFUi9sSVNPUAB2tXGJfDghR63in76Wo25-tInInyJ64TkenVz1CpuXZ2ncrSm1MGLNEkx6J5mql0Zvuoc5l9AF8pvhs95i3N9oqzwqkYyhCRRcJFPfJkFpA1d-n9VUJD9XOZig27)

Error: npm WARN saveError ENOENT: no such file or directory, open '/home/bims/package.json'

Correction:The error states unable to locate Package.json folder. This is going to be  installed along with the 'npm init' command below. 

Create a folder named Books.

mkdir Books

Sudo npm init

![](https://lh3.googleusercontent.com/3BIsADzeS8kaG-TDU6fRDB3YImWygYRUZQjwHlK4hwGOIZmFZBuAQOGFXsiKH79cq7rKvBX7vXozPV5_gcyeak-WFa1yUOT2SVFu1_ukLLHT6V7Q1_nua85JsCA3iodZGKWaggff)

Install Express

Npm install express

![](https://lh6.googleusercontent.com/WF1u5rmlVnSWEShKFPZmOGSzi_cSuL1wV3OWF9nczm23M1mZojhUq5dN8L7uv9-R_XSTvjT5_DAElydcbJGDd-1WQex0fCidFciGeWXo_2i416ZJl6TVMNhShpSEjtok8sZvCBzz)

Now instal body -parser:  sudo npm install body-parser

![](https://lh5.googleusercontent.com/NdWcTXBVIE7pWVYoix1lRJ-KBD0IZh4KFGV7eE__0A106-h7-_ay4UR9WjvO50lLSsMX1-W-iMsgVFmFg3DKG-7jTw_1QWEFb96w6oAxXvAn600C6txye3ZjFfwlmqU2QvRneXo_)

Add a file named server.js: touch server.js

Copy and paste the web server code below into the server.js file.

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

Save and close - Ctrl +X; Y; Enter

![](https://lh6.googleusercontent.com/hBQ2FbL7I3Kq9yFKSSwcvuihWoJmioNsUtYfgjNAlOokQyBsMVNtwqFUHsnUkcpYb-KTyLc1txGHDTz7HtFBGKagNwMu4G_QiZpPNW7Bc2Miwx4yiYcinyW9SfHxddb2r5LVoSlp)

####  3. Install Express and set up routes to the server

Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. Express is used in this tutorial to pass book information to and from our MongoDB database. Mongoose provides a straight-forward, schema-based solution to model your application data. Mongoose is used in this project  to provide a book schema for the database.

In the Books folder, install express mongoose:  

sudo npm install express mongoose

In the Books folder, create a folder named apps

mkdir apps

Move into apps folder

cd apps

Add a file named routes.js

touch routes.js

Copy and paste the code below into routes.js

nano routes.js

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

 Save and closeL Ctrl + X; Y; Enter

![](https://lh5.googleusercontent.com/FC9hlPCdNGpdImA8EHflzK2N2yU9aJ2tT1-zxpo-kE3sCuzqzkxoE-1pxPofNqDFyNhJ1_jtYGZxoijqby-qyEbkrvh02qGelhnvMdJY1qZvQlv9fmQP31Dpi96GkT3Ubr9Lcr1U)

In the apps folder, create a folder named models

mkdir models

Add a file named book.js

touch books.js

Copy and paste the code below into book.js

Nano book.js

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

![](https://lh5.googleusercontent.com/pKKYcNd0GfkGVpRxVrjRksvpLgpf-b1acMCT7onl7MIT3jzMxEjmonRTstp6DP13aNadRs-bKf230Fa2ZNWAYoIgD79tr-8hek0QhiXZlQgOsmnhXuv0L8DTHZC_nGK8qDkj7s7G)

### 4\. Access the routes with AngularJS

[AngularJS](https://angularjs.org/) provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book database.

Change the directory back up to Books

cd ../..

Create a folder named public

mkdir public

Move into public

cd public

Add a file named script.js

touch script.js

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

![](https://lh5.googleusercontent.com/4gc6HAbjeh9iRw1GbqdoqR01ylNQkIQNVfJq1nC11GRpAMhfWor_DqyD_9Y5rY__CN3iTyg1G4IzpelubXJeJbxnx6tRZaIhcOQTJ-aYbyqs8ulUXN5UX7LIsDzJvArdVe43Bqp6)

![](https://lh4.googleusercontent.com/dEUqNw8pT6I9kwDucwohhSEqvnLGvIk9pN1D0iMlirUHzLJJ26oqM01g1wKHywZEOcQLuur0hMp8SnbaRDy3ip-EZ9X39NHPAQBPt7zjIRzw4pq7DFDVbbpqRxbzRc7Xv_8C5WI7)

In the public folder, create a file named index.html

touch index.html

Copy and paste the code below into the index.html file.

nano index.html

![](https://lh5.googleusercontent.com/jwDAufq0Z4iINLVTZ0C6k8HXWPe2YzgQ99_k4q89Syhvc2waNO_Cdj4x6Zra6gynk2OtgGcSJG_uRVufv6u1w-A3YYAgLSCukMfXW3QuujJOWY9hYYbo_R3zOyYsf8MtJUscDCZg)

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

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>

          <td>{{book.name}}</td>

          <td>{{book.isbn}}</td>

          <td>{{book.author}}</td>

          <td>{{book.pages}}</td>

        </tr>

      </table>

    </div>

  </body>

</html>

Save and close

![](https://lh4.googleusercontent.com/SxF5mEw-L4i3zyUL1EI5Jrq_gMP339UEdSGWVTfDq5FhVIJ4f8UlkKioWcUz_FsCx35HmefjQUXjWRmP1Fxr-UnkhBJa1H4upRhNVnyCakKHwJXe_Tn3zD7iyOTgiuvHwCWLEZEA)

![](https://lh4.googleusercontent.com/WQawcNnLBmHZAmXAQMoVBNr-EcnBNfeWaH349cFV5sKc87M4hdP2foCB1Xj_WSbW21pSGDmltZ8TmHXaVeub5hUoERA7u-KHuNtD4xOWPpdkJjU0DHTVKpF-ByBp4l8UfVp7JWKB)

Change the directory back up to Books

cd ..

Start the server by running this command:

node server.js

![](https://lh6.googleusercontent.com/AFn55nCi7Td9rOusQmioFTMfch0T5H7z2AaG4pTrocOhPvt5BGHwNa5nCVrQcS-N1IHpnSj2V4T_aoAOr_lH9OYC9aMbctgnJ9AnNZtY4Fbq6dud6_D8JZ8bOiAY3DyG0N5lBY3J)

Error: Cannot find Module Mongoose

Correction: Install mongoose - npm install mongoose 

![](https://lh3.googleusercontent.com/o7kJ_cQ-y9chxG5lH5PvsUelDHQAUHLMIF-R6tKmm7B6NHByPOF3PgNYnJ3_1Foo2aAmZ7ziSAXv-AvR6j7_myzduMhObGBW0N0lcxlwck2ijOSHdORwzeZmuAHYg2fVpOja7zL5)

![](https://lh6.googleusercontent.com/cr4SFg4y2WJ7KZEqWGo4QrFnmZU6s8cjMH204XTAEvZ7ycS6_YFAaZ6Z3w9MnIf-NoezA4djLOSEHfGax4wjAduIGrKNF29DLg6sw1JjiGctRQUwIMqB63FGrXYEhSbBk3Cr0Lwl)

Open a web browser to the address http://localhost:3300

![](https://lh5.googleusercontent.com/AvRhekSphLPm2tO5SDX3vH6co_Te297XUqu1dffSdU34AuXaCxj_W7IHUrPGJ4xsHXfPtqLIG1piemvcsoyx2-Sj5fWPrAwVr70o-5QC5l1JQnBxkqG5n-_QCcilCBfeMzuG-LM5)

Input any data, press add and refresh

![](https://lh3.googleusercontent.com/qQz0qB7Ldp0SlAvXRxL0mJsW6ZV4SDerJZ4EJ4hqxDNCQFyEOM93RraOlJz8ovtoBNSHIax-VdBXVPTEXuCsb7VA341i_4mxozoPFi6056YVldbaFnIPX4uGMd_g0vIyeTS5jllx)

In this project, book records have been added to MongoDB which contains the book name, isbn number, author, and number of pages.  

The AngularJS has been used to connect our web page with Express and used to perform actions on our book database.

Credits:

<https://darey.io/>

https://devcenter.heroku.com/articles/mean-apps-restful-api

https://angular-templates.io/tutorials/about/learn-how-to-build-a-mean-stack-application

https://vitux.com/how-to-uninstall-programs-from-your-ubuntu-system/
