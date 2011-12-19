# Dokimon

This project is supposed to end up in a nodejs module used for writing and running automated tests of various kinds. 
It's fast and easy to write the tests and you run them using a command line interface.

<strong>Example usages:</strong>

  - Verify that your online services is up and running
  - Verfify that your RESTfull API is responding and behaving as expected
  - Validate that your website is spitting out expected html code 

## Getting started

### 1) Install node and npm
Take a look at http://nodejs.org if you don't already have node and npm installed since before

### 2) Create project directory and install dokimon
Create a directory where you find suitable, go to the directory in your shell interpreter and run 
``npm install dokimon``

### 4) Create test directory
You write your tests in files that has the extension .dokimon...

### 3) config.json
This file should be placed in the root of your project directory and contain a JSON object with 
the properties "host", "verbose" and "testdir". 

  - <strong>host</strong> (String) —  Specifies against which host you're going to run the tests. 
  - <strong>verbose</strong> (Boolean) — Makes it possible to get a more verbose output when running the tests
  - <strong>testdir</strong> (String) — Relative path to the directory where you have your test scripts.
  This directory should be placed in the project directory.

It's possible to change all of these paremeters on the fly when using the command line interface. It's also
possible to completely switch to another configuration file when using the command line interface. Dokimon 
will search for a file named config.json in current working directory if not specified when running the tests.

<strong>Example config.json</strong>

```
{
  "host" : "api.myservice.com",
  "verbose" : false,
  "testdir" : "tests"
}
```

<strong>Example project layout</strong>

```
/Users/john/nodetests/
    tests/
      - myscript.dokimon
    - config.json
```

## Writing tests

You can have one or several dokimon scripts, each containing one or several tests. All dokimon scripts should have
the extension <em>.dokimon</em> and be located in the test directory defined in config.json. The scripts is written as ordinary node modules (http://howtonode.org/creating-custom-modules). 

<strong>Basic example (myscript.dokimon)</strong>

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

Assuming that I've written this code in a file residing in my test directory (defined in config.json) and that the file has <em>.dokimon</em> as extension I can now run my tests by calling `dokimon -r` in the project directory. I can also choose to only run one the tests like `dokimon -rs myscript HomePageIsRunning`

[Read more about writing tests here](https://github.com/victorjonsson/dokimon/wiki/Writing-tests)

## CLI
```
dokimon -r
```
Run all dokimon scripts in the test directory defined in config.json

```
dokimon -r api website
```
Run the scripts named api.dokimon and website.dokimon residing in the test directory defined in config.json. You can 
also write the paths to the scripts, -r tests/api.dokimon tests/website.dokimon

```
dokimon -rs website HomePageIsRunning
```
Run the test named <em>HomePageIsRunning</em> that's located in the script website.dokimon residing in the test
directory defined in config.json

```
dokimon -l
```
List all scripts (and their tests) that is located in the test directory, defined in config.json

```
dokimon -s website
```
List all available tests in the script website.dokimon

### Optional arguments

`-verbose` - Gives you a more verbose output when running the tests

`-config /Users/john/dokimon/production.json` - Use another config file than the one that is automatically loaded by dokimon

`-host stage.myservice.com` - Override the host defined in config.json

`-testdir /var/nodetests/` - Use another test directory than the one defined in config.json

