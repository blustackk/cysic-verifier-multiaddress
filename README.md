# Cysic-Verifier-MultiAddress
* Update Sistem & Install Docker + Docker Compose
# Update sistem
```bash
sudo apt update && sudo apt upgrade -y
```
# Install Docker
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```
# Tambahkan user ke grup docker
```bash
sudo usermod -aG docker $USER
```
# Install Docker Compose
```bash 
sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.7/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
