#### backend
1. init sanity
2. setup schemas
3. sanity start

.eslintrc
```
{
  "extends": "@sanity/eslint-config-studio",
  "presets": ["@babel/preset-env"]
}
```

#### frontend
npm install @sanity/client @sanity/image-url react-icons react-loader-spinner react-masonry-css react-router-dom uuid
npm install react-google-login --legacy-peer-deps

src > client.js

1. backend folder terminal : `sanity manage` to get projectId, token (api>token>Editor)
2. also add http://localhost:3000 check credential to Cors Origin



#### Google Login

https://console.cloud.google.com/

APIs and Services > Credentials > Create Project

OAuth consent screen > external > add info(no logo), add test user

Credentials > Create Credentials > OAuth Client ID > web application > 
```
Authorised JavaScript origins: 
http://localhost
http://localhost:3000

Authorised redirect URIs
http://localhost
http://localhost:3000
```




