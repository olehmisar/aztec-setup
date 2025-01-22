# Aztec Test Action

This GitHub Action allows you to run Aztec TXE tests in your CI/CD pipeline.

## Usage

Add the following step to your GitHub Actions workflow:

```yaml
# Using major version (recommended for most users)
- uses: ClarifiedLabs/aztec-test@v1

# Using specific version (for version pinning)
- uses: ClarifiedLabs/aztec-test@v1.1.0
  with:
    # Optional: Enable/disable Docker image caching (default: 'true')
    enable-caching: 'true'

    # Optional: Specify Aztec version (default: '0.71.0')
    aztec-version: '0.71.0'

    # Optional: Working directory (default: ${{ github.workspace }})
    workdir: ${{ github.workspace }}

    # Optional: Test arguments (default: 'test --silence-warnings --oracle-resolver http://txe:8081')
    test-args: 'test --silence-warnings --oracle-resolver http://txe:8081'

    # Optional: Nargo timeout in milliseconds (default: 300000)
    nargo-timeout: '300000'
```

## Example Workflow

Here's a complete example workflow that runs Aztec tests:

```yaml
name: Aztec Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Run Aztec Tests
        uses: ClarifiedLabs/aztec-test@v1
        with:
          aztec-version: '0.71.0'
          # Optional: Disable caching if needed
          # enable-caching: 'false'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `enable-caching` | Enable Docker image caching | No | 'true' |
| `aztec-version` | Version of Aztec to use | No | '0.71.0' |
| `workdir` | Working directory for tests | No | ${{ github.workspace }} |
| `test-args` | Arguments to pass to the test command | No | 'test --silence-warnings --oracle-resolver http://txe:8081' |
| `nargo-timeout` | Timeout for Nargo foreign calls (in milliseconds) | No | '300000' |

## How it Works

This action is based on the `aztec test` command from the [Aztec Sandbox](https://docs.aztec.network/guides/getting_started#install-and-run-the-sandbox).

It performs the following steps:
1. Handles Docker image caching (enabled by default):
   - Caches the Aztec Docker images to speed up subsequent runs
   - Can be disabled by setting `enable-caching: false`

2. Sets up a Docker Compose environment with two services:
   - `txe`: Runs the Aztec transaction execution engine
   - `aztec-nargo`: Runs the actual tests using Nargo

3. Configures the services with the provided inputs

4. Runs the tests and reports the results

5. Automatically cleans up containers after the tests complete

## Requirements

- GitHub-hosted runner or self-hosted runner with Docker and Docker Compose installed
- Access to pull the Aztec Docker images
