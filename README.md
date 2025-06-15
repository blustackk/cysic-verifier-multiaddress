# Cysic-Verifier-MultiAddress

# 1. Update System & Install Docker + Docker Compose
```bash
# 1. Update system
sudo apt update && sudo apt upgrade -y

# 2. Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# 3. Add user ke grup docker
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world

# 4. instalasi Docker Compose 
sudo apt install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 5. Update
sudo apt update

# 6. Install Docker Compose plugin (v2 syntax: `docker compose`)
sudo apt install -y docker-compose-plugin

```

# 2. Create directory
```bash
mkdir -p ~/cysic
mkdir -p ~/.cysic
cd ~/cysic
```

# 3. Create `env`
```bash
nano .env
```
```bash
REWARD_ADDRESS1=0xYourAddress
REWARD_ADDRESS2=0xYourAddress
REWARD_ADDRESS3=0xYourAddress
KEY_FILE1=/root/.cysic/wallet1.key
KEY_FILE2=/root/.cysic/wallet2.key
KEY_FILE3=/root/.cysic/wallet3.key
```
Change your `0xYourAdress` with your wallet address

# 4. Create `Dockerfile`
```bash
cat <<'EOF' > Dockerfile
FROM ubuntu:22.04

RUN apt-get update && \
    apt-get install -y curl git wget bash ca-certificates && \
    curl -L https://github.com/cysic-labs/cysic-phase3/releases/download/v1.0.0/setup_linux.sh -o /root/setup_linux.sh && \
    chmod +x /root/setup_linux.sh

WORKDIR /root

ENV REWARD_ADDRESS=""
ENV KEY_FILE=""

CMD ["bash", "-c", "bash /root/setup_linux.sh $REWARD_ADDRESS && cd /root/cysic-verifier && bash start.sh"]
EOF
```

# 5. Create `docker-compose.yml`
```bash
cat <<'EOF' > docker-compose.yml
services:
  verifier1:
    build:
      context: .
    container_name: cysic-verifier1
    volumes:
      - /root/.cysic:/root/.cysic
    environment:
      - REWARD_ADDRESS=${REWARD_ADDRESS1}
      - KEY_FILE=${KEY_FILE1}
    restart: unless-stopped
    env_file:
      - .env

  verifier2:
    build:
      context: .
    container_name: cysic-verifier2
    volumes:
      - /root/.cysic:/root/.cysic
    environment:
      - REWARD_ADDRESS=${REWARD_ADDRESS2}
      - KEY_FILE=${KEY_FILE2}
    restart: unless-stopped
    env_file:
      - .env

  verifier3:
    build:
      context: .
    container_name: cysic-verifier3
    volumes:
      - /root/.cysic:/root/.cysic
    environment:
      - REWARD_ADDRESS=${REWARD_ADDRESS3}
      - KEY_FILE=${KEY_FILE3}
    restart: unless-stopped
    env_file:
      - .env
EOF
```

# 6. Build and Run
```bash
docker compose up -d
```

# 7. If you want stop and delete it
```bash
cd cysic
docker compose down
cd ~
rm -rf cysic
rm -rf .cysic
```
