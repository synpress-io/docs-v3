---
description: Supported Networks & Custom Networks for Synpress testing.
---

# ⛓ Networks

### 1. Supported Networks:

By default, Synpress supports testing on the following Networks:\
`mainnet`, `goerli`, `sepolia` and `localhost`\
\
In your `fixtures.ts initialSetup()` file you can select your network using the network name:

```typescript
await initialSetup(chromium, {
  secretWordsOrPrivateKey:
    "test test test test test test test test test test test junk", //Private key
  network: "sepolia", //Can use mainnet | goerli | sepolia | localhost
  password: "Tester@1234", //login password
  enableAdvancedSettings: true,
});
```

### 2. Custom Networks:

Synpress also supports using custom networks, to do so pass in the Network Configuration Object:

```typescript
const networkConfiguration = {
    networkName: 'Polygon',
    rpcUrl: 'https://polygon.llamarpc.com/',
    chainId: '137',
    symbol: 'Matic',
    isTestnet: false
}

await initialSetup(chromium, {
  secretWordsOrPrivateKey:
    "test test test test test test test test test test test junk", //Private key
  network: networkConfiguration, //Custom Network
  password: "Tester@1234", //login password
  enableAdvancedSettings: true,
});
```

### 3.  Adding Network during testing:

You can add or switch to different networks in your wallet:

```typescript
import * as metamask from '@synthetixio/synpress/commands/metamask';

test('add and switch to matic network in wallet', async ({ page }) => {
    await metamask.addNetwork({
        networkName: 'Polygon',
        rpcUrl: 'https://polygon.llamarpc.com/',
        chainId: '137',
        symbol: 'Matic',
        isTestnet: false
    });
    await metamask.changeNetwork('Polygon');
}
```
