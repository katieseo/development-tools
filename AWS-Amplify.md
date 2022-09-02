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
 
---

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


--- 

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
    
---
#### Create

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



#### Update

import { DataStore } from '@aws-amplify/datastore';
import { Post } from './models';

/* Models in DataStore are immutable. To update a record you must use the copyOf function
 to apply updates to the item’s fields rather than mutating the instance directly */
await DataStore.save(Post.copyOf(CURRENT_ITEM, item => {
    // Update the values on {item} variable to update DataStore entry
}));


#### Delete
import { DataStore } from '@aws-amplify/datastore';
import { Post } from './models';

const modelToDelete = await DataStore.query(Post, 123456789);
DataStore.delete(modelToDelete);


#### Query
import { DataStore } from '@aws-amplify/datastore';
import { Post } from './models';


const models = await DataStore.query(Post);
console.log(models);


## Amplify App
#### 1. Host App
	1. Create a new project locally
	2. Create a new repository and commit the code
	3. @ Amplify console: Create a new app (or get started) > Select **Host Web App** ( previously Deploy, Deliver ), NOT develop or build
	4. Set up Github workflow for building, deplying, and hosting > Save an deploy
	5. Check deployed and live running app

#### 2. Intialize Local App | Amplify CLI
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
	   
	   To view the Amplify project in the dashboard
	   ```sh
	   amplify console
	   ```
	   
#### 3. Add Authentication | Amazon Cognito
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

	6. Run the app locally (npm start)
	
	7. Set up CI/CD of the front end and backend
	
	```sh
	amplify console
	```
	- **App settings > Build settings** (sidebar)
	- and modify it to add the **backend section (lines 2-7 in the code below)** to your amplify.yml
	- edit and save
	
	```js
	version: 1
	backend:
	  phases:
	    build:
	      commands:
		- '# Execute Amplify CLI with the helper script'
		- amplifyPush --simple
	frontend:
	  phases:
	    preBuild:
	      commands:
		- yarn install
	    build:
	      commands:
		- yarn run build
	  artifacts:
	    baseDirectory: build
	    files:
	      - '**/*'
	  cache:
	    paths:
	      - node_modules/**/*

	```
	- Next, update front end branch to point to the backend environment you just created. 
	  Under the branch name, choose **Edit**, and then point your **master** branch to the **dev** backend you just created.
	  ( not sure: if it is dev.. it shows only staging )
	  Choose **Save**
	  
	- Git commit ("added auth")

#### 4. Add API and database | AWS AppSync Amazon DynamoDB
	1. Create a GraphQL API and database
	Add a GraphQL API
	```
	amplify add api
	```
	schema.graphql
	```
	type Note @model {
	  id: ID!
	  name: String!
	  description: String
	}

	```
	Save
	
	2. Deploy the API
	```
	amplify push --y
	```
		This will do 3 things:
		Create the AppSync API
		Create a DynamoDB table
		Create the local GraphQL operations in a folder located at src/graphql that you can use to query the API
	
	To view the GraphQL API 
	```
	amplify console api

	> Choose GraphQL
	```
	
	To view the Amplify app
	```
	amplify console
	```
	
	3. Write front-end code to interact with the API
	```
	import React, { useState, useEffect } from 'react';
	import './App.css';
	import { API } from 'aws-amplify';
	import { withAuthenticator, AmplifySignOut } from '@aws-amplify/ui-react';
	import { listNotes } from './graphql/queries';
	import { createNote as createNoteMutation, deleteNote as deleteNoteMutation } from './graphql/mutations';

	const initialFormState = { name: '', description: '' }

	function App() {
	  const [notes, setNotes] = useState([]);
	  const [formData, setFormData] = useState(initialFormState);

	  useEffect(() => {
	    fetchNotes();
	  }, []);

	  async function fetchNotes() {
	    const apiData = await API.graphql({ query: listNotes });
	    setNotes(apiData.data.listNotes.items);
	  }

	  async function createNote() {
	    if (!formData.name || !formData.description) return;
	    await API.graphql({ query: createNoteMutation, variables: { input: formData } });
	    setNotes([ ...notes, formData ]);
	    setFormData(initialFormState);
	  }

	  async function deleteNote({ id }) {
	    const newNotesArray = notes.filter(note => note.id !== id);
	    setNotes(newNotesArray);
	    await API.graphql({ query: deleteNoteMutation, variables: { input: { id } }});
	  }

	  return (
	    <div className="App">
	      <h1>My Notes App</h1>
	      <input
		onChange={e => setFormData({ ...formData, 'name': e.target.value})}
		placeholder="Note name"
		value={formData.name}
	      />
	      <input
		onChange={e => setFormData({ ...formData, 'description': e.target.value})}
		placeholder="Note description"
		value={formData.description}
	      />
	      <button onClick={createNote}>Create Note</button>
	      <div style={{marginBottom: 30}}>
		{
		  notes.map(note => (
		    <div key={note.id || note.name}>
		      <h2>{note.name}</h2>
		      <p>{note.description}</p>
		      <button onClick={() => deleteNote(note)}>Delete note</button>
		    </div>
		  ))
		}
	      </div>
	      <AmplifySignOut />
	    </div>
	  );
	}

	export default withAuthenticator(App);
	```
	
	There are 3 main functions in our app:

	fetchNotes - This function uses the API class to send a query to the GraphQL API and retrieve a list of notes.
	createNote - This function also uses the API class to send a mutation to the GraphQL API, the main difference is that in this function we are passing in the variables needed for a GraphQL mutation so that we can create a new note with the form data.
	deleteNote - Like createNote, this function is sending a GraphQL mutation along with some variables, but instead of creating a note we are deleting a note.


#### 5. Add storage | Amazon S3
	```
	amplify add storage
	```
	Please provide bucket name: <your-unique-bucket-name>
	
	Update the GraphQL scheme
	amplify/backend/api/notesapp/schema.graphql
	```
	type Note @model {
	  id: ID!
	  name: String!
	  description: String
	  image: String
	}
	```
	```
	amplify push --y
	```
	
	App.js
	a. First add the Storage class to your Amplify imports:
	```
	import { API, Storage } from 'aws-amplify';
	```
	
	b. In the main App function, create a new onChange function to handle the image upload:

	```
	async function onChange(e) {
	  if (!e.target.files[0]) return
	  const file = e.target.files[0];
	  setFormData({ ...formData, image: file.name });
	  await Storage.put(file.name, file);
	  fetchNotes();
	}
	```
	
	c. Update the fetchNotes function to fetch an image if there is an image associated with a note:
	
	```
	async function fetchNotes() {
	  const apiData = await API.graphql({ query: listNotes });
	  const notesFromAPI = apiData.data.listNotes.items;
	  await Promise.all(notesFromAPI.map(async note => {
	    if (note.image) {
	      const image = await Storage.get(note.image);
	      note.image = image;
	    }
	    return note;
	  }))
	  setNotes(apiData.data.listNotes.items);
	}
	```
	
	d. Update the createNote function to add the image to the local image array if an image is associated with the note:
	```
	async function createNote() {
	  if (!formData.name || !formData.description) return;
	  await API.graphql({ query: createNoteMutation, variables: { input: formData } });
	  if (formData.image) {
	    const image = await Storage.get(formData.image);
	    formData.image = image;
	  }
	  setNotes([ ...notes, formData ]);
	  setFormData(initialFormState);
	}
	```
	
	e. Add an additional input to the form in the return block:
	```
	<input
	  type="file"
	  onChange={onChange}
	/>
	```
	
	f. When mapping over the notes array, render an image if it exists:
	```
	{
	  notes.map(note => (
	    <div key={note.id || note.name}>
	      <h2>{note.name}</h2>
	      <p>{note.description}</p>
	      <button onClick={() => deleteNote(note)}>Delete note</button>
	      {
		note.image && <img src={note.image} style={{width: 400}} />
	      }
	    </div>
	  ))
	}
	```


## Book Project

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











