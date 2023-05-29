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

| Option                     | Description                                                                        |
| -------------------------- | ---------------------------------------------------------------------------------- |
| `--browser`, `-b`          | Run on specified browser (default: `chrome`)                                       |
| `--config`, `-c`           | Set configuration values, separate multiple values with a comma                    |
| `--configFile`, `-cf`      | Specify a path to `*.js` file where configuration values are set.                  |
| `--env`, `-e`              | Set environment variables, separate multiple values with comma                     |
| `--spec`, `-s`             | Run only provided spec files                                                       |
| `--noExit`, `-ne`          | Keep runner open after tests finish                                                |
| `--project`, `-pr`         | Run with specific project path                                                     |
| `--quiet`, `-q`            | Only test runner output in console                                                 |
| `--reporter`, `-r`         | Specify mocha reporter                                                             |
| `--reporterOptions`, `-ro` | Specify mocha reporter options, separate multiple values with comma                |
| `--record`, `-r`           | \[dashboard] Record video of tests running after setting up your project to record |
| `--key`, `-k`              | \[dashboard] Set record key                                                        |
| `--parallel`, `-p`         | \[dashboard] Run recorded specs in parallel across multiple machines               |
| `--group`, `-g`            | \[dashboard] Group recorded tests together under a single run                      |
| `--tag`, `-t`              | \[dashboard] Add tags to the dashboard for a test run                              |
| `--help`, `-h`             | Output usage information                                                           |

