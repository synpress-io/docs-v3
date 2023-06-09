---
description: >-
  Synpress can be used as a Playwright plugin. You can continue to write your
  regular Playwright logic but will need Synpress when you are interacting with
  MetaMask.
coverY: 0
---

# 🪐 Using with Playwright

[Synpress](https://github.com/Synthetixio/synpress) can be used as a plugin for [Playwright](https://playwright.dev/). In this tutorial, we will look into **how to set up Playwright and Synpress and write our first E2E test**.&#x20;

## Prerequisites

1. [General knowledge of the notion of E2E testing](https://katalon.com/resources-center/blog/end-to-end-e2e-testing)
2. Basic knowledge in Playwright  —  [Getting Started With Playwright](https://playwright.dev/docs/intro)

## Synpress & Playwright&#x20;

### [Install Synpress ](../getting-starting/installing-synpress.md)& Playwright

Add Synpress to your project dependencies.&#x20;

```bash
yarn add -D @synthetixio/synpress @playwright/test
```

### Add Playwright Config File&#x20;

Because we will use Synpress with Playwright, you will need to have `playwright.config.ts` file (It is a Playwright-specific file). You can find all the available options in the [Playwright Test Configuration Guide](https://playwright.dev/docs/test-configuration).&#x20;

```typescript
// playwright.config.ts
import { defineConfig } from "@playwright/test";

export default defineConfig({
  testDir: "./tests",
  timeout: 30 * 1000,
  expect: {
    timeout: 5000,
  },
  fullyParallel: true,
  retries: 0,
  workers: 1,
  reporter: "html",
  outputDir: "test-results",
});
```

### Install & Setup MetaMask&#x20;

Create a file and name it `fixtures.js`. The goal of this file:&#x20;

1. Create shared test context (multiple pages opened at the same time)&#x20;
2. Overwrite Playwright `test` and `expect`&#x20;
3. Install & Setup MetaMask&#x20;

All this is achieved using the code snippet provided below.&#x20;

```typescript
// fixtures.js
import { test as base, chromium, type BrowserContext } from "@playwright/test";
import { initialSetup } from "@synthetixio/synpress/commands/metamask";
import { prepareMetamask } from "@synthetixio/synpress/helpers";

export const test = base.extend<{
  context: BrowserContext;
}>({
  context: async ({}, use) => {
    // Required for synpress
    global.expect = expect;
    // Download metamask
    const metamaskPath = await prepareMetamask(
      process.env.METAMASK_VERSION || "10.25.0"
    );
    
    // Prepare browser args
    const browserArgs = [
      `--disable-extensions-except=${metamaskPath}`,
      `--load-extension=${metamaskPath}`,
      "--remote-debugging-port=9222",
    ];
    
    if (process.env.CI) {
      browserArgs.push("--disable-gpu");
    }
    
    if (process.env.HEADLESS_MODE) {
      browserArgs.push("--headless=new");
    }
    // launch browser
    const context = await chromium.launchPersistentContext("", {
      headless: false,
      args: browserArgs,
    });
    // Wait for Metamask window to be shown.
    await context.pages()[0].waitForTimeout(3000);
    // Setup metamask
    await initialSetup(chromium, {
      secretWordsOrPrivateKey:
        "test test test test test test test test test test test junk",
      network: "sepolia",
      password: "Tester@1234",
      enableAdvancedSettings: true,
    });
    await use(context);
  },
});

export const expect = test.expect;

```

#### Examples

{% embed url="https://github.com/neuodev/synpress-demo/blob/main/pw-fixtures.ts" %}
Example on how to run Synpress with Playwright
{% endembed %}

Note how we are defining which wallet & network should be used to run the tests. Take a look at [MetaMask Setup API](../synpress-api.md#setup-metamask) for more info.  &#x20;

<pre class="language-typescript"><code class="lang-typescript">// Setup metamask
<strong>await initialSetup(chromium, {
</strong>  secretWordsOrPrivateKey: "test test test test test test test test test test test junk",
  network: "sepolia",
  password: "Tester@1234",
  enableAdvancedSettings: true,
});
</code></pre>

### Write your first E2E test!!

All tests will be included in the `tests/` directory. You can still write regular Playwright logic and use the [Synpress MetaMask API](../synpress-api.md) when needed. Like in the example below :point\_down:



```typescript
// tests/example.test.js
import { test, expect } from "../fixtures";
import * as metamask from "@synthetixio/synpress/commands/metamask";

let sharedPage;

test.describe.configure({ mode: "serial" });

test.beforeAll(async ({ page }) => {
  sharedPage = page;
  // dApp URL 
  // Note: dApp should be running in the background
  page.goto("http://localhost:8080");
});

test.afterAll(async ({ context }) => {
  await context.close();
});

test("should connect to MetaMask and display wallet address", async () => {
  await expect(sharedPage.locator("#address")).toHaveText("Address: ??");
  await expect(sharedPage.locator("#connected")).toHaveText("Connected: NO");

  await sharedPage.locator("#connect-btn").click();
  // Use MetaMask API provided by Synpress 
  await metamask.acceptAccess();

  await expect(sharedPage.locator("#address")).toHaveText(
    "Address: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266"
  );
  await expect(sharedPage.locator("#connected")).toHaveText("Connected: YES");

  sharedPage.locator("#disconnect-btn").click();
  await expect(sharedPage.locator("#address")).toHaveText("Address: ??");
  await expect(sharedPage.locator("#connected")).toHaveText("Connected: NO");
});

```

Take a look at [**Synpress MetaMask API**](../synpress-api.md) to get to know all the available functionalities and how to use them. &#x20;

### Update `package.json`

```json5
"test:pw": "playwright test --browser chromium",
"test:e2e:pw": "start-server-and-test 'yarn start' http://localhost:8080 'yarn test:pw'"
```

the `test:pw` script will run the tests but you will need to have the dApp running in the background (from another terminal for example).&#x20;

Another way to do it is to use the [start-server-and-test](https://www.npmjs.com/package/start-server-and-test) package which will run the dApp and lunch the tests from the same terminal and will close the server once the tests are done.&#x20;

{% embed url="https://www.npmjs.com/package/start-server-and-test" %}
Starts server, waits for URL, then runs test command; when the tests end, shuts down server
{% endembed %}



I created [Synpress & Playwright demo](https://github.com/neuodev/synpress-demo/) on GitHub. It has all the essentials to run use Synpress with Playwright.&#x20;

{% embed url="https://github.com/neuodev/synpress-demo/" %}
Synpress & Playwright Demo
{% endembed %}

If you had successfully setup Synptess & Playwright, you should be able to see something like this&#x20;

<figure><img src="../.gitbook/assets/ezgif.com-video-to-gif (1).gif" alt=""><figcaption></figcaption></figure>

