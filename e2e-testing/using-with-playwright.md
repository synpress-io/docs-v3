# Using with Playwright

### Clone Example

```
git clone https://github.com/drptbl/synpress-examples.git
```

* [isolated-state](https://github.com/drptbl/synpress-examples/tree/master/playwright/isolated-state) => example setup of Playwright with synpress using isolated state meaning that each test is run in a separate browser context with fresh instance of metamask extension. Test isolation is preferred way of running tests, but it takes more time for setting up metamask extension before each test.
* [shared-state](https://github.com/drptbl/synpress-examples/tree/master/playwright/shared-state) => example setup of Playwright with synpress using shared state meaning that each test is run in same browser context and share same instance of metamask extension.
* [eslint](https://github.com/drptbl/synpress-examples/tree/master/playwright/eslint) => example setup of Playwright with synpress and eslint using isolated state.

```
cd synpress-examples/playwright/isolated-state
```

### Install&#x20;

```
yarn install
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 110303.jpg" alt=""><figcaption></figcaption></figure>

### Write Test file

All test file available at `tests/pages` folder

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 110445.jpg" alt=""><figcaption></figcaption></figure>

Take a look at the following example to see how to write a test.

```javascript
import { test, expect } from "../../fixtures";
import * as metamask from "@synthetixio/synpress/commands/metamask";


test.beforeEach(async ({ page }) => {
  await page.goto('https://metamask.github.io/test-dapp');
});

test.describe('connect wallet spec', () => {

  test.only('connect wallet', async ({ page }) => {
    // Navigate to sign-in page
    await page.getByRole('button', { name: 'Connect' }).click();
    await metamask.acceptAccess();
    const account = await page.getByText('Accounts:');
    await expect(account).toContainText("0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266");
  });

});
```

### Navigation

Most of the tests will start with navigating page to the URL. After that, test will be able to interact with the page elements.

```
await page.goto('https://metamask.github.io/test-dapp');
```

Synpress will wait for page to reach the load state prior to moving forward.

### Query for an element <a href="#step-2-query-for-an-element" id="step-2-query-for-an-element"></a>

Performing actions starts with locating the elements. To get one or more DOM elements by selector or alias, we'll use page.getByRole(). [Learn more about locator](https://playwright.dev/docs/api/class-locator)

<pre><code><strong>await page.getByRole('button', { name: 'Connect' });
</strong></code></pre>

### Interaction with element

Ok, now we want to click on the link we found. How do we do that? Add a [.click()](https://docs.cypress.io/api/commands/click) command to the end of the previous command, like so:

```
await page.getByRole('button', { name: 'Connect' }).click();
```

#### Basic actions[​](https://playwright.dev/docs/writing-tests#basic-actions) <a href="#basic-actions" id="basic-actions"></a>

This is the list of the most popular Playwright actions. Note that there are many more, so make sure to check the [Locator API](https://playwright.dev/docs/api/class-locator) section to learn more about them.

| Action                                                                                           | Description                             |
| ------------------------------------------------------------------------------------------------ | --------------------------------------- |
| [locator.check()](https://playwright.dev/docs/api/class-locator#locator-check)                   | Check the input checkbox                |
| [locator.click()](https://playwright.dev/docs/api/class-locator#locator-click)                   | Click the element                       |
| [locator.uncheck()](https://playwright.dev/docs/api/class-locator#locator-uncheck)               | Uncheck the input checkbox              |
| [locator.hover()](https://playwright.dev/docs/api/class-locator#locator-hover)                   | Hover mouse over the element            |
| [locator.fill()](https://playwright.dev/docs/api/class-locator#locator-fill)                     | Fill the form field (fast)              |
| [locator.focus()](https://playwright.dev/docs/api/class-locator#locator-focus)                   | Focus the element                       |
| [locator.press()](https://playwright.dev/docs/api/class-locator#locator-press)                   | Press single key                        |
| [locator.setInputFiles()](https://playwright.dev/docs/api/class-locator#locator-set-input-files) | Pick files to upload                    |
| [locator.selectOption()](https://playwright.dev/docs/api/class-locator#locator-select-option)    | Select option in the drop down          |
| [locator.type()](https://playwright.dev/docs/api/class-locator#locator-type)                     | Type text character by character (slow) |

### Interaction with metamask

Now, after we click on the 'Connect Wallet' button, we need to interact with MetaMask. In this case, we should choose 'Accept MetaMask Access.' We can accomplish this by using the command metamask.acceptAccess().&#x20;

```
await metamask.acceptAccess();
```

You can learn more about all MetaMask interaction command at [Synpress API](../synpress-api.md)



### Assertions <a href="#assertions" id="assertions"></a>

Let's make an assertion to make sure wallet was connected successfully to website. We can do that by looking up the account element and use expect(value) and choose a matcher that reflects the expectation

Here's what that looks like:

```
const account = await page.getByText('Accounts:');
await expect(account).toContainText("0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266");
```

Here is the list of the most popular async assertions. Note that there are [many more](https://playwright.dev/docs/test-assertions) to get familiar with:

| Assertion                                                                                                                         | Description                       |
| --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| [expect(locator).toBeChecked()](https://playwright.dev/docs/api/class-locatorassertions#locator-assertions-to-be-checked)         | Checkbox is checked               |
| [expect(locator).toBeEnabled()](https://playwright.dev/docs/api/class-locatorassertions#locator-assertions-to-be-enabled)         | Control is enabled                |
| [expect(locator).toBeVisible()](https://playwright.dev/docs/api/class-locatorassertions#locator-assertions-to-be-visible)         | Element is visible                |
| [expect(locator).toContainText()](https://playwright.dev/docs/api/class-locatorassertions#locator-assertions-to-contain-text)     | Element contains text             |
| [expect(locator).toHaveAttribute()](https://playwright.dev/docs/api/class-locatorassertions#locator-assertions-to-have-attribute) | Element has attribute             |
| [expect(locator).toHaveCount()](https://playwright.dev/docs/api/class-locatorassertions#locator-assertions-to-have-count)         | List of elements has given length |
| [expect(locator).toHaveText()](https://playwright.dev/docs/api/class-locatorassertions#locator-assertions-to-have-text)           | Element matches text              |
| [expect(locator).toHaveValue()](https://playwright.dev/docs/api/class-locatorassertions#locator-assertions-to-have-value)         | Input element has value           |
| [expect(page).toHaveTitle()](https://playwright.dev/docs/api/class-pageassertions#page-assertions-to-have-title)                  | Page has title                    |
| [expect(page).toHaveURL()](https://playwright.dev/docs/api/class-pageassertions#page-assertions-to-have-url)                      | Page has URL                      |
| [expect(page).toHaveScreenshot()](https://playwright.dev/docs/api/class-pageassertions#page-assertions-to-have-screenshot-1)      | Page has screenshot               |

#### Test Isolation[​](https://playwright.dev/docs/writing-tests#test-isolation) <a href="#test-isolation" id="test-isolation"></a>

Playwright Test is based on the concept of [test fixtures](https://playwright.dev/docs/test-fixtures) such as the [built in page fixture](https://playwright.dev/docs/test-fixtures#built-in-fixtures), which is passed into your test. Pages are isolated between tests due to the Browser Context, which is equivalent to a brand new browser profile, where every test gets a fresh environment, even when multiple tests run in a single Browser.

tests/example.spec.ts

```
test('basic test', async ({ page }) => {
  ...
```

#### Using Test Hooks[​](https://playwright.dev/docs/writing-tests#using-test-hooks) <a href="#using-test-hooks" id="using-test-hooks"></a>

You can use various [test hooks](https://playwright.dev/docs/api/class-test) such as `test.describe` to declare a group of tests and `test.beforeEach` and `test.afterEach` which are executed before/after each test. Other hooks include the `test.beforeAll` and `test.afterAll` which are executed once per worker before/after all tests.

tests/example.spec.ts

```javascript
import { test, expect } from "../../fixtures";
import * as metamask from "@synthetixio/synpress/commands/metamask";


test.beforeEach(async ({ page }) => {
  await page.goto('https://metamask.github.io/test-dapp');
});

test.describe('connect wallet spec', () => {

  test.only('connect wallet', async ({ page }) => {
    // Navigate to sign-in page
    await page.getByRole('button', { name: 'Connect' }).click();
    await metamask.acceptAccess();
    const account = await page.getByText('Accounts:');
    await expect(account).toContainText("0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266");
  });

});
```

### Run test

```
yarn test
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 111622.jpg" alt=""><figcaption></figcaption></figure>

### Report&#x20;

After all test files have run, you will be able to see a test report that contains all the steps and results. You can also using command

```
npx playwright show-report
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-25 113001.jpg" alt=""><figcaption></figcaption></figure>
