# Nodejs-focus-Core.md

## MVC Frameworks

MVC (Model View Controller) architecture is a useful design pattern that splits application logic into three parts — models to define the shape of the data, views to organize the user interface, and controllers to communicate between the two. Node frameworks that support MVC architecture are useful if you don’t want to spend too much time worrying about how to organize your code.

- Express
- Sails

## Full-Stack Frameworks

Full-stack frameworks provide structure and functionality for your entire application – from the client to the server to the database. A full-stack framework might come with many features, including templating engines, WebSocket libraries, and ORMs. Depending on the size of your team and application, these features could be very useful or they could be more than you need.

- Meteor
```
Meteor is the second-most starred, viewed and forked Node framework on Github. The community is vibrant and the documentation is extensive. Documentation includes best practices, recommended style guides and many tutorials and technical articles. Meteor comes bundled with npm, and its own package manager called Atmosphere, as well as built-in support for a Mongo datastore and easy integration with React, Angular or Blaze. It is more opinionated than Express or Sails.

Meteor uses “data on the wire” as opposed to server-side rendering — so the server sends data, not HTML, and the client renders it. The Meteor build tool provides out of the box support for mobile through Cordova and supports hot reloading.

```

## REST API Frameworks

If you have the client-side of your application covered, you might just need a framework for the server part of your stack. In this case, you might go with a simple REST API framework just to handle CRUD requests to your server. You could pretty much do this with Express, but there are also frameworks specifically geared toward handling this particular case.


- Loopback

```
Loopback is the second most popular REST API framework, according to Github. Developed by IBM, it is a “highly extensible, open-source Node framework based on Express that allows you to quickly create APIs and microservices.” It comes with a command-line tool for generating projects and creating controllers and models with ease and provides add-on support for easy authentication and authorization. It doesn’t provide any support for views or templating or a dedicated ORM, as it is intended to be used only as an API.

Loopback allows you to create a dynamic API in minutes with minimal coding. The development cycle is very quick, and the file structure is clean and useful. Setup includes options to configure eslint, prettier, mocha and docker right out of the box.
```

