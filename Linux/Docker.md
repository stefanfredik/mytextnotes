# Instalasi DOCKER

## Setup REPO

Update Repo

```bash
apt update
```

Install beberapa packet yang dibutuhkan

```bash
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

Menambahkan official GPG KEY dari DOCKER

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Menambahkan Repo Docker

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Update Repo

```
sudo apt update
```

## Install DOCKER Engine

Update Repo dan KEY

```
 sudo chmod a+r /etc/apt/keyrings/docker.gpgs
 sudo apt-get update
```

Install Docker dan beberapa aplikasi yang dibutuhkan

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
