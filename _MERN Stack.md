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

---
---
---
## Restful API using Node, Express, and MongoDB

```js
npm init -y
npm i express mongoose nodemon dotenv
```

#### package.json
```
"scripts": {
    "start": "nodemon index.js",
    ...
},
```

#### index.js
```js
require("dotenv").config();

const express = require("express");
const mongoose = require("mongoose");
const mongoString = process.env.DATABASE_URL;
const routes = require("./routes/routes");

mongoose.connect(mongoString);
const database = mongoose.connection;

database.on("error", (error) => console.log(error));
database.once("connected", () => console.log("database connected"));

const app = express();
app.use(express.json());

app.use("/api", routes);

//app.get("/", (req, res) => {
//  res.status(200).json({ success: true });
//});

app.listen(3001, () => {
  console.log(`Server started at ${3001}`);
});
```

#### Configure the MongoDB Database

1. Click MongoDB Compass and download
2. Copy the connection string and open MongoDB Compass with the string
3. .env file 
```
DATABASE_URL = mongodb+srv://username:*******@cluster0.gpbdvxq.mongodb.net/test
```
4. .gitignore
```
.env
node_modules
```

#### routes/routes.js
```js
const express = require("express");
const router = express.Router();
const Model = require("../model/model");

router.post("/post", async (req, res) => {
  //res.send("Post API");
  const data = new Model({
    name: req.body.name,
    age: req.body.age,
  });

  try {
    const dataToSave = await data.save();
    res.status(200).json(dataToSave);
  } catch (error) {
    res.status(400).json({ message: error.message });
  }
});

router.get("/getAll", async (req, res) => {
  //res.send("Get All API");
  try {
    const data = await Model.find();
    res.json(data);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

router.get("/getOne/:id", async (req, res) => {
  //res.send(req.params.id);
  try {
    const data = await Model.findById(req.params.id);
    res.json(data);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

router.patch("/update/:id", async (req, res) => {
  //res.send("Update by ID API");
  try {
    const id = req.params.id;
    const updatedData = req.body;
    const options = { new: true };

    const result = await Model.findByIdAndUpdate(id, updatedData, options);
    res.send(result);
  } catch (error) {
    res.status(400).json({ message: error.message });
  }
});

router.delete("/delete/:id", async (req, res) => {
  //res.send("Delete by ID API");
  try {
    const id = req.params.id;
    const data = await Model.findByIdAndDelete(id);
    res.send(`Document with ${data.name} has been deleted.`);
  } catch (error) {
    res.status(400).json({ message: error.message });
  }
});

module.exports = router;
```

#### models/model.js
```js
const mongoose = require("mongoose");
const dataSchema = new mongoose.Schema({
  name: {
    required: true,
    type: String,
  },
  age: {
    required: true,
    type: Number,
  },
});

module.exports = mongoose.model("Data", dataSchema);
```

#### Test with Postman and check the MongoDB Compass app

Post | localhost:3000/api/post

Body
select) raw, JSON

{
    "name": "my name",
    "age": 20
}

Send

---
---
---

## HTTP Status Codes

```
* Success
200 OK
201 Created
204 No Content

* Redirection
304 Not Modified (cashing information)

* Client Error
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict

* Server Error
500 Internal Server Error

```
