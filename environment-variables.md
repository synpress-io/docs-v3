---
description: >-
  Synpress is designed with the mindset that it will run in different
  environments (local development & CI/CD). Besides it can be used for all sorts
  of testing (eg. doing aggressive testing on mainnet,
coverY: 0
---

# ⚙ Environment Variables

**Tip:** It is recommended to keep all E2E test environment variables in their own `.env` file to avoid [name collision](https://en.wikipedia.org/wiki/Name\_collision) and for separation of concerns.  This is easily achievable using [env-cmd](https://www.npmjs.com/package/env-cmd).&#x20;

```json5
// package.json
// Load Synpress required environment variables from the `.env.e2e` file.
"synpress:run": "env-cmd -f .env.e2e synpress run --configFile synpress.config.ts"
```

| Variable                | Description                                                                                                                                                                                                  |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `SECRET_WORDS`          | Space separated words for the test wallet recovery phrase (mnemonic; 12 words)                                                                                                                               |
| `PRIVATE_KEY`           | Test wallet private key                                                                                                                                                                                      |
| `NETWORK_NAME`          | Network name (eg `NETWORK_NAME=Optimism`)                                                                                                                                                                    |
| `RPC_URL`               | Network RPC (eg`RPC_URL=https://mainnet.optimism.io`)                                                                                                                                                        |
| `CHAIN_ID`              | Network ID (eg`CHAIN_ID=10`)                                                                                                                                                                                 |
| `SYMBOL`                | Native chain token ticker (eg `SYMBOL=OP`)                                                                                                                                                                   |
| `IS_TESTNET`            | `boolean` indicates that the added network is testnet                                                                                                                                                        |
| `BLOCK_EXPLORER`        | Blockchain explorer (eg `BLOCK_EXPLORER=https://optimistic.etherscan.io/`)                                                                                                                                   |
| `SYNDEBUG`              | Set debugging mode to be on                                                                                                                                                                                  |
| `STABLE_MODE`           | Introduce delay between main actions, 300ms by default (eg `STABLE_MODE=300ms`, `STABLE_MODE=true`)                                                                                                          |
| `SLOW_MODE`             | Introduce delay between every action, 50ms by default (eg `SLOW_MODE=true`, `SLOW_MODE=200ms`)                                                                                                               |
| `METAMASK_VERSION`      | MetaMask version to be installed                                                                                                                                                                             |
| `SKIP_METAMASK_INSTALL` | Will skip MetaMask installation                                                                                                                                                                              |
| `SKIP_METAMASK_SETUP`   | Will skip MetaMask initial setup                                                                                                                                                                             |
| `GH_USERNAME`           | GitHub username (used to avoid rate-limit issue while downloading MetaMask)                                                                                                                                  |
| `GH_PAT`                | GitHub [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) (used to avoid rate-limit issues while downloading MetaMask) |
| `ETHERSCAN_KEY`         | [Etherscan key](https://info.etherscan.com/etherscan-developer-api-key/) (used only for etherscan-related commands)                                                                                          |
| `FAIL_ON_ERROR`         | Fail a test if there are any browser console errors                                                                                                                                                          |
| `CYPRESS_GROUP`         | [Group tests](https://docs.cypress.io/guides/guides/command-line#cypress-run-group-lt-name-gt)                                                                                                               |
| `CI`                    | A Boolean value indicates that tests are running from CI/CD pipeline                                                                                                                                         |



## Environment Variables For MetaMask Setup

By default, after running [`synpress run`](synpress-cli.md), Synpress will look for some environment variables that are required to set up MetaMask.&#x20;

#### **1. Looking for wallet mnemonics or private key**

Synpress will look for `SECRET_WORDS` or `PRIVATE_KEY` environment variables. The restored wallet will be the default wallet in MetaMask. Synpress will use it when running the tests (you can still [add more wallets](synpress-api.md#import-metamask-account) and [accounts](synpress-api.md#create-metamask-account)).&#x20;

> :warning: Synpress with through error if both `SECRET_WORDS` and `PRIVATE_KEY` are missing. \
> At lest one should be provided ([example](https://github.com/neuodev/synpress-demo/blob/main/.env#L1)).&#x20;

#### 2. Looking for network info \[optional]

Synpress will look for the environment variables needed to add a new network to MetaMask. The added network will be used to run the tests by default (you can still being able to [switch to another network](synpress-api.md#change-metamask-network) or [add a new one](synpress-api.md#add-metamask-network))

Required environment variables to add a new network are: `NETWORK_NAME`, `RPC_URL`, `CHAIN_ID`.

You can optionally add `SYMBOL`,`IS_TESTNET`, and `BLOCK_EXPLORER`.

{% embed url="https://github.com/neuodev/synpress-demo/blob/main/.env#L2,L6" %}
EXample of the environment variables needed to add a new network to Synpress during Metamask Setup
{% endembed %}

```bash
NETWORK_NAME='polygonMumbai'
CHAIN_ID=80001
RPC_URL='https://matic-mumbai.chainstacklabs.com'
SYMBOL="MATIC"
BLOCK_EXPLORER='https://mumbai.polygonscan.com/'
IS_TESTNET=true
```



## `SKIP_METAMASK_SETUP`

By default, Synpress will **automatically (AutoSetup)** set up Metamask (load the extension, restore a wallet, add a new network, ...) using environment variables. **You can disable this behavior by adding the `SKIP_METAMASK_SETUP` environment variable**.&#x20;

> :warning: If you disabled Synpress AutoSetup using `SKIP_METAMASK_SETUP`, you will need to set up MetaMask Manually using [setupMetamask API](synpress-api.md#setup-metamask) ([example](https://github.com/Synthetixio/synpress/blob/dev/tests/e2e/specs/metamask-spec.js#L6,L12)).



{% tabs %}
{% tab title="Cypress" %}
```typescript
cy.setupMetamask(
  "secret, words, ...",
  {
    networkName: "name",
    rpcUrl: "https://eth.llamarpc.com",
    chainId: 1,
    symbol: "ETH",
    blockExplorer: "https://etherscan.io/",
    isTestnet: true,
  },
  "metamask_password"
);
```

{% embed url="https://github.com/Synthetixio/synpress/blob/dev/tests/e2e/specs/metamask-spec.js#L6,L12" %}
Setup MetaMask using Cypress example
{% endembed %}
{% endtab %}

{% tab title="Playwright" %}
```typescript
import {chromium} from "@playwright/test";
import { initialSetup } from "@synthetixio/synpress/commands/metamask";

// Setup Metamask
await initialSetup(chromium, {
  secretWordsOrPrivateKey:"test test test test test test test test test test test share",
  network: "sepolia",
  password: "Tester@1234",
  enableAdvancedSettings: true,
});
```

{% embed url="https://github.com/drptbl/synpress-examples/blob/master/playwright/isolated-state/fixtures.ts#L35,L41" %}
Setup MetaMask using Playwright example
{% endembed %}
{% endtab %}
{% endtabs %}



