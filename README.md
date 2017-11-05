![CF](https://camo.githubusercontent.com/70edab54bba80edb7493cad3135e9606781cbb6b/687474703a2f2f692e696d6775722e636f6d2f377635415363382e706e67) 16: Basic Auth
===

## Notes & Instructions On How My Lab Works


RUNNING IN DIFFERENT TERMINALS:
//serve up mongodb in another terminal
mkdir db
mongod --dbpath=./db
//start server in one terminal
nodemon server.js  
//run the Tests in another terminal
npm test
//to open the mongo console in a different terminal
mongo
//to create a user or to sign up, run this command in another terminal
_$ echo '{"username": "PeanutButter", "password": "yummyyum", "email":"peanut@restaurants.com"}' | http post localhost:3000/signup_
//to signin a user, run this command in the terminal
http :3000/signin -a PeanutButter:yummyyum

#REFERENCES
1. https://www.flickr.com/photos/girliemac/sets/72157628409467125/
2. google mongo Shell Quick Reference for useful commands to run in your mongo console and to verify that you have data going in and to see where things are going wrong
3. https://github.com/auth0/node-jsonwebtoken

1. make a db folder for each of your project: _$mkdir db_
2. make sure db is in your gitignore, so it's not commited up to github
3. compared to SQL, there is very little setup in Mongodb
4. to run the mongo server or mongo daemon type in the following mongo command into the console
_$mongod --dbpath=./db_
5. after running the --dbpath=./db, you will see a message at the bottom "waiting for connections on port 27017"
6. to open a mongo console, type in _mongo_ in another terminal
7. _show dbs_
8. _use dbs_ //should see 'switched to db dbs'
9. _db.users.find({}).pretty()_
10. to start my server, run this command in another terminal: $ nodemon server.js
11. to create a user, open another terminal and type in this example user:
_$ echo '{"username": "PeanutButter", "password": "yummyyum", "email":"peanut@restaurants.com"}' | http post localhost:3000/signup_

////////SIGNING UP FOR A USERNAME AND PASSWORD
////////////IF USING .then(res.send.bind(res))  in auth-routes.js

`authRouter.post('/signup', jsonParser, (req, res, next) => {
  const password = req.body.password;
  delete req.body.password;
  (new User(req.body)).generateHash(password)
    .then((user) => {
      user.save()
        .then(res.send.bind(res))
        //.then(user => res.send(user.generateToken()))
        .catch(next);
    })
    .catch(next);
});`

//////THEN should see this result in the terminal

`[hanhthaoluu@MacBook-Pro]~/401/labs/lab-16-basic-auth[lab-thao !]:$ echo '{"username": "PeanutButter", "password": "yummyyum", "email":"peanut@restaurants.com"}' | http post localhost:3000/signup
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 175
Content-Type: application/json; charset=utf-8
Date: Sat, 04 Nov 2017 05:49:08 GMT
ETag: W/"af-jy/gPO47Gjfjs5Z39UomFLBKsv8"
X-Powered-By: Express

{
    "__v": 0,
    "_id": "59fd54d48ec1125d33f983dd",
    "email": "peanut@restaurants.com",
    "password": "$2a$10$foaOrFNsYUnO9efwpszfxO.bOknpLyF3f7ZAp5PY80YievGaB6uFu",
    "username": "PeanutButter"
}`

////////////IF USING .then(user => res.send(user.generateToken())) in auth-routes.js

`authRouter.post('/signup', jsonParser, (req, res, next) => {
  const password = req.body.password;
  delete req.body.password;
  (new User(req.body)).generateHash(password)
    .then((user) => {
      user.save()
        //.then(res.send.bind(res))
        .then(user => res.send(user.generateToken()))
        .catch(next);
    })
    .catch(next);
});`

//////THEN should see this result in the terminal
/////token generated below is
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjU5ZmQ1MDYyNWQ5OWY2NTllMThlM2FkNyIsImlhdCI6MTUwOTc3MzQxMH0.EMKfoN4_8YVY1iE-Xmq4BEenImSPvKKSN_Qh-9saxVE

`[hanhthaoluu@MacBook-Pro]~/401/labs/lab-16-basic-auth[lab-thao !]:$ echo '{"username": "Bamboo Garden", "password": "Vegetarian", "email":"restaurant@restaurants.com"}' | http :3000/signup
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 148
Content-Type: text/html; charset=utf-8
Date: Sat, 04 Nov 2017 05:30:10 GMT
ETag: W/"94-0E4/pxSCevvoxIRr/8wbarssAis"
X-Powered-By: Express

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjU5ZmQ1MDYyNWQ5OWY2NTllMThlM2FkNyIsImlhdCI6MTUwOTc3MzQxMH0.EMKfoN4_8YVY1iE-Xmq4BEenImSPvKKSN_Qh-9saxVE`

//OPEN MONGO CONSOLE IN ANOTHER TERMINAL
`[hanhthaoluu@MacBook-Pro]~/401/labs/lab-16-basic-auth[lab-thao !]:$ mongo
MongoDB shell version v3.4.9
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.9
Server has startup warnings:
2017-11-03T22:34:19.166-0700 I CONTROL  [initandlisten]
2017-11-03T22:34:19.166-0700 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2017-11-03T22:34:19.166-0700 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2017-11-03T22:34:19.166-0700 I CONTROL  [initandlisten]
> show dbs
admin     0.000GB
auth_dev  0.000GB
local     0.000GB
> use auth_dev
switched to db auth_dev
> show collections
users
> db.users.find()
{ "_id" : ObjectId("59fd50625d99f659e18e3ad7"), "password" : "$2a$10$qOTKnjPQy./8cOA2hq7oSuhXIIBIMKtnn92nhJOthDNzHYevBXbOG", "username" : "Bamboo Garden", "email" : "restaurant@restaurants.com", "__v" : 0 }
{ "_id" : ObjectId("59fd54d48ec1125d33f983dd"), "password" : "$2a$10$foaOrFNsYUnO9efwpszfxO.bOknpLyF3f7ZAp5PY80YievGaB6uFu", "username" : "PeanutButter", "email" : "peanut@restaurants.com", "__v" : 0 }
> db.users.find().pretty()
{
  "_id" : ObjectId("59fd50625d99f659e18e3ad7"),
  "password" : "$2a$10$qOTKnjPQy./8cOA2hq7oSuhXIIBIMKtnn92nhJOthDNzHYevBXbOG",
  "username" : "Bamboo Garden",
  "email" : "restaurant@restaurants.com",
  "__v" : 0
}
{
  "_id" : ObjectId("59fd54d48ec1125d33f983dd"),
  "password" : "$2a$10$foaOrFNsYUnO9efwpszfxO.bOknpLyF3f7ZAp5PY80YievGaB6uFu",
  "username" : "PeanutButter",
  "email" : "peanut@restaurants.com",
  "__v" : 0
}
>`

TO SIGIN TYPE THE FOLLOWING COMMAND INTO THE TERMINAL
http :3000/signin -a PeanutButter:yummyyum
//-a is for basic authentication
//make a GET request to the sign-in page, do the basic auth with the username and password
/////should see this
`[hanhthaoluu@MacBook-Pro]~/401/labs/lab-16-basic-auth[lab-thao !]:$ http :3000/signin -a PeanutButter:yummyyum
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 148
Content-Type: text/html; charset=utf-8
Date: Sat, 04 Nov 2017 08:19:40 GMT
ETag: W/"94-kaSvn6cWfvcxm8Wgow9nQXFVB18"
X-Powered-By: Express

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjU5ZmQ1NGQ0OGVjMTEyNWQzM2Y5ODNkZCIsImlhdCI6MTUwOTc4MzU4MH0.dbue2wjCxTySvgBZQv_3oeoh9QZj-VCRyUAAbfRZTcI`




AUTHENTICATION: the methods that determine whether you are who you say you are. You have something you know
AUTHORIZATION: process that determine whether are you allowed to access what you are trying to access

_WHEN YOU ARE BUILDING APIs, NEVER STORE PASSWORDS, CREDIT CARDS INFO IN PLAIN TEXT ANYWHERE IN YOUR DATABASE_
Confidential info need to be sent via HTTPs.  HTTPs creates an encriptioned tunnel between your server and someone who is using your server.  Don't use proprietary encryption algorithm. Only use open source encryption algorithm, so you can see actually how things work.

HASHED ALGORITHM vs. ENCRYPTION ALGORITHM
1. Hashed is a one way algorithm. With hashing, you can never get the plain text back.  In order to get the plain text you have to know the plain text.  With hash, you don't need to know the password in order to compare the hashed password in our database. If you run the same password through the same algorithm, you should get the same result in hash. When a user signs in again with his or her password, we rehash that password and compare it to the hashed password that we store in our database.  Hashing uses the original plain text as the encryption key, rather than having separate secret keys.

How does hashing work? Bcrypt uses blowfish cipher, which is an encryption algorithm. blowfish cipher is used for encryption. Cipher itself is an encryption algorithm. We are using it in this lab as a hashing algorithm. It takes characters from random bytes from the plain text password coming in and uses that as an encryption key. Normally, we have secret and a plain text. With the hashing algorithm, the secret and the plain text comes from the same source material

Salting is the way to insert semi random data into our hashes.

2. With encryption algorithm, if you know the secret then you can retrieve the plain text back


THE PROCESS TO CREATE A USER
1. Create a model with mongoose
2. Once we have a server running, we send a user to our server via HTTP POST and save json
3. Take the json password and run it through a hash algorithm, hashing the password. Then save the hashed password to the database
4. Make sure the password is not in memory. The password at some point is a form of a buffer and we will need to zero out that buffer. Because buffer is allocated in random memory, so we want to make sure to override this memory. This way, no plain text password is floating somewhere in our server memory.
5. we send back a json web token JWT. JWT is a json data that contains the hashed algorithm. It uses the secret that we have on our server.  The secret is usually the long randomly generated string of characters.  JWT is used to make sure that the user is authenticated in our system. MAKE SURE THAT JWT CAME FROM OUR SERVER. There is no client side in this lab, no cookies to store data going back and forth from the client. So in this lab, it is JWT to authenticate the user for other routes. This way the client don't have to provide username and password in plain text everytime the client send data to our server


THE PROCESS TO SIGN IN A USER
1. In the basic HTTP POST, we send username and password to our server

//remember this does not look like plain text, but it is plain text
  1.headers: {
    'Authorization': 'Basic ' + Base64(username:password)
    //basic http of authorization
//the string 'Basic ' is to indicate that we are using basic http version of authorization
  }

  when the request for login comes in to our server in the basic http format(GET request to sign in, POST is to create), we parse the basic HTTP data to give us the username and pw; find the corresponding user in db; we then use bcrypt to hash incoming pw  and then compare the resulting hashed pw to the hashed pw in db that we have for that user.  Then send back JWT

SUMMARY OF MOVING PIECES
1. for our user model we pull in the module bcrypt to hash password
2. basic HTTP processing middleware
3. generate a JWT
3. verify a particular JWT comes from our server

IN THE CASE OF 'FORGETTING YOUR PASSWORD' AND A NEW PW NEEDS TO BE REGENERATED: with the new password, it should
generate a random hash for the URL, this random hash in the URL should correspond to an entry in the database; hash the new password and save it in place of the old password. You should never recover your old password from any websites.

TO INSTALL DEPENDENCIES PACKAGES
npm init --y && npm i --save bcrypt bluebird body-parser express jsonwebtoken mongoose
https://github.com/auth0/node-jsonwebtoken
_With encryption, make sure that you are always on the latest versions.  So change the versions for bcrypt and jsonwebtoken in package.json to the "lastest" versions_

//server.js//

`'use strict';

const mongoose = require('mongoose');
mongoose.Promise = require('bluebird');
mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost:27017/auth_dev', {useMongoClient: true});

const app = require('express')();

//use the sign-up and sign-in routes
app.use(require(__dirname + '/routes/auth-routes'));

//this is an error middleware; this goes at the end when all routes have been defined, so that it is not being used in every single route
app.use((err, req, res, next) => {
  console.log(err);
  res.status(500 || err.statusCode).send(err.message || 'server error');
});

app.listen(process.env.PORT || 3000);`


///  routes/auth-routes.js  ///

`'use strict';

//pull in the user object to hash our password and store in our db
const User = require(__dirname + '/../models/user');
const basicHTTP = require(__dirname + '/../lib/basic-http');
const jsonParser = require('body-parser').json();

const authRouter = module.exports = require('express').Router();


/////the middleware is (req, res, next)
authRouter.post('/signup', jsonParser, (req, res, next) => {
  //////our req.body has the username and password; we don't want to save our password into the database because the password is in the plain text format; so we are going to delete the password property from request.body
  const password = req.body.password;
  delete req.body.password;
  ////////.generateHash function returns the new user object from the first .then; so we are passing in the user object inside the .then
  ///////.generateHash takes the plain text password and set it on the user object as this.password
  (new User(req.body)).generateHash(password)
    .then((user) => {
      /////user.save() generates a promise
      user.save()
        //////sending back the user object with the hashed password that we got back from the database; sending the user back to whoever's making the response
        .then(user => res.send(user.generateToken()))

        //////.catch(next) will be called with the error that catch catches
        .catch(next);
    })
    .catch(next);
});

authRouter.get('/signin', basicHTTP, (req, res, next) => {
  User.findOne({username: req.auth.username})
    .then(user => {
      if(!user) next({statusCode: 403, message: 'Forbidden'});
      //comaparing the password with the req.auth.password that came in
      user.comparePassword(req.auth.password)
      //sending back just the response with the user that we got from the database
      .then(user => res.send(user.generateToken()))
      .catch(err => next({statusCode: 403, message: 'Unsuccessful Authentication'}));
    }).catch(next);
});`




///  models/user.js  ///
//creating methods/functions on our mongoose model/ user model; adding additional methods that every instance of user will have access to: generateHash, compareHash; generateHash comes from bcrypt

www.npmjs.com/package/bcrypt
//this is async version of hash;
//saltRounds = rounds of salts, adding semi-random data; bcrypt algorithm knows how to figure out what are the salts; it just looks random to an outside observer; bcrypt knows how to extrapolate the information it needs with the salts/random data in it.
_Technique 2 (auto-gen a salt and hash)_

bcrypt.hash(myPlaintextPassword, saltRounds, function(err, hash) {
  //store hash in your password DB
});

//the hash password will be a byte string, looks like a string of random characters;
//our goal here is to generate a user's hash that is different from another user's hash; if someone hashes my password through the bcrypt cipher, their version of the hash will look different than my version of the hash; if two users have the same password then each version of the hash will look different for each user because of salting.

`'use strict';

const mongoose = require('mongoose');
//jwt is not async so we don't need to promisify jwt
const jwt = require('jsonwebtoken');

//requiring in bluebird without saving it into a constant; then call the .promisifyAll of bluebird and pass into it a require of bcrypt; so this bcrypt is the promisified version of bcrypt!!!!! which creates for every method on the bcrypt object;

const bcrypt =  require('bluebird').promisifyAll(require('bcrypt'));

const userSchema = new mongoose.Schema({
  username: {type: String, required: true, unique: true},
  password: {type: String, required: true},
  email: {type: String, required: true, unique: true}
});


///////as we increase the number of saltRounds, it takes an exponentially longer amount of time to run the process; 10 saltRounds is the moderate amount of saltRounds

///////no arrow functions because we are creating methods/adding methods onto userSchema.methods; .methods is similar to .prototype; similar to adding methods to the prototype of a constructor function

userSchema.methods.generateHash = function(password) {


  ////////.hashAsync returns a promise, takes the same parameters and returns a promise instead of having a callback function; we are calling hashAsync and passing in the password and number of saltRounds; passing in the password; then hashing password with 10 saltRounds; then setting the hashed password on that instance of the user;
  return bcrypt.hashAsync(password, 10)

    /////////we use an arrow function here so we can set this.password = hash; so when we call .then on the promise that we get from generateHash, the first .then that we call will have the entire user object so we can send back to the database or send something back to the user or use the ID to generate a JWT...
    /////.then is going to be called with the resulting hashed password
    .then((hash) => {

      //because we are using the arrow function here, the this. is the same as the this. for generateHash function, which will be the same as whatever instance of the user we currently have; this. is referring to the instance of a user that we are currently working with

      this.password = hash;
      /////returning the instance of the user; so when we call the next we will have the full user object
      return this;

      ///////there is no .catch here because we want to call then(next); THERE IS NO MIDDLWARE FUNCTION INSIDE OF OUR MODEL BECAUSE WE ARE NOT INSIDE OF A ROW
    });
};


<!-- //   https://github.com/kelektiv/node.bcrypt.js

with promises

bcrypt uses whatever Promise implementation is available in global.Promise. NodeJS >= 0.12 has a native Promise implementation built in. However, this should work in any Promises/A+ compliant implementation.

Async methods that accept a callback, return a Promise when callback is not specified if Promise support is available.

bcrypt.hash(myPlaintextPassword, saltRounds).then(function(hash) {
  // Store hash in your password DB.
});
// Load hash from your password DB.
bcrypt.compare(myPlaintextPassword, hash).then(function(res) {
  // res == true
});
bcrypt.compare(someOtherPlaintextPassword, hash).then(function(res) {
  // res == false
}); -->

userSchema.methods.comparePassword = function(password) {
  //comparing the plain text password that has come in with this.password, which is the hashed password that was saved in the database
  //compareAsync only takes in plain text password for comparison
  return bcrypt.compareAsync(password, this.password)
    .then(res => {
      //if res is a truthy value then return the user
      if(res) return this;
      //otherwise throw the new error
      throw new Error('Password did not match');
    });
};

userSchema.methods.generateToken = function() {
  //the application secret is the process.env.SECRET
  //the process.env.SECRET is being used to generate the token
  //in case process.env.SECRET is not setup then use 'changethis'
  //sending jwt to the user; jwt contains the digital signature from the server; jwt allows the user what types of info the user can access
  return jwt.sign({id: this._id}, process.env.SECRET || 'changethis');
};

module.exports = mongoose.model('User', userSchema);`


// lib/basic-http.js //

//authentication data comes from the authorization header, this reflects how we historically used http

`'use strict';

module.exports = (req, res, next) => {
  try {
    let authHeader = req.headers.authorization;
    ////split creates an array where every element is separated by 'Basic '; then we want to grab the second element, which is base64 string
    let base64Header = authHeader.split('Basic ')[1];
    //put this into a buffer because we want to convert from base64 into utf8
    //'base64' to inform node
    let base64Buf = new Buffer(base64Header, 'base64');
    //toString will give us the utf8 version of username:password
    let stringHeader = base64Buf.toString();
    let authArray = stringHeader.split(':');
    let authObject = {
      username: authArray[0],
      password: authArray[1]
    };
    if (!authObject.username || !authObject.password) throw new Error('Unsuccessful Authentication');
    /////in the middleware, when we are changing data/processing data, if you want that change/update to be accessible in the next piece of middleware then you have to set it on the request object. So that it will be the same request object as it goes through each one of the middleware chunks

    req.auth = authObject;

    next();
  } catch(e) {
    next(e);
  }
};
`
//base64 gets rid off all white spaces.
//try this in the console
(new Buffer('hello world')).toString('base64');


_USE AUTHENTICAT IN PROJECTS_

## Submission Instructions
  * fork this repository & create a new branch for your work
  * write all of your code in a directory named `lab-` + `<your name>` **e.g.** `lab-susan`
  * push to your repository
  * submit a pull request to this repository
  * submit a link to your PR in canvas
  * write a question and observation on canvas

## Learning Objectives  
* students will be able to create basic authorization middleware
* students will be able to test basic authorization for signup/signin routes

## Requirements
#### Configuration
* `package.json`
* `.eslintrc`
* `.gitignore`
* `.env`
* `README.md`

## Feature Tasks
* create the following directories to organize your code:
  * **lib**
  * **model**
  * **route**
  * **test**
* create an HTTP server using `express`
* using `mongoose`, create a **User** model with the following properties and options:
  * `username` - *required and unique*
  * `email` - *required and unique*
  * `password` - *required - this must be hashed and can not stored as plain text*
  * `findHash` - *unique*
* use the **npm** `debug` module to log function calls that are used within your application
* use the **express** `Router` to create a custom router for allowing users to **sign up** and **sign in**
* use the **npm** `dotenv` module to house the following environment variables:
  * `PORT`
  * `MONGODB_URI`
  * `APP_SECRET` *(used for signing and verify tokens)*

## Server Endpoints
### `/api/signup`
* `POST` request
* the client should pass the username and password in the body of the request
* the server should respond with a token (generated using `jwt` and `findHash`
* the server should respond with **400 Bad Request** to a failed request

### `/api/signin`
* `GET` request
* the client should pass the username and password to the server using a `Basic:` authorization header
* the server should respond with a token for authenticated users
* the server should respond with **401 Unauthorized** for non-authenticated users

## Tests
* create a test that will ensure that your API returns a status code of **404** for any routes that have not been registered
* `/api/signup`
* `POST` - test **400**, if no request body has been provided or the body is invalid
* `POST` - test **200**, if the request body has been provided and is valid
* `/api/signin`
* `GET` - test **401**, if the user could not be authenticated
* `GET` - test **200**, responds with token for a request with a valid basic authorization header
