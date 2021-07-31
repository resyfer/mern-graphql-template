# MERN Templating steps

This is a list of instructions by [resyfer](https://github.com/resyfer) on how to create a proper MERN stack app template.

DATE : July 21, 2021

## Creating directories:

There's a [shortcut](#shortcut) below for creating directories, this long way is for peeps who want to know what all is being done

1) Either make a new github repository and clone it or create a local directory where your app will be, eg. `my-app`

2) cd into the folder using `cd my-app`

3) Make two directories inside it named `client` and `server` such that your file structure now looks like:
```
my-app/
├── client/
├── server/
├── .gitignore
├── README.md
└── LICENSE
```
4) Run the command
```
cd client && npx create-react-app .
```
This cds into the client folder and initiates the create-react-app in the current (client) directory.

Remember to remove the `.git` directory

6) Inside the `src` directory, remove the vitals and testing from the directory and from the `App.js` and `index.js` files, and the unnecessary css in `App.css` and `index.css` (change their extensions to `.scss` if you want as well). Remove the `README.md` and `.gitignore` in the client directory.

7) Add two directories inside `src` named `components` and `pages`, and make a `css` directory in each of them.

8) Go to the `public` directory and remove all the logos, favicons, `manifest` (if you don't need it) and add an `img` directory if you wish, such that your file structure should look something like this now:
```
my-app/
├── client/
|     ├── node_modules/
|     ├── public/
|     |     ├── img/
|     |     ├── index.html
|     |     └── robots.txt
|     ├── src/
|     |     ├── components/
|     |     |     ├── css/
|     |     |     |     └── Navbar.scss
|     |     |     └── Navbar.jsx
|     |     ├── pages/
|     |     |     └── css/
|     |     |     |     └── Home.scss
|     |     |     └── Home.jsx
|     |     ├── context/
|     |     ├── hooks/
|     |     ├── App.js
|     |     ├── App.scss
|     |     ├── index.js
|     |     └── index.scss
|     ├── package-lock.json
|     └── package.json
├── server/
├── .gitignore
├── README.md
└── LICENSE
```

9) This finishes the front end file structure. Now, the backend. cd into the `server` directory and create a `server.js` and run the command
```
npm init -y
```
This initializes the `package.json` and `package-lock.json`

10) Inside the `server` directory, make directories `config`, `controllers`, `models` and `routes`, such that your folder looks your app directory looks something like this:

```
my-app/
├── client/
|     ├── node_modules/
|     ├── public/
|     |     ├── img/
|     |     ├── index.html
|     |     └── robots.txt
|     ├── src/
|     |     ├── components/
|     |     |     ├── css/
|     |     |     |     └── Navbar.scss
|     |     |     └── Navbar.jsx
|     |     ├── pages/
|     |     |     └── css/
|     |     |     |     └── Home.scss
|     |     |     └── Home.jsx
|     |     ├── context/
|     |     ├── hooks/
|     |     ├── App.js
|     |     ├── App.scss
|     |     ├── index.js
|     |     └── index.scss
|     └── package.json
├── server/
|     ├── config/
|     |     ├── .env
|     |     └── .env.example
|     ├── controllers/
|     ├── models/
|     ├── node_modules/
|     ├── routes/
|     ├── server.js
|     └── package.json
├── .gitignore
├── README.md
└── LICENSE
```

## Shortcut

Follow everything from below here. Make sure you're cd-ed into the app folder
```
touch .gitignore
mkdir client server
cd client && npx create-react-app .
npm i react-router-dom sass
npm i -D nodemon standard
rm -rf .git
rm -f .gitignore README.md
cd src && rm -f reportWebVitals.js setupTests.js logo.svg App.test.js && mkdir components pages context hooks
cd components && mkdir css && touch Navbar.jsx && cd css && touch Navbar.scss
cd ../..
cd pages && mkdir css && touch Home.jsx && cd css && touch Home.scss
cd ../../..
cd public && rm -f favicon.ico logo192.png logo512.png manifest.json && mkdir img
cd ../..
cd server && mkdir config controllers models routes
cd config && touch .env .env.example && cd ..
touch server.js && npm init -y
npm i cors dotenv express mongoose
npm i -D standard
cd ..
```

## File changes:

- `server/config/.env`

```
NODE_ENV = development
MONGO_URI = <URL>
```

- `server/server.js`

```
require('dotenv').config({ path: './config/.env' });
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');

//Import Routes


//Middle Ware
const app = express();
app.use(cors());
app.use(express.json());
app.use(express.urlencoded({extended: true}));

// Global Values
const port = process.env.PORT || 5000;
const url = (process.env.NODE_ENV === 'development') ? 'http://localhost:3000' : 'WEBSITE_URL'; // Every URL will be : url + '/path'

//Routes


// API Error
app.get('/api/*', (req, res) => res.send('API Not Available'));

// Client Redirect
app.get('/', (req, res) => res.redirect(url));

//DB Connect & App Listen
mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(app.listen(port, () => console.log(`Server is listening on port ${port}`)))
  .catch(err => console.log(err));
```

- Whitelist IP in MongoDB
