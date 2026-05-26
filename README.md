# NetVibe Linux Binaries

This repository publishes versioned Linux binaries for NetVibe.

NetVibe provides two Linux tools:

- `netvibe-cli`
- `netvibe-deploy`

Supported architectures:

- amd64
- arm64

Each release is published under a versioned directory.

## NetVibe Components

### netvibe-cli

`netvibe-cli` is the NetVibe Answerer runtime.

It connects to the NetVibe infrastructure, establishes WebRTC sessions, and continuously performs broadband delay measurements.

Typical use cases:

- running standalone NetVibe answerers
- broadband measurement nodes
- local testing and debugging
- persistent monitoring

### netvibe-deploy

`netvibe-deploy` is the NetVibe deployment and orchestration tool.

Rather than running measurements directly, it manages and orchestrates NetVibe answerers across infrastructure.

Internally, `netvibe-deploy` fetches and manages `netvibe-cli` binaries on remote systems and controls their lifecycle.

Current deployment support:

- SaltStack / PINOT infrastructure

The deployment framework is extensible and can support additional infrastructure backends in the future.

Examples include:

- SaltStack
- custom infrastructure deployments
- cluster wide NetVibe orchestration
- automated fleet deployment

## Install NetVibe CLI

Download the binary for your architecture.

Example for amd64:

```sh
wget https://raw.githubusercontent.com/Manni-MinM/linux-netvibe/master/v3.5.1/netvibe-cli-linux-amd64
```

Make the binary executable:

```sh
chmod +x netvibe-cli-linux-amd64
```

Move it to your PATH:

```sh
sudo mv netvibe-cli-linux-amd64 /usr/local/bin/netvibe-cli
```

Verify installation:

```sh
netvibe-cli --help
```

## Configure NetVibe CLI

Run setup and enter your Vibepass when prompted:

```sh
netvibe-cli setup
```

Configuration is stored locally and reused for future runs.

## Run NetVibe CLI

### Test Mode

Run NetVibe interactively:

```sh
netvibe-cli test
```

This mode is useful for:

- debugging connectivity
- validating configuration
- verifying WebRTC sessions

### Daemon Mode

Run NetVibe continuously:

```sh
netvibe-cli daemon
```

Typical use cases:

- long running measurement nodes
- persistent monitoring
- server deployments

## Install NetVibe Deploy

Download the deployment binary for your architecture.

Example for amd64:

```sh
wget https://raw.githubusercontent.com/Manni-MinM/linux-netvibe/master/v3.5.1/netvibe-deploy-linux-amd64
```

Make the binary executable:

```sh
chmod +x netvibe-deploy-linux-amd64
```

Move it to your PATH:

```sh
sudo mv netvibe-deploy-linux-amd64 /usr/local/bin/netvibe-deploy
```

Verify installation:

```sh
netvibe-deploy --help
```

## Deployment Architecture

`netvibe-deploy` orchestrates NetVibe answerers using `netvibe-cli`.

The deployment workflow is:

1. connect to infrastructure backend
2. fetch or update `netvibe-cli`
3. configure answerers using Vibepass credentials
4. start and manage answerer daemons
5. control lifecycle operations such as stop and reset

The deployment tool acts as an orchestration layer while `netvibe-cli` performs the actual measurements.

## PINOT Deployment

NetVibe Deploy currently supports PINOT infrastructure using SaltStack.

### Prerequisites

Before deployment:

1. Login to the NetVibe UI
2. Add the required PINOT devices
3. Use the PINOT minion ID as the device name
4. Fetch the Vibepass for each device

### Start Deployment

Start deployment using `MINION=VIBEPASS` pairs:

```sh
netvibe-deploy start pinot MINION1=VIBEPASS1 MINION2=VIBEPASS2
```

This command:

- fetches or updates `netvibe-cli`
- runs setup using Vibepass credentials
- starts the NetVibe daemon on each minion

### Stop Deployment

Stop running answerers:

```sh
netvibe-deploy stop pinot MINION1 MINION2
```

This command stops running `netvibe-cli` processes on each minion.

### Reset Deployment

Reset deployment state:

```sh
netvibe-deploy reset pinot MINION1 MINION2
```

This command:

- stops running processes
- removes NetVibe binaries
- removes state directories
- removes PID files

## Upgrade

Download the latest version and replace the installed binaries.

## Uninstall

Remove installed binaries:

```sh
sudo rm -f /usr/local/bin/netvibe-cli
sudo rm -f /usr/local/bin/netvibe-deploy
```
