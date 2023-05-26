---
description: Project structure for getting your first E2E tests running
---

# Writing Your First E2E test

### Writing your first test

{% tabs %}
{% tab title="ES-Lint + TS" %}
Setting up with ESLint and TSConfig\
\
Project Structure:

```
project_dir
└── src
└── tests
    └── e2e
        └── .eslintrc.js
        └── support.js
        └── tsconfig.json
        └── specs
            └── example-spec.js
        └── pages
            └── example-page.js
```

#### 1. In your test folder create `.eslintrc.js`

```javascript
const path = require('path');
const synpressPath = path.join(
  process.cwd(),
  '/node_modules/@synthetixio/synpress',
);

module.exports = {
  extends: `${synpressPath}/.eslintrc.js`,
};
```

#### 2. In the same folder create `support.js`

<pre class="language-javascript"><code class="lang-javascript"><strong>import '@synthetixio/synpress/support/index';
</strong></code></pre>

#### 3. Finally create `tsconfig.json`

```json
{
  "compilerOptions": {
    "allowJs": true,
    "baseUrl": "../../node_modules",
    "types": [
      "cypress",
      "@synthetixio/synpress/support",
      "cypress-wait-until",
      "@testing-library/cypress"
    ],
    "outDir": "./output"
  },
  "include": ["**/*.*"]
}
```
{% endtab %}

{% tab title="Vanilla JS" %}
Project Structure:

```
project_dir
└── src
└── tests
    └── e2e
        └── .eslintrc.js
        └── support.js
        └── specs
            └── example-spec.js
        └── pages
            └── example-page.js
```

### 1. Add support.js

```javascript
import '@synthetixio/synpress/support/index';
```

### 2. Add `.eslintrc.js`

```javascript
const path = require('path');
const synpressPath = path.join(
  process.cwd(),
  '/node_modules/@synthetixio/synpress',
);

module.exports = {
  extends: `${synpressPath}/.eslintrc.js`,
};
```


{% endtab %}
{% endtabs %}

### Your first test:

{% tabs %}
{% tab title="Vanilla TS/JS" %}
1. Create a file `connect-wallet-spec.js`\
   Then add the following:
2. ```javascript
   describe('connect wallet spec', () => {
     before(() => {
       cy.visit('/');
     });

     it('should connect wallet with success', () => {
       cy.get('#connectButton').click();
       cy.acceptMetamaskAccess();
       cy.get('#accounts').should(
         'have.text',
         '0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266',
       );
     });

     it('import private key and connect wallet using imported metamask account', () => {
       cy.disconnectMetamaskWalletFromAllDapps();
       cy.importMetamaskAccount(
         '0xdbda1821b80551c9d65939329250298aa3472ba22feea921c0cf5d620ea67b97',
       );
       cy.get('#connectButton').click();
       cy.acceptMetamaskAccess();
       cy.get('#accounts').should(
         'have.text',
         '0x23618e81e3f5cdf7f54c3d65f7fbc0abf5b21e8f',
       );
     });
   });
   ```
{% endtab %}

{% tab title="Playwright - Isolated State" %}
### 1.&#x20;


{% endtab %}

{% tab title="Playwright - Shared State" %}

{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Synpress Config" %}
In your tests folder add this `synpress.json`

```json
{
  "baseUrl": "http://localhost:3000",
  "userAgent": "synpress",
  "retries": { "runMode": 0, "openMode": 0 },
  "integrationFolder": "integration",
  "screenshotsFolder": "screenshots",
  "videosFolder": "videos",
  "video": true,
  "chromeWebSecurity": true,
  "viewportWidth": 1366,
  "viewportHeight": 850,
  "component": {
    "componentFolder": ".",
    "testFiles": "**/*spec.{js,jsx,ts,tsx}"
  },
  "env": {
    "coverage": false
  },
  "defaultCommandTimeout": 30000,
  "pageLoadTimeout": 30000,
  "requestTimeout": 30000,
  "supportFile": "support/index.js"
}
```

| Key       | Value                                                         |
| --------- | ------------------------------------------------------------- |
| `baseUrl` | Whatever URL you will be testing on, typically localhost:3000 |
{% endtab %}
{% endtabs %}

### Synpress Testing Framework

* Supported Frameworks:
  * Playwright
  * Cypress
  * Synpress
* Wallet Integration:
  * Synpress seamlessly supports Metamask wallets, enabling simulation of user interactions with web3 wallets during testing.
* Available Networks:
  * Out of the box, Synpress provides support for the following networks:
    * Mainnet
    * Goerli
    * Sepolia
    * Localhost
* Custom Network Extension:
  * Synpress can be easily extended to support custom networks, allowing developers to tailor the testing environment to their specific requirements.
