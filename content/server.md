# Server Development Guidelines

## Configuration

* Every server project should have a ```config``` folder with configuration files.

* Config files must be either JSON or YAML, not code.

* Divide sensitive and non-sensitive configuration into separate files.

* Any configuration files intended to be modified for deployment should not be tracked by Git.  Their paths should be added to .gitignore.

* For every configuration file a server uses, there should be either a default or sample configuration file.  These files should not be edited on deployment.

* A default configuration has ```-default``` appended to the file name such as ```config-default.json```.  Default configuration files are loaded first and then overriden by an optional configuration file.

* A sample configuration file has ```-sample``` appended to the file name such as ```config-sample.json```.  Sample configuration files are not read by the server. A copy without ```-sample``` must be created for the server to run.

* Configuration should not be stored in OS environment variables.


## Development

* A server project only needs to run on Linux for production, but should support Linux, Mac, and Windows for local development.

* A server project that cannot be run and tested locally is broken and not ready for production use.


## Web Services

### Interface

* All web request bodies should be JSON.

* JSON web service requests without the ```application/json``` content type are invalid.

* A web service response should always have the content type of ```application/json```.

* A web service should never return HTML.

* A web service response body should always be valid JSON, even if it is just an empty dictionary.

* A web service should never return a redirect (any HTTP status code within the ```300``` range).  Redirects are an HTML UX mechanic.

* Unless a web service is only consumed by a single website or by an internal server infrastructure, web services must support versioning and have a required version parameter.

* Web service URL paths must be distinctly separate from static content URL paths.

    * Bad: `/images/` and `/users`
    
    * Good:  `/images/` and `/api/users`
    
* Never pass sensitive data such as user credentials in a url.  Pass sensitive data through the HTTP body.

* Do not pass credentials on every request.  Use sessions/tokens.

* The interface of an HTTP `get` request should be treated as immutable.  In other words, the public functionality of an HTTP `get` request should never modify server state.  Under the hood the server can change server state when handling a `get` request such as caching `get` responses.
 
* URL paths should always place nouns before verbs.
    
    * Bad: `run/app`
    
    * Good: `app/run`
    
* Keep in mind that while tradition has been to write HTTP keywords in all caps, the official HTTP 1 spec is case-insensitive, and the HTTP 2 spec is exclusively lowercase. The future convention of HTTP will be to write `post`, not `POST`. 

### Implementation

* Aside for advanced fringe cases such as uploading files where lower level HTTP handling is needed, the higher level Vineyard model of web services should be used:

    * Endpoint handler functions should not be passed a response object.
    
    * Endpoint handler functions should not be passed a request object.
    
    * Endpoint handler functions should be passed a dictionary that contains a mix of query parameters and post data.
    
    * Endpoint handler functions should return a promise containing a dictionary object as the response body.
    
    * When an endpoint handler encounters an error, it should throw an exception object that contains an HTTP status code and message.
     
    * There should be only one place in your server project that sends web service responses.  This place should be divided into two parts, one that sends HTTP ```200``` responses and one that sends error responses.
       
    * When the response code catches an exception without an HTTP status code, the response status code should be ```500```.     
    
    * [Vineyard Lawn](https://github.com/silentorb/vineyard-lawn) is a thin web service layer that provides all of these features.


## Code

* Aside for a single entry point like index.js, source code files should not be in the root folder of a project

* There should be no IP addresses or URLs in source code.

* There should be no port numbers in source code except as optional default values.


## Architecture

* Client and server projects should always be stored in separate repositories.

* Aside for legacy projects and synchronizing client connections across Node.js clusters, do not use Redis!  Whatever problem you are trying to solve with Redis, there is a better tool for the job.

* IP table customization should only be used for an extra layer of security after an external firewall.  Do not use IP tables for any form of routing or logic.


### Node.js

* Never serve static content from Node.js.  Use a dedicated HTTP daemon like Nginx or Apache.

* Always lock dependencies to a major version using either exact versions or the carot symbol

    * VERY GOOD: "2.0.1"

    * GOOD: "^2.0.1"

    * BAD: ">=2.0.1"

    * VERY BAD: "*"

* Changing the major version of a dependency requires incrementing the major version of any dependent module.

* This is not a universal convention, but packages you create that have a major version number of zero should be considered alpha or beta. (Example: 0.5.3)
