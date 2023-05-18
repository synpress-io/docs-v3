---
description: List of all Playwright APIs used in Synpress
---

# Playwright API

### `extensionId`

`Get Metamask extension ID`

```typescript
function extensionId(): string;
```

#### Example&#x20;

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

let extId: string = metamask.extensionId(); 
```

### `extensionUrls`

Get all Metamask page urls&#x20;

```typescript
function extensionUrls(): {
  extensionInitialUrl: string;
  extensionHomeUrl: string;
  extensionSettingsUrl: string;
  extensionAdvancedSettingsUrl: string;
  extensionExperimentalSettingsUrl: string;
  extensionAddNetworkUrl: string;
  extensionNewAccountUrl: string;
  extensionImportAccountUrl: string;
  extensionImportTokenUrl: string;
};
```

#### Example

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

let urls = metamask.extensionUrls(); 
```

### `walletAddress`

Get the currently selected wallet address from Metamask&#x20;

```typescript
function walletAddress(): string;
```

### `goTo`

Navigate to any page in the Metamask extension.

```typescript
function goTo(url: string): Promise<void>;
```

#### Example&#x20;

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

let urls = metamask.extensionUrls(); 
await metamask.goTo(urls.extensionHomeUrl);
```

## `goToHome`

Go to the **home page** in Metamask**.**

```typescript
function goToHome(): Promise<void>;
```

## `goToSettings`

Go to the **setting page** in Metamask.&#x20;

```typescript
function goToSettings(): Promise<void>;
```

## `goToAdvancedSettings`

Go to the **advanced settings page** in Metamask.&#x20;

```typescript
function goToAdvancedSettings(): Promise<void>;
```

## `goToExperimentalSettings`

Go to the **Experimental Settings page** in Metamask.&#x20;

```typescript
function goToExperimentalSettings(): Promise<void>;
```

## `goToAddNetwork`

Go to **add network page** in Metamask.&#x20;

```typescript
function goToAddNetwork(): Promise<void>;
```

## `goToImportAccount`

Go to the **import account** page in Metamask.&#x20;

```typescript
function goToImportAccount(): Promise<void>;
```

## `goToImportToken`

Go to the **import token** page in the Metamask&#x20;

```typescript
function goToImportToken(): Promise<void>;
```

## `getExtensionDetails`

Get extension id and URLs

```typescript
function getExtensionDetails(): {
  extensionId: string,
  extensionInitialUrl: string;
  extensionHomeUrl: string;
  extensionSettingsUrl: string;
  extensionAdvancedSettingsUrl: string;
  extensionExperimentalSettingsUrl: string;
  extensionAddNetworkUrl: string;
  extensionNewAccountUrl: string;
  extensionImportAccountUrl: string;
  extensionImportTokenUrl: string;
};
```

## `closePopupAndTooltips`

Close any opened tooltips or popups&#x20;

```typescript
function closePopupAndTooltips(): Promise<boolean>;
```

## `closeModal`

Close any opened models/dialogs if any.&#x20;

```typescript
function closeModal(): Promise<void>;
```

## `unlock`

Unlock Metamask by entering the password&#x20;

```typescript
function unlock(password: string): Promise<boolean>;
```

#### Example&#x20;

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

await metamask.unlock('my_password');
```

#### Preview&#x20;

<figure><img src="https://raw.githubusercontent.com/synpress-io/docs/main/.gitbook/assets/unlock_metamask.png" alt=""><figcaption><p>Unlock Metamask</p></figcaption></figure>

## `importWallet`

Import wallet into Metamask using secret words and password.&#x20;

```typescript
function importWallet(secretWords: string, password: string): Promise<boolean>;
```

#### Example&#x20;

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

await metamask.importWallet(
    'test test test ...',
    'my_password',
);
```

#### Preview

<figure><img src="../.gitbook/assets/Screenshot 2023-05-18 at 9.14.22 AM.png" alt="Import wallet page in Metamask"><figcaption><p>Import wallet page in Metamask</p></figcaption></figure>

## `createWallet`

TODO:

## `importAccount`

Import new account into Metamask using its private key&#x20;

```typescript
function importAccount(privateKey: string): Promise<boolean>;
```

#### Example

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

await metamask.importAccount('0x00001');
```

#### Preview&#x20;

<figure><img src="https://raw.githubusercontent.com/synpress-io/docs/main/.gitbook/assets/import_account.png" alt=""><figcaption></figcaption></figure>

## `createAccount`

Create new account&#x20;

```typescript
function createAccount(accountName?: string): Promise<boolean>;
```

#### Example&#x20;

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

await metamask.createAccount('my_account');
```

#### Preview&#x20;

<figure><img src="https://raw.githubusercontent.com/synpress-io/docs/main/.gitbook/assets/create_account.png" alt=""><figcaption></figcaption></figure>

## `switchAccount`

Switch to another Metamask account using account number or account name.&#x20;

```typescript
function switchAccount(accountNameOrAccountNumber: string | number): Promise<boolean>;
```

#### Example

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

await metamask.switchAccount('my_account');
await metamask.switchAccount('Account 13');
await metamask.switchAccount(13);
```

#### Preview&#x20;

<figure><img src="https://raw.githubusercontent.com/synpress-io/docs/main/.gitbook/assets/switch_account.png" alt="Switch Metamask accounts"><figcaption><p>Switch Metamask accounts</p></figcaption></figure>

## `changeNetwork`

Change current network in Metamask using network name&#x20;

```typescript
function changeNetwork(network: string | NetworkInfo): Promise<boolean>;
```

#### Example&#x20;

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

await metamask.changeNetwork('mainnet');
await metamask.changeNetwork('optimism network');
```

#### Preview&#x20;

<figure><img src="https://raw.githubusercontent.com/synpress-io/docs/main/.gitbook/assets/switch_network.png" alt=""><figcaption></figcaption></figure>

## `addNetwork`

Add new network to Metamask&#x20;

```typescript
function addNetwork(network: {
  networkName: string;
  rpcUrl: string;
  chainId: string;
  symbol?: string;
  blockExplorer?: string;
  isTestnet: boolean;
}): Promise<boolean>;
```

#### Example&#x20;

```typescript
import * as metamask from "@synthetixio/synpress/commands/metamask";

await metamask.addNetwork({
  networkName: "Optimism Network",
  rpcUrl: "https://mainnet.optimism.io",
  chainId: "10",
  symbol: "OETH",
  blockExplorer: "https://optimistic.etherscan.io",
  isTestnet: false,
});
```

#### Preview&#x20;

<figure><img src="https://raw.githubusercontent.com/synpress-io/docs/main/.gitbook/assets/add_network.png" alt="Add new network to Metamask"><figcaption><p>Add new network to Metamask</p></figcaption></figure>

## `disconnectWalletFromDapp`

Disconnect Metamask wallet from last connected dApp.

```typescript
function disconnectWalletFromDapp(): Promise<boolean>;
```

#### Preview&#x20;

<figure><img src="https://raw.githubusercontent.com/synpress-io/docs/main/.gitbook/assets/disconnect_dapp.png" alt="Disconnect Metamask wallet from last connected dApp"><figcaption><p>Disconnect Metamask wallet from last connected dApp</p></figcaption></figure>

## `disconnectWalletFromAllDapps`

Disconnect Metamask wallet from all connected dApps.

```typescript
function disconnectWalletFromAllDapps(): Promise<boolean>;
```

#### Preview&#x20;

<figure><img src="https://raw.githubusercontent.com/synpress-io/docs/main/.gitbook/assets/disconnect_all_dapps.png" alt="Disconnect Metamask wallet from all connected dApps."><figcaption><p>Disconnect Metamask wallet from all connected dApps.</p></figcaption></figure>

## `activateAdvancedGasControl`

## `activateShowHexData`

## `activateTestnetConversion`

## `activateShowTestnetNetworks`

## `activateCustomNonce`

## `activateDismissBackupReminder`

## `activateEthSignRequests`

## `activateImprovedTokenAllowance`

## `resetAccount`

## `confirmSignatureRequest`

## `confirmDataSignatureRequest`

## `rejectDataSignatureRequest`

## `importToken`

## `confirmAddToken`

## `rejectAddToken`

## `confirmPermissionToSpend`

## `rejectPermissionToSpend`

## `acceptAccess`

## `confirmTransaction`

## `rejectTransaction`

## `confirmEncryptionPublicKeyRequest`

## `rejectEncryptionPublicKeyRequest`

## `confirmDecryptionRequest`

## `rejectDecryptionRequest`

## `confirmPermisionToApproveAll`

## `rejectPermisionToApproveAll`

## `allowToAddNetwork`

## `rejectToAddNetwork`

## `allowToSwitchNetwork`

## `rejectToSwitchNetwork`

## `allowToAddAndSwitchNetwork`

## `getWalletAddress`

## `initialSetup`









































