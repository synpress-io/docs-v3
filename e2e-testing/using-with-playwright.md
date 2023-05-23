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

All test file available at `tests` folder

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 110445.jpg" alt=""><figcaption></figcaption></figure>

### Run test

```
yarn test
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 111622.jpg" alt=""><figcaption></figcaption></figure>
