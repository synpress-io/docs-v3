---
description: Adding/switching networks to Metamask in Cypress and Playwright.
---

# ⛓ Networks

Synpress gives you the ability to[ add a new network to Metamask](synpress-api.md#add-metamask-network) and [switches between different networks](synpress-api.md#change-metamask-network).



TODO: adding/switching networks will be updated after merking this PR&#x20;

{% embed url="https://github.com/Synthetixio/synpress/pull/770" %}

## Supported Networks

By default, Synpress supports testing on the following Networks:\
`mainnet`, `goerli`, `sepolia` and `localhost`\


{% tabs %}
{% tab title="Cypress" %}
<pre class="language-typescript"><code class="lang-typescript">it(`setupMetamask should finish metamask setup using secret words`, () => {
  cy.setupMetamask(
   'test test test test test test test test test test test junk',
<strong>   'sepolia',
</strong>   'Tester@1234',
  ).then(setupFinished => {
   expect(setupFinished).to.be.true;
  });
});
</code></pre>

#### :fire: Note:&#x20;

Note that Synpress will call `setupMetamask` for you and will look for the `PRIVATE_KEY` or `SECRET_WORDS` and `NETWORK_NAME` environment variables.&#x20;

If you want to disable this behavior, you can  add `SKIP_METAMASK_SETUP` environment variable and then you can call `setupMetamask` yourself.&#x20;
{% endtab %}

{% tab title="Playwright" %}
<pre class="language-typescript"><code class="lang-typescript">// fixtures.ts
await initialSetup(chromium, {
  secretWordsOrPrivateKey:
  "test test test test test test test test test test test junk",
<strong>  network: "sepolia",
</strong>  password: "Tester@1234",
  enableAdvancedSettings: true,
});
</code></pre>

{% embed url="https://github.com/drptbl/synpress-examples/blob/master/playwright/isolated-state/fixtures.ts" %}
An example on how to setup Synpress with Playwright
{% endembed %}
{% endtab %}
{% endtabs %}

## Custom Networks

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
    "test test test test test test test test test test test junk", 
  network: networkConfiguration,
  password: "Tester@1234",
  enableAdvancedSettings: true,
});
```

## Adding new network during testing

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



## See Also &#x20;

1. [Environment variables for MetaMask setup](environment-variables.md#environment-variables-for-metamask-setup)
2. [Add MetaMask network](synpress-api.md#add-metamask-network)
3. [Change MetaMask Network](synpress-api.md#change-metamask-network)&#x20;
