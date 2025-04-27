# DROSERA-NETWORK
In this Guide, we contribute to Drosera testnet by:

1. Installing the CLI
2. Setting up a vulnerable contract
3. Deploying a Trap on testnet
4. Connecting an operator to the Trap

# Recommended System Requirement

- 2 CPU Cores
- 4 GB RAM
- 20 GB Disk Space
- Get started with a low-budget VPS for as low as $5! [Purchase here](https://my.hostbrr.com/order/forms/)
- Create your own Ethereum Holesky RPC in [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/).

## Insstall Depenndencies 

```
sudo apt-get update && sudo apt-get upgrade -y
```
```
sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```
## Install Docker
```
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world
```

<h1 style="text-align: center;">Trap Setup</h1>

## 1. Configure Environment

### Drosera CLI:
```
curl -L https://app.drosera.io/install | bash
```
```
source /root/.bashrc
```
```
droseraup
```
### Foundry CLI:
```
curl -L https://foundry.paradigm.xyz | bash
```
```
source /root/.bashrc
```
```
foundryup
```
### Bun:
```
curl -fsSL https://bun.sh/install | bash

source /root/.bashrc
```

## 2. Deploy Contract & Trap
```
mkdir my-drosera-trap
```
```
cd my-drosera-trap
```
### Replace <mark>Github_Email</mark> & <mark>Github_Username</mark> :
```
git config --global user.email "Github_Email"
git config --global user.name "Github_Username"
```
### Initialize Trap:
```
forge init -t drosera-network/trap-foundry-template
```
### Compile Trap:
```
curl -fsSL https://bun.sh/install | bash

source /root/.bashrc

bun install
```
```
forge build
```
| skip Warning !

### Deploy Trap:
```
DROSERA_PRIVATE_KEY=xxx drosera apply
```
- Replace <mark>xxx</mark> with your EVM wallet <mark>privatekey</mark> (Ensure it's funded with <mark>Holesky ETH</mark>)
- Enter the command, when prompted, write <mark>ofc</mark> and press Enter.
![](https://i.ibb.co.com/HL0N1rqY/1.png)
