# ui-lab

Automated UI testing lab for ITDS362, Software Quality Assurance and Testing.

The tool parts that I chose are Part 3, Robot Framework, and Part 4, Cypress.
Both automate the same written test case, QP-01 "A custom pizza reaches the
admin list", on https://quickpizza.grafana.com, and both also test the web form
at https://www.selenium.dev/selenium/web/web-form.html.

The results of every step and the answers of Part B are in
[RESULTS.md](RESULTS.md).

## robot (Part 3, Robot Framework)

This folder holds the Robot Framework tests `first.robot`, `web_form.robot`
and `pizza_scenario.robot`, and the keywords and locators of QuickPizza in
`resources/quickpizza.resource`. The folder `results` holds the logs, reports
and screenshots of two full runs of QP-01.

Set up once, from the `robot` folder. The tests read the path of ChromeDriver
from the environment variable `CHROMEDRIVER`.

```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Run the tests from the `robot` folder.

```
robot first.robot web_form.robot pizza_scenario.robot
```

## cypress (Part 4, Cypress)

This folder holds the Cypress project. The specs `web-form.cy.js` and
`pizza-scenario.cy.js` are in `cypress/e2e`, and the custom command
`adminLogin` is in `cypress/support/commands.js`. The folders `cypress/videos`
and `cypress/screenshots` hold the evidence of a full run.

Set up once, from the `cypress` folder.

```
npm install
```

Run the tests from the `cypress` folder.

```
npx cypress run --browser chrome
```
