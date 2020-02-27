# How to join Kamut Enigma Blockchain Testnet as a Genesis Validator

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

### 2. Install the enigmachain package:

```bash
sudo dpkg -i enigmachain_0.0.2_amd64.deb
```

### 3. Update the configuration file that sets up the system service with your current user as the user this service will run as.

_Note: Even if we are running this command and the previous one with sudo, this package does not need to be run as root_.

```bash
sudo perl -i -pe "s/XXXXX/$USER/" /etc/systemd/system/enigma-node.service
```
### 3.1 Initiate Enigmad

```bash
enigmad init <moniker> --chain-id kamut-testnet-1

```

### 4. Create a key to hold your Validator account & Choose a **moniker** for yourself that will be public, and replace `<MONIKER>` with your moniker below

```bash
enigmacli keys add <MONIKER>
```

### 4.1 Show the address of your created Validator (Save this in a text file for your future reference)

```bash
enigmacli keys show <MONIKER> -a
```

### 5. Download a copy of the Kamut Testnet Genesis Block file: `genesis.json`

```bash
wget -O ~/.enigmad/config/genesis.json "https://raw.githubusercontent.com/chainofsecrets/kamut-testnet/master/genesis.json"
```

### 6. Sign the Genesis.json

```bash
enigmad gentx --name <MONIKER>
``` 

### 7. Upload your signed Genesis transaction JSON to pastebin and link it into the Kamut Testnet TG channel.

```bash
pastebinit -b pastebin.com ~/.enigmad/config/gentx/*.json
```

### 8. Download the new copy of Kamut Testnet Genesis Block file which has genesis validators: `genesis.json`

```bash
DO NOT PROCEED PAST HERE: waiting for link after all validators sign genesis
```

### 9. Validate Genesis

```bash
enigmad validate-genesis
```
### 10. Add Kamut Testnet Bootstrap Node as a persistent peer in your configuration file.

```bash
perl -i -pe 's/persistent_peers = ""/persistent_peers = "7b574eafec435c6b20a9142ae76811bc008d0dbd\@45.79.143.29:26656"/' ~/.enigmad/config/config.toml
```

### 11. Enable enigma-node as a system service:

```bash
sudo systemctl enable enigma-node
```

### 12. Start enigma-node as a system service:

```bash
sudo systemctl start enigma-node
```

### 13. If everything above worked correctly, the following command will show your node streaming blocks after all genesis validators come up (this is for debugging purposes only, kill this command anytime with Ctrl-C):

```bash
journalctl -f -u enigma-node
```
