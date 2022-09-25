## Email

```
//nodemailer + mailtrap
const nodemailer = require("nodemailer");

const email = {
  host: "smtp.mailtrap.io",
  port: 2525,
  auth: {
    user: "ef001c5325d08c",
    pass: "41397e85c53214",
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
  from: "honeyliketime@gmail.com",
  to: "honeyliketime@gmail.com",
  subject: "node test mail",
  text: "nodejs is so fun!",
};

send(emailData);
```
