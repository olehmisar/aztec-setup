# Aztec Setup Action

This GitHub Action allows you to setup Aztec CLI in your CI/CD pipeline.

## Usage

Add the following step to your GitHub Actions workflow:

```yaml
# Using specific version (for version pinning)
- uses: olehmisar/aztec-setup@v1
  with:
    # Specify Aztec version
    aztec-version: '0.72.0'

    # Optional: Enable/disable Docker image caching (default: 'true')
    enable-caching: 'true'
```

## Example Workflow

Here's a complete example workflow that setups Aztec CLI:

```yaml
name: Aztec Tests

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: olehmisar/aztec-setup@v1
        with:
          aztec-version: '0.71.0'
          # Optional: Disable caching if needed
          # enable-caching: 'false'
      
      - name: Run Aztec sandbox
        run: |
          aztec start --sandbox 2>&1 &

      - name: Run e2e tests
        run: npm run test
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `aztec-version` | Version of Aztec to use | Yes | - |
| `enable-caching` | Enable Docker image caching | No | 'true' |

## Requirements

- GitHub-hosted runner or self-hosted runner with Docker and Docker Compose installed
- Access to pull the Aztec Docker images
