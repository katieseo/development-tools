#### dependencies:
```js
npm i express dotenv mongoose colors
npm i -D nodemon
npm i express-async-handler

npm i bcryptjs
npm i jsonwebtoken
```

#### packages.json:
```js
"scripts": {
  "start": "node backend/server.js",
  "server": "nodemon backend/server.js"
}
```
#### Setup controller and router for skills

#### MongoDB
1. Create a Project
2. Build a database (shared)
3. Cluster Name: khsCluster
4. Create a database user - username/pw
5. Add my current IP Address
6. Finish and Close
7. Collection > Add My Own Data (Database name: mernapp, Collection name: skills)

1. Overview > Connect
2. connect with MongoDB Compass (Url - update password, appname test>mernapp)
3. connect application, application code - copy and add in .env (MONGO_URI)

#### Setup controller and router for users

#### JWT (Json Web Tokens) / bcryptjs

#### Postman Test: login > copy token > 
  - Header - Key: Authroization, value: paste
  OR - Authorization - choose Bearer Token and paste
  
  Post skill with text, Bearer Token added



