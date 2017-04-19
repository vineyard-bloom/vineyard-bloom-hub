# Web App Development Guidelines

## Tools

* React - Javascript Library created by Facebook

* React-Router - Declarative routing for React.

* Webpack - Module bundler which takes modules with dependencies and generates static assets by bundling them together based on some configuration

* Babel - Javacript compiler

* Redux - A predictable state container for JavaScript apps

* Axios - Promise based HTTP client

* Bootstrap - Javascript front-end Framwork

* ECMAScript 6 (ES6)

* JSX


## Configuration

* Every project should have a ```config``` folder with configuration files.

* Config files must be either JSON or YAML, not code.

* Do not store server URL's directly into your requests.  Store them in a config file to easily change between dev and prod.


## Project Structure

* This file structure is for a basic webapp.  If you need more separation with redux, you can also group actions and reducers into a respective folder

```
public/
   index.html
src/
   components/
      pages/
	     main.jsx
	     page1.jsx
	     page2.jsx
	  component1.jsx
	  component2.jsx
   actions.jsx
   reducer.jsx (optional)
   store.jsx
   config.js
   app.js
.babelrc
package.json
webpack.config.js
```

## Project Setup

### Creating your directory
```
mkdir *project name*
```
```
cd *project name*
```

* Next initialize npm and start adding packages
```
npm init
```

### Webpack/Babel
```
npm i --save-dev webpack webpack-dev-server
```

* Now we need to create a webpack config file
```touch webpack.config.js```

* Inside 'webpack.config.js' add this code

```
const path = require('path');
const BUILD_DIR = path.join(__dirname, 'public/');
const APP_DIR = path.join(__dirname, 'src/');
module.exports = {
    entry: APP_DIR + "/index.jsx",
    output: {
        filename: "bundle.js",
        path: BUILD_DIR,
    },
    resolve: {
        extensions: ['.js', '.jsx', '.json']
    },
    module: {
        loaders: [
            { test: /\.js$/, loader: 'babel-loader', exclude: /node_modules/ },
            { test: /\.jsx$/, loader: 'babel-loader', exclude: /node_modules/ }
        ]
    },
    devServer: {
        publicPath: "/",
        contentBase: "./public",
        port: 3000,
        historyApiFallback: true
    },
};
```

* This tells webpack that our main application file (index.jsx) is the entry point and the bundled application should be output to the public folder

* Next we need to add Babel
```npm i --save-dev babel-loader babel-core babel-preset-es2015 babel-preset-react```

* We must now create a '.babelrc' file in the project root and add presets
```
touch .babelrc
```
```
{
  "presets": [
    "es2015",
    "react"
  ]
}
```

### React & React Router
* Let's create our web app! 
```
npm i react react-dom react-router react-router-dom --save
```
* create a file named 'main.jsx' inside 'src/components/pages' and put a simple page inside

```
import React from 'react';
import { render } from 'react-dom';

class Main extends React.Component {

    constructor(props) {
        super(props);
        this.state = {  }
    }

    componentDidMount() {
    }


    render() {
        return <div>
            My React Site!
        </div>
    }
}

export default Main

```

* In your 'src/' folder create a file called 'index.jsx' this is where all the routing will be handled.  Add this code to it
```
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter, Route } from 'react-router-dom'
import Main from './components/pages/main.jsx';

class App extends React.Component {


    render() {
        return (
            <BrowserRouter>
                <Route path='/' component={Main}/>
            </BrowserRouter>
        )
    }
}


export default App

ReactDOM.render(<App />,document.getElementById('root'));


```
* In your 'public/' folder create a file called 'index.html' this is the main layout for the web app.  Add this code to it
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Website Title</title>
</head>
<body>
<div id="root">

</div>
<script type="text/javascript" src="bundle.js"></script>
</body>
</html>
```



* In your package.json file add two 'scripts' entries:
```
"dev": "webpack && webpack-dev-server --hot --inline"",
```
```
"start": "webpack -w"
```

* Now run 'npm run start' and you should see 'My React Site!' displayed on the page

### Redux

* Now that we have a basic React web app working, it's time to add Redux

```
npm i redux react-redux redux-promise-middleware redux-thunk --save
```

* Create a new file called store.jsx inside your src folder. Inside this folder add this code
```
import { applyMiddleware, compose, createStore } from "redux"
import promise from "redux-promise-middleware" // Promise middleware for redux
import thunk from 'redux-thunk'; // Thunk allows functions to be passed through redux

const middleware = compose(applyMiddleware(promise(), thunk))

export default createStore(reducer, middleware)


function reducer(state = {}, action) {

    switch (action.type) {
        case "ACTION": {
            return {
                state,
                propName: action.data
            }
        }
    }

    return state
}

```

* At the top where I specified the file structure I mentioned the reducer class was optional.  For now we are putting it in the store.  You can separate this out into files or a directory of files if you would like.

* Open your 'src/components/index.jsx' file and change the last line to match this
```
ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root'));
```

* Also make sure to use the correct imports
```
import { Provider } from 'react-redux'
import store from "./store.jsx"
```

* Now we have to connect our props to each component change your main.jsx file to match this format
```
import React from 'react';
import { render } from 'react-dom';
import { connect } from "react-redux"

class Main extends React.Component {

    constructor(props) {
        super(props);
        this.state = {  }
    }

    componentDidMount() {
    }


    render() {
        return <div>
            My React site!!
        </div>
    }
}

const mapStateToProps = (state) => {
    return state
}

export default connect(mapStateToProps)(Main)
```

* Now you can dispatch actions by importing the function from actions.jsx and then calling this.props.dispatch(functionName)

### Axios

* Now that we have incorporated redux into our app, we can add our HTTP client.

```
npm i axios --save
```

* Inside your src directory, create a file called 'actions.jsx' (This is another case if you prefer you can create a directory of files instead of one file)

* Here is an example of an action getting the bitcoin price from coincap
```
import axios from "axios";

export function getBtcPrice() {
    return function (dispatch) {
        return axios.get('http://www.coincap.io/global')
            .then((response) => {
                dispatch({ type: "GET_BTC_PRICE", payload: response.data.btcPrice })
            })
            .catch((err) => {
                console.log('Error in getBtcPrice', err)
            })
    }
}
```

* Here axios pings coincap.io/global and returns a response, then we dispatch the bitcoin price from the response to our reducer of type "GET_BTC_PRICE.  We then set up a reducer method to handle the data as follows:

```
...
switch (action.type) {
        case "GET_BTC_PRICE": {
            return {
                state,
                btcPrice: action.data
            }
        }
    }
...
```

### Bootstrap

* Now that we have all our components installed, it's time to setup bootstrap

```
npm install react-bootstrap --save
```

* In the 'head' tag of 'index.html' add this line
```
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/latest/css/bootstrap.min.css">
```

* Now you will be able to use Bootstrap in your web app!








