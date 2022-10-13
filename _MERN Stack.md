Full Stack Mern Social Media App

client
```
copy package.json
npm install --legacy-peer-deps

```

server
```
npm i body-parser cors express mongoose nodemon


package.json
...
"main": "index.js",
"type": "module",   <--- add to able to import

"scripts": {
    "start": "nodemon index.js"
},
```

*tip: 5000 already in use 
sudo pkill node



---
---

https://github.com/bradtraversy/mern-tutorial

#### git init
entry point: server.js
license: MIT

#### .gitignore
node_modules
.env

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

#### git init
git add .
git commit -m "initial commit"

#### .env
NODE_ENV = development
PORT = 8000

#### server.js
```js
const express = require("express");
const dotenv = require("dotenv").config();
const port = process.env.PORT || 5000;

const app = express();

// for post, put (Body: add text in x-www-form-urlencoded fields)
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

app.use("/api/goals", require("./routes/goalRoutes"));

// move these into goalRoutes.js
app.get("/api/goals", (req, res) => {
  //res.send("get goals");
  res.json({ message: "Get goals" });
});

app.put("/api/goals/:id", (req, res) => {
  res.json({ message: `update ${req.params.id}` });
});

app.listen(port, () => {
  console.log(port);
});

```

#### Setup router, controller for goals, errorMiddleware
```js
goalRoutes.js

const express = require("express");
const router = express.Router();
const {
  getGoals,
  setGoal,
  updateGoal,
  deleteGoal,
} = require("../controllers/goalController");

router.route("/").get(getGoals).post(setGoal);
router.route("/:id").put(updateGoal).delete(deleteGoal);

// router.get("/", getGoals);
// router.post("/", setGoal);
// router.put("/:id", updateGoal);
// router.delete("/:id", deleteGoal);

module.exports = router;
```

#### MongoDB
1. Create a Project
2. Build a database (shared)
3. Build a Database (Cluster Name: blaCluster)

Security Quickstart
4. Create a database user - username/pw
5. Add my current IP Address
6. Finish and Close

7. Browse Collections > Add My Own Data (Database name: mern, Collection name: goals)

1. Overview > Connect
2. connect with MongoDB Compass (Url - update password, appname test>mern)
3. connect application, application code - copy and add in .env (MONGO_URI)


#### config/db.js
```js
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGO_URI);
    console.log(`MongoDB connected ${conn.connection.host}`.cyan.underline);
  } catch (error) {
    console.log(error);
    process.exit(1);
  }
};

module.exports = connectDB;
```

#### models/goalModel.js

#### add goalModel into controller

- - - - - - - - - -

#### Create userModel, controller and router for users

#### JWT (Json Web Tokens) / bcryptjs

#### authMiddleware

#### Postman Test: login > copy token >
- Header - Key: Authroization, value: Bearer + token
OR 
- Authorization - choose Bearer Token and paste

#### protect goalRoutes
Post goal with text and should add Bearer Token

- - - - - - - - - -

#### scripts
npm i -D concurrently
```
"scripts": {
    "start": "node backend/server.js",
    "server": "nodemon backend/server.js",
    "client": "npm start --prefix frontend",
    "dev": "concurrently \"npm run server\" \"npm run client\""
  },

```

#### frontend
npm install
---template redux
react-router-dom
axios
react-toastify
react-icons

* frontend package.json add proxy
```
"name": "frontend",
"version": "0.1.0",
"proxy": "http://localhost:5000",
```


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
