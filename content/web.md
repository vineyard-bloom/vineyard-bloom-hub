# Web App Development Guidelines


## Configuration

* Every project should have a ```config``` folder with configuration files.

* Config files should be either JSON or YAML, not code.

* Do not store URL's directly in your source code.  Store them in a config file.

* Configuration files should not be tracked by Git.  Their paths should be added to .gitignore.

* For every configuration file there should be a sample configuration file tracked by Git.

* A sample configuration file has ```-sample``` appended to the file name such as ```config-sample.json```.  Sample configuration files are not read by the server. A copy without ```-sample``` must be created for the app to compile/run.


## Architecture

* A project repo should not contain both client and server code.

* Client projects should not depend on server projects and vice versa.

* Whenever possible, use a trusted CDN for serving third-party libraries instead of packaging them.

* A web app should support easy switching between minified and non-minified JavaScript files for both its own code and third-party libraries.

* If a web app is compiled it should generate source map files.

