# nulink-2.0
Minimum System Requirements
```bash
Debian/Ubuntu 20.04
4 CPU processors
8 GB Ram
30GB Available Storage
x86 Architecture
Static IP address
Port 9151 opened (make sure it's not occupied)
```
Open Port (Warning:Be carefull what you're doing here ! DYOR)
```bash
sudo apt install ufw
sudo ufw allow 9151
sudo ufw allow ssh
sudo ufw enable
```
 Download Geth
```bash
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.23-d901d853.tar.gz
```
Unzip the Iinstallation Package
```bash
tar -xvzf geth-linux-amd64-1.10.23-d901d853.tar.gz
```
Enter the Directory
```bash
cd geth-linux-amd64-1.10.23-d901d853/
```
Create Ethereum account and keystore (You'll be asked to create and confirm the password. Remember this password for future use !)
```bash
./geth account new --keystore ./keystore
```
There are 3 items you should save/remember for future use
```bash
1. Account Password
2. Public Address
3. Path of the Key Store/Secret File
```
Install Docker (skip if you already install it)
```bash
sudo apt-get update
```
```bash
sudo apt-get install ca-certificates curl gnupg
```
```bash
sudo install -m 0755 -d /etc/apt/keyrings
```
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```bash
sudo apt-get update
```
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Pull the latest NuLink image
```bash
docker pull nulink/nulink:latest
```
Create your directory in your host machine
```bash
cd /root
mkdir nulink
```
Copy the the secret key file/key sotre given to you before to nulink directory
```bash
cp /root/<Path of your key store> /root/nulink
```
example :
```bash
cp /root/codespaces-blank/geth-linux-amd64-1.10.23-d901d853/keystore/UTC--2023-12-31T17-42-14.316243885Z--f3defb90c2f03e904bd9662a1f16dcd1ca69b00a /root/nulink
```
Ensure that this directory has 777 permissions
```bash
chmod -R 777 /root/nulink
```
Install Python3 (skip if you already install it)
```bash
apt install python3-pip
```
Install virtual environment
```bash
pip install virtualenv
```
Create a Virtual Environment for Nulink
```bash
virtualenv /root/nulink-venv
```
Activate Nulink virtual environment
 ```bash
source /root/nulink-venv/bin/activate
```
Install the Nulink python3 package
```bash
wget https://download.nulink.org/release/core/nulink-0.5.0-py3-none-any.whl
```
Install python3 package
```bash
pip install nulink-0.5.0-py3-none-any.whl
```
Verify your Nulink setup
```bash
source /root/nulink-venv/bin/activate
```
```bash
python -c "import nulink"
```
```bash
nulink --help
**you'll see list of nulink help command
```
Lock and unlock the private storage created by the NuLink Worker, use the password you created earlier annd edit the following codes according to your password
```bash
export NULINK_KEYSTORE_PASSWORD=<YOUR_PASSWORD>
```
```bash
export NULINK_OPERATOR_ETH_PASSWORD=<YOUR_PASSWORD>
```
Initialize Node Configuration
```bash
docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/<YOUR_KEYSTORE_BEGIN_WITH_UTC> \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--payment-network bsc_testnet \
--operator-address <YOUR_PUBLIC_ADDRESS> \
--max-gas-price 10000000000
```
Check Node Status
```bash
docker logs -f ursula
```
