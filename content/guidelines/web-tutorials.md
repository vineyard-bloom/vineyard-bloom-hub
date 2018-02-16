# Web Tutorials

## Tools

* React - Javascript Library created by Facebook

* React-Router - Declarative routing for React.

* Webpack - Module bundler which takes modules with dependencies and generates static assets by bundling them together based on some configuration

* Babel - Javacript compiler

* Axios - Promise based HTTP client

* Bootstrap - Javascript front-end Framwork

* Bootstrap-React - React wrapper components for Bootstrap

* ECMAScript 6 (ES6)

* JSX


## Project Structure

* This file structure is for a basic webapp.

```
config/
    config.json
    config-sample.json
res/
   images/
   styles/
      main.css
   index.html
src/
   helpers/
   components/
      pages/
	     main.jsx
	     page1.jsx
	     page2.jsx
	  component1.jsx
	  component2.jsx
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
import { BrowserRouter, Route, Switch } from 'react-router-dom'
import Main from './components/pages/main.jsx';
import Component1 from './components/pages/component1.jsx';
import Component2 from './components/pages/component2.jsx';

class App extends React.Component {


    render() {
        return (
            <BrowserRouter>
                <Switch>
                    <Route exact path='/' component={Main}/>
                    <Route path='/component1' component={Component1}/>
                    <Route path='/component2' component={Component2}/>
                </Switch>
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
    <link rel="stylesheet" href="style/main.css">
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
