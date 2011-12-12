# Dokimon

This project is supposed to end up in a nodejs module used for writing and running automated tests of various kinds. 
It's fast and easy to write the tests and you run them using a command line interface.

<strong>Example tests:</strong>

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
This file should contain a JSON object with the properties "host", "verbose" and "testdir". 

  - <strong>host</strong> (String) —  Specifies against which host you're going to run the tests. 
  - <strong>verbose</strong> (Boolean) — Makes it possible to get a more verbose output when running the tests
  - <strong>testdir</strong> (String) — Path to the directory where you have your test scripts

It's possible to change all of these paremeters on the fly when using the command line interface. It's also
possible to completely switch to another configuration file when using the command line interface. Dokimon 
will search for a file named config.json in current working directory if not specified when running the tests.

<strong>Example config.json</strong>

```
{
  "host" : "api.myservice.com",
  "verbose" : false,
  "testdir" : "./tests"
}

```

## Writing tests

Lorem te ipsum

## CLI
  - <strong>-r</strong> Run all test scripts residing in your test directory
  - <strong>-rs</strong> Run
  - <strong>-l</strong> Lorem
  - <strong>-s</strong> Lorem
