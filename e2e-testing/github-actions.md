---
description: Guide on how to run Synpress from GitHub Workflows or GitLab CI/CD Pipeline.
coverY: 0
---

# 🧑🚀 CI/CD Pipeline

## Prerequisites

1. Basic knowledge of **GitHub Action**s -> [Understanding GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
2. Basic knowledge of **GitLab CI/CD** -> [CI/CD Introduction](https://docs.gitlab.com/ee/ci/introduction/)
3. Basic knowledge of **Docker** -> ([Containerize an application](https://docs.docker.com/get-started/02\_our\_app/), [Docker Compose](https://docs.docker.com/compose/gettingstarted/))
4. Exiting **Synpress Setup** -> ([Cypress](using-with-cypress.md), [Playwright](using-with-playwright.md))



## How To Run Synpress E2E Tests From CI/CD Pipeline?

### 1. Create Dockerfile

You should create a **docker image of your dApp** based on [synthetixio/docker-e2e](https://hub.docker.com/r/synthetixio/docker-e2e/tags) docker image.&#x20;

> ATM, [synthetixio/docker-e2e](https://hub.docker.com/r/synthetixio/docker-e2e/tags) only works for `linux/amd64` based machines. which means it will not work on the new Apple Silicon Chips. You can still try to [run it under Rosetta 2](https://levelup.gitconnected.com/docker-on-apple-silicon-mac-how-to-run-x86-containers-with-rosetta-2-4a679913a0d5). **With that being said, it will not affect the CI/CD as the CI/CD VM is already running under `linux/amd64`.**

<figure><img src="../.gitbook/assets/Screenshot 2023-05-29 at 4.27.10 PM.png" alt=""><figcaption><p>Use Rosetta for x86/amd64 emulation on Apple Silicon</p></figcaption></figure>

The end goal of having this `Dockerfile`  is being able to **run the docker image (of the dApp) from a docker container** using `docker-compose` .

The example below is a basic `Dockerfile` setup (yours might be different).

```docker
# syntax=docker/dockerfile:1
FROM --platform=linux/amd64 synthetixio/docker-e2e:18.13-ubuntu as base

RUN mkdir /app
WORKDIR /app

COPY package.json ./
COPY yarn.lock ./

RUN yarn --frozen-lockfile --prefer-offline --no-audit
COPY . .
```

Here is an example of a `Dockerfile`.&#x20;

{% embed url="https://github.com/Synthetixio/synpress/blob/dev/Dockerfile" %}
Example Dockerfile to create a Docker Image of the codebase
{% endembed %}

### 2. Create `docker-compose.yml`

We will use [docker-compose](https://docs.docker.com/compose/) to create a container of the image we created in the previous step.&#x20;

The example below is a basic `docker-compose.yml` setup.&#x20;

{% code lineNumbers="true" %}
```yaml
# docker-compose.yml 
version: "3.9"
services:
  synpress:
    profiles:
      - synpress
    container_name: synpress
    # Path to your Dockerfile 
    build: .
    environment:
      - DISPLAY=display:0.0
      - CYPRESS_DOCKER_RUN=true
      - CI=true
    entrypoint: []
    working_dir: /app
    # Replace "yarn test:e2e" with your test run script 
    command: > 
      yarn test:e2e
    depends_on:
      - display
    networks:
      - x11

  display:
    profiles:
      - synpress
    container_name: display
    image: synthetixio/display:016121eafdfff448414894d0ca5a50b1d72b62eb-base
    environment:
      - RUN_XTERM=no
      - DISPLAY_WIDTH=800
      - DISPLAY_HEIGHT=600
    ports:
      - "8080:8080"
    networks:
      - x11

networks:
  x11:
```
{% endcode %}

Feel free to look at more [complex examples](https://github.com/Synthetixio/synpress/blob/dev/docker-compose.ci.yml), if you are interested.

{% embed url="https://github.com/Synthetixio/synpress/blob/dev/docker-compose.ci.yml" %}
docker-compose.yml example
{% endembed %}

### 3. Run From CI/CD&#x20;

Once you created a docker container (using [docker-compose](github-actions.md#2.-create-docker-compose.yml)), you should be able to run Synpress E2E tests from **any CI/CD Pipeline**. Below, there are 2 examples of **how to run Synpress from GitHub Workflow and from GitLab CI/CD**.&#x20;



{% tabs %}
{% tab title="GitHub" %}
### Create a new workflow&#x20;

In the example below, we set up [QEMU](https://medium.com/@nullbyte.in/docker-and-qemu-a-powerful-combination-for-accelerating-edge-computing-development-and-optimizing-42da00259a02) and Docker [BuildX](https://github.com/docker/buildx) and run the docker container we created in the previous step.&#x20;

<pre class="language-yaml" data-line-numbers><code class="lang-yaml"><strong># .github/workflows/e2e.yml
</strong>name: dApp E2E

on:
  push:
  pull_request:
    branches: [main]

concurrency:
  group: "${{ github.workflow }} @ ${{ github.event.pull_request.head.label ||
    github.head_ref || github.ref }}"
  cancel-in-progress: true

jobs:
  e2e:
    if: github.ref == 'refs/heads/main' || github.event_name == 'pull_request'
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@ac593985615ec2ede58e132d2e21d2b1cbd6127c # pin@v2

      - name: Set up QEMU
        uses: docker/setup-qemu-action@e81a89b1732b9c48d79cd809d8d81d79c4647a18 # pin@v1

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@f03ac48505955848960e80bbb68046aa35c7b9e7 # pin@v1

      - name: Cache Docker layers
        uses: actions/cache@69d9d449aced6a2ede0bc19182fadc3a0a42d2b0 # pin@v2
        with:
          path: /tmp/.buildx-cache
          key: ${{ runner.os }}-buildx-${{ github.sha }}
          restore-keys: |
            ${{ runner.os }}-buildx-

      - name: Run e2e tests
        run: |
          docker-compose -f docker-compose.yml up synpress
        env:
          COMPOSE_DOCKER_CLI_BUILD: 1
          DOCKER_BUILDKIT: 1
          DOCKER_DEFAULT_PLATFORM: linux/amd64
</code></pre>

The example above can get you up and running in most cases! Feel free to look at a [more complex setup with more features](https://github.com/Synthetixio/synpress/tree/dev/.github/workflows), if you are interested. \
\


{% embed url="https://github.com/Synthetixio/synpress/tree/dev/.github/workflows" %}
Synpress GitHub Workflow examples
{% endembed %}

Below is a working example of how to run Synpress E2E tests from GitHub Workflow.&#x20;

{% embed url="https://github.com/neuodev/synpress-demo" %}
Working example of how to run Synpress E2E tests from GitHub Workflow
{% endembed %}
{% endtab %}

{% tab title="GitLab" %}
### Update `.gitlab-ci.yml`

We will use the [docker](https://hub.docker.com/\_/docker) image itself to run a docker container from the CI/CD

```yaml
dapp-e2e:
  image: docker
  services:
    - docker:dind
  script:
    - docker-compose -f docker-compose.yml up synpress
```

For more information about how to use docker from GitLab CI/CD, Read [GitLab Docker Integration](https://docs.gitlab.com/ee/ci/docker/)

{% embed url="https://docs.gitlab.com/ee/ci/docker/" %}
GitLab CI/D Docker Integration
{% endembed %}

Below is a working example of how to run Synpress E2E tests from GitLab CI/CD Pipeline.&#x20;

{% embed url="https://gitlab.com/AhmedIbrahim336/synpress-demo" %}
Working example on how to run synpress E2E tests from GitLab CI/CD Pipeline
{% endembed %}
{% endtab %}
{% endtabs %}



## Summary

1. Create a docker image of your dApp&#x20;
2. Run the docker image inside a docker container using docker-compose&#x20;
3. Run the container from within the CI/CD&#x20;
