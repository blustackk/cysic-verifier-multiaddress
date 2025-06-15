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
mkdir -p ~/cysic
mkdir -p ~/.cysic
```
you can add as much as you want

# 3. Create `env`
```bash
nano .env
```
```bash
REWARD_ADDRESS1=0xYourAddress1
REWARD_ADDRESS2=0xYourAddress2
REWARD_ADDRESS3=0xYourAddress3
KEY_FILE1=/root/.cysic/wallet1.key
KEY_FILE2=/root/.cysic/wallet2.key
KEY_FILE3=/root/.cysic/wallet3.key
```
Change your `0xyourwalletaddress` with your wallet address

# 4. Create `Dockerfile`
```bash
nano Dockerfile
```
```bash
FROM ubuntu:22.04

RUN apt-get update && \
    apt-get install -y curl git wget bash ca-certificates && \
    curl -L https://github.com/cysic-labs/cysic-phase3/releases/download/v1.0.0/setup_linux.sh -o /root/setup_linux.sh && \
    chmod +x /root/setup_linux.sh

WORKDIR /root

ARG REWARD_ADDRESS
ARG KEY_FILE
ENV REWARD_ADDRESS=${REWARD_ADDRESS}
ENV KEY_FILE=${KEY_FILE}

CMD bash /root/setup_linux.sh ${REWARD_ADDRESS} && \
    cd /root/cysic-verifier && bash start.sh
```

# 5. Create `docker-compose.yml`
```bash
nano docker-compose.yml
```
```bash
version: '3.8'

services:
  verifier1:
    build:
      context: .
      args:
        REWARD_ADDRESS: ${REWARD_ADDRESS1}
        KEY_FILE: ${KEY_FILE1}
    container_name: cysic-verifier1
    volumes:
      - /root/.cysic:/root/.cysic
    restart: unless-stopped
    env_file:
      - .env

  verifier2:
    build:
      context: .
      args:
        REWARD_ADDRESS: ${REWARD_ADDRESS2}
        KEY_FILE: ${KEY_FILE2}
    container_name: cysic-verifier2
    volumes:
      - /root/.cysic:/root/.cysic
    restart: unless-stopped
    env_file:
      - .env

  verifier3:
    build:
      context: .
      args:
        REWARD_ADDRESS: ${REWARD_ADDRESS3}
        KEY_FILE: ${KEY_FILE3}
    container_name: cysic-verifier3
    volumes:
      - /root/.cysic:/root/.cysic
    restart: unless-stopped
    env_file:
      - .env
```

# 6. Run docker
```bash
docker-compose up -d
```
