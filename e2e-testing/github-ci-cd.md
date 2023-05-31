---
description: Guide on how to run Synpress E2E tests from GitLab CI/CD pipeline.
coverY: 0
---

# 🧑🚀 GitLab CI/CD

## Prerequisites

1. Basic knowledge of GitLab CI/CD -> [CI/CD Introduction](https://docs.gitlab.com/ee/ci/introduction/)
2. Basic knowledge of Docker -> ([Containerize an application](https://docs.docker.com/get-started/02\_our\_app/), [Docker Compose](https://docs.docker.com/compose/gettingstarted/))
3. Exiting Synpress Setup -> ([Cypress](using-with-cypress.md), [Playwright](using-with-playwright.md))

## Run Synpress E2E Tests From GitLab CI/CD

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

## 2. Create `docker-compose.yml`

We will use [docker-compose](https://docs.docker.com/compose/) to create a container of the image that we just created in the previous step.&#x20;

The example below is a basic `docker-compose.yml` setup. Feel free to take a look at more [complex examples](https://github.com/Synthetixio/synpress/blob/dev/docker-compose.ci.yml), if you are interested.

{% embed url="https://github.com/Synthetixio/synpress/blob/dev/docker-compose.ci.yml" %}
docker-compose.yml example
{% endembed %}

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

## 3. Update `.gitlab-ci.yml`

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
Docker Integration Into GitLab CI/D
{% endembed %}



Below is a working example of running Synpress E2E tests from GitLab CI/CD&#x20;

{% embed url="https://gitlab.com/AhmedIbrahim336/synpress-demo" %}

















