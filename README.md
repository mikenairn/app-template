Template App - Getting Started
===============

This project is intended to be a starting point for all future apps built on the [FeedHenry](http://www.feedhenry.com)
platform, it contains the basic app structure and [LESS](http://lesscss.org/) styles to speed up the building and theming of apps. Gruntjs is used for automating tasks, like running JShint and all app tests. Tests are in place for both client and cloud parts, and can be run individually from the terminal, or via a grunt task.

---

![ScreenShot](https://raw.github.com/Crosbie/app-template/master/screenshot.png)

---

Below is an outline of the template structure:

### client
This is the directory containing all of the client-side code for the app, and client side tests. It
contains the following files/folders:

+ **default** - The default client _package_.
    + **index.html** - The main HTML file of the client app.
    + **app.js** - The entry point into the app.

    + **app** - folder containing all models, views, controllers and templates for app.
    + **css** - folder containing compressed styles (do not edit styles.css as it is auto-generated by the LESS compiler).
    + **js** - folder containing global preferences, settings etc...
    + **less** - folder containing all LESS styles and mixins.
    + **lib** - folder containing all 3rd party libraries.

+ **test** - The _package_ containing all client-side tests. Mocha-PhantomJs is the test framework used. The index file within this package contains instructions on how to get your tests up and running.


### cloud
So far just contains a sample login function, setup so that it can easily tie into an external back-end system.
The cloud code also contains tests written in nodeunit.

---

## Development Requirements

To get started with local development, you must first clone this project into your own GIT repo.
Once this is done, create a GIT backed project in the Feedhenry Studio. 
Three things are now needed to move onto local development:
+ **App GUID** - found in the details screen of the app.
+ **Studio Domain** - the address at which the studio is (xxxx.feedhenry.com).
+ **FHC** - [Feedhenry Command Line tool](http://docs.feedhenry.com/v2/fhc_command_line_tool.html).


There are some node modules that must be installed before beginning development

+ **LESS** 				    - `sudo npm install -g less`
+ **Mocha-PhantomJS** 	    - `sudo npm install -g mocha-phantomjs`
+ **PhantomJS** 		    - `sudo npm install -g phantomjs`

To set up FHC local, you must do the following:

+ Target the studio domain where your app lives - `fhc target xxxx`
+ Login to said domain - `fhc login <user> <password>`
+ Initialize local project - `fhc initlocal <appGuid>`
+ Run fhc local - `fhc local <appGuid>`
+ Browse to 127.0.0.1:8000 you should see Hello World

---

## Grunt

The template has a basic Grunt setup, for linting and testing your client and cloud code. 
Follow this link to learn more about grunt:
[GruntJS](http://blog.strongloop.com/use-grunt-js-and-the-power-of-javascript-to-automating-repetitive-tasks/?goback=%2Egde_121615_member_249211688).
Below are the steps needed to setup your development environment to use Grunt also:

+ **Grunt Command Line**    - `sudo npm install -g grunt-cli`
+ **Grunt**                 - `sudo npm install grunt --save-dev`
+ **Grunt jshint**          - `sudo npm install grunt-contrib-jshint --save-dev`
+ **Grunt mocha-phantomjs** - `sudo npm install grunt-mocha-phantomjs --save-dev`
+ **Grunt nodeunit**        - `sudo npm install grunt-contrib-nodeunit --save-dev`
+ **Grunt grunt-bg-shell**  - `sudo npm install grunt-bg-shell --save-dev`

Once these steps have been completed, run `grunt` from the terminal. This will run JShint over your code,
then run all your client tests, followed by all your cloud tests.

---

## Testing

The testing frameworks used are mocha-phantomJS (client) and nodeunit (cloud). A test coverage tool can be used also to generate code coverage reports. Here are the steps to set up 'jscoverage' with the app template.

+ Clone jscoverage from github into an empty directory on your machine [LINK](http://blog.alexanderseville.com/post/25512014087/node-jscoverage-with-vows-js). And follow install instructions
+ Install json2htmlcov
    `sudo npm install -g json2htmlcov`
+ Instrument your code, from your client directory, run:
    `jscoverage default default-inst`
+ In your test package, create a symbolic link to the new 'default-inst' package
    `ln -s ../default-inst default`

Now you are ready to start generating coverage reports. As the reports are typically output in JSON format, we have an extra step to convert them to a _user-friendly_ html page

+ To create the JSON file, From your test package, run:
    `mocha-phantomjs -R json-cov index.html > report.json`
+ Them to convert the file to html:
    `cat report.json | json2htmlcov > report.html`

This will leave you will a report.html page that you can open in the broswer to show your code coverage % for all of your code! Simples! Now that you have put down the ground work, the next time you need to generate a coverage report, you can simply write `grunt coverage`. NB - if your tests contain any console.logs, the coverage report will fail to parse!

