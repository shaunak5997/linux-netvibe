# NetVibe Linux Binaries

This repository publishes versioned Linux binaries for NetVibe.

Available binaries:

- `netvibe-cli`
- `netvibe-deploy`

Supported architectures:

- amd64
- arm64

Each release is published under a versioned directory.

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

Verify the installation:

```sh
netvibe-cli --help
```

## Configure NetVibe CLI

Run the setup command and enter your Vibepass when prompted:

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

### Daemon Mode

Run NetVibe continuously:

```sh
netvibe-cli daemon
```

## Install NetVibe Deploy

Download the deployment CLI for your architecture.

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

## PINOT Deployment

NetVibe Deploy manages NetVibe answerers on PINOT infrastructure using SaltStack.

### Prerequisites

Before deployment:

1. Login to the NetVibe UI
2. Add the required PINOT devices
3. Use the PINOT minion ID as the device name
4. Fetch the Vibepass for each device

### Usage

Start deployment using `MINION=VIBEPASS` pairs:

```sh
netvibe-deploy start pinot MINION1=VIBEPASS1 MINION2=VIBEPASS2
```

Stop running deployments:

```sh
netvibe-deploy stop pinot MINION1 MINION2
```

Reset deployments and remove state:

```sh
netvibe-deploy reset pinot MINION1 MINION2
```

### Commands

`start`

- clones or updates the NetVibe binary repository on each minion
- runs NetVibe setup using the provided Vibepass
- starts the NetVibe daemon
- requires `MINION=VIBEPASS` arguments

`stop`

- stops running `netvibe-cli` processes on each minion

`reset`

- stops running processes
- removes binary repositories
- removes state directories and PID files

## Upgrade

Download the latest binary version and replace the existing executable.

## Uninstall

Remove the installed binaries:

```sh
sudo rm -f /usr/local/bin/netvibe-cli
sudo rm -f /usr/local/bin/netvibe-deploy
```
