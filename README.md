# Static Web Site Helm Chart

## Overview
This Helm chart deploys a static web site that was published as an Azure DevOps artifact and will be served by a Caddy web server.
The chart creates a series of Kubernetes resources as follow:

### Secret
The Azure DevOps PAT access token to get the site artifact

### Deployment
Deploy an init container that runs before the Caddy web server container to setup the site artifact code and configuration required to deploy and start the web server.

### ConfigMap
As part of the deployment, we download the site artifact to the /var/www/html folder. ConfigMap will update the Caddyfile to point to this new path, causing the web server to serve the new HTML site.

### Service & Ingress 
used together to expose the static web site.


## Prerequisites
- Kubernetes cluster
- Helm 3
- kubectl installed and configured


## Installation
helm repo add static-app https://ger-is-registry.caas.intel.com/chartrepo/pda
helm upgrade --install static-app -f values.yaml --namespace <namespace-name>

## Parameters

### site parameters

| Name       | Description     | Value |
| ---------- | --------------- | ----- |
| `site.url` | static site url | `""`  |

### site artifact parameters

| Name             | Description                                                   | Value |
| ---------------- | ------------------------------------------------------------- | ----- |
| `artifact.token` | auth token to get the artifact published by artifact-pipeline | `""`  |
| `artifact.url`   | url to the artifact published by artifact-pipeline            | `""`  |

### proxy parameters

| Name          | Description          | Value                              |
| ------------- | -------------------- | ---------------------------------- |
| `proxy.http`  | set proxy connection | `http://proxy-chain.intel.com:912` |
| `proxy.https` | set proxy connection | `http://proxy-chain.intel.com:912` |

### imagePullSecrets parameters

| Name                       | Description                          | Value            |
| -------------------------- | ------------------------------------ | ---------------- |
| `imagePullSecrets[0].name` | image pull secrets to dockerhub repo | `dockerhub-repo` |
| `imagePullSecrets[1].name` | image pull secrets to harbor repo    | `harbor-repo`    |
