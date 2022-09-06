## Node Rest API using Express and SQLite

```js
npm init -y
```

```
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



