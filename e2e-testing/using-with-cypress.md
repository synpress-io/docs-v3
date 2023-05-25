# Using with Cypress

### Clone Example

```
git clone https://github.com/drptbl/synpress-examples.git
```

* [isolated-state](https://github.com/drptbl/synpress-examples/tree/master/cypress/isolated-state) => example setup of Cypress with synpress using isolated state meaning that each test is run on fresh instance of tested website, but in same browser (works differently than Playwright which uses new browser context). Metamask extension state is shared all the time. Test isolation is preferred way of running tests.
* [shared-state](https://github.com/drptbl/synpress-examples/tree/master/cypress/shared-state) => example setup of Cypress with synpress using shared state meaning that each test is run on same instance of tested website. Metamask extension state is also shared all the time.

```
cd synpress-examples/cypress/isolated-state
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 103026 (1).jpg" alt=""><figcaption></figcaption></figure>

### Install package

```
yarn install
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 103611.jpg" alt=""><figcaption></figcaption></figure>

### Config .env file

Config Environment Variables at .env file.

You can check list of Environment Variables can be config at [Environment Variables](../environment-variables.md)

```
NETWORK_NAME=sepolia
SECRET_WORDS='test test test test test test test test test test test junk'
```

### First Test

All of your test files will display inside folder `cypress/e2e`

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 103455.jpg" alt=""><figcaption></figcaption></figure>

Take a look at the following example to see how to write a test.

```javascript
describe("connect wallet spec", () => {
  beforeEach(() => {
    cy.visit("https://metamask.github.io/test-dapp");
  });

  afterEach(() => {
    cy.disconnectMetamaskWalletFromAllDapps();
    cy.resetMetamaskAccount();
  });

  it("should connect wallet with success", () => {
    cy.get("#connectButton").click();
    cy.acceptMetamaskAccess();
    cy.get("#accounts").should(
      "have.text",
      "0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266"
    );
  });

  it("import private key and connect wallet using imported metamask account", () => {
    cy.importMetamaskAccount(
      "0xdbda1821b80551c9d65939329250298aa3472ba22feea921c0cf5d620ea67b97"
    );
    cy.get("#connectButton").click();
    cy.acceptMetamaskAccess();
    cy.get("#accounts").should(
      "have.text",
      "0x23618e81e3f5cdf7f54c3d65f7fbc0abf5b21e8f"
    );
  });
});
```

### Navigation <a href="#navigation" id="navigation"></a>

Most of the tests will start with navigating page to the URL. After that, test will be able to interact with the page elements.

```
await cy.visit('https://metamask.github.io/test-dapp');
```

Synpress will wait for page to reach the load state prior to moving forward.

### Query for an element <a href="#step-2-query-for-an-element" id="step-2-query-for-an-element"></a>

Performing actions starts with locating the elements. To get one or more DOM elements by selector or alias, we'll use cy.get()[^1]. [Learn more](https://docs.cypress.io/api/commands/get)

```
cy.get("#connectButton");
```

### Interaction with element

Ok, now we want to click on the link we found. How do we do that? Add a [.click()](https://docs.cypress.io/api/commands/click) command to the end of the previous command, like so:

```
cy.get("#connectButton").click();
```

### Interaction with metamask

Now, after we click on the 'Connect Wallet' button, we need to interact with MetaMask. In this case, we should choose 'Accept MetaMask Access.' We can accomplish this by using the command cy.acceptMetamaskAccess().&#x20;

```
cy.acceptMetamaskAccess();
```

You can learn more about all MetaMask interaction command at [Synpress API](../synpress-api.md)

### Assertions <a href="#assertions" id="assertions"></a>

Let's make an assertion to make sure wallet was connected successfully to website. We can do that by looking up the account element and chaining an assertion to it with [.should()](https://docs.cypress.io/api/commands/should).

Here's what that looks like:

```
cy.get("#accounts").should(
      "have.text",
      "0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266"
);
```

### Run test

```
yarn cypress:run
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 104618.jpg" alt=""><figcaption></figcaption></figure>

[^1]: 
