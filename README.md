# Aztec Test Action

This GitHub Action allows you to run Aztec tests in your CI/CD pipeline using Docker Compose.

## Usage

Add the following step to your GitHub Actions workflow:

```yaml
- uses: ClarifiedLabs/aztec-test@v1
  with:
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
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `aztec-version` | Version of Aztec to use | No | '0.71.0' |
| `workdir` | Working directory for tests | No | ${{ github.workspace }} |
| `test-args` | Arguments to pass to the test command | No | 'test --silence-warnings --oracle-resolver http://txe:8081' |
| `nargo-timeout` | Timeout for Nargo foreign calls (in milliseconds) | No | '300000' |

## How it Works

This action:

1. Sets up a Docker Compose environment with two services:
   - `txe`: Runs the Aztec transaction execution engine
   - `aztec-nargo`: Runs the actual tests using Nargo

2. Configures the services with the provided inputs

3. Runs the tests and reports the results

4. Automatically cleans up containers after the tests complete

## Requirements

- GitHub-hosted runner or self-hosted runner with Docker and Docker Compose installed
- Access to pull the Aztec Docker images
