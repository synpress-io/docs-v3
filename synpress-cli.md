---
coverY: 0
---

# 💻 Synpress CLI

```bash
npx synpress run
```

Or by using `yarn`

```typescript
yarn synpress run
```

or&#x20;

```bash
./node_modules/.bin/synpress run
```

Or add it to `package.json`

```json5
{
  // npm: npm run synpress:run
  // yarn: yarn synpress:run 
  "scripts": {
    "synpress:run": "synpress run"
  }
}
```

## Commands

### `synpress run`

```bash
synpress run [options]
```

#### Options&#x20;

| Option                     | Description                                                                                                                                                                        |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--browser`, `-b`          | [Run on specified browser (default: `chrome`)](synpress-cli.md#cypress-run-browser-less-than-browser-name-greater-than)                                                            |
| `--config`, `-c`           | [Set configuration values, separate multiple values with a comma](https://docs.cypress.io/guides/guides/command-line#cypress-run-config-lt-config-gt)                              |
| `--configFile`, `-cf`      | [Specify a path to `*.js` file where configuration values are set.](synpress-cli.md#synpress-run-configfile-less-than-file-path-greater-than)                                      |
| `--env`, `-e`              | [Set environment variables, and separate multiple values with commas.](https://docs.cypress.io/guides/guides/command-line#cypress-open-env-lt-env-gt)                              |
| `--spec`, `-s`             | [Run only provided spec files](https://docs.cypress.io/guides/guides/command-line#cypress-run-spec-lt-spec-gt)                                                                     |
| `--noExit`, `-ne`          | Keep runner open after tests finish                                                                                                                                                |
| `--project`, `-pr`         | [Run with specific project path](https://docs.cypress.io/guides/guides/command-line#cypress-run-project-lt-project-path-gt)                                                        |
| `--quiet`, `-q`            | Only test runner output in console                                                                                                                                                 |
| `--reporter`, `-r`         | [Specify mocha reporter](https://docs.cypress.io/guides/guides/command-line#cypress-run-reporter-lt-reporter-gt)                                                                   |
| `--reporterOptions`, `-ro` | Specify mocha reporter options, separate multiple values with comma                                                                                                                |
| `--record`, `-r`           | [\[dashboard\] Record video of tests running after setting up your project to record.](https://docs.cypress.io/guides/guides/command-line#cypress-run-record-key-lt-record-key-gt) |
| `--key`, `-k`              | [\[dashboard\] Set record key](https://docs.cypress.io/guides/guides/command-line#cypress-run-record-key-lt-record-key-gt)                                                         |
| `--parallel`, `-p`         | [\[dashboard\] Run recorded specs in parallel across multiple machines.](https://docs.cypress.io/guides/guides/command-line#cypress-run-record-key-lt-record-key-gt)               |
| `--group`, `-g`            | [\[dashboard\] Group recorded tests together under a single run](https://docs.cypress.io/guides/guides/command-line#cypress-run-group-lt-name-gt).                                 |
| `--tag`, `-t`              | [\[dashboard\] Add tags to the dashboard for a test run](https://docs.cypress.io/guides/guides/command-line#cypress-run-tag-lt-tag-gt)                                             |
| `--help`, `-h`             | Output usage information                                                                                                                                                           |



## **`synpress run --browser <browser-name>`**

```bash
synpress run --browser chrome 
```

Available options are `chrome`, `chromium`, `edge`, `electron`, and `firefox`.

It works the same way the "browser" option works in [Cypress](https://docs.cypress.io/guides/guides/command-line#cypress-run-browser-lt-browser-name-or-path-gt).&#x20;



## `synpress run --configFile <file-path>`

```bash
synpress run --configFile synpress.config.js
```

The "**configFile**" option accepts the path to the Synpress config file ([example](https://github.com/neuodev/synpress-demo/blob/main/synpress.config.js)) that is required to lunch the tests.&#x20;
