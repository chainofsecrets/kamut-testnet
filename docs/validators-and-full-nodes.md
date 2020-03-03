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
wget https://github.com/enigmampc/enigmachain/releases/download/v0.0.2/enigmachain_0.0.2_amd64.deb
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
### 4. 

```bash
enigmacli keys add <keyalias>
```

### 5. 

```bash
enigmad init [moniker] --chain-id kamut-testnet-1
```

### 6 
```bash
perl -i -pe 's/persistent_peers = ""/persistent_peers = "4efa7d9e6d4970fea88da74d49de90433d8bc78b\@198.74.53.44:26656"/' ~/.enigmad/config/config.toml
```

### 7. Enable enigma-node as a system service:

```bash
sudo systemctl enable enigma-node
```

### 8. Start enigma-node as a system service:

```bash
sudo systemctl start enigma-node
```

### 9. If everything above worked correctly, the following command will show your node streaming blocks after all genesis validators come up (this is for debugging purposes only, kill this command anytime with Ctrl-C):

```bash
journalctl -f -u enigma-node
```

# Join as Kamut Testnet as new Validator

Firstly, please run the following and paste the address into testnet telegram so we can give you some SCRT tokens in this account to create your validator.

```bash
enigmacli keys show -a <key-alias>
```

After you have a private node up and running, run the following command:

```bash
enigmacli tx staking create-validator \
  --amount=100000uscrt \ # This is the amount of coins you put at stake. i.e. 100000uscrt
  --pubkey=$(enigmad tendermint show-validator) \
  --moniker="<name-of-your-moniker>" \
  --chain-id=kamut-testnet-1 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --gas="auto" \
  --gas-prices="0.025uscrt" \
  --from=<name or address> # Name or address of your existing account
```

To check if you got added to the validator-set by running:

```bash
enigmacli q tendermint-validator-set
```

