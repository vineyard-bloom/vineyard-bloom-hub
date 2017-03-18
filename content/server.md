# Server Development Guidelines

## General

* A server project should not be platform specific. It needs to work on Linux, Mac, and Windows.

* Aside for a single entry point like index.js, source code files should not be in the root folder of a project

* There should be no IP addresses or URLs in source code.

* There should be no port numbers in source code except as optional default values.

## Configuration

* Every server project should have a ```config``` folder with configuration files.

* Config files must be either JSON or YAML, not code.

* Divide sensitive and non-sensitive configuration into separate files.

* Any configuration files intended to be modified for deployment should not be tracked by Git.  Their paths should be added to .gitignore.

* For every configuration file a server uses, there should be either a default or sample configuration file.  These files should not be edited on deployment.

* A default configuration has ```-default``` appended to the file name such as ```config-default.json```.  Default configuration files are loaded first and then overrided by an optional configuration file.

* A sample configuration file has ```-sample``` appended to the file name such as ```config-sample.json```.  Sample configuration files are not read by the server. A copy of them must be created without the ```-sample``` prefix for the server to run.

* Configuration should not be stored in OS environment variables.

## Web Services

### Interface

* All web request bodies should be JSON.

* JSON web service requests without the ```application/json``` content type are invalid.

* A web service response should always have the content type of ```application/json```.

* A web service should never return HTML.

* A web service response body should always be valid JSON, even if it is just an empty dictionary.

* A web service should never return a redirect (any HTTP status code within the ```300``` range).  Redirects are an HTML UX mechanic.

* Unless a web service is only consumed by a single website or by an internal server infrastructure, web services must support versioning and have a required version parameter.

* Web service URLs should be all lowercase and use hyphens to separate words.

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
   