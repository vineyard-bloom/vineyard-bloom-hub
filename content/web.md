# Web App Development Guidelines

## Tools

* React - Javascript Library created by Facebook

* React-Router - Declarative routing for React.

* Webpack - Module bundler which takes modules with dependencies and generates static assets by bundling them together based on some configuration

* Babel - 5

* Redux - A predictable state container for JavaScript apps

* Axios - Promise based HTTP client

* ECMAScript 6 (ES6)

* JSX


## Configuration

* Every project should have a ```config``` folder with configuration files.

* Config files must be either JSON or YAML, not code.

* Do not store server URL's directly into your requests.  Store them in a config file to easily change between dev and prod.


## Project Structure

```
public/ *auto generated*
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
   index.js
.babelrc
package.json
webpack.config.js
```



