---
description: List of all the API that Synpress exposes and can be used from Cypress
---

# Cypress API

## **`initPlaywright`**

Connect Playwright with Cypress instance.

#### Syntax&#x20;

```typescript
function initPlaywright(): Chainable<boolean>;
```

#### Example

```javascript
cy.initPlaywright();
```

## **`assignWindows`**

Assign currently open tabs with Playwright.

#### Syntax&#x20;

```typescript
function assignWindows(): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.assignWindows();
```

## **`assignActiveTabName`**

Assigns currently active tab.

#### Syntax&#x20;

```typescript
function assignActiveTabName(tabName: string): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.assignActiveTabName("my-tab"); 
```

## **`isMetamaskWindowActive`**

Checks if the currently active window is Metamask.

#### Syntax

```typescript
function isMetamaskWindowActive(): Chainable<boolean>; 
```

#### Example

```javascript
cy.isMetamaskWindowActive();
```

## **`isCypressWindowActive`**

Checks if the currently active window is Cypress.

#### Syntax

```typescript
function isCypressWindowActive(): Chainable<boolean>;
```

#### **Example**

```javascript
cy.isCypressWindowActive();
```

## **`switchToCypressWindow`**

#### **Syntax**

```typescript
function switchToCypressWindow(): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.switchToCypressWindow();
```

## **`switchToMetamaskWindow`**

#### **Syntax**

```typescript
function switchToMetamaskWindow(): Chainable<boolean>;
```

#### Example&#x20;

```typescript
cy.switchToMetamaskWindow();
```

## **`switchToMetamaskNotification`**

#### Syntax&#x20;

```typescript
function switchToMetamaskNotification(): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.switchToMetamaskNotification();
```

## **`getNetwork`**

Get current network info&#x20;

#### Syntax&#x20;

```typescript
function getNetwork(): Chainable<{
    networkName: string,
    networkDisplayName: string, 
    networkId: number, 
    isTestnet: boolean,
}>;
```

#### Example&#x20;

```typescript
cy.getNetwork().then(network => {
    expect(network.networkName).to.be.equal('sepolia');
    expect(network.networkId).to.be.equal(11155111);
    expect(network.isTestnet).to.be.true;
});
```

## **`addMetamaskNetwork`**

Add network in MetaMask (and also switch to the newly added network).

#### Syntax&#x20;

```typescript
function addMetamaskNetwork(network: {
    networkName: string,
    rpcUrl: string,
    chainId: string,
    symbol?: string,
    blockExplorer?: string,
    isTestnet: boolean,
}): Chainable<boolean>
```

#### Example&#x20;

```typescript
cy.addMetamaskNetwork({
  networkName: 'Optimism Network',
  rpcUrl: 'https://mainnet.optimism.io',
  chainId: '10',
  symbol: 'OETH',
  blockExplorer: 'https://optimistic.etherscan.io',
  isTestnet: false,
});
```

## **`changeMetamaskNetwork`**

Change network in MetaMask.

Predefined networks: `mainnet`, `goerli`, `sepolia` , `localhost`&#x20;

If no network is provided, it will default to using the `NETWORK_NAME` environment variable. If not, it will default to `goerli` the network.&#x20;

#### Syntax&#x20;

<pre class="language-typescript"><code class="lang-typescript"><strong>function changeMetamaskNetwork(networkName?: string): Chainable&#x3C;boolean>;
</strong></code></pre>

#### Example&#x20;

```javascript
// Pass one of the predefined networks (mainnet, goerli, sepolia , localhost)
cy.changeMetamaskNetwork('mainnet');
// Pass the network name (from the Metamask networks dropdown menu)
cy.changeMetamaskNetwork('optimism network');
cy.changeMetamaskNetwork('sepolia');
```

## **`importMetamaskAccount`**

Import a new account in Metamask using a private key.

#### Syntax&#x20;

```typescript
function importMetamaskAccount(pk: string): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.importMetamaskAccount(
  '0x2a871d0798f97d79848a013d4936a73bf4cc922c825d33c1cf7073dff6d409c6',
);
cy.importMetamaskAccount(Cypress.env('PRIVATE_KEY_WITH_FUNDS'));
```

## **`createMetamaskAccount`**

#### Syntax&#x20;

```typescript
function createMetamaskAccount(accountName?: string): Chainable<boolean>;
```

#### Example&#x20;

```typescript
// Create a new account with the default name
cy.createMetamaskAccount();
cy.createMetamaskAccount('custom-wallet');
```

## **`switchMetamaskAccount`**

```typescript
function switchMetamaskAccount(
  accountNameOrAccountNumber: string | number,
): Chainable<boolean>;
```

#### Example&#x20;

```typescript
 // Switch using the account number
 cy.switchMetamaskAccount(2);
 // Switch using the account name
 cy.switchMetamaskAccount('account 1')
```

## **`getMetamaskWalletAddress`**

Get the current wallet address of Metamask wallet.

```typescript
function getMetamaskWalletAddress(): Chainable<string>;
```

#### Example&#x20;

```typescript
cy.getMetamaskWalletAddress().then(address => {
  expect(address).to.be.equal(
    '0x70997970C51812dc3A010C7d01b50e0d17dc79C8',
  );
});
```

## **`disconnectMetamaskWalletFromDapp`**

Disconnects Metamask wallet from last connected dApp.

```typescript
function disconnectMetamaskWalletFromDapp(): Chainable<boolean>;
```

## **`disconnectMetamaskWalletFromAllDapps`**

Disconnects Metamask wallet from all connected dApps.

```typescript
function disconnectMetamaskWalletFromAllDapps(): Chainable<boolean>;
```

## **`confirmMetamaskSignatureRequest`**

Confirm Metamask's permission to sign a "regular" message.

```typescript
function confirmMetamaskSignatureRequest(): Chainable<boolean>;
```

## **`confirmMetamaskDataSignatureRequest`**

Confirm Metamask's permission to sign a Data "type 4" message.

```typescript
confirmMetamaskDataSignatureRequest(): Chainable<boolean>;
```

## **`rejectMetamaskSignatureRequest`**

Reject Metamask permission to sign a "regular" message.

<pre class="language-typescript"><code class="lang-typescript"><strong>function rejectMetamaskSignatureRequest(): Chainable&#x3C;boolean>;
</strong></code></pre>

## **`rejectMetamaskDataSignatureRequest`**

Reject Metamask's permission to sign a Data "Type 4" message.

```typescript
function rejectMetamaskDataSignatureRequest(): Chainable<boolean>;
```

## **`confirmMetamaskEncryptionPublicKeyRequest`**

Confirm Metamask's request for the public encryption key.

```typescript
function confirmMetamaskEncryptionPublicKeyRequest(): Chainable<boolean>;
```

## **`rejectMetamaskEncryptionPublicKeyRequest`**

Reject Metamask's request for the public encryption key.

```typescript
function rejectMetamaskEncryptionPublicKeyRequest(): Chainable<boolean>;
```

## **`confirmMetamaskDecryptionRequest`**

Confirm Metamask's request to decrypt a message with the private key.

```typescript
function confirmMetamaskDecryptionRequest(): Chainable<boolean>;
```

## **`rejectMetamaskDecryptionRequest`**

Reject Metamask's request to decrypt the message with the private key.

```typescript
function rejectMetamaskDecryptionRequest(): Chainable<boolean>;
```

## **`importMetamaskToken`**

Add custom token to Metamask.

```typescript
function importMetamaskToken(
    tokenConfig: { address: string, symbol: string} | string
): Chainable<{
    tokenContractAddress: string,
    tokensymbol: string,
    tokenDecimals: string,
    imported: boolean,
}>;
```

#### Example&#x20;

```typescript
it(`importMetamaskToken should import token to metamask`, () => {
  const USDCContractAddressOnSepolia = '0xda9d4f9b69ac6C22e444eD9aF0CfC043b7a7f53f';
  cy.importMetamaskToken(USDCContractAddressOnSepolia).then(tokenData => {
    expect(tokenData.tokenContractAddress).to.be.equal(
       USDCContractAddressOnSepolia,
    );
    expect(tokenData.tokenSymbol).to.be.equal('USDC');
    expect(tokenData.tokenDecimals).to.be.equal('6');
    expect(tokenData.imported).to.be.true;
  });
});
```

## **`confirmMetamaskAddToken`**

Confirm Metamask's request to add a token.

```typescript
function confirmMetamaskAddToken(): Chainable<boolean>;
```

## **`rejectMetamaskAddToken`**

Reject Metamask's request to add a token.

```typescript
function rejectMetamaskAddToken(): Chainable<boolean>;
```

## **`confirmMetamaskPermissionToSpend`**

Confirm Metamask's permission to spend assets.

```typescript
function confirmMetamaskPermissionToSpend(spendLimit?: string): Chainable<boolean>;
```

## **`rejectMetamaskPermissionToSpend`**

Reject Metamask's permission to spend assets.

```typescript
function rejectMetamaskPermissionToSpend(): Chainable<boolean>;
```

## **`acceptMetamaskAccess`**

Accept Metamask access request.

TODO: this API must be updated with merging [#738](https://github.com/Synthetixio/synpress/pull/738)

```typescript
function acceptMetamaskAccess(options?: {
  allAccounts?: boolean;
  signInSignature?: boolean;
}): Chainable<Subject>;
```

#### Example&#x20;

```typescript
// Use default Metamask options (will click next with the default selected account)
cy.acceptMetamaskAccess();
// Will accept the signing request after accepting the access. 
cy.acceptMetamaskAccess({ signInSignature: true });
// Accept MetaMask connection to all accounts
cy.acceptMetamaskAccess({ allAccounts: true });
```

## MetaMask Settings&#x20;

### **`activateAdvancedGasControlInMetamask`**

Activate the ability (in Metamask settings) to specify custom gas prices and limits while doing transactions in Metamask.

```typescript
function activateAdvancedGasControlInMetamask(
  skipSetup?: boolean,
): Chainable<boolean>;
```

### **`activateShowHexDataInMetamask`**

Activate the ability (in Metamask settings) to show hex data while doing transactions in Metamask.

```typescript
function activateShowHexDataInMetamask(skipSetup?: boolean): Chainable<boolean>;
```

### **`activateTestnetConversionInMetamask`**

Activate the ability (in Metamask settings) to show fiat conversions on testnets in Metamask.

```typescript
function activateTestnetConversionInMetamask(
  skipSetup?: boolean,
): Chainable<boolean>;
```



### **`activateShowTestnetNetworksInMetamask`**

Activate the ability (in Metamask settings) to show testnet networks in Metamask.

```typescript
function activateShowTestnetNetworksInMetamask(
  skipSetup?: boolean,
): Chainable<boolean>;
```

### **`activateCustomNonceInMetamask`**

Activate the ability (in Metamask settings) to specify custom nonce while doing transactions in Metamask.

```typescript
function activateCustomNonceInMetamask(skipSetup?: boolean): Chainable<boolean>;
```

### **`activateDismissBackupReminderInMetamask`**

Activate the ability (in Metamask settings) to dismiss secret recovery phrase reminders in Metamask.

```typescript
function activateDismissBackupReminderInMetamask(
  skipSetup?: boolean,
): Chainable<boolean>;
```

### **`activateEthSignRequestsInMetamask`**

Activate eth sign requests in Metamask settings.

<pre class="language-typescript"><code class="lang-typescript"><strong>function activateEthSignRequestsInMetamask(skipSetup?: boolean): Chainable&#x3C;boolean>;
</strong></code></pre>

### **`activateImprovedTokenAllowanceInMetamask`**

Activate improved token allowance in Metamask settings (experimental).

```typescript
function activateImprovedTokenAllowanceInMetamask(
  skipSetup?: boolean,
): Chainable<boolean>;
```

### **`resetMetamaskAccount`**

Reset the Metamask account state in settings.

```typescript
function resetMetamaskAccount(): Chainable<boolean>;
```



\
