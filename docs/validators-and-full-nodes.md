# How to join Kamut Testnet Validator

This document details how to join the Kamut Enigma Blockchain `Testnet` as a validator.

## Requirements

- Ubuntu/Debian host (with ZFS or LVM to be able to add more storage easily)
- A public IP address
- Open ports `TCP 26656 & 26657` _Note: If you're behind a router or firewall then you'll need to port forward on the network device._
- Reading https://docs.tendermint.com/master/tendermint-core/running-in-production.html

## Installation

### 1. Download the [EnigmaChain package installer](https://github.com/enigmampc/enigmachain/releases/download/v0.0.2/enigmachain_0.0.2_amd64.deb) (Debian/Ubuntu):

```bash
wget -O enigmachain_0.0.2_amd64.deb
```

```bash
https://github.com/enigmampc/enigmachain/releases/download/v0.0.2/enigmachain_0.0.2_amd64.deb
```

([How to verify releases](/docs/verify-releases.md))

### 2. Install the enigmachain package:

```bash
sudo dpkg -i enigmachain_0.0.2_amd64.deb
```

### 3. Update the configuration file that sets up the system service with your current user as the user this service will run as.

_Note: Even if we are running this command and the previous one with sudo, this package does not need to be run as root_.

```bash
sudo perl -i -pe "s/XXXXX/$USER/" /etc/systemd/system/enigma-node.service
```

### 4. Initialize your installation of the enigmachain. Choose a **moniker** for yourself that will be public, and replace `<MONIKER>` with your moniker below

```bash
enigmad init <MONIKER> --chain-id kamut-testnet-1
```

### 5. Download a copy of the Kamut Testnet Genesis Block file: `genesis.json`

```bash
wget -O ~/.enigmad/config/genesis.json "https://raw.githubusercontent.com/chainofsecrets/kamut-testnet/master/genesis.json"
```

### 6. 
