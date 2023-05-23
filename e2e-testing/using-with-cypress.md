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

### Write Test file

All of your test files will display inside folder `cypress/e2e`

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 103455.jpg" alt=""><figcaption></figcaption></figure>

We will try to change the url website to [https://metamask.github.io/test-dapp](https://metamask.github.io/test-dapp)&#x20;

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

### Run test

```
yarn cypress:run
```

<figure><img src="../.gitbook/assets/Screenshot 2023-05-23 104618.jpg" alt=""><figcaption></figcaption></figure>
