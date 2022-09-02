## Amplify App
1. Host App
	1. Create a new project locally
	2. Create a new repository and commit the code
	3. @ Amplify console: Create a new app (or get started) > Select **Host Web App** ( previously Deploy, Deliver ), NOT develop or build
	4. Set up Github workflow for building, deplying, and hosting > Save an deploy
	5. Check deployed and live running app
2. Intialize Local App
	1. Insall the Amplify CLI
	```sh
	curl -sL https://aws-amplify.github.io/amplify-cli/install | bash && $SHELL
	```
	OR
	```sh
	npm install -g @aws-amplify/cli
	```
	
	2. Configure the Amplify CLI
	   : Amazon IAM (Identity and Access Management) enables you to manage users and user permissions in AWS

	```sh
	amplify configure
	```
	Press Enter to Continue in terminal, setup in the browser, continue add keys in terminal
	
	3. Deploy a back end and initialize the backend evironment locally
	   @ Amplify console : 
	   1. click Backkend environments > Get started
	   2. Go back to the backend envorioment tab - click Launch Studio ( priviously : Open admin UI )
	   3. Go back to the backend envorioment tab - open the **Local Setup instructions**
	   4. Copy the command to the terminal > setup
	   * To view the Amplify project in the dashboard
	   ```sh
	   amplify console
	   ```
3. Add Authentication
	1. install the Amplify libraries
	   ```sh
	   npm install aws-amplify @aws-amplify/ui-react
	   ```
	2. Create the authentication service
	   ```sh
	   amplify add auth
	   ```
	3. Deploy the auathentication service
	   ```sh
	   amplify push --y
	   ```
	4. Configure the React project with Amplify resources
```js
** index.js

import Amplify from 'aws-amplify';
import config from './aws-exports';
Amplify.configure(config);

```
	5. Add the authentication flow in App.js
```js
import logo from "./logo.svg";
import "@aws-amplify/ui-react/styles.css";
import {
  withAuthenticator,
  Button,
  Heading,
  Image,
  View,
  Card,
} from "@aws-amplify/ui-react";

function App({ signOut }) {
  return (
    <View className="App">
      <Card>
        <Image src={logo} className="App-logo" alt="logo" />
        <Heading level={1}>We now have Auth!</Heading>
      </Card>
      <Button onClick={signOut}>Sign Out</Button>
    </View>
  );
}

export default withAuthenticator(App);

```



## Amplify Studio

https://ui.docs.amplify.aws/


1. setup data, Figma UI (modify SocialA)

2. npm i aws-amplify @aws-amplify/ui-react
    Local setup (pull) > front end & backend connection

3. index.js
    import config from "./aws-exports";
    import Amplify from "aws-amplify";

    import { AmplifyProvider } from "@aws-amplify/ui-react";
    import "@aws-amplify/ui-react/styles.css";

    Amplify.configure(config);

    const root = ReactDOM.createRoot(document.getElementById("root"));
    root.render(
      <AmplifyProvider>
        <App />
      </AmplifyProvider>
    );
    
4. App.js
    import PostCollection from "./ui-components/PostCollection";

    function App() {
      return (
        <PostCollection />
      );
    }
    
5. index.css
    @import url('https://fonts.googleapis.com/css2?family=Inter&display=swap');
    
6. Pagination
https://ui.docs.amplify.aws/react/components/collection

  App.js
  
  function App() {
    return <PostCollection isPaginated itemsPerPage={3} />;
  }

7. Overides
  <PostCollection
      isPaginated
      itemsPerPage={3}
      overrides={{
        "Collection.SocialA[0]": {
          overrides: {
            "Flex.Flex[1].Text[0]": {
              as: "a",
              href: "http://console.aws.amazon.com",
            },
          },
        },
      }}
      
 8. https://sandbox.amplifyapp.com/
 
//////////------------------------------------------------------------

Authentication

  4. App.js
    import { withAuthenticator } from "@aws-amplify/ui-react";
    import PostCollection from "./ui-components/PostCollection";

    function App({ signOut, user }) {
      return (
        <>
          {user.attributes.email}
          <button onClick={signOut}>Sing Out</button>
          <PostCollection isPaginated itemsPerPage={3} />
        </>
      );
    }

    export default withAuthenticator(App, {
      socialProviders: ["apple"],
    });
    
  5. index.js
      (theme)
      
     const theme = {
        name: 'pretty-princess',
        tokens: {
          colors: {
            bakground: {
              primary: {value: 'hotpink'}
            }
          }
        }
      }

      const root = ReactDOM.createRoot(document.getElementById("root"));
      root.render(
        <AmplifyProvider theme={theme}>
          <App />
        </AmplifyProvider>
      );


//////////------------------------------------------------------------

Social Media

 1. Data=======
    Post (id, content, postedAt[AWSDateTime], likeds[int])
    User (profilePic[AWSURL], handle[string], name[string])
    Post - Add relationship: Iser - One post to one User
    
    Fig========
    SocialB
    edit > Group and Rename to CardContent
    
    UI library: find SocialB [Configure] > tide data (Value: @ +:user.handle), (user name- prop: as, value: "a"| prop:href, value: '/users/',+:user.name)
                cardContent (prop: onMouseEnter, Action: Modify.element.property, Element: CardContent, set prop: backgroundColor, to: "#000000")
                            (prop: onMouseLeave, Action: Modify.element.property, Element: CardContent, set prop: backgroundColor, to: "transparent")
                            
 4. import {DataStore} from 'aws-amplify'
    import { Post, User } from './models'
    import {useEffect, useState} from 'react'
    import { SocialB } from './ui-components'
    
    function App(){
      cosnt [posts, setPosts] = useState([])
      const getPosts = async () => {
        const data = await DataStore.query(Post)
        setPosts(data)
      }    
      
      useEffect(()=>{
        getPost()      
      },[])
    
      return (
        <div className='App'>
          {posts.map(post => <SocialB post={post} user={post.User} key={post.id} />)}
        </div>
      )
    }
                
7. overrides (like function)
        {posts.map(post => <SocialB post={post} user={post.User} key={post.id}
          overrides = {{
            getPosts, //update dynamically
            Share: {
              onClick: async e => {
                e.preventDefault()
                const postToChange = await DataStore.query(Post, post.id)
                await DataStore.save(Post.copyOf(postToChange, updated => {
                  updated.likes += 1
                } ))
                getPosts //update dynamically
              }
            }
          }}
             
        />)}
    
//////////------------------------------------------------------------

////////Create

import { DataStore } from '@aws-amplify/datastore';
import { Post } from './models';

await DataStore.save(
    new Post({
		"title": "Lorem ipsum dolor sit amet",
		"comments": [],
		"content": "Lorem ipsum dolor sit amet",
		"author": "Lorem ipsum dolor sit amet",
		"img":  /* Provide init commands */,
		"liked": 1020
	})
);



////////Update

import { DataStore } from '@aws-amplify/datastore';
import { Post } from './models';

/* Models in DataStore are immutable. To update a record you must use the copyOf function
 to apply updates to the item’s fields rather than mutating the instance directly */
await DataStore.save(Post.copyOf(CURRENT_ITEM, item => {
    // Update the values on {item} variable to update DataStore entry
}));


////////Delete
import { DataStore } from '@aws-amplify/datastore';
import { Post } from './models';

const modelToDelete = await DataStore.query(Post, 123456789);
DataStore.delete(modelToDelete);



/////////Query
import { DataStore } from '@aws-amplify/datastore';
import { Post } from './models';


const models = await DataStore.query(Post);
console.log(models);





## Project

1) Setup Project

  1. curl -sL https://aws-amplify.github.io/amplify-cli/install | bash && $SHELL

  2. amplify configure > browser login > terminal info > browser user setting > terminal add accessKey

  3. amplify init (environment name: prod)


2) Adding authentication with Cognito

  1. amplify add auth


3) Create S3 bucket to store image

  1. amplify add storage 
    - access: both auth and guest
    - Authenticated user: check create read delete
    - guest user: only read
    - Lambda Trigger : no


4) Lambda functions to process book orders
  *order:  1. charge user using stripe 2.create the order and then link the order with book static request
  *to handle these > use pipeline resolver in Appsink

    1. amplify add function
      - Lambda function
      - function name: processPayment
      - advnaced settings: N
      - edit now? N

    2. amplify add function
      -function name: createOrder

   🗂️ amplify folder > backend > function


5) Adding GraphQL API (frontend the react app can communicate with Appsink)

	1. amplify add api
		- GraphQL
		
		- default authorization type: ⭐ Amazon Cognito User Pool(Login user)
		- Configure additional auth types? Yes
		- authorization types to configure for the API: ⭐ API Key (guest user)
		- Enter a description for the API Key: Guest user access to books
		- Expire: 365
		- Congifure conflict detection? N
		- do you have an annotated GraphQL schema? N
		- Do you want a quided schema creation? Yes
		- What best describes your project: Single object with fields (e.g. "Todo" with ID, name. description)
		- Do you wnat to edit the schema now? Yes
	2. 🗂️ amplify > backend > api/mindfulbooks > edit schema.graphql
	3. 🗂️ amplify > backend > / function / eidt codes
	4. Fill up dependencies in the lambda functions
		 🗂️ amplify > backend > / function / processPayment / index.js
		 	const USER_POOL_ID = "<userpool_id>";
			const stripe = require("stripe")("<strip_private_key>");
			
6) Create Cloud Resources (Userpool, S3 bucket, AppSync API(api category), DynamoDB Tables(as resolver), Lambda functions(pipeline resolvers)

	1. amplify push











