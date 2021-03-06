# hetzner-api-client [![Build status](https://api.travis-ci.org/Radiergummi/hetzner-api-client.svg)](https://travis-ci.org/Radiergummi/hetzner-api-client)
A nodeJS client for the Hetzner Robot API  

*Please note: I am in no way affiliated with Hetzner Online GmbH. I am just a developer trying to ease his work with a flexible API client.*

## Installation
Install using NPM: 

````javascript
npm i --save hetzner-api-client
````

## Usage
First off, create a new API client instance:

````javascript
var APIClient = require('hetzner-api-client'),
    robot = new APIClient({
      username: '<your Robot username>',
      password: '<your Robot password>'
    });
````

The client is all ready now and you can start using its methods:

````javascript
robot.queryReverseDns('123.123.123.123').then(
  response => console.log(response),
  error => console.error(error)
);
````

Sample response for a valid IP address:

````json
{
  "rdns": {
    "ip": "123.123.123.123",
    "ptr": "sub.domain.tld"
  }
}
````

Sample response for an invalid IP address:

````
IP_NOT_FOUND: ip not found
````

Error messages are structured like so:

````
<ERROR_CODE>: <error message>
````

Oh. Did I mention the client is completely promise-based? It will always return a promise to your requests.

### Custom server proxies
Say you own one or two servers and want to execute multiple methods on them. Kind of enervating to have to specify the server IP each time, isn't it? Therefore, you can (since `0.0.5`) create a proxy:

````javascript
var thanos = robot.registerServer('123.123.123.123');

// see? no additional IP parameter necessary.
thanos.queryReverseDns().then( /* ... */ )
thanos.updateReverseDns('new.pointer.record.tld').then( /* ... */ )
````

## Current status
I'm still in the process of adding API methods. Currently, around 40% are implemented, some need a final naming scheme and if necessary, some more will be rewritten. So even if the current version works as is, you should not rely on this in production. Though of course you can speed this up by committing :)

## A word on tests
To start the tests, simply use `npm test`.  
Obviously many API methods are pretty destructive, so in order to be able to test everything thoroughly, I decided to implement a simple pseudo-Hetzner API webserver based on [restify](http://restify.com/). You can find it in the `test` folder as `apiServer.js`. It starts on port 8080 and tries to behave as exactly as possible as the original Hetzner API. That way, I can test the client safely and you can test the scripts you'll use it for. 
  This is a freaking load of additional work though, so I'd appreciate some help here.

## API Documentation
Official API docs can be found [here](https://wiki.hetzner.de/index.php/Robot_Webservice/en).
