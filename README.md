# Prerequisites

## System Update

```bash
sudo apt update && sudo apt full-upgrade -y
```

## Curl Installation

```bash
sudo apt install curl -y
```

## Install Required Packages

```bash
sudo apt install apt-transport-https ca-certificates software-properties-common -y
```

## Docker Installation - Ubuntu

### **Add Docker APT Key**

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
```

### **Add Docker APT Repository**

```bash
echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
```

## Docker Installation - Debian

### **Add Docker APT Key**

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
```

### **Add Docker APT Repository**

```bash
echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
```

## **Install Docker**

```bash
sudo apt-get update
sudo apt-get install docker-ce -y
```

## **Download and Install Docker Compose**

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-Linux-x86_64" -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```

## Ansible Installation

```bash
sudo apt install ansible -y
```

# Homepage Docker Deployment

## Creation of Homepage Directory

1. First we need to create the directory for <mark style="color:red;">`homepage`</mark>and jump to the new directory.

```bash
mkdir /opt/homepage
cd /opt/homepage
```

2. Then we need to create a <mark style="color:red;">`.env`</mark>  file with the command <mark style="color:red;">`nano .env`</mark> with the following information:

```bash
PUID=1000
PGID=1000
```

3. This is the information of the <mark style="color:red;">`UID`</mark> and <mark style="color:red;">`GID`</mark> of your user, you can check this information by tipping <mark style="color:red;">`id username`</mark>.

## Configuration of the docker-compose file

1. We need to create a <mark style="color:red;">`docker-compose.yml`</mark> file with the following information:

```yaml
version: "3.3"
services:
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    container_name: homepage
    ports:
      - 8000:3000
    env_file: .env
    volumes:
      - ./config:/app/config
    environment:
      PUID: $PUID
      PGID: $PGID
```

## Docker Compose

1. Start the Homepage deployment using docker-compose:

```bash
docker-compose up -d
```

2. After that navigate to the dashboard with <mark style="color:red;">`http://your_ip_address:8000`</mark>.
