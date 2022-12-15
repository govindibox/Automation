# Automation

- Make sure sure nodejs already installed in the system by enter following node version command in command prompt
```sh
		>>node -v	
```
    When its show version number, then nodejs already installed in the system, otherwise follow the steps
- Download nodejs application from following url and install
```sh
		https://nodejs.org/download/release/v16.15.1/node-v16.15.1-x64.msi
```
    No need to change any options while install, just click "next" button in all window and finally "finish".
- Now open new command prompt and enter following command to check installed node version
```sh
		>>node -v
```
	  Its output like "v16.15.1"
- Navigate to working directory [Example : E:\AutomationTesting\test-case-automation]
- Run following command and enter details of project (or just press "enter" key to set default/empty value)
```sh
	>>npm init
```
- Run following command to install cypress package in our project
```sh
	>>npm install cypress@10.11.0 --save-dev
```
- After install, open "E:\AutomationTesting\test-case-automation\package.json" and update the line as follows
```sh
	"test": "cypress open"
```
- Run following command to create cypress folders
```sh
	>>npm run test  
```
  If any error produced like `"Error loading V8 startup snapshot file"` while start cypress, then run following command to reinstall cypress
```sh
	>>.\node_modules\.bin\cypress.cmd install --force
```
- Make sure "cypress" folder created in project root folder
- Run the following command to install Cucumber
```sh
  >>npm install -D @badeball/cypress-cucumber-preprocessor
  >>npm install -D @bahmutov/cypress-esbuild-preprocessor
```
- Add below lines in `cypress.config.js` for cucumber preprocessor
```sh
const { defineConfig } = require("cypress");
const createBundler = require("@bahmutov/cypress-esbuild-preprocessor");
const addCucumberPreprocessorPlugin =
require("@badeball/cypress-cucumber-preprocessor").addCucumberPreprocessorPlugin;
const createEsbuildPlugin =
require("@badeball/cypress-cucumber-preprocessor/esbuild").createEsbuildPlugin;
module.exports = defineConfig({
	e2e: {
		async setupNodeEvents(on, config) {
			const bundler = createBundler({
				plugins: [createEsbuildPlugin(config)],
			});

			on("file:preprocessor", bundler);
			await addCucumberPreprocessorPlugin(on, config);

			return config;
		},
		specPattern: "cypress/e2e/**/*.feature",
	},
});
```
- Create a new file named `.cypress-cucumber-preprocessorrc.json` in root directory and add following lines
```sh
{
	"stepDefinitions": [
		"cypress/stepDefinitions/*.js"
	]
}
```
- Create feature file in below path
```sh
		cypress\e2e\features\login.feature
```
- Add following line of code in "login.feature"	
```sh
		Feature: Login
			Scenario: Login user with correct email and password
				Given I navigate to automation exercise website
				When I enter login credentials
				Then I should be logged in
```
- Create steps file in below path
```sh
	cypress\stepDefinitions\loginSteps.js
```
- Add following line of code in "loginSteps.js"
```sh
	import { Given, When, Then } from "@badeball/cypress-cucumber-preprocessor";
	Given("I navigate to automation exercise website", () => {
	  cy.visit("https://automationexercise.com/");
	});
	When("I enter login credentials", () => {
	  cy.get("a[href='/login']").click();
	  cy.contains("Login to your account").should("be.visible");
	  cy.get("input[data-qa='login-email']").type("example1@test.com");
	  cy.get("input[data-qa='login-password']").type("foobar");
	  cy.get("button[data-qa='login-button']").click();
	});
	Then("I should be logged in", () => {
	  cy.contains(" Logged in as ").should("be.visible");
	});
```	
- Run following command to test
```sh
	>>npm run test 
```

- Run following command to install typescript package and initialize in our project
```sh
	>>npm install  typescript --save-dev
	>>npx tsc --init --types cypress --lib dom,es6
```
- Update the following line in `.cypress-cucumber-preprocessorrc.json` in root directory
```sh
{
	"stepDefinitions": [		
		"cypress/stepDefinitions/*.js",
		"cypress/stepDefinitions/*.ts"
	]
}
```
