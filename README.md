# ui-lab

ITDS362 lab on automated UI testing. I chose Part 3 (Robot Framework) and Part 4 (Cypress). My results and the answers to Part B are in RESULTS.md.

## robot

Robot Framework tests with SeleniumLibrary. first.robot and web_form.robot test Selenium's web form, pizza_scenario.robot runs test case QP-01 on QuickPizza with the keywords in resources/quickpizza.resource, and results/run1 and results/run2 hold the log, report and screenshots of two full runs.

Run from the robot folder with the virtual environment active and CHROMEDRIVER set:

```
robot .
```

## cypress

Cypress project. cypress/e2e/web-form.cy.js tests the web form and cypress/e2e/pizza-scenario.cy.js runs QP-01 with the adminLogin command from cypress/support/commands.js, and cypress/videos and cypress/screenshots are from the last full run.

Run from the cypress folder after npm install:

```
npx cypress run --browser chrome
```
