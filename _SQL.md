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

---
---
---

## Node Rest API using Express and SQLite

```js
npm init -y
```

```
File structure

db/
    cars.js
    knex.js
cwc.sqlite3
package.json
server.js
```

#### package.json
```js
"scripts": {
    "start": "node server.js"
    ...
},
"dependencies": {
    "body-parser": "^1.20.0",
    "express": "^4.18.1",
    "knex": "^2.3.0",
    "sqlite3": "^5.0.11"
}
```

#### cwc.sqlite3 (DB Browser for SQLite)
```
DB Browser for SQLite > Open Database > select cwc.sqlite3 > set up tables
id | INTEGER | check PK(primary key) & AI(auto increment)
make | TEXT
model | TEXT
year | INTEGER

and SAVE
```

## db/knex.js
```js
const knex = require("knex");

const connectedKnex = knex({
  client: "sqlite3",
  connection: {
    filename: "cwc.sqlite3",
  },
});

module.exports = connectedKnex;
```

#### db/cars.js
```js
const knex = require("./knex");

function createCar(car) {
  return knex("cars").insert(car);
}

function getAllCars() {
  return knex("cars").select("*");
}

function deleteCar(id) {
  return knex("car").where("id", id).del();
}

function updateCar(id, car) {
  return knex("cars").where("id", id).update(car);
}

module.exports = {
  createCar,
  getAllCars,
  deleteCar,
  updateCar,
};
```

#### server.js
```js
const express = require("express");
const app = express();
const bodyParser = require("body-parser");
const db = require("./db/cars");

app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

app.post("/cars", async (req, res) => {
  const results = await db.createCar(req.body);
  res.status(201).json({ id: results[0] });
});

app.get("/cars", async (req, res) => {
  const cars = await db.getAllCars();
  res.status(200).json({ cars });
});

app.get("/cars/:id", async (req, res) => {
  const id = await db.updateCar(req.param.id, req.body);
  res.status(200).json({ id });
});

app.delete("/cars/:id", async (req, res) => {
  await db.deleteCar(req.params.id);
  res.status(200).json({ success: true });
});

app.get("/test", (req, res) => {
  res.status(200).json({ success: true });
});

app.listen(4000, () => console.log("server is running on port 4000"));

```

#### Test using Postman Agent 
```
POST http://localhost:4000/cars/

Body 
row json

{
    "make": "honda",
    "model": "civic"
    "year": 2013
}

GET http://localhost:4000/cars/

DELETE http://localhost:4000/cars/1

```


