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
function goToHome(): Promise<void>;
```

## `goToAdvancedSettings`

Go to the **advanced settings page** in Metamask.&#x20;

```typescript
function goToHome(): Promise<void>;
```

## `goToExperimentalSettings`

Go to the **Experimental Settings page** in Metamask.&#x20;

```typescript
function goToHome(): Promise<void>;
```

## `goToAddNetwork`

Go to **add network page** in Metamask.&#x20;

```typescript
function goToHome(): Promise<void>;
```

## `goToImportAccount`

Go to the **import account** page in Metamask.&#x20;

```typescript
function goToHome(): Promise<void>;
```

## `goToImportToken`

Go to the **import token** page in the Metamask&#x20;

```typescript
function goToHome(): Promise<void>;
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

## `closeModal`

## `unlock`

## `optOutAnalytics`

## `importWallet`

## `createWallet`

## `importAccount`

## `createAccount`

## `switchAccount`

## `changeNetwork`

## `addNetwork`

## `disconnectWalletFromDapp`

## `disconnectWalletFromAllDapps`

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









































