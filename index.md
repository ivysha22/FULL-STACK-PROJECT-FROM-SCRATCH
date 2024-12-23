<!-- HERE IS WHAT IS REQUIRED IN ORDER TO START A FULL STACK PROJECT FROM SCRATCH -->

                               GUIDE

# 1. Define the project's requirements and goals

# 2. Choose a programming language and framework

FRAMEWORKS I USE FOR NOW
a.express.js framework for node js (backend)

TEMPLATING ENGINE
b.i use embeded javascript in short ejs to write js from the server

(EJS is often used as a view engine in Express.js to render dynamic HTML pages)

# 3. Set up the project structure and dependencies

c.HERE IS A LIST OF DEPENDENCIES YOU WILL USE OR INSTALL
npm install------

<!-- Core Backend Dependencies -->

0.dotenv
1.express
2.mysql

<!-- Middleware -->

3.body-parser
4.multer

<!-- Authentication and Security -->

5.bcryptjs

<!-- View Engine -->

6.EJS

<!-- Utilities -->

7.express-session

<!-- restart the server -->

8.node

# 4. Create the database schema and implement data modeling

# 5. Implement the backend API using the chosen framework

# 6. Implement the frontend using a suitable library or framework

# 7. Integrate the frontend and backend

# 8. Test and debug the application

# 9. Deploy the application to a production environment


                                  ARRANGEMENT ON THE SERVER

<!-- declarations  -->

//connect to express js
const express = require("express");
const session = require("express-session");
const server = express();
const mysql = require("mysql");
const bodyParser = require("body-parser");
const bcrypt = require("bcryptjs"); // bcrypt declaration after we npm install bcrypt
const salt = bcrypt.genSaltSync(3);
const { Agent } = require("http");
const multer = require("multer");
const upload = multer({ dest: "public/listingimages" });
const profile = multer({ dest: "public/profilepic" });
require("dotenv").config();

<!-- setting up the database -->

// Import MySQL
const connection = mysql.createConnection({
host: "localhost",
user: "root",
password: "",
database: "househunt",
});

<!-- session management -->

//express-session
server.use(
session({
secret: "ivy",
resave: false,
saveUninitialized: false,
})
);

<!-- middleware -->

// Session middleware
server.use((req, res, next) => {
res.locals.user = req.session.user;
res.locals.isLoggedIn = req.session.isLoggedIn;
if (req.path == "/account" && req.session.role == "owner") {
res.locals.modify = true;
}

if (
!req.session.isLoggedIn &&
[
"/account",
"/profile",
"/update-profile",
"/addnewproperty",
"/myproperties",
].includes(req.path)
) {
res.render("401.ejs");
} else {
if (
req.session.role !== "admin" &&
["/adminsonly", "/agent", "/owner"].includes(req.path)
) {
res.render("401.ejs");
} else {
next();
}
}
});

<!-- parsers -->

// Parse incoming JSON requests
server.use(bodyParser.json());

// Set up EJS,middleware and public folder
server.set("view engine", "ejs");
server.use(express.static("PUBLIC"));
server.use(express.urlencoded({ extended: true }));

<!-- routes -->
