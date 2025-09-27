# GitHub Actions Managed-Host Hotstar Website CICD DevOps — Setup Guide


This README collects useful commands and links to install common DevOps, CI/CD, and security tooling on Ubuntu systems. Always review commands for your environment and needs.

> **Note:** Replace all `<VERSION>`, `<your-server-ip>`, `<jenkins-ip>`, `<sonar-ip-address>`, `<ACCOUNT_ID>`, and similar placeholders with your actual values.

---
![img alt](https://github.com/hanumantrh/gitHub-action-hotstar/blob/main/image.png?raw=true)
---

## Table of Contents

- [Ports to Enable in Security Group](#ports-to-enable-in-security-group)
- [Prerequisites](#prerequisites)
- [System Update & Common Packages](#system-update--common-packages)
- [Docker](#docker)
- [SonarQube (Docker)](#sonarqube-docker)
- [npm Installation](#npm-installation)



---

## Ports to Enable in Security Group

| Service    | Port  |
|------------|-------|
| HTTP       | 80    |
| SSH        | 22    |
| SonarQube  | 9000  |

---
## System Update & Common Packages

```bash
sudo apt update
sudo apt upgrade -y

# Common tools
sudo apt install -y bash-completion wget git zip unzip curl jq net-tools build-essential ca-certificates apt-transport-https gnupg fontconfig
```

Reload bash completion if needed:
```bash
source /etc/bash_completion
```

**Install latest Git:**
```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git -y
```

---

## Docker

Official docs: [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add user to docker group (log out / in or newgrp to apply)
sudo usermod -aG docker $USER
newgrp docker
docker ps
```


Check Docker status:
```bash
sudo systemctl status docker
```

---


## SonarQube (Docker)

```bash
docker run -d --name sonarqube \
  -p 9000:9000 \
  -v sonarqube_data:/opt/sonarqube/data \
  -v sonarqube_logs:/opt/sonarqube/logs \
  -v sonarqube_extensions:/opt/sonarqube/extensions \
  sonarqube:lts-community
```

---


## Github Credentials to Store

| Purpose       | ID            | Type          | Notes                               |
|---------------|---------------|---------------|-------------------------------------|
| Email         | EMAIL_USER    | emailaddress@gmail.com|                                  |
| Email-app-Pass     | EMAIL_PASS   | Secret text   | From app password         |
| Docker-username    | DOCKER_USERNAME   | your-docker-id   | From your Docker Hub profile       |
| Docker-username    | DOCKER_PASSWORD   | token   | From your Docker Hub token       |
| sonar-qube    | follow the same step | manully  entries
