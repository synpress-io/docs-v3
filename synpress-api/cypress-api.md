---
description: List of all the API that Synpress exposes and can be used from Cypress
---

# Cypress API

## **`initPlaywright`**

Connect Playwright with Cypress instance.

### Syntax&#x20;

```typescript
function initPlaywright(): Chainable<boolean>;
```

#### Example

```javascript
cy.initPlaywright().then(isConnected => {
    expect(isConnected).to.be.true;
});
```

## **`assignWindows`**

Assign currently open tabs with Playwright.

### Syntax&#x20;

```typescript
function assignWindows(): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.assignWindows().then(assigned => {
   expect(assigned).to.be.true;
});
```

## **`assignActiveTabName`**

Assigns currently active tab.

### Syntax&#x20;

```typescript
function assignActiveTabName(tabName: string): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.assignActiveTabName("my-tab"); 
```

## **`isMetamaskWindowActive`**

Checks if the currently active window is Metamask.

### Syntax

```typescript
function isMetamaskWindowActive(): Chainable<boolean>; 
```

#### Example

```javascript
cy.isMetamaskWindowActive().then(isActive => {
    expect(isActive).to.be.true;
});
```

## **`isCypressWindowActive`**

Checks if the currently active window is Cypress.

### Syntax

```typescript
function isCypressWindowActive(): Chainable<boolean>;
```

#### **Example**

```javascript
cy.isCypressWindowActive().then(isActive => {
    expect(isActive).to.be.true;
});
```

## **`switchToCypressWindow`**

### **Syntax**

```typescript
function switchToCypressWindow(): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.switchToCypressWindow().then(switched => {
    expect(switched).to.be.true;
});
```

## **`switchToMetamaskWindow`**

### **Syntax**

```typescript
function switchToMetamaskWindow(): Chainable<boolean>;
```

#### Example&#x20;

```typescript
cy.switchToMetamaskWindow().then(switched => {
    expect(switched).to.be.true;
});
```

## **`switchToMetamaskNotification`**

### Syntax&#x20;

```typescript
function switchToMetamaskNotification(): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.switchToMetamaskNotification().then(switched => {
    expect(switched).to.be.true;
});
```

## **`getNetwork`**

Get current network info&#x20;

### Syntax&#x20;

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

### Syntax&#x20;

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
}).then(networkAdded => {
  expect(networkAdded).to.be.true;
});
```

## **`changeMetamaskNetwork`**

Change network in MetaMask.

Predefined networks: `mainnet`, `goerli`, `sepolia` , `localhost`&#x20;

If not network provided, it will default to using the `NETWORK_NAME` environment variable. If not, will default to `goerli` network.&#x20;

### Syntax&#x20;

<pre class="language-typescript"><code class="lang-typescript"><strong>function changeMetamaskNetwork(networkName?: string): Chainable&#x3C;boolean>;
</strong></code></pre>

#### Example&#x20;

```javascript
cy.changeMetamaskNetwork('mainnet').then(networkChanged => {
    expect(networkChanged).to.be.true;
});
cy.changeMetamaskNetwork('optimism network').then(networkChanged => {
    expect(networkChanged).to.be.true;
});
cy.changeMetamaskNetwork('sepolia');
```

## **`importMetamaskAccount`**

Import new account in metamask using a private key.

### Syntax&#x20;

```typescript
function importMetamaskAccount(pk: string): Chainable<boolean>;
```

#### Example&#x20;

```javascript
cy.importMetamaskAccount(
  '0x2a871d0798f97d79848a013d4936a73bf4cc922c825d33c1cf7073dff6d409c6',
).then(imported => {
  expect(imported).to.be.true;
});

cy.importMetamaskAccount(Cypress.env('PRIVATE_KEY_WITH_FUNDS'));
```



