# Package Changelog

## v. 0.0.14
    - First beta release

## v. 0.0.15
    - Moved callback into a class function to make it possible for extending class to override the execute() function

## v 0.0.16
    - Added comments
    - Added some verbose output
    - Possible to use cookies over several requests (keeping a server session)
    - TestPostForm now works as supposed

## v 0.0.17
    - Added comments
    - Now following redirects
    - Fixed bug, callback function did not run on request error
    - Fixed bug, not possible to use the same cookies for several tests

## v 0.0.18
    - Extension of test files changed from .dokimon to .djs
    - minor bug fixes

## v 0.0.19
    - Messages about failing tests is now sent to console.error
    - Added some coloring of messages

## v 0.1.0
    - New feature: tests can now be dependent on each other

## v 0.1.3
    - Some minor bugs fixed

## v 0.1.4
    - Now possible to define a base path to run the tests against