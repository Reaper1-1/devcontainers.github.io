---
layout: implementors
title:  "Prebuilt Dev Container Images"
shortTitle: "Prebuilt Images"
author: Microsoft
index: 9
---

**Prebuilt Dev Container Images** allow you to package your development environment configuration into a container image that can be reused across different projects and shared with other developers. By prebuilding images, you can significantly improve container startup time, ensure consistency across environments, and simplify your `devcontainer.json` configuration.

## <a href="#overview" name="overview" class="anchor"> Overview </a>

Prebuilt dev container images combine the benefits of reproducible development environments with faster startup times. When you prebuild a dev container image:

1. All dev container configuration, including Features, are baked into the image
2. Startup time is reduced as dependencies are pre-installed
3. The image can be version-tagged for stability and reproducibility
4. Metadata is stored in image labels for seamless use across projects

Prebuilt images are particularly valuable for:
- Teams that want consistent development environments
- Projects with complex dependency requirements
- CI/CD workflows that benefit from reproducible environments
- Open source projects that want to provide contributors with ready-to-use environments

## <a href="#creating" name="creating" class="anchor"> Creating Prebuilt Images </a>

You can create prebuilt images using the [Dev Container CLI](/implementors/reference), GitHub Actions, or other CI tools.

### <a href="#using-cli" name="using-cli" class="anchor"> Using the Dev Container CLI </a>

To prebuild an image with the Dev Container CLI:

1. Install the CLI:
   ```bash
   npm install -g @devcontainers/cli
   ```

2. Build and push your image to a container registry:
   ```bash
   devcontainer build --workspace-folder . --push true --image-name <registry>/<namespace>/<image-name>:<tag>
   ```

### <a href="#using-github-actions" name="using-github-actions" class="anchor"> Using GitHub Actions </a>

For automated builds, you can use the [Dev Container Build and Run Action](https://github.com/marketplace/actions/dev-container-build-and-run-action):

```yaml
name: Build and Push Dev Container Image

on:
  push:
    branches: [ main ]
    paths:
      - '.devcontainer/**'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build and Push Dev Container Image
        uses: devcontainers/ci@v0.3
        with:
          imageName: ghcr.io/your-org/your-image
          imageTag: latest
          push: always
          # Optional: Registry authentication
          # registryPassword: ${{ secrets.GITHUB_TOKEN }}
```

### <a href="#using-azure-devops" name="using-azure-devops" class="anchor"> Using Azure DevOps </a>

You can also use the [Dev Container Task for Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=devcontainers.ci):

```yaml
steps:
- task: DevcontainersCi@0
  inputs:
    imageName: 'your-registry/your-image'
    imageTag: 'v1'
    push: true
    # Optional: Registry authentication
    # registryPassword: $(registryPassword)
```

## <a href="#using" name="using" class="anchor"> Using Prebuilt Images </a>

Once you have a prebuilt image available in a container registry, you can reference it directly in your `devcontainer.json`:

```json
{
    "image": "ghcr.io/your-org/your-image:latest"
}
```

You can also use prebuilt images as a base in a Dockerfile:

```dockerfile
FROM ghcr.io/your-org/your-image:latest

# Add additional customizations here
```

Or in a Docker Compose file:

```yaml
version: '3'
services:
  devcontainer:
    image: ghcr.io/your-org/your-image:latest
    volumes:
      - ../..:/workspaces:cached
    command: sleep infinity
```

## <a href="#metadata" name="metadata" class="anchor"> Metadata in Image Labels </a>

When you prebuild an image using the Dev Container CLI or supporting tools, dev container configuration and Feature metadata are automatically stored in image labels. This makes your image self-contained and all settings will be automatically applied when the image is used.

The metadata is stored in the `devcontainer.metadata` label as an array of JSON snippets:

```json
[
  {
    "remoteUser": "devcontainer",
    "postCreateCommand": "npm install",
    "features": {
      "ghcr.io/devcontainers/features/node:1": {
        "version": "18"
      }
    }
  }
]
```

You can also manually add metadata to an image label in your Dockerfile:

```dockerfile
LABEL devcontainer.metadata='[{ \
  "capAdd": [ "SYS_PTRACE" ], \
  "remoteUser": "devcontainer", \
  "postCreateCommand": "yarn install" \
}]'
```

See the [Dev Container metadata reference](/implementors/json_reference) for information on which properties are supported in labels.

## <a href="#best-practices" name="best-practices" class="anchor"> Best Practices </a>

When working with prebuilt images, follow these best practices:

### <a href="#versioning" name="versioning" class="anchor"> Versioning </a>

- Use semantic versioning for your images (e.g., `v1.0.0`)
- Provide a `latest` tag for the most recent version
- Consider using date-based tags for regular builds (e.g., `20250512`)

### <a href="#organization" name="organization" class="anchor"> Organization </a>

- Keep your build configuration in a separate repository from consumer projects
- Document the included tools and configurations
- Use continuous integration to build and test images regularly

### <a href="#security" name="security" class="anchor"> Security </a>

- Rebuild images regularly to incorporate security updates
- Scan your images for vulnerabilities
- Use specific versions of base images rather than the `latest` tag
- Consider using multi-stage builds to reduce image size

### <a href="#optimizing" name="optimizing" class="anchor"> Optimizing </a>

- Clean up package caches in your Dockerfile to reduce image size
- Group related RUN commands to reduce layer count
- Consider using [Features](/features) for modular functionality

## <a href="#examples" name="examples" class="anchor"> Examples </a>

### <a href="#example-kubernetes" name="example-kubernetes" class="anchor"> Kubernetes Development </a>

The [kubernetes-devcontainer](https://github.com/craiglpeters/kubernetes-devcontainer) project maintains a prebuilt dev container image for Kubernetes development:

```json
{
    "image": "ghcr.io/craiglpeters/kubernetes-devcontainer:latest"
}
```

### <a href="#example-language" name="example-language" class="anchor"> Language-specific Image </a>

The [devcontainers/images](https://github.com/devcontainers/images) repository maintains language-specific prebuilt images:

```json
{
    "image": "mcr.microsoft.com/devcontainers/python:3"
}
```

## <a href="#ci-integration" name="ci-integration" class="anchor"> CI Integration </a>

Beyond local development, prebuilt dev container images provide consistency between development and CI environments. You can use the same container image for both:

```yaml
name: CI Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Run tests in Dev Container
        uses: devcontainers/ci@v0.3
        with:
          imageName: ghcr.io/your-org/your-image
          runCmd: npm test
```

## <a href="#references" name="references" class="anchor"> References </a>

- [Dev Container CLI Reference](/implementors/reference#prebuilding)
- [In-depth guide on prebuilds](/guide/prebuild)
- [Using Images, Dockerfiles, and Docker Compose](/guide/dockerfile)
- [Dev Container metadata reference](/implementors/json_reference)
- [Dev Container Features](/features)
