[![build status](https://secure.travis-ci.org/victorjonsson/nodejs-dokimon.png)](http://travis-ci.org/victorjonsson/nodejs-dokimon)
# Dokimon

This node module is used to create automated acceptance tests. It's also a great alternative (or complement) to 
client test tools such as Selenium. You write your tests in a heartbeat and run them using the command line.

<strong>Example usages:</strong>

  - Verify that your online services is up and running
  - Verify that your RESTful API is responding and behaving as expected
  - Validate that your website generates the expected html code

## Getting started

### 1) Install node and npm
Take a look at http://nodejs.org if you don't already have node and npm installed since before

### 2) Create project directory and install dokimon
Create a directory where you find suitable, go to the directory in your shell interpreter and run 
``npm install -g dokimon``

### 4) Create test directory
You write your tests in files that has the extension .djs (more information about writing the tests below).

### 3) config.json
This file should be placed in the root of your project directory and contain a JSON file with
the properties "host", "verbose" and "testdir". 

  - <strong>host</strong> (String) —  Specifies against which host you're going to run the tests. 
  - <strong>verbose</strong> (Boolean) — Makes it possible to get a more verbose output when running the tests
  - <strong>testdir</strong> (String) — Relative path to the directory where you have your test scripts.
  This directory should be placed in the project directory.

It's possible to change all of these parameters on the fly when using the command line interface. It's also
possible to completely switch to another configuration file when using the command line interface. Dokimon 
will search for a file named config.json in the current working directory if not specified when running the tests.

<strong>Example config.json</strong>

```
{
  "host" : "api.myservice.com",
  "verbose" : false,
  "testdir" : "tests",
  "path" : ""
}
```

<strong>Example project layout</strong>

```
/Users/john/nodetests/
    tests/
      - myscript.djs
    - config.json
```

## Writing tests

You can have one or several script files in your test directory, each containing one or several tests. All
script files in the test directory should have the extension <em>.djs</em>. The script files is ordinary
node modules (http://howtonode.org/creating-custom-modules).

<strong>Basic example (myscript.djs)</strong>

```js
var dokimon = require('dokimon'),
    assert = require('assert');

var checkHomepage = new dokimon.Test(
  'HomePageIsRunning', 
  {url : '/'}, 
  function(res, body) {
    assert.equal(res.statusCode, 200, 'My website is not responding');
  }
);
      
module.exports = checkHomepage;
```

You can also export an array with tests. 

```js
var dokimon = require('dokimon'),
    assert = require('assert');

var checkHomepage = new dokimon.Test(
  'HomePageIsRunning'...
);

var checkSiteSearch = new dokimon.TestPostForm(
  'SearchIsWorking', 
  {
    url : '/search/',
    write : {s : 'hockey', sortby : 'date', sortorder : 'desc'}
  },
  function(res, body) {
    assert.equal(res.statusCode, 200, 'Search is down');
    // and some other assertions that validates expected search result
  }
);
  
module.exports = [checkHomepage,checkSiteSearch];
```

Assuming that I've written this code in a file located in my test directory (defined in config.json) and
that the file has <em>.djs</em> as extension I can now run my tests by calling `dokimon -r` in the project
directory. I can also choose to only run one of the tests by calling `dokimon -rs myscript HomePageIsRunning`

[Read more about writing tests here](https://github.com/victorjonsson/nodejs-dokimon/wiki/Writing-tests)

[Example, tests for a RESTful API](https://github.com/victorjonsson/nodejs-dokimon/wiki/Example:-RESTful-API)

[Example, tests for wordpress](https://github.com/victorjonsson/nodejs-dokimon/wiki/Example:-Wordpress)


## CLI
```
dokimon -r
```
Run all dokimon scripts in the test directory defined in config.json

```
dokimon -r api website
```
Run the scripts named api.djs and website.djs located in the test directory defined in config.json. You can 
also write the paths to the scripts, -r tests/api.djs tests/website.djs

```
dokimon -rs website HomePageIsRunning
```
Run the test named <em>HomePageIsRunning</em> that's located in the script website.djs located in the test
directory defined in config.json

```
dokimon -l
```
List all scripts (and their tests) that is located in the test directory, defined in config.json

```
dokimon -s website
```
List all available tests in the script website.djs

### Optional arguments

`-verbose` - Gives you a more verbose output when running the tests

`-config /Users/john/dokimon/production.json` - Use another config file than the one that is automatically loaded by dokimon

`-host stage.myservice.com` - Override the host defined in config.json

`-testdir /var/nodetests/` - Use another test directory than the one defined in config.json

