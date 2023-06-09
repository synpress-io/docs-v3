---
description: >-
  Synpress can be used as a Cypress plugin. You can continue to write your
  regular Cypress logic but will need Synpress when you are interacting with
  MetaMask.
coverY: 0
---

# 🌙 Using with Cypress

[Synpress](https://github.com/Synthetixio/synpress) can be used as a plugin for [Cypress](https://www.cypress.io/). In this tutorial, We will address the following&#x20;

1. [How to start testing a new project with Synpress & Cypress](using-with-cypress.md#how-to-start-testing-a-new-project-with-synpress-+-cypress)
2. [Add Synpress to an existing Cypress setup](using-with-cypress.md#add-synpress-to-an-existing-cypress-setup)&#x20;

## Prerequisites

1. [General knowledge of the notion of E2E testing](https://katalon.com/resources-center/blog/end-to-end-e2e-testing)
2. Basic knowledge in Cypress  —  [Write your first test with Cypress](https://docs.cypress.io/guides/end-to-end-testing/writing-your-first-end-to-end-test#Write-your-first-test)&#x20;

## How To Start Testing A New Project With Synpress & Cypress

### [1. Install Synpress ](../getting-starting/installing-synpress.md)

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

For more information about [writing and organizing tests in Cypress read this guide ](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests)&#x20;

### 3. Create Synpress Config File&#x20;

Synpress config file is exactly the same as [Cypress config file](https://docs.cypress.io/guides/references/configuration) with some modifications. Add the config file to the project root directory.&#x20;

```typescript
// synpress.config.js
const { defineConfig } = require("cypress");
// import synpress node events
const setupNodeEvents = require("@synthetixio/synpress/plugins/index");
// Set timeout (in milliseconds) for Cypress & Synpress to wait before failing. 
// Note: big timeout can slow the tests down. Slow timeouts can cause the test to fail. 
// Read more about timeouts: https://docs.cypress.io/guides/references/configuration#Timeouts
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
    // Url for the test dApp
    baseUrl: "http://127.0.0.1:8080/",
    // Where all tests can be found. 
    specPattern: "cypress/e2e/**/*.{js,jsx,ts,tsx}",
    // Path for your support file your setup early
    supportFile: "cypress/support/e2e.ts",
  },
});
```

All Cypress configs are still valid in Synpress config file. If you want to know the rest of the options, feel free to go through the [configuration guide in Cypress docs](https://docs.cypress.io/guides/references/configuration).&#x20;

### 4. Add required environment variables

**Tip:** It is recommended to keep all E2E test environment variables in their own `.env` file to avoid [name collision](https://en.wikipedia.org/wiki/Name\_collision) and for separation of concerns.  This is easily achievable using [env-cmd](https://www.npmjs.com/package/env-cmd).

{% embed url="https://www.npmjs.com/package/env-cmd" %}
A simple node program for executing commands using an environment from an env file.
{% endembed %}

<pre class="language-bash"><code class="lang-bash"><strong># .env.e2e 
</strong><strong># The recovery phrase for the wallet that will be restored while preparing Metamask 
</strong><strong># Will be the selected wallet by default when connecting to the dApp 
</strong><strong>SECRET_WORDS='battle raccoon helmet please deliver keep kiss round orphan frame update message'
</strong><strong># Network info at which the tests will run. 
</strong>NETWORK_NAME='Mumbai'
CHAIN_ID=80001
RPC_URL='https://matic-mumbai.chainstacklabs.com'
SYMBOL="MATIC"
IS_TESTNET=true
</code></pre>

If you want to know the rest of the available environment variables, feel free to go through it [here](../environment-variables.md).&#x20;

### 5. Add Synpress run script&#x20;

Synpress has Command Line Interface (CLI) to help in launching & managing tests from the terminal. [Get to know Synpress CLI here](../synpress-cli.md).&#x20;

Below we are adding a new script to `package.json` . &#x20;

```json5
// package.json  
"synpress:run": "env-cmd -f .env.e2e synpress run --configFile synpress.config.js",
"test:e2e": "start-server-and-test 'yarn start' http://localhost:8080 'yarn synpress:run'"
```

* Notice how loading the required environment variables from `.env.e2e` file using [\`env-cmd\`](https://www.npmjs.com/package/env-cmd) package `env-cmd -f .env.e2e`.&#x20;
* Get to know all the available environment variables and their usage [here](../environment-variables.md).&#x20;
* Notice how are passing the path to the synpress config file we created earlier `--configFile synpress.config.js`.&#x20;
* Note: you will need to have your dApp running in the background or you can use `start-server-and-test` (mostly needed in the CI/CD). \


{% embed url="https://www.npmjs.com/package/start-server-and-test" %}
Starts server, waits for URL, then runs test command; when the tests end, shuts down server
{% endembed %}

### 6. Write your first test :tada:&#x20;

Below is a simple dApp ([synpress-demo](https://gitlab.com/AhmedIbrahim336/synpress-demo)) written in React + [WAGMI](https://wagmi.sh/) that I created on GitLab. Feel free to clone the repo and play around with it (don't forget to add the environment variables!!). &#x20;

<figure><img src="../.gitbook/assets/image (1).png" alt="Synpress dApp Demo"><figcaption><p>Synpress Demo</p></figcaption></figure>

Below is a test case example for connecting dApp to MetaMask. Note that almost all of the logic is regular Cypress! _**Synpress comes to play when the dApp is interacting with Metamask (e.g. connect, send Tx, signing...)**_

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// cypress/e2e/example.cy.js
describe("Synpress Demo", () => {
  it("should connect to MetaMask and display wallet address", () => {
    cy.visit("http://localhost:8080/");
    cy.get("#address").contains("??");
    cy.get("#connected").contains("NO");

    cy.get("#connect-btn").click();
    cy.acceptMetamaskAccess(); // <------ Synpress API 

    cy.get("#address").contains("0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266");
    cy.get("#connected").contains("YES");

    cy.get("#disconnect-btn").click();
    cy.get("#address").contains("??");
    cy.get("#connected").contains("NO");
  });
});
```
{% endtab %}

{% tab title="TypeScript" %}
```typescript
describe("Synpress Demo", () => {
  it("should connect to MetaMask and display wallet address", () => {
    cy.visit("http://localhost:8080/");
    cy.get("#address").contains("??");
    cy.get("#connected").contains("NO");

    cy.get("#connect-btn").click();
    cy.acceptMetamaskAccess(); // <------ Synpress API 

    cy.get("#address").contains("0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266");
    cy.get("#connected").contains("YES");

    cy.get("#disconnect-btn").click();
    cy.get("#address").contains("??");
    cy.get("#connected").contains("NO");
  });
});
```
{% endtab %}
{% endtabs %}

Get to know all that Synpress has to offer throughout the [Synpress API](../synpress-api.md)

## Add Synpress To An Existing Cypress Setup

### [1. Install Synpress ](../getting-starting/installing-synpress.md)

Add Synpress to your project dependencies.&#x20;

```bash
yarn add -D @synthetixio/synpress
```

### [2. Update your support file](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests#Support-file)

```typescript
import "@synthetixio/synpress/support/index";
```

### 3. Update `cypress.config.js`

You can simply import the Synpress plugin into `cypress.config.js` file as stated below.&#x20;

<pre class="language-diff"><code class="lang-diff"><strong>+ const setupNodeEvents = require("@synthetixio/synpress/plugins/index");
</strong>module.exports = defineConfig({
+ userAgent: "synpress",
  e2e: {
+   setupNodeEvents,
  },
});
</code></pre>

### [4. Add required environment variables](using-with-cypress.md#4.-add-required-environment-variables)

### [5. Add Synpress run script](using-with-cypress.md#5.-add-synpress-run-script)

### [6. Write your first test](using-with-cypress.md#6.-write-your-first-test) :tada:

## See also&#x20;

1. [Synpress Environment Variables](../environment-variables.md)&#x20;
2. [Synpress CLI ](../synpress-cli.md)
3. [Synpress API](../synpress-api.md)
4. [<mark style="background-color:orange;">End-to-end testing using Synpress</mark>](https://klaytn.foundation/synpress-setup-tutorial/)

:writing\_hand: [Edit this page](using-with-cypress.md)&#x20;

