# Docker Build & Push

Build the application's Dockerfile, and push it to the given registry.

Some notes:
- Will fallback to docker hub registry (if none is provided)
- Image name will default to the current repository name
- Tag will default to the current branch

## Input

```yaml
inputs:
  container-registry:
    description: Container registry endpoint (fallback to Docker Hub)
    required: false
  container-registry-username:
    description: Container registry username
    required: true
  container-registry-password:
    description: Container registry password
    required: true
  github-access-token:
    description: GitHub access token (GITHUB_TOKEN)
    required: true
  image:
    description: Image name (fallback to repository name)
    required: false
  tag:
    description: Image tag (fallback to branch name)
    required: false
  platforms:
    description: List of platforms (architectures, comma separated, defaults to `linux/amd64`)
    required: false
  target:
    description: Docker target (build-stage) you want to build.
    required: false
```

## Output

```yaml
outputs:
  digest:
    description: Result image digest
  manifest:
    description: Image manifest (registry + image + tag)
```

## Usage

```yaml
name: Pull Request
on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build-and-push-to-digitalocean:
    uses: wisemen-digital/devops-ga-docker-build-and-push@main
    with:
      container-registry: ${{ vars.CONTAINER_REGISTRY_ENDPOINT }}
      container-registry-username: ${{ secrets.DIGITALOCEAN_API_USER }}
      container-registry-password: ${{ secrets.DIGITALOCEAN_API_TOKEN }}
      github-access-token: ${{ secrets.GITHUB_TOKEN }}
      tag: development

  build-and-push-to-scaleway:
    uses: wisemen-digital/devops-ga-docker-build-and-push@main
    with:
      container-registry: ${{ vars.CONTAINER_REGISTRY_ENDPOINT }}
      container-registry-username: nologin
      container-registry-password: ${{ secrets.SCALEWAY_SECRET_KEY }}
      github-access-token: ${{ secrets.GITHUB_TOKEN }}
      tag: development
```
