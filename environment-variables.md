---
description: >-
  Synpress is designed with the mindset that it will run in different
  environments (local development & CI/CD). Besides it can be used for all sorts
  of testing (eg. doing aggressive testing on mainnet,
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
| `METAMASK_VERSION`      | Metamask version to be installed                                                                                                                                                                             |
| `SKIP_METAMASK_INSTALL` | Will skip MetaMask installation                                                                                                                                                                              |
| `SKIP_METAMASK_SETUP`   | Will skip MetaMask initial setup                                                                                                                                                                             |
| `GH_USERNAME`           | GitHub username (used to avoid rate-limit issue while downloading metamask)                                                                                                                                  |
| `GH_PAT`                | GitHub [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) (used to avoid rate-limit issues while downloading Metamask) |
| `ETHERSCAN_KEY`         | [Etherscan key](https://info.etherscan.com/etherscan-developer-api-key/) (used only for etherscan-related commands)                                                                                          |
| `FAIL_ON_ERROR`         | Fail a test if there are any browser console errors                                                                                                                                                          |
| `CYPRESS_GROUP`         | [Group tests](https://docs.cypress.io/guides/guides/command-line#cypress-run-group-lt-name-gt)                                                                                                               |
| `CI`                    | A Boolean value indicates that tests are running from CI/CD pipeline                                                                                                                                         |

