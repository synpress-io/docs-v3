---
description: Project structure for getting your first E2E tests running
---

# 🛸 Writing Your First E2E test

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

*
