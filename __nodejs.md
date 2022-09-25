## Email

```js
//nodemailer + mailtrap
const nodemailer = require("nodemailer");

const email = {
  host: "smtp.mailtrap.io",
  port: XX,
  auth: {
    user: "XX",
    pass: "XX",
  },
};

const send = async (option) => {
  nodemailer.createTransport(email).sendMail(option, (error, info) => {
    if (error) {
      console.log(error);
    } else {
      console.log(info);
      return info.response;
    }
  });
};

let emailData = {
  from: "xx@gmail.com",
  to: "xx@gmail.com",
  subject: "node test mail",
  text: "nodejs is so fun!",
};

send(emailData);
```

## Express
```js
const express = require("express");
const app = express();

app.listen(8000, () => console.log("wow"));

app.get("/", (req, res) => {
  res.send("Hello!");
});

//ejs + render
app.set("views", __dirname + "/views");
app.set("view engine", "ejs");
app.engine("html", require("ejs").renderFile);

app.get("/about", (req, res) => {
  res.render("about.html");
});
```
