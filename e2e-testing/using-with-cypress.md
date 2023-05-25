# 🌙 Using with Cypress

[Synpress](https://github.com/Synthetixio/synpress) can be used as a plugin for [Cypress](https://www.cypress.io/). In this tutorial, We will address the following&#x20;

1. [How to start testing a new project with Synpress + Cypress](using-with-cypress.md#how-to-start-testing-a-new-project-with-synpress-+-cypress)
2. Add Synpress for the existing Cypress setup.&#x20;

## Prerequisites

1. [General knowledge of the notion of E2E testing](https://katalon.com/resources-center/blog/end-to-end-e2e-testing)
2. Basic knowledge in Cypress  —  [Write your first test with Cypress](https://docs.cypress.io/guides/end-to-end-testing/writing-your-first-end-to-end-test#Write-your-first-test)&#x20;
3. \[optional] Basic knowledge is React

## How to start testing a new project with Synpress + Cypress

### 1. Install Synpress&#x20;

Add Synpress to your project dependencies.&#x20;

```bash
yarn add -D @synthetixio/synpress
```

### 2. Create Test Files and Directories

At the root of your project, create `cypress/e2e/example.cy.[js,ts]` and `cypress/support/e2e.[js,ts]`&#x20;



{% tabs %}
{% tab title="JavaScript" %}
```
/cypress/e2e/example.cy.js
/cypress/support/e2e.js
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-25 at 9.06.04 AM.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="TypeScript" %}
```typescript
/cypress/e2e/example.cy.ts
/cypress/support/e2e.ts
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-25 at 9.05.00 AM.png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

* `cypress/e2e/*` -> Where all of your E2E tests will live.
* `cypress/support/e2e.[js,ts]` -> Where you will import Synpress plugins.

#### Add Synpress to your support file

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// cypress/support/e2e.js
/// <reference types="Cypress" />

import "@synthetixio/synpress/support/index";
```
{% endtab %}

{% tab title="TypeScript" %}
```typescript
// cypress/support/e2e.ts
/// <reference types="Cypress" />

import "@synthetixio/synpress/support/index";
```
{% endtab %}
{% endtabs %}

For more information about [Writing and Organizing Tests in Cypress read this guide](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests) &#x20;

### 3. Create Synpress Config File&#x20;

Synpress config file is exactly the same as [Cypress config file](https://docs.cypress.io/guides/references/configuration) with some modifications. Add the config file to the project root directory.&#x20;

```typescript
// synpress.config.js
const { defineConfig } = require("cypress");
const setupNodeEvents = require("@synthetixio/synpress/plugins/index");
const supportFile = "cypress/support/e2e.ts";
const timeout = process.env.SYNDEBUG ? 9999999 : 30000;

module.exports = defineConfig({
  userAgent: "synpress",
  retries: {
    runMode: process.env.CI ? 1 : 0,
    openMode: 0,
  },
  fixturesFolder: "@synthetixio/synpress/fixtures",
  chromeWebSecurity: true,
  viewportWidth: 1920,
  viewportHeight: 1080,
  video: false,
  env: {
    coverage: false,
  },
  defaultCommandTimeout: timeout,
  pageLoadTimeout: timeout,
  requestTimeout: timeout,
  e2e: {
    testIsolation: false,
    setupNodeEvents,
    baseUrl: "http://127.0.0.1:8080/",
    specPattern: "cypress/e2e/**/*.{js,jsx,ts,tsx}",
    supportFile,
  },
});
```







```json5
// package.json  
"synpress:run": "env-cmd -f .env synpress run --configFile synpress.config.js",
"test:e2e": "start-server-and-test 'yarn dev' http://localhost:8080 'yarn synpress:run'"
```

###
