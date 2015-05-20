[![Project Status: Active - The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/0.1.0/active.svg)](http://www.repostatus.org/#active)

OWASP Hackademic Challenges project
===================================

The **OWASP Hackademic Challenge** project helps you test your knowledge on web application security. You can use it to actually attack web applications in a realistic but also controllable and safe environment.

The latest production version is the `next` branch, the development version is the `next-dev` branch.


Description
-----------

The Hackademic challenges implement realistic scenarios iwth known vulnerabilities in a safe, controllable environment. Users can attempt to discover and exploit these vulnerabilities in order to learn important concepts of information security through the attacker's perspective.

Currently, there are 10 web application security scenarios available.

You can choose to start from the one that you find most appealing, although we suggest to follow the order presented on the first page. We intend to expand the available challenges with additional scenarios that involve cryptography  and even vulnerable systems implemented in download-able virtual machines.




Deployment
----------

The project can be easily deployed to any PHP-capable web server running MySQL/MariaDB. 
Just clone the project somewhere in your websites folder (In most linux systems that would be `/var/www/`) make sure the folder is writable by the webserver, open your browser and visit 
`http://127.0.0.1/<the path to your hackademic installation>/`

Road Map and Getting Involved
-----------------------------

We maintain an up to date list of open issues on the platform on our [issues](https://github.com/Hackademic/hackademic/issues)

For a list of features we would like implemented you can see either the issues page or our [Google Summer Of Code ideas page](https://www.owasp.org/index.php/GSoC2013_Ideas#OWASP_Hackademic_Challenges_-_New_challenges_and_Improvements_to_the_existing_ones)

Involvement in the development and promotion of the Hackademic Challenges is actively encouraged!
You do not have to be a security expert in order to contribute.
Some of the ways you can help:

* Write Documentation
* Write Unit tests
* Develop themes and plugins
* Write Challenges or Articles or contribute security courses


Tips for developers
-------------------

Hackademic is a young project, however, it's gaining momentum fast. If you are interested in contributing you should follow some basic guidelines.

* The latest development version is the `next-dev` branch, you should check it out and make all your pull requests there.
* We would really appreciate it if your new features came with unit tests. If you don't know how ask us.
* The latest development version is the `next-dev` branch, you should check it out and make all your pull requests there.
* Unit Tests: We are trying to introduce unit tests which is usually a very good idea. So:
  * New features should come with php-unit or Selenium (or better both)  unit tests testing that the feature works as expected
  * All the code should pass the existing unit tests before merging
  * If you have to modify a test make sure that nothing else breaks
* Coding - Style:
A couple of years ago we decided to adopt the Drupal coding style, so with some lag it's a very good idea to do that.
  * Every commit shouldn't generate any errors with PHP_CodeSniffer (to fix most errors you can use PHP Coding Standards Fixer )
* Commits: Clean commits make it easy to review code, also commits that do only one thing are easier to manage bug-wise. So:
  * One commit should fix one problem or introduce one feature only, please don't commit fixes all around the place.
  * Commits should be atomic, each commit should work by itself
  * Only what you modified goes in the commit (for instance you shouldn't commit vim/project files or anything unrelated)
  * The commit message should explain what you did briefly
  * Every commit should merge with `next-dev` without conflicts (rebase often)
* Pull Requests
  * All pull requests should be for the `next-dev` branch.
  * In case you found an important bug in a previous version and you think people running the version should get the fix then issue a pull request for that specific version. (It should still merge without any conflicts though)

Translations
------------
We're using gettext for translations coupled with the apropriate smarty plugin.
If you'd like to provide a tanslation you can look at the locale directory under your desired language.
The project's language is changed either on install or by setting the correct value in the config file.

For devs:
Handy tutorial on gettext:
`http://www.sitepoint.com/localizing-php-applications-1/`

The smarty plugin:
`https://github.com/smarty-gettext/smarty-gettext`

2 line instructions:
The settings are loaded in the master controller,
In the templates include whatever you want to be translated in {}.

Contact Us
----------

If you have any questions or would like to chat 
you can find us on the #hackademic-dev channel on Freenode.
Also our mailing list is owasp-hackademic-challenges@lists.owasp.org
you can join [here](https://lists.owasp.org/mailman/listinfo/owasp-hackademic-challenges).

Both channels are very low traffic.


How to create a challenge
-------------------------

Hackademic challenges are simple websites or web applications that simulate a vulnerability.
In our current version we use regular expressions to check if the provided string is correct.
In order to initialize your challenge you need to include the following.

```php
include_once dirname(__FILE__).'/../../init.php';
session_start();
require_once(HACKADEMIC_PATH."pages/challenge_monitor.php");
$monitor->update(CHALLENGE_INIT, $_GET);
$_SESSION['init'] = true;
```

Then in order to register a success you call
```php
$monitor->update(CHALLENGE_SUCCESS, $_GET);
```
and for a failure 
```php
$monitor->update(CHALLENGE_FAILURE, $_GET);
```


Packaging Challenges
-----------------------------

In order to package a challenge you have to create an XML file named after the challenge. The `.xml` file should have the structure found here:
`https://github.com/Hackademic/hackademic/blob/next/challenges/Example/example.xml`

Then you package everything in a zip file and ship it.


Frontend testing Documentation and how to create a test
-------------------------------------------------------

We use phpunit and Selenium webDriver with Facebook's php bindings to create functional tests.

We've included everything as a composer dependency so you only need to download composer and run
```
composer install
```
in order to fetch the dependencies.


Running tests
-------------

To run the tests you first need to start the remote webdriver server.
For convenience a reasonably recent server executable is included with the project.
You can start the server by running:
```
java -jar selenium-server-standalone-2.45.0.jar
```
from the tests directory.
This will start a local webdriver server that listens on localhost and port 4444.
Then you execute tests by running:
```
vendor/bin/phpunit <path_to_your_test>
```


Writing your own tests:
-----------------------

To write your own tests you can check `<tests_dir>/admin/model/AddUserControllerTest.php` for an example a `BaseTest` class with some helper functions is provided.


General guilines
----------------
(from a guy with limited experience on writing tests so suggestions are welcome):

0. Test for normal behavior first
1. Test if it generates all the erros second
3. Identify the edge cases and write tests for them
4. In order to get a unique css path for the element you want to click you can inspect it in Firefox or Chrome and right-click on the element and select Copy CSS Path
5. Cleanup after your test, `tearDown()` exists for that reason too.
6. Each class should test one feature. E.g. `AddUserTest` should test if a user can be added sucessfully.
