# Cysic-Verifier-MultiAddress

# 1. Update System & Install Docker + Docker Compose
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# Add user ke grup docker
sudo usermod -aG docker $USER

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.7/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

# 2. Create directory
```bash
mkdir -p ~/cysic/wallet1 ~/cysic/wallet2 ~/cysic/wallet3
mkdir -p ~/.cysic/wallet1 ~/.cysic/wallet2 ~/.cysic/wallet3
```
you can add as much as you want

# 3. Create env
```bash
cat <<EOF > ~/cysic/.env
WALLET1=0xYourWalletAddress1
WALLET2=0xYourWalletAddress2
WALLET3=0xYourWalletAddress3
EOF
```
Change your `0xyourwalletaddress` with your wallet address

