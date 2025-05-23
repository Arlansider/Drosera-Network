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
![script](https://i.ibb.co.com/HL0N1rqY/1.png)

🚨 Error: You may get several errors (.eg #429) due to <mark>rpc</mark> issues, to fix, you can enter bellow command by adding <mark>--eth-rpc-url</mark>

```
DROSERA_PRIVATE_KEY=xxx drosera apply --eth-rpc-url RPC
```
- Replace <mark>RPC</mark> with your own Ethereum Holesky rpc by registering and creating one in [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/).

## 3. Check Trap in Dashboard

1. Connect your Drosera wallet: https://app.drosera.io/
2. Click on <mark>Traps Owned</mark> to see your deployed Traps OR search your Trap address.

![script](https://i.ibb.co.com/C5snP8Fy/2.png)

## 4. Bloom Boost Trap

Open your Trap on Dashboard and Click on <mark>Send Bloom Boost</mark> and deposit some <mark>Holesky ETH</mark> on it.

![script](https://i.ibb.co.com/M5ht0YGP/3.png)

## 5. Fetch Blocks
```
drosera dryrun
```

# OPERATOR SETUP

## 1. Whitelist Your Operator

1- Edit Trap configuration:

```
cd my-drosera-trap
nano drosera.toml
```

Add the following codes at the bottom of <mark>drosera.toml</mark>:
```
private_trap = true
whitelist = ["Operator_Address"]
```

- Replace <mark>Operator_Address</mark> with your EVM wallet <mark>Public Address</mark> between " " symbols
- Your <mark>Public Address</mark> is your <mark>Operator_Address</mark>.

2- Update Trap Configuration:
```
DROSERA_PRIVATE_KEY=xxx drosera apply
```
- Replace xxx with your EVM wallet privatekey
- If RPC issue, use DROSERA_PRIVATE_KEY=xxx drosera apply --eth-rpc-url RPC and replace RPC with your own.

Your Trap should be private now with your operator address whitelisted internally.

![script](https://i.ibb.co.com/HDdSFDSq/4.png)

## 2. Operator CLI
```
cd ~
```
```
# Download
curl -LO https://github.com/drosera-network/releases/releases/download/v1.16.2/drosera-operator-v1.16.2-x86_64-unknown-linux-gnu.tar.gz

# Install
tar -xvf drosera-operator-v1.16.2-x86_64-unknown-linux-gnu.tar.gz
```
Test the CLI with ./drosera-operator --version to verify it's working.
```
# Check version
./drosera-operator --version

# Move path to run it globally
sudo cp drosera-operator /usr/bin

# Check if it is working
drosera-operator
```

## 3. Install Docker image
```
docker pull ghcr.io/drosera-network/drosera-operator:latest
```

## 4. Register Operator
```
drosera-operator register --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com --eth-private-key PV_KEY
```
- Replace PV_KEY with your Drosera EVM privatekey. We use the same wallet as our trap wallet.

## 5. Open Ports
```
# Enable firewall
sudo ufw allow ssh
sudo ufw allow 22
sudo ufw enable

# Allow Drosera ports
sudo ufw allow 31313/tcp
sudo ufw allow 31314/tcp
```

## 6. Install & Run Operator

Choose one Installation Method:

Method 1: [Install using Docker](https://github.com/Arlansider/Drosera-Network?tab=readme-ov-file#method-1-docker)
Method 2: [Install using SystemD](https://github.com/Arlansider/Drosera-Network?tab=readme-ov-file#method-2-systemd)

## Method 1: Docker

## 6-1-1: Configure Docker
- Make sure you have installed Docker in Dependecies step.
If you are currently running via old systemd method, stop it:
```
sudo systemctl stop drosera
sudo systemctl disable drosera
```
```
git clone https://github.com/0xmoei/Drosera-Network
```
```
cd Drosera-Network
```
```
cp .env.example .env
```

Edit .env file:
```
nano .env
```
- Replace your_evm_private_key and your_vps_public_ip
- To save: CTRL+X, Y & ENTER.

Edit docker-compose.yaml file:
```
nano docker-compose.yaml
```

Replace default rpc to your private [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/) Ethereum Holesky RPCs.
To save: CTRL+X, Y & ENTER.

## 6-1-2: Run Operator
```
docker compose up -d
```

## 6-1-3: Check health
```
docker logs -f drosera-node
```
![script](https://i.ibb.co.com/BHbFhZGV/5.png)

## 6-1-4: Optional Docker commands

```
# Stop node
cd Drosera-Network
docker compose down -v

# Restart node
cd Drosera-Network
docker compose up -d
```

Now running your node using Docker, you can Jump to step 7.


## Method 2: SystemD

## 6-2-1: Configure SystemD service file

Enter this command in the terminal, But first replace:

- PV_KEY with your privatekey
- VPS_IP with your solid vps IP (without anything else)
- Replace default https://ethereum-holesky-rpc.publicnode.com to your private [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/) Ethereum Holesky RPCs.

```
sudo tee /etc/systemd/system/drosera.service > /dev/null <<EOF
[Unit]
Description=drosera node service
After=network-online.target

[Service]
User=$USER
Restart=always
RestartSec=15
LimitNOFILE=65535
ExecStart=$(which drosera-operator) node --db-file-path $HOME/.drosera.db --network-p2p-port 31313 --server-port 31314 \
    --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com \
    --eth-backup-rpc-url https://1rpc.io/holesky \
    --drosera-address 0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8 \
    --eth-private-key PV_KEY \
    --listen-address 0.0.0.0 \
    --network-external-p2p-address VPS_IP \
    --disable-dnr-confirmation true

[Install]
WantedBy=multi-user.target
EOF
```

## 6-2-2: Run Operator

```
# reload systemd
sudo systemctl daemon-reload
sudo systemctl enable drosera

# start systemd
sudo systemctl start drosera
```
## 6-2-3: Check Node Health

```
journalctl -u drosera.service -f
```

![script](https://i.ibb.co.com/C33tpT5G/6.png)

## 6-2-4: Optional commands
```
# Stop node
sudo systemctl stop drosera

# Restart node
sudo systemctl restart drosera
```

## Now running your node using SystemD, you can Jump to step 7.

## 7. Opt-in Trap

In the dashboard., Click on Opti in to connect your operator to the Trap

![script](https://i.ibb.co.com/bjqyrRPG/7.png)

## 8. Check Node Liveness

Your node will start producing greeen blocks in the dashboard

![script](https://i.ibb.co.com/xqrZVX6x/8.png)

## Credit

This guide is based on the original work by [0xmoei](https://github.com/0xmoei/Drosera-Network) who provided the initial Drosera node setup instructions. This version was expanded and formatted for clarity and ease of use.