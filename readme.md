# Express and React Lab

During this lab we will be creating a full stack portfolio page using Express and React to see how you can create a full stack project using a simple API build with express and a frontend application with React.

## Setup

- Fork and clone this repo
- cd into Express_React_Portfolio_Lab

- Inside this folder create a folder for our backend app called `backend` this will house our express application

- Then we will generate a react project for a our frontend (the command should be run from the express_react_folder so make sure your terminal is in that folder)...

  - `npx create-react-app frontend`

- The end result should be the following folder structure

```
- /Express_React_Portfolio_Lab
  > /backend
  > /frontend
```

## Express app setup

- Open up your terminal inside the backend folder

- create a new npm project with the command `npm init -y`

- install the following
  - `npm install express cors`
  - `npm install --save-dev nodemon`

#### What we installed

1. express: The backend web framework for generating a web server

2. cors: middleware to make sure we don't get cors errors when our react app makes a request to our express app

3. nodemon: development tool to auto restart our server whenever

- update the package.json with the following main and scripts

```json
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
},
```

#### Our Data

Instead of using a database we will use JSON files to store the data for our project for now.

Run the following command in the `backend` folder to create our files

- `touch server.js projects.json about.json`

#### projects.json

In this file you should use the below example but replace it with your projects from the previous units. (json files don't need to be exported, node knows how to read them).

Essentially this file is an array of objects that represent your projects.

```json
[
  {
    "name": "project1",
    "live": "https://app.herokuapp.com/whatever",
    "git": "http://www.github.com/username/reponame",
    "image": "http://www.imgur.com/pictureofproject.png"
  },
  {
    "name": "project2",
    "live": "https://app.netlify.app/whatever",
    "git": "http://www.github.com/username/reponame",
    "image": "http://www.imgur.com/pictureofproject.png"
  }
]
```

#### about.json

This file will be one big option with information about you to use in your portfolio.

```json
{
  "name": "Bob Smith",
  "email": "Bob@BobSmith.dev",
  "headshot": "http://www.imgur.com/pictureofproject.png",
  "bio": "Bob Smith graduated from General Assembly in 2017. Afterwords, he went to work for XYZ Technologies where he maintained a full stack application using Meteor and Ember. He also recently started started learning Prolog, cause why not waste time."
}
```

#### server.js

Now we can make our server, here is the overview of what we will do.

- import our dependencies and json files
- create our app object
- add our cors middleware
- create a home route to test our app
- create a `/projects` route to retrieve our projects
- create a `/about` route to retrieve our about info
- setup our server listener

```js
// Import Dependencies
require("dotenv").config();
const express = require("express");
const cors = require("cors");

// Import JSON files
const projects = require("./projects.json");
const about = require("./about.json");

// Create our app object
const app = express();

// set up middleware
app.use(cors());

//home route for testing our app
app.get("/", (req, res) => {
  res.send("Hello World");
});

// route for retrieving projects
app.get("/projects", (req, res) => {
  // send projects via JSON
  res.json(projects);
});

// route for retrieving about info
app.get("/about", (req, res) => {
  // send projects via JSON
  res.json(about);
});

//declare a variable for our port number
const PORT = process.env.PORT || 4000;

// turn on the server listener
app.listen(PORT, () => console.log(`Listening on port ${PORT}`));
```

- run your server `npm run dev`

- go to `localhost:4000` and make sure you see "hello world"

- go to `localhost:4000/projects` and make sure you see your projects as JSON

- go to `localhost:4000/about` and make sure you see your about info as json

- Our Backend is complete, now to deploy on Render.

## Building the Frontend

Make sure to keep your Render url handy, we'll need it in a moment.

- open your terminal to the `frontend` folder

- install react router `npm install react-router react-router-dom`

## Setting up React Router

open up `src/index.js` and make the following changes

```js
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";
// import router
import { BrowserRouter as Router } from "react-router-dom";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <Router>
    <React.StrictMode>
      <App />
    </React.StrictMode>
  </Router>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

## Getting things scoped out

- create a `src/components` folder and `src/pages` folder

- in components create `Header.js` and `Footer.js`

`src/components/Header.js`

```jsx
function Header(props) {
  return <h1>Header</h1>;
}

export default Header;
```

`src/components/Footer.js`

```jsx
function Footer(props) {
  return <h1>Footer</h1>;
}

export default Footer;
```

- In `src/pages` create `Home.js`, `About.js` and `Projects.js`

`src/pages/Home.js`

```jsx
function Home(props) {
  return <h1>Home</h1>;
}

export default Home;
```

`src/pages/About.js`

```jsx
function About(props) {
  return <h1>About</h1>;
}

export default About;
```

`src/pages/Projects.js`

```jsx
function Projects(props) {
  return <h1>Projects</h1>;
}

export default Projects;
```

## App.js

Here is the plan

- import all our components
- import the Route and Routes component from Router
- Setup our routes
- create a variable called URL with our Render URL
- pass the URL as a prop to about and projects so they can make a call to our API

`src/App.js`

```jsx
import "./App.css";

// IMPORT COMPONENTS
import Header from "./components/Header";
import Footer from "./components/Footer";
import { Route, Routes } from "react-router-dom";
// IMPORT PAGES
import Home from "./pages/Home";
import About from "./pages/About";
import Projects from "./pages/Projects";



function App() {
  // YOUR Render URL should be in your env; make sure you include the trailing slash
  const URL = process.env.REACT_APP_URL || "http://localhost:4000/";

  return (
    <div className="App">
      <Header />
      <Routes>
        <Route exact path="/" element={<Home />}/>
        <Route path="/projects" element={<Projects URL={URL} />} />
        <Route path="/about" element={<About URL={URL} />} />
      </Routes>
      <Footer />
    </div>
  );
}

export default App;
```

## The Navigation

Right now we can't switch between our routes with Link components, so let's build our navigation so we can switch between pages. Our navigation should be in our header.

`src/components/Header.js`

```jsx
import { Link } from "react-router-dom";

function Header(props) {
  //inline style for the nav tag
  const navStyle = {
    display: "flex",
    justifyContent: "space-around",
    border: "3px solid black",
    padding: "8px",
    width: "90%",
    margin: "auto",
  };

  return (
    <header>
      <h1>My Portfolio Page</h1>
      <nav style={navStyle}>
        <Link to="/">
          <div>HOME</div>
        </Link>
        <Link to="/about">
          <div>ABOUT</div>
        </Link>
        <Link to="/projects">
          <div>PROJECTS</div>
        </Link>
      </nav>
    </header>
  );
}

export default Header;
```

You should be able to navigate between our pages but they are only one word at the moment. Let's populate our projects and about pages.

## About Page

We will do the following...

- create a state variable to hold the about data
- create a function to make the api call and update state
- call the function within a useEffect to avoid an infinite loop
- Use a ternary to render one thing if we have the data from the api and something else if we don't

`src/pages/About.js`

```jsx
import { useState, useEffect } from "react";

function About(props) {
  // create state to hold about data
  const [about, setAbout] = useState(null);

  // create function to make api call
  const getAboutData = async () => {
    // make api call and get response
    const response = await fetch(props.URL + "about");
    // turn response into javascript object
    const data = await response.json();
    // set the about state to the data
    setAbout(data);
  };

  // make an initial call for the data inside a useEffect, so it only happens once on component load
  useEffect(() => {
    getAboutData()
  }, [])

  // define a function that will return the JSX needed once we get the data
  const loaded = () => (
    <div>
      <h2>{about.name}</h2>
      <h3>{about.email}</h3>
      <p>{about.bio}</p>
    </div>
  );

  // if data arrives return the result of loaded, if not, an h1 that says loading
  return about ? loaded() : <h1>Loading...</h1>;
}

export default About;

```

## Projects

We will use the same pattern for our projects

- create a state variable to hold the projects data
- create a function to make the api call and update state
- call the function within a useEffect to avoid an infinite loop
- Use a ternary to render one thing if we have the data from the api and something else if we don't
- our loaded function will map over the array of projects and return the jsx for project

`src/pages/Projects.js`

```jsx
import { useState, useEffect } from "react";

function Projects(props) {
  // create state to hold projects
  const [projects, setProjects] = useState(null);

  //create function to make api call
  const getProjectsData = async () => {
    //make api call and get response
    const response = await fetch(props.URL + "projects");
    // turn response into javascript object
    const data = await response.json();
    // set the projects state to the data
    setProjects(data);
  };

  // make an initial call for the data inside a useEffect, so it only happens once on component load
  useEffect(() => {
    getProjectsData();
  }, []);

  // define a function that will return the JSX needed once we get the data
  const loaded = () => {
    return projects.map((project, index) => (
      <div key={index}>
        <h1>{project.name}</h1>
        <img src={project.image} alt={project.name} />
        <a href={project.git}>
          <button>Github</button>
        </a>
        <a href={project.live}>
          <button>live site</button>
        </a>
      </div>
    ));
  };

  return projects ? loaded() : <h1>Loading...</h1>;
}

export default Projects;
```

## Deploy Frontend

Once everything seems working, do the following.


- add all files to staging `git add .`

- commit `git commit -m "frontend complete"`

- head over to netlify and create a new project based on your frontend, it should auto detect the build command and deploy.

- if you run into any issues refer to this [guide for deployment](https://tuts.alexmercedcoder.com/2021/1/deployreact/)

- You've deployed your portfolio!!!

## What Now?

- Add some content to the home page
- Spend some time styling your frontend
- Add content to the footer

## Hungry For More

#### Styling Challenges (choose 1)

- Style using the Styled Components Library `npm install styled-components`
- Style using sass `npm install sass` (after install change the extension on your css files scss)
- Try using `bulma-react-components` a [library of components pre-made using Bulma](https://www.npmjs.com/package/react-bulma-components)

#### Express Challenges

- Try adding a form to your React project and a post route on your express app to go with it
- Convert from JSON files to using a mongo database for your project (not really necessary for the about info)
