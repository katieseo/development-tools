#### backend
// Sanity

1. init sanity
2. setup schemas
3. sanity start

.eslintrc
```js
{
  "extends": "@sanity/eslint-config-studio",
  "presets": ["@babel/preset-env"]
}
```


#### Google OAuth get clientID

https://console.cloud.google.com/

APIs and Services > Credentials > Create Project

OAuth consent screen > external > add info(no logo), add test user

Credentials > Create Credentials > OAuth Client ID > web application > 
```js
Authorised JavaScript origins: 
http://localhost
http://localhost:3000

Authorised redirect URIs
http://localhost
http://localhost:3000
```


#### frontend

* .env variables should start with 'REACT_APP_'

npm install @sanity/client @sanity/image-url react-icons react-loader-spinner react-masonry-css react-router-dom uuid
npm install react-google-login --legacy-peer-deps

// sanity
src > client.js

1. backend folder terminal : `sanity manage` to get projectId, token (api>token>Editor)
2. also add http://localhost:3000 check credential to Cors Origin

https://www.sanity.io/docs/how-queries-work



// google Oauth

1.
public/index.html > add script
```js
<script src="https://accounts.google.com/gsi/client" async defer></script>
```
https://developers.google.com/identity/gsi/web/guides/client-library

2.
Login.js

npm i jwt-decode

https://developers.google.com/identity/gsi/web/reference/js-reference

```js
import React, { useEffect } from "react";
import { useNavigate } from "react-router-dom";
import { client } from "../client";
import jwt_decode from "jwt-decode";

const navigate = useNavigate();

const handleCredentialResponse = (response) => {
  // decode Encoded JWT ID token
  let userObejct = jwt_decode(response.credential);
  console.log(userObejct);

  localStorage.setItem("user", JSON.stringify(userObejct));
  const { name, sub, picture } = userObejct;
  const doc = {
    _id: sub,
    _type: "user",
    userName: name,
    image: picture,
  };
  client.createIfNotExists(doc).then(() => {
    navigate("/", { replace: true });
  });
};

useEffect(() => {
    /* global google */     <---for eslint
    google.accounts.id.initialize({
      client_id:
        "my client id",
      callback: handleCredentialResponse,
    });

    google.accounts.id.renderButton(document.getElementById("signIn"), {
      theme: "outline",
      size: "large",
    });
  }, []);

return <div id="signIn"></div>
```






