# Nightwatch
## Project Setup
Fit a folder structure like this into your project
```
├── bin
│   ├── chromedriver
│   └── selenium-server-standalone-2.45.0.jar
├── nightwatch.js
├── nightwatch.json
├── package.json
├── readme.md
└── tests
    └── exampleTest.js
```
You can grab selenium [here](http://www.seleniumhq.org/download/) & the chromedriver [here](http://chromedriver.storage.googleapis.com/index.html?path=2.15/) and make the chromedriver executable
```
chmod 767 bin/chromedriver
```
## Config
Copy this setup into `nightwatch.json`
```
{
  "src_folders" : ["./tests"],
  "output_folder" : "./tests/reports",
  "custom_commands_path" : "",
  "custom_assertions_path" : "",
  "page_objects_path" : "",
  "globals_path" : "",

  "selenium" : {
    "start_process" : true,
    "server_path" : "./bin/selenium-server-standalone-2.45.0.jar",
    "log_path" : false,
    "host" : "127.0.0.1",
    "port" : 4444,
    "cli_args" : {
      "webdriver.chrome.driver" : "./bin/chromedriver",
      "webdriver.ie.driver" : ""
    }
  },

  "test_settings" : {
    "default" : {
      "launch_url" : "http://localhost",
      "selenium_port"  : 4444,
      "selenium_host"  : "localhost",
      "silent": true,
      "screenshots" : {
        "enabled" : false,
        "path" : ""
      },
      "desiredCapabilities": {
        "browserName": "chrome",
        "javascriptEnabled": true,
        "acceptSslCerts": true
      }
    }
  }
}
```
and this into `nightwatch.js`
```
#!/usr/bin/env node
require('nightwatch/bin/runner.js')
```
So now you can add this config to the package.json file
```
"scripts": {
    "test": "./nightwatch.js --test tests/*.js"
  }
```
Now you can run tests with `npm test` here is an inital test to past into `exampleTest.js`
```
module.exports = {
  'Demo test Google' : function (client) {
    client
      .url('http://www.google.com')
      .waitForElementVisible('body', 1000)
      .assert.title('Google')
      .assert.visible('input[type=text]')
      .setValue('input[type=text]', 'rembrandt van rijn')
      .waitForElementVisible('button[name=btnG]', 1000)
      .click('button[name=btnG]')
      .pause(1000)
      .assert.containsText('ol#rso li:first-child',
        'Rembrandt - Wikipedia')
      .end();
  }
};
```