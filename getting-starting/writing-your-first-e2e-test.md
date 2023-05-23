# Writing Your First E2E test

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

### Writing your first test

{% tabs %}
{% tab title="Project Structure" %}
Setting up with ESLint and TSConfig

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

```javascript
import '@synthetixio/synpress/support/index';
```

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
{% endtabs %}

{% tabs %}
{% tab title="Synpress Config" %}
In your tests folder add this `synpress.json`

```json
{
  "baseUrl": "http://localhost:4783",
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
{% endtab %}
{% endtabs %}
