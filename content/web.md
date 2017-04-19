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
src/
   components/
      pages/
	     main.jsx
	     page1.jsx
	     page2.jsx
	  component1.jsx
	  component2.jsx
   actions.jsx
   reducer.jsx
   store.jsx
   config.js
   index.html
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

### Webpack/babel
```
npm i --save-dev webpack webpack-dev-server
```

* Now we need to create a webpack config file
```touch webpack.config.js```

* Inside 'webpack.config.js' add this code

```
module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: __dirname + "public",
  }
};
```

* This tells webpack that our main application file (index.js) is the entry point and the bundled application should be output to the public folder

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

* Also, add a js/jsx loader to your 'webpack.config.js' file
```
...
  resolve: {
    extensions: ['', '.js', '.jsx', '.json']
  },
  module: {
    loaders: [
      { test: /\.js$/, loader: 'babel-loader', exclude: /node_modules/ },
      { test: /\.jsx$/, loader: 'babel-loader', exclude: /node_modules/ }
    ]
  }
...
```

### React & React Router
* Let's create our web app! 
```
npm i react react-dom react-router --save
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
</body>
</html>
```





* Now run 'npm run start' and you should see 'My React Site!' displayed on the page












